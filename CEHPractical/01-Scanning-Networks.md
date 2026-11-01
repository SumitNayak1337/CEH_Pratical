# 🔍 Module 1: Scanning Networks

> **Exam mein sabse pehle ye karna hai — target dhundho, ports dhundho, OS dhundho!**

---

## 🚀 Step 0: Always Be Root
```bash
sudo su
```

---

## ⚡ STEP 1: SABSE PEHLE — Alive Hosts Dhundho (ARP Scan / Netdiscover)

> **⚠️ Exam mein bahut saare IPs hote hai subnet mein. Pehle ye karo taaki pata chale kaun kaun ALIVE hai!**

### 🥇 Netdiscover (Best for LAN — Passive + Active ARP Scan)
```bash
# Active scan — subnet ke saare alive hosts dikhayega
netdiscover -i eth0 -r 192.168.1.0/24

# Specific range scan
netdiscover -i eth0 -r 10.10.10.0/24

# Passive mode (chupke se — sirf listen karo, packets mat bhejo)
netdiscover -i eth0 -p

# Fast scan with count limit
netdiscover -i eth0 -r 192.168.1.0/24 -c 5
```

#### Netdiscover Output Samjho:
```
 _____________________________________________________________________________
   IP            At MAC Address     Count     Len  MAC Vendor / Hostname
 -----------------------------------------------------------------------------
   192.168.1.1     aa:bb:cc:dd:ee:01    1      60  Cisco Systems
   192.168.1.10    aa:bb:cc:dd:ee:02    1      60  VMware, Inc.
   192.168.1.20    aa:bb:cc:dd:ee:03    1      60  Microsoft Corporation
```
> 💡 **Jo IPs dikhe woh ALIVE hai — inhi pe aage scan karo!**

#### Netdiscover Flags:
| Flag | Meaning |
|------|---------|
| `-i` | Interface (eth0, wlan0) |
| `-r` | Range/Subnet to scan |
| `-p` | Passive mode (only listen) |
| `-c` | Number of ARP requests per IP |
| `-s` | Sleep time between requests (ms) |
| `-l` | Read list of ranges from file |

---

### 🥈 ARP-Scan (Fast & Accurate)
```bash
# Scan entire subnet
arp-scan -l
arp-scan --localnet

# Scan specific range
arp-scan -I eth0 192.168.1.0/24

# Scan specific interface
arp-scan -I eth0 -l

# Scan from a file
arp-scan -I eth0 -f targets.txt
```

#### ARP-Scan Flags:
| Flag | Meaning |
|------|---------|
| `-l` | Scan local network |
| `-I` | Specify interface |
| `-f` | Read targets from file |
| `--localnet` | Scan local network (same as -l) |

---

### 🥉 Nmap Host Discovery
```bash
# Ping Sweep — subnet mein kaun kaun alive hai
nmap -sn 192.168.1.0/24

# ARP ping (local network only — very accurate)
nmap -PR 192.168.1.0/24

# List Scan (DNS resolution only, no scan)
nmap -sL 192.168.1.0/24

# Save alive hosts to file (for later use)
nmap -sn 192.168.1.0/24 -oG - | grep "Up" | awk '{print $2}' > alive_hosts.txt

# Then scan only alive hosts
nmap -A -iL alive_hosts.txt
```

### Hping3 Host Discovery
```bash
# ICMP ping
hping3 -1 192.168.1.10 -c 3
```

---

### ✅ EXAM WORKFLOW — Pehla Step Hamesha:
```
1. sudo su
2. netdiscover -i eth0 -r <subnet>/24      ← Alive hosts dhundho
   ya: arp-scan -I eth0 <subnet>/24
   ya: nmap -sn <subnet>/24
3. Alive IPs note karo
4. Phir target pe: nmap -A -T4 <alive_ip>  ← Detail scan
```

---

## 🔎 Port Scanning

### Basic Scans
```bash
# Full aggressive scan (OS + Version + Scripts + Traceroute)
nmap -A -T4 <target_ip>

# TCP Connect Scan (full handshake)
nmap -sT -v <target_ip>

# SYN Scan / Half-Open / Stealth Scan
nmap -sS -v <target_ip>

# UDP Scan
nmap -sU -v <target_ip>

# Scan ALL ports (1-65535)
nmap -p- <target_ip>

# Scan specific ports
nmap -p 21,22,80,443,3389 <target_ip>

# Top 100 ports
nmap --top-ports 100 <target_ip>

# Scan a range of IPs
nmap -sT -v 192.168.1.1-254

# Scan from a file
nmap -iL targets.txt
```

