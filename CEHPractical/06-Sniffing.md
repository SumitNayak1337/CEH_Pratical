# 📶 Module 6: Sniffing (Wireshark)

> **Wireshark se packets analyze karo — passwords dhundho, attacks detect karo!**

---

## 🚀 Wireshark Basics

```bash
# Launch Wireshark
wireshark

# Open a capture file
wireshark capture.pcap
```

---

## 🔍 Essential Wireshark Filters

### Display Filters (MUST KNOW!)

| Filter | Purpose |
|--------|---------|
| `http` | Show only HTTP traffic |
| `dns` | Show only DNS traffic |
| `tcp` | Show only TCP traffic |
| `udp` | Show only UDP traffic |
| `icmp` | Show only ICMP (ping) traffic |
| `arp` | Show only ARP traffic |
| `ftp` | Show only FTP traffic |
| `ssh` | Show only SSH traffic |
| `telnet` | Show only Telnet traffic |
| `smtp` | Show only SMTP traffic |

### IP Based Filters
```
ip.addr == 192.168.1.10              # Traffic to/from this IP
ip.src == 192.168.1.10               # Traffic FROM this IP
ip.dst == 192.168.1.10               # Traffic TO this IP
ip.addr == 192.168.1.0/24            # Entire subnet
!(ip.addr == 192.168.1.10)           # Exclude this IP
```

### Port Based Filters
```
tcp.port == 80                        # HTTP traffic
tcp.port == 443                       # HTTPS traffic
tcp.port == 21                        # FTP
tcp.port == 22                        # SSH
tcp.port == 23                        # Telnet
tcp.port == 3389                      # RDP
udp.port == 53                        # DNS
udp.port == 161                       # SNMP
```

### HTTP Filters (Password Finding!)
```
http.request.method == POST           # POST requests (passwords!)
http.request.method == GET            # GET requests
http.request.uri contains "login"     # Login pages
http.request.uri contains "admin"     # Admin pages
http.host == "target.com"             # Specific host
http.cookie                           # Requests with cookies
```

### TCP Flags Filters (Attack Detection!)
```
tcp.flags.syn == 1 and tcp.flags.ack == 0     # SYN packets (new connections / machines count)
tcp.flags.syn == 1                             # SYN packets (DoS attack source)
tcp.flags.reset == 1                           # RST packets
tcp.flags.fin == 1                             # FIN packets
tcp.flags == 0x002                             # SYN only
tcp.flags == 0x012                             # SYN-ACK
```

### Protocol Specific
```
tls                                   # TLS/SSL traffic
dhcp                                  # DHCP traffic
nbns                                  # NetBIOS Name Service
smb                                   # SMB traffic
smb2                                  # SMB2 traffic
```

---

## 🔓 Finding Passwords in Wireshark

### Method 1: HTTP POST Filter
```
1. Apply filter: http.request.method == POST
2. Click on each POST request
3. Expand "HTML Form URL Encoded" in packet details
4. Look for username/password fields
```

### Method 2: String Search
```
1. Edit → Find Packet
2. Change "Display Filter" to "String"
3. Change "Packet list" to "Packet details"
4. Change encoding to "Narrow UTF-8 & ASCII"
5. Search for: pwd, password, passwd, login, user
```

### Method 3: Credentials Tool
```
1. Go to: Tools → Credentials
2. Shows all captured credentials automatically
```

### Method 4: Follow TCP Stream
```
1. Right-click on packet → Follow → TCP Stream
2. Look for login/password data in the stream
3. Green = sent by client, Red = sent by server
```

---

## 🖥️ Finding Number of Machines (DoS Detection)

### How Many Machines in the Capture?
```
# Filter for unique source IPs with SYN (new connections)
tcp.flags.syn == 1 and tcp.flags.ack == 0

# Then go to:
Statistics → IPv4 Statistics → Source and Destination Addresses
# Count unique IPs
```

### Which Machine is Doing DoS?
```
# Filter
tcp.flags.syn == 1

# Go to: Statistics → Conversations → IPv4 tab
# Sort by packets — highest count = attacker

# Or: Statistics → Endpoints → IPv4 tab
```

---

## 🕵️ OS Detection via TTL (ICMP)

```
1. Filter: icmp
2. Click on ICMP packet
3. Expand "Internet Protocol Version 4"
4. Check "Time to Live" value
```

| TTL | OS |
|-----|-----|
| 64 | Linux / macOS |
| 128 | Windows |
| 254 | Solaris / AIX |
| 255 | Cisco / Network Device |

---

## 📊 Useful Statistics Menu

| Menu Path | Use Case |
|-----------|----------|
| Statistics → Protocol Hierarchy | Overview of protocols used |
| Statistics → Conversations | See all connections between IPs |
| Statistics → Endpoints | All unique IPs |
| Statistics → IPv4 Statistics | IP address stats |
| Statistics → HTTP → Requests | All HTTP requests |
| Statistics → IO Graph | Traffic patterns over time |

---

## 🔐 Analyzing Specific Attacks

### ARP Poisoning Detection
```
# Filter
arp

# Look for:
# - Duplicate IP addresses with different MACs
# - Gratuitous ARP packets
# - ARP reply without ARP request
```

### DNS Spoofing Detection
```
# Filter
dns

# Look for:
# - Multiple answers for same query
# - Unexpected DNS responses
# - Different IPs for same domain
```

### SYN Flood Detection
```
# Filter
tcp.flags.syn == 1 and tcp.flags.ack == 0

# Look for:
# - Massive SYN packets from single IP
# - No corresponding SYN-ACK
# Statistics → Conversations (sort by packets)
```

---

## ✅ Wireshark Quick Exam Workflow
```
1. Open .pcap file in Wireshark
2. Read the question carefully:
   - Password? → http.request.method == POST → Find packet → Search "pwd"
   - How many machines? → tcp.flags.syn==1 and tcp.flags.ack==0 → Statistics
   - DoS attack? → tcp.flags.syn==1 → Check top sender
   - OS? → icmp filter → Check TTL
   - What protocol? → Statistics → Protocol Hierarchy
```

