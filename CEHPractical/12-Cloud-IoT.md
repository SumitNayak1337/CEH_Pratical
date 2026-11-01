# ☁️ Module 12: Cloud Computing & IoT

> **Cloud security aur IoT attacks — naye exam syllabus mein added!**

---

## ☁️ Cloud Security

### S3 Bucket Enumeration (AWS)

```bash
# Check if S3 bucket exists
aws s3 ls s3://<bucket_name> --no-sign-request

# List contents
aws s3 ls s3://<bucket_name>/ --no-sign-request

# Download files
aws s3 cp s3://<bucket_name>/file.txt . --no-sign-request

# Download entire bucket
aws s3 sync s3://<bucket_name>/ ./downloaded/ --no-sign-request

# Common bucket naming patterns to try:
# companyname-backup
# companyname-data
# companyname-files
# companyname-logs
```

### S3Scanner
```bash
# Install
pip install s3scanner

# Scan a list of bucket names
s3scanner --list buckets.txt

# Scan a single bucket
s3scanner scan --bucket <bucket_name>
```

### Cloud Enumeration Tools
```bash
# lazys3 — Brute force S3 buckets
ruby lazys3.rb <company_name>

# cloud_enum
python3 cloud_enum.py -k <keyword>
```

---

## 🔎 Shodan (IoT Search Engine)

```
# Web Interface
https://www.shodan.io

# Common Shodan Searches:
port:5555 "Android Debug Bridge"     # Open ADB devices
port:23 "Telnet"                      # Open Telnet
port:21 "FTP" "Anonymous"            # Open FTP with anon login
port:3389 "Remote Desktop"           # Open RDP
webcam                                # Open webcams
"default password"                    # Default credentials
os:"Windows XP"                       # Windows XP machines
country:"IN"                          # India-based devices
city:"Mumbai"                         # City-specific

# Shodan CLI
shodan search "port:5555"
shodan host <ip_address>
```

---

## 🌐 IoT Hacking

### Common IoT Ports
| Port | Protocol | Device Type |
|------|----------|-------------|
| 23 | Telnet | Routers, Cameras, IoT |
| 80/443 | HTTP/HTTPS | Web interface |
| 1883 | MQTT | IoT messaging |
| 5683 | CoAP | IoT communication |
| 8080 | HTTP Alt | Web interface |
| 8883 | MQTT SSL | Secure IoT messaging |
| 48101 | Custom | Mirai botnet |

### MQTT Enumeration
```bash
# Subscribe to all topics
mosquitto_sub -h <target_ip> -t '#' -v

# Publish a message
mosquitto_pub -h <target_ip> -t 'test/topic' -m 'Hello'

# Nmap MQTT scan
nmap -p 1883 --script mqtt-subscribe <target_ip>
```

### Default Credentials (IoT)
| Device | Username | Password |
|--------|----------|----------|
| Admin Panel | admin | admin |
| Router | admin | password |
| Camera | admin | 12345 |
| IoT Default | root | root |

---

## 🐳 Docker Security

```bash
# List containers
docker ps -a

# Access container shell
docker exec -it <container_id> /bin/bash

# Check Docker socket access
ls -la /var/run/docker.sock

# Docker images
docker images
```

---

## ✅ Cloud & IoT Quick Reference
```
1. S3 Bucket → aws s3 ls s3://bucket --no-sign-request
2. IoT Device → Check Shodan / nmap common IoT ports
3. MQTT → mosquitto_sub -h <ip> -t '#'
4. Default creds → Try admin:admin, root:root
```

