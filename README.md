# 🛡️ CEH Practical Exam — Ultimate Cheat Sheet

> **100+ students ne is repo se exam clear kiya hai. Ab ye UPGRADED version hai — exam hall mein open karke use karo!**

![CEH](https://img.shields.io/badge/CEH-Practical-red?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Exam%20Ready-brightgreen?style=for-the-badge)
![Open Book](https://img.shields.io/badge/Open%20Book-Allowed-blue?style=for-the-badge)

---

## 📋 Exam Info
| Detail | Info |
|--------|------|
| **Duration** | 6 Hours |
| **Questions** | 20 Practical Challenges |
| **Passing Score** | 70% (14/20) |
| **Format** | iLabs Environment (Parrot OS + Windows) |
| **Open Book** | ✅ Yes — CMD, Browser, Tools sab use kar sakte ho |

---

## 📂 Quick Navigation — Module Wise

| # | Module | File | Key Tools |
|---|--------|------|-----------|
| 1 | 🔍 Scanning & Recon | [01-Scanning-Networks.md](CEHPractical/01-Scanning-Networks.md) | Nmap, Hping3, Netdiscover |
| 2 | 📡 Enumeration | [02-Enumeration.md](CEHPractical/02-Enumeration.md) | Nmap NSE, enum4linux, SNMP, LDAP |
| 3 | 💻 System Hacking | [03-System-Hacking.md](CEHPractical/03-System-Hacking.md) | Hydra, John, Hashcat, Metasploit |
| 4 | 🌐 Web App Hacking | [04-Web-Application-Hacking.md](CEHPractical/04-Web-Application-Hacking.md) | SQLMap, Burp, ZAP, WPScan, Gobuster |
| 5 | 💉 SQL Injection | [05-SQL-Injection.md](CEHPractical/05-SQL-Injection.md) | SQLMap, Manual Payloads |
| 6 | 📶 Sniffing | [06-Sniffing.md](CEHPractical/06-Sniffing.md) | Wireshark Filters |
| 7 | 🔐 Cryptography & Stego | [07-Cryptography-Steganography.md](CEHPractical/07-Cryptography-Steganography.md) | OpenStego, Snow, HashCalc, Veracrypt |
| 8 | 📱 Android Hacking | [08-Android-Hacking.md](CEHPractical/08-Android-Hacking.md) | ADB, PhoneSploit |
| 9 | 📡 WiFi Cracking | [09-WiFi-Cracking.md](CEHPractical/09-WiFi-Cracking.md) | Aircrack-ng |
| 10 | 🖥️ Web Server Hacking | [10-Web-Server-Hacking.md](CEHPractical/10-Web-Server-Hacking.md) | Hydra, Nikto, Netcat |
| 11 | 🛡️ IDS/Firewall Evasion | [11-IDS-Firewall-Evasion.md](CEHPractical/11-IDS-Firewall-Evasion.md) | Nmap Evasion, Hping3 |
| 12 | ☁️ Cloud & IoT | [12-Cloud-IoT.md](CEHPractical/12-Cloud-IoT.md) | S3Scanner, Shodan |
| 13 | ⚡ Quick Reference | [13-Quick-Reference.md](CEHPractical/13-Quick-Reference.md) | All-in-One Cheat Sheet |

---

## 🎯 Exam Day Strategy

### ⏰ Time Management
- **6 hours = 360 minutes** → ~18 min per question
- Easy questions pehle solve karo (Scanning, Enum, Stego)
- SQL Injection aur System Hacking mein zyada time lagta hai

### 🔑 First Steps (Har Question Mein)
```bash
# Step 1: Root ban jao
sudo su

# Step 2: Network scan karo — alive hosts dhundho
nmap -sn 192.168.1.0/24

# Step 3: Aggressive scan on target
nmap -A -T4 <target_ip>

# Step 4: OS aur services identify karo
nmap -O -sV <target_ip>
```

### 💡 Pro Tips
1. **Pehle nmap -A karo** — Almost har question ka starting point yahi hai
2. **Wireshark** — Agar .pcap file di hai toh filters use karo
3. **Hydra** — Password cracking ke liye wordlist dhundho (Desktop/CEH_TOOLS/Wordlists)
4. **SQLMap** — Cookie copy karna mat bhulna
5. **Screenshots lo** — Evidence ke liye kaam aata hai

---

## ⭐ Star this repo and share with your batchmates!

> Made with ❤️ for CEH Practical aspirants

