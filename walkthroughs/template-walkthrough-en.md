# Walkthrough Template (EN)

> **Note:** This file is an example template in English. Also create a French version (template-walkthrough-fr.md)

## 📋 Information

- **Machine:** [Machine Name]
- **Platform:** HackTheBox / TryHackMe / VulnHub / Other
- **OS:** Linux / Windows
- **Difficulty:** Easy / Medium / Hard / Insane
- **Solved Date:** [Date]

---

## 🎯 Summary

Brief description of the machine and techniques used to solve it.

**Required Skills:**
- Skill 1
- Skill 2
- Skill 3

**Skills Learned:**
- New skill 1
- New skill 2

---

## 🔍 Reconnaissance

### Nmap Scan

```bash
nmap -sC -sV -oN nmap/initial 10.10.10.X
```

**Results:**
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1
80/tcp open  http    Apache httpd 2.4.41
```

### Web Enumeration

```bash
gobuster dir -u http://10.10.10.X -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

**Important Discoveries:**
- `/admin` - Admin panel
- `/upload` - File upload feature
- `/backup` - Backup files

---

## 🚪 Initial Foothold

### Identified Vulnerability

Description of the found vulnerability (SQL Injection, XSS, File Upload, etc.)

**Example vulnerable request:**
```http
POST /login HTTP/1.1
Host: 10.10.10.X
Content-Type: application/x-www-form-urlencoded

username=admin' OR 1=1--&password=anything
```

### Exploitation

Detailed steps to exploit the vulnerability:

1. **Step 1:** Description
```bash
command used
```

2. **Step 2:** Description
```bash
another command
```

### Initial Shell

```bash
nc -lvnp 4444
```

Getting a shell as `www-data` or another user.

---

## 👤 User Flag

### Local Enumeration

```bash
# Check users
cat /etc/passwd | grep -v nologin

# Check SUID files
find / -perm -4000 2>/dev/null

# Check capabilities
getcap -r / 2>/dev/null
```

### User Escalation

Description of the method used to get user access.

**User Flag:**
```
user_flag_here
```

---

## 🔐 Root Flag

### Privilege Enumeration

```bash
# Check sudo
sudo -l

# Check groups
id

# Check running processes
ps aux
```

### Privilege Escalation

Detailed description of the privilege escalation technique used.

**Exploit Used:**
```bash
# Commands for exploitation
exploit command here
```

### Root Access

```bash
cat /root/root.txt
```

**Root Flag:**
```
root_flag_here
```

---

## 🎓 Lessons Learned

### What I Learned

1. **Technique 1:** Description of what was learned
2. **Technique 2:** Description of what was learned
3. **New Tool:** How to use it

### Mistakes Made

- Mistake 1 and how I fixed it
- Mistake 2 and the lesson learned

### Key Points to Remember

- ⚠️ Always check X before Y
- 💡 Think about Z during enumeration
- 🔑 Don't forget to...

---

## 🛠️ Tools Used

- **Nmap** - Port scanning and enumeration
- **Gobuster** - Directory brute-forcing
- **Burp Suite** - Web request analysis
- **Metasploit** - Exploitation framework
- **LinPEAS** - Linux enumeration
- Other tools...

---

## 📚 Resources

- [Link to relevant documentation](https://example.com)
- [Blog article about the vulnerability](https://example.com)
- [CVE or reference](https://example.com)

---

## ⚠️ Disclaimer

This walkthrough is for educational purposes only. Use these techniques only on machines you have permission to access.

---

**Written:** [Date]  
**Author:** Samuel Tesson
