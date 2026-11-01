# 📡 Module 2: Enumeration

> **Scanning ke baad enumeration — services se information nikalo!**

---

## 🔵 NetBIOS Enumeration (Port 137-139)

```bash
# Windows CMD — NetBIOS name table
nbtstat -a <target_ip>
nbtstat -c                   # Cache table

# Nmap NSE script
nmap -sV -v --script nbstat.nse <target_ip>

# enum4linux (Linux — Best for SMB/NetBIOS)
enum4linux -a <target_ip>                           # All info
enum4linux -u <user> -p <pass> -a <target_ip>       # With creds
enum4linux -u <user> -p <pass> -n <target_ip>       # Namelist
enum4linux -u <user> -p <pass> -P <target_ip>       # Password policy
enum4linux -u <user> -p <pass> -o <target_ip>       # OS info
enum4linux -u <user> -p <pass> -G <target_ip>       # Group info
enum4linux -u <user> -p <pass> -S <target_ip>       # Share info

# Net view (Windows CMD)
net view \\<target_ip>
net view /domain
```

### NetBIOS Name Suffixes
| Suffix | Type | Meaning |
|--------|------|---------|
| `<00>` | UNIQUE | Workstation |
| `<00>` | GROUP | Domain name |
| `<03>` | UNIQUE | Messenger Service |
| `<20>` | UNIQUE | File Server |
| `<1D>` | UNIQUE | Master Browser |
| `<1B>` | UNIQUE | Domain Master Browser |

---

## 📧 SNMP Enumeration (Port 161 - UDP)

```bash
# Check if SNMP port is open
nmap -sU -p 161 <target_ip>

# SNMP enumeration (Parrot/Kali)
snmp-check <target_ip>

# Nmap SNMP scripts
nmap -sU -p 161 --script snmp-brute <target_ip>
nmap -sU -p 161 --script snmp-info <target_ip>
nmap -sU -p 161 --script snmp-processes <target_ip>
nmap -sU -p 161 --script snmp-win32-software <target_ip>
nmap -sU -p 161 --script snmp-interfaces <target_ip>

# snmpwalk
snmpwalk -v1 -c public <target_ip>
snmpwalk -v2c -c public <target_ip>

# Community strings
# Default: public (read-only), private (read-write)
```

---

## 🌐 DNS Enumeration (Port 53)

```bash
# DNS Recon
dnsrecon -d <domain> -z

# DNS zone transfer
dnsrecon -d <domain> -t axfr
dig axfr <domain> @<dns_server>

# Nmap DNS scripts
nmap --script dns-brute <domain>
nmap -T4 -p 53 --script dns-brute <domain>

# nslookup
nslookup
> set type=any
> <domain>

# dig
dig <domain> ANY
```

---

## 📂 SMB Enumeration (Port 445, 139)

```bash
# Nmap SMB scripts
nmap -p 445 --script smb-os-discovery <target_ip>
nmap -p 445 --script smb-enum-shares <target_ip>
nmap -p 445 --script smb-enum-users <target_ip>
nmap -p 445 --script smb-enum-groups <target_ip>
nmap -p 445 --script smb-enum-services <target_ip>
nmap -p 445 --script smb-vuln* <target_ip>

# smbclient (list shares)
smbclient -L \\\\<target_ip>
smbclient \\\\<target_ip>\\<share_name> -U <username>

# enum4linux
enum4linux -S <target_ip>    # Shares
enum4linux -U <target_ip>    # Users
```

---

## 📒 LDAP Enumeration (Port 389)

```bash
# Nmap LDAP scripts
nmap -p 389 --script ldap-brute <target_ip>
nmap -p 389 --script ldap-search <target_ip>
nmap -p 389 -sV <target_ip>

# Find FQDN
nmap -p 389 -sV <target_ip>
nmap -p 389 -sV -iL targets.txt

# ldapsearch
ldapsearch -x -h <target_ip> -b "dc=example,dc=com"
ldapsearch -x -h <target_ip> -s base namingcontexts
```

---

## 📨 SMTP Enumeration (Port 25)

```bash
# Nmap SMTP scripts
nmap -p 25 --script smtp-enum-users <target_ip>
nmap -p 25 --script smtp-open-relay <target_ip>
nmap -p 25 --script smtp-commands <target_ip>

# Manual SMTP enum (telnet)
telnet <target_ip> 25
VRFY root
VRFY admin
EXPN admin
```

---

## 📦 FTP Enumeration (Port 21)

```bash
# Check FTP port
nmap -p 21 -sV <target_ip>

# Nmap FTP scripts
nmap -p 21 --script ftp-anon <target_ip>        # Anonymous login check
nmap -p 21 --script ftp-brute <target_ip>        # Brute force

# FTP connect
ftp <target_ip>
# Try: anonymous / anonymous
```

---

## 🗄️ MySQL Enumeration (Port 3306)

```bash
# Check MySQL port
nmap -p 3306 -sV <target_ip>

# Nmap MySQL scripts
nmap -p 3306 --script mysql-info <target_ip>
nmap -p 3306 --script mysql-enum <target_ip>
nmap -p 3306 --script mysql-brute <target_ip>
nmap -p 3306 --script mysql-databases --script-args mysqluser=root,mysqlpass=<pass> <target_ip>

# MySQL connect
mysql -u root -p -h <target_ip>
```

---

## 🔐 RDP Check (Port 3389)

```bash
# Check RDP
nmap -p 3389 <target_ip>
nmap -p 3389 -iL targets.txt | grep open

# RDP connection
rdesktop <target_ip>
xfreerdp /u:<user> /p:<pass> /v:<target_ip>
```

---

## ✅ Enumeration Quick Cheat Sheet

| Service | Port | Best Tool | Command |
|---------|------|-----------|---------|
| NetBIOS | 137-139 | enum4linux | `enum4linux -a <ip>` |
| SMB | 445 | nmap | `nmap -p445 --script smb-os-discovery <ip>` |
| SNMP | 161(UDP) | snmp-check | `snmp-check <ip>` |
| DNS | 53 | dnsrecon | `dnsrecon -d <domain> -z` |
| LDAP | 389 | nmap | `nmap -p389 -sV <ip>` |
| SMTP | 25 | nmap | `nmap -p25 --script smtp-enum-users <ip>` |
| FTP | 21 | nmap | `nmap -p21 --script ftp-anon <ip>` |
| MySQL | 3306 | nmap | `nmap -p3306 --script mysql-info <ip>` |
| RDP | 3389 | nmap | `nmap -p3389 <ip>` |

