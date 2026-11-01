# 🔐 Module 7: Cryptography & Steganography

> **Data hide karo, encrypt karo, hash verify karo!**

---

## 🖼️ Steganography (Data Hiding)

### Snow (Whitespace Steganography — Windows/Linux)

```bash
# Hide a message in a text file
snow -C -m "My secret message here" -p "password123" original.txt output.txt
# -C = Compress
# -m = Message to hide
# -p = Password
# original.txt = Source file
# output.txt = Output with hidden data

# Extract/Display hidden message
snow -C -p "password123" output.txt
# This will reveal: My secret message here
```

### OpenStego (Image Steganography — GUI Tool)

```
# Hide Data:
1. Open OpenStego
2. Select "Data Hiding"
3. Message File: Select file to hide (e.g., secret.txt)
4. Cover File: Select image (e.g., picture.jpg)
5. Output Stego File: Choose output name (e.g., stego_image.bmp)
6. Set password (optional)
7. Click "Hide Data"

# Extract Data:
1. Open OpenStego
2. Select "Extract Data"
3. Input Stego File: Select the stego image
4. Output Folder: Select where to save
5. Enter password (if set)
6. Click "Extract Data"
```

### Steghide (Linux CLI — Image Steganography)

```bash
# Hide data in image
steghide embed -cf cover_image.jpg -ef secret.txt -p password123

# Extract data from image
steghide extract -sf stego_image.jpg -p password123

# Get info about stego file
steghide info stego_image.jpg
```

### Stegseek (Fast Steghide Cracker)
```bash
# Crack steghide password
stegseek stego_image.jpg /usr/share/wordlists/rockyou.txt
```

---

## 🔒 Encryption Tools

### VeraCrypt (Disk Encryption — GUI)

```
# Create Encrypted Volume:
1. Open VeraCrypt
2. Click "Create Volume"
3. Select "Create an encrypted file container"
4. Standard VeraCrypt volume
5. Select location and filename
6. Choose encryption (AES is default)
7. Set volume size
8. Set password
9. Format

# Mount Volume:
1. Select a drive letter
2. Click "Select File" → Choose the volume
3. Click "Mount"
4. Enter password
5. Access from the assigned drive letter

# Dismount:
1. Select mounted volume
2. Click "Dismount"
```

### GnuPG / GPG (File Encryption — Linux)

```bash
# Symmetric encryption (password-based)
gpg -c secret.txt                    # Creates secret.txt.gpg
gpg -d secret.txt.gpg                # Decrypt

# Generate key pair
gpg --gen-key

# Encrypt with public key
gpg -e -r "recipient@email.com" file.txt

# Decrypt with private key
gpg -d file.txt.gpg

# List keys
gpg --list-keys
```

### CrypTool (Windows — GUI)
```
1. Open CrypTool
2. Encrypt/Decrypt → Symmetric/Asymmetric
3. Common: AES, DES, RC4, RSA
4. Paste text → Select algorithm → Encrypt/Decrypt
```

---

## #️⃣ Hashing

### HashCalc (Windows GUI)
```
1. Open HashCalc
2. Select file or enter text
3. Check hash types: MD5, SHA-1, SHA-256, etc.
4. Click "Calculate"
5. Compare hash values
```

### Linux CLI Hashing
```bash
# MD5
md5sum file.txt
echo -n "text" | md5sum

# SHA-1
sha1sum file.txt
echo -n "text" | sha1sum

# SHA-256
sha256sum file.txt
echo -n "text" | sha256sum

# SHA-512
sha512sum file.txt

# Compare two files
md5sum file1.txt file2.txt
# If hashes match → files are identical
```

### Windows CMD Hashing
```cmd
:: CertUtil for hashing
certutil -hashfile filename MD5
certutil -hashfile filename SHA1
certutil -hashfile filename SHA256
```

### Online Hash Tools
```
https://www.md5hashgenerator.com/
https://crackstation.net/          (Hash cracking)
https://hashkiller.io/
https://hashes.com/en/decrypt/hash
```

---

## 🔑 Certificate & PKI

```bash
# View SSL certificate
openssl s_client -connect target.com:443

# View certificate details
openssl x509 -in cert.pem -text -noout

# Generate self-signed certificate
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365
```

---

## 🔢 Encoding/Decoding

### Base64
```bash
# Encode
echo -n "Hello" | base64
# Output: SGVsbG8=

# Decode
echo -n "SGVsbG8=" | base64 -d
# Output: Hello

# Encode file
base64 file.txt > encoded.txt

# Decode file
base64 -d encoded.txt > decoded.txt
```

### Windows CMD
```cmd
certutil -encode input.txt encoded.txt
certutil -decode encoded.txt decoded.txt
```

---

## 📋 Algorithm Reference

### Symmetric Encryption
| Algorithm | Key Size | Notes |
|-----------|----------|-------|
| AES | 128/192/256 bit | Most secure, widely used |
| DES | 56 bit | Old, insecure |
| 3DES | 168 bit | Better than DES, still old |
| RC4 | Variable | Stream cipher, deprecated |
| Blowfish | 32-448 bit | Fast, good alternative |

### Asymmetric Encryption
| Algorithm | Use |
|-----------|-----|
| RSA | Encryption & Digital Signatures |
| DSA | Digital Signatures only |
| Diffie-Hellman | Key Exchange |
| ECC | Efficient encryption |

### Hash Functions
| Hash | Output | Security |
|------|--------|----------|
| MD5 | 128 bit (32 hex) | ❌ Broken |
| SHA-1 | 160 bit (40 hex) | ❌ Broken |
| SHA-256 | 256 bit (64 hex) | ✅ Secure |
| SHA-512 | 512 bit (128 hex) | ✅ Secure |

---

## ✅ Crypto/Stego Quick Exam Workflow
```
1. Steganography question?
   → Text file: Use Snow
   → Image file: Use OpenStego or Steghide
2. Hash comparison?
   → Use md5sum/sha256sum or HashCalc
3. Encrypt/Decrypt file?
   → VeraCrypt for disk, GPG for files
4. Decode something?
   → Try Base64 decode first
```

