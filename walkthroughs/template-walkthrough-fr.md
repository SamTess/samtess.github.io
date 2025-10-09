# Template de Walkthrough (FR)

> **Note:** Ce fichier est un template exemple en français. Créez aussi une version anglaise (template-walkthrough-en.md)

## 📋 Informations

- **Machine:** [Nom de la machine]
- **Plateforme:** HackTheBox / TryHackMe / VulnHub / Autre
- **OS:** Linux / Windows
- **Difficulté:** Easy / Medium / Hard / Insane
- **Date de résolution:** [Date]

---

## 🎯 Résumé

Brève description de la machine et des techniques utilisées pour la résoudre.

**Compétences requises:**
- Compétence 1
- Compétence 2
- Compétence 3

**Compétences apprises:**
- Nouvelle compétence 1
- Nouvelle compétence 2

---

## 🔍 Reconnaissance

### Scan Nmap

```bash
nmap -sC -sV -oN nmap/initial 10.10.10.X
```

**Résultats:**
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1
80/tcp open  http    Apache httpd 2.4.41
```

### Énumération Web

```bash
gobuster dir -u http://10.10.10.X -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

**Découvertes importantes:**
- `/admin` - Panel d'administration
- `/upload` - Fonctionnalité d'upload de fichiers
- `/backup` - Fichiers de backup

---

## 🚪 Point d'entrée initial

### Vulnérabilité identifiée

Description de la vulnérabilité trouvée (SQL Injection, XSS, File Upload, etc.)

**Exemple de requête vulnérable:**
```http
POST /login HTTP/1.1
Host: 10.10.10.X
Content-Type: application/x-www-form-urlencoded

username=admin' OR 1=1--&password=anything
```

### Exploitation

Étapes détaillées pour exploiter la vulnérabilité:

1. **Étape 1:** Description
```bash
commande utilisée
```

2. **Étape 2:** Description
```bash
autre commande
```

### Shell initial

```bash
nc -lvnp 4444
```

Obtention d'un shell en tant que `www-data` ou autre utilisateur.

---

## 👤 User Flag

### Énumération locale

```bash
# Vérifier les utilisateurs
cat /etc/passwd | grep -v nologin

# Vérifier les fichiers SUID
find / -perm -4000 2>/dev/null

# Vérifier les capabilities
getcap -r / 2>/dev/null
```

### Élévation vers l'utilisateur

Description de la méthode utilisée pour obtenir l'accès utilisateur.

**Flag User:**
```
user_flag_here
```

---

## 🔐 Root Flag

### Énumération privilèges

```bash
# Vérifier sudo
sudo -l

# Vérifier les groupes
id

# Vérifier les processus en cours
ps aux
```

### Privilege Escalation

Description détaillée de la technique d'élévation de privilèges utilisée.

**Exploit utilisé:**
```bash
# Commandes pour l'exploitation
exploit command here
```

### Root Access

```bash
cat /root/root.txt
```

**Flag Root:**
```
root_flag_here
```

---

## 🎓 Lessons Learned

### Ce que j'ai appris

1. **Technique 1:** Description de ce qui a été appris
2. **Technique 2:** Description de ce qui a été appris
3. **Outil nouveau:** Comment l'utiliser

### Erreurs commises

- Erreur 1 et comment je l'ai corrigée
- Erreur 2 et la leçon apprise

### Points clés à retenir

- ⚠️ Toujours vérifier X avant Y
- 💡 Penser à Z lors de l'énumération
- 🔑 Ne pas oublier de...

---

## 🛠️ Outils utilisés

- **Nmap** - Scan de ports et énumération
- **Gobuster** - Directory brute-forcing
- **Burp Suite** - Analyse des requêtes web
- **Metasploit** - Framework d'exploitation
- **LinPEAS** - Énumération Linux
- Autres outils...

---

## 📚 Ressources

- [Lien vers documentation pertinente](https://example.com)
- [Article de blog sur la vulnérabilité](https://example.com)
- [CVE ou référence](https://example.com)

---

## ⚠️ Disclaimer

Ce walkthrough est à but éducatif uniquement. N'utilisez ces techniques que sur des machines auxquelles vous avez l'autorisation d'accéder.

---

**Date de rédaction:** [Date]  
**Auteur:** Samuel Tesson
