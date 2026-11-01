# 🖥️ Module 10: Web Server Hacking

> **Web servers pe attack karo — FTP crack, netcat recon, nikto scan!**

---

## 🔍 Web Server Footprinting

### Netcat & Telnet (Banner Grabbing)
```bash
# Netcat banner grab
nc -vv <target_ip> 80
GET / HTTP/1.0
# Press Enter twice

# Alternative
echo "GET / HTTP/1.0\r\n\r\n" | nc <target_ip> 80

# Telnet banner grab
telnet <target_ip> 80
GET / HTTP/1.0
# Press Enter twice

# Grab specific service banners
nc -vv <target_ip> 21     # FTP
nc -vv <target_ip> 22     # SSH
nc -vv <target_ip> 25     # SMTP
nc -vv <target_ip> 443    # HTTPS
```

### Nmap Service/Version Scan
```bash
# Service version detection
nmap -sV <target_ip>

# HTTP enumeration
nmap -sV --script http-enum <target_ip>

# HTTP methods
nmap --script http-methods <target_ip>

# HTTP headers
nmap --script http-headers <target_ip>

# Web Application Firewall detection
nmap --script http-waf-detect <target_ip>
```

---

## 🔎 Nikto (Web Vulnerability Scanner)

```bash
# Basic scan
nikto -h http://<target_ip>

# Scan specific port
nikto -h http://<target_ip>:8080

# Scan with SSL
nikto -h https://<target_ip>

# Save output
nikto -h http://<target_ip> -o report.html -Format html

# Scan with authentication
nikto -h http://<target_ip> -id admin:password
```

---

## 🔓 FTP Server Hacking

### Check FTP Access
```bash
# Scan FTP port
nmap -p 21 -sV <target_ip>

# Check anonymous login
nmap -p 21 --script ftp-anon <target_ip>

# Connect to FTP
ftp <target_ip>
# Username: anonymous
# Password: anonymous (or blank)

# FTP Commands:
ls                    # List files
cd <directory>        # Change directory
get <filename>        # Download file
put <filename>        # Upload file
mget *                # Download all files
pwd                   # Current directory
bye                   # Exit
```

### Brute Force FTP
```bash
# Hydra
hydra -L usernames.txt -P passwords.txt ftp://<target_ip>
hydra -l admin -P /home/attacker/Desktop/CEH_TOOLS/Wordlists/Password.txt ftp://<target_ip>

# Nmap brute force
nmap -p 21 --script ftp-brute <target_ip>

# Medusa
medusa -h <target_ip> -U users.txt -P pass.txt -M ftp
```

---

## 🔓 SSH Server Hacking

```bash
# Brute Force SSH
hydra -L usernames.txt -P passwords.txt ssh://<target_ip>
hydra -l root -P passwords.txt ssh://<target_ip>

# Nmap SSH brute
nmap -p 22 --script ssh-brute <target_ip>

# Connect after cracking
ssh <user>@<target_ip>
```

---

## 🔓 Telnet Hacking

```bash
# Check Telnet
nmap -p 23 -sV <target_ip>

# Brute Force
hydra -L users.txt -P pass.txt telnet://<target_ip>

# Connect
telnet <target_ip>
```

---

## 🌐 Whatweb (Website Fingerprinting)

```bash
# Basic scan
whatweb <target_url>

# Verbose
whatweb -v <target_url>

# Aggressive
whatweb -a 3 <target_url>
```

---

## ✅ Web Server Hacking Quick Workflow
```
1. nmap -sV -p 21,22,80,443,8080 <target>  → Find services
2. nikto -h http://<target>                  → Vulnerability scan
3. nc -vv <target> 80 → GET / HTTP/1.0       → Banner grab
4. If FTP open: Try anonymous login
5. If login required: hydra -l user -P pass.txt ftp://<target>
6. Login with found credentials
```