### Scan Types Explained
| Flag | Scan Type | Use Case |
|------|-----------|----------|
| `-sT` | TCP Connect | Full connection, reliable but noisy |
| `-sS` | SYN/Stealth | Half-open, fast & stealthy |
| `-sU` | UDP | For DNS(53), SNMP(161), DHCP(67) |
| `-sN` | NULL | No flags set, evasion |
| `-sF` | FIN | Only FIN flag, evasion |
| `-sX` | XMAS | FIN+PSH+URG flags, evasion |
| `-sA` | ACK | Firewall rule detection |
| `-sV` | Version | Service version detection |
| `-O`  | OS | OS detection |
| `-A`  | Aggressive | OS + Version + Scripts + Traceroute |

---

## 🖥️ OS Detection

```bash
# OS Detection
nmap -O <target_ip>

# Aggressive scan (includes OS)
nmap -A <target_ip>

# SMB OS Discovery (for Windows targets)
nmap --script smb-os-discovery.nse <target_ip>
```

### TTL Values → OS Identification (Wireshark)
| TTL Value | Operating System |
|-----------|-----------------|
| **64** | Linux / macOS / FreeBSD |
| **128** | Windows |
| **254** | Solaris / AIX |
| **255** | Cisco / Network devices |

> 💡 **Wireshark mein**: Filter `icmp`, check TTL field in packet details

---

## 🛡️ Firewall/IDS Evasion Scans

```bash
# Fragment packets (bypass firewall)
nmap -f <target_ip>

# Specific MTU
nmap --mtu 24 <target_ip>

# Decoy scan (hide your IP)
nmap -D RND:10 <target_ip>

# Source port manipulation
nmap -g 80 <target_ip>
nmap --source-port 53 <target_ip>

# Append random data
nmap --data-length 25 <target_ip>

# MAC address spoofing
nmap --spoof-mac 0 <target_ip>

# Timing (slow scan to avoid detection)
nmap -T0 <target_ip>    # Paranoid
nmap -T1 <target_ip>    # Sneaky
```

---

## 🔧 Hping3 (Packet Crafting)

```bash
# SYN scan on port 80
hping3 -S <target_ip> -p 80 -c 5

# ACK scan (firewall detection)
hping3 -A <target_ip> -p 80 -c 5

# UDP scan
hping3 -2 <target_ip> -p 161 -c 5

# ICMP ping
hping3 -1 <target_ip> -c 5

# SYN Flood (DoS)
hping3 -S --flood -V -p 80 <target_ip>

# Scan a range of ports
hping3 -8 1-1000 -S <target_ip>
```

| Hping3 Flag | Meaning |
|-------------|---------|
| `-S` | SYN flag |
| `-A` | ACK flag |
| `-F` | FIN flag |
| `-1` | ICMP mode |
| `-2` | UDP mode |
| `-p` | Port number |
| `-c` | Packet count |
| `--flood` | Send packets as fast as possible |

---

## 📋 Common Port Numbers (Yaad Rakho!)

| Port | Service |
|------|---------|
| 21 | FTP |
| 22 | SSH |
| 23 | Telnet |
| 25 | SMTP |
| 53 | DNS |
| 67/68 | DHCP |
| 80 | HTTP |
| 110 | POP3 |
| 111 | RPCBind |
| 135 | MSRPC |
| 137-139 | NetBIOS |
| 143 | IMAP |
| 161 | SNMP |
| 389 | LDAP |
| 443 | HTTPS |
| 445 | SMB |
| 993 | IMAPS |
| 995 | POP3S |
| 1433 | MS SQL |
| 3306 | MySQL |
| 3389 | RDP |
| 5555 | ADB (Android) |
| 8080 | HTTP Proxy |

---

## ✅ Quick Exam Workflow
```
1. sudo su
2. nmap -sn <subnet>/24          → Alive hosts
3. nmap -A -T4 <target>          → Full scan
4. nmap -O <target>              → OS detect
5. nmap -sV -p- <target>         → All ports + versions
6. Note down: IP, OS, Open Ports, Services
```

