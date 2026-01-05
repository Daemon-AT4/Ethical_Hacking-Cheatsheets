<!-- CYBERPUNK HEADER -->
<div align="center">

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                                                                              ║
║   ░██████╗██╗░░██╗░█████╗░██████╗░░█████╗░███╗░░██╗                          ║
║   ██╔════╝██║░░██║██╔══██╗██╔══██╗██╔══██╗████╗░██║                          ║
║   ╚█████╗░███████║██║░░██║██║░░██║███████║██╔██╗██║                          ║
║   ░╚═══██╗██╔══██║██║░░██║██║░░██║██╔══██║██║╚████║                          ║
║   ██████╔╝██║░░██║╚█████╔╝██████╔╝██║░░██║██║░╚███║                          ║
║   ╚═════╝░╚═╝░░╚═╝░╚════╝░╚═════╝░╚═╝░░╚═╝╚═╝░░╚══╝                          ║
║                                                                              ║
║        ▀█▀ █▀█ ▀█▀ █▀▀ █▀█ █▄░█ █▀▀ ▀█▀   █▀▄▀█ █▀█ █▀█ █▀█ █▀▀ █▀█         ║
║        ░█░ █ █ ░█░ ██▄ █▀▄ █░▀█ ██▄ ░█░   █░▀░█ █▀█ █▀▀ █▀▀ ██▄ █▀▄         ║
║                                                                              ║
║                    ═══[ CYBER RECONNAISSANCE ]═══                            ║
║                                                                              ║
║          ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓                  ║
║          ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░                  ║
║                                                                              ║
║             > Scanning the digital realm...                                  ║
║             > Mapping the IoT landscape...                                   ║
║             > Discovering vulnerable systems...                              ║
║                                                                              ║
║                  [████████████████████] 100%                                 ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

<!-- BADGES -->
![Tool](https://img.shields.io/badge/TOOL-Shodan-red?style=for-the-badge&logo=shodan&logoColor=white)
![Reconnaissance](https://img.shields.io/badge/RECONNAISSANCE-00FF00?style=for-the-badge&logo=hackthebox&logoColor=black)
![IoT](https://img.shields.io/badge/IoT_SCANNING-FF6B6B?style=for-the-badge&logo=internetofthings&logoColor=white)

[![Author](https://img.shields.io/badge/Author-0xNetrunner-00FF00?style=flat-square&logo=github)](https://github.com/00xNetrunner)
![Category](https://img.shields.io/badge/Category-Enumeration-purple?style=flat-square)
![Protocol](https://img.shields.io/badge/Protocol-HTTP%2FSSL%2FBanner-blue?style=flat-square)

</div>

---

> **`[SYSTEM]`** The search engine for Internet-connected devices worldwide
>
> **`[STATUS]`** Module: `LOADED` | Target: `GLOBAL_NETWORK` | Protocol: `MULTI`

---

## 📋 >_ Table.Of.Contents()

- [Overview](#-overview)
- [Getting Started](#-getting-started)
- [Query Syntax Reference](#-query-syntax-reference)
- [Finding Cameras](#-finding-cameras)
- [Tips and Tricks](#-tips-and-tricks-for-advanced-shodan-searching)
- [Finding Vulnerable Servers](#-finding-vulnerable-servers)
- [Geographic Filters](#-searching-by-geographic-filters)
- [Plex Media Servers](#-finding-plex-media-servers)
- [Raspberry Pi Devices](#-finding-raspberry-pi-devices)
- [Proxmox Servers](#-finding-proxmox-servers)
- [Web Cameras & Streaming](#-finding-web-cameras--video-streaming)
- [IoT Devices](#-finding-iot-devices)
- [Industrial Control Systems](#-finding-industrial-control-systems)
- [Network Attached Storage](#-finding-network-attached-storage-nas)
- [Database Servers](#-finding-database-servers)
- [VPN & Remote Access](#-finding-vpn--remote-access-services)
- [Printers & Multifunction Devices](#-finding-printers--multifunction-devices)
- [Game Servers](#-finding-game-servers)
- [Cloud Services](#-finding-cloud-services--apis)
- [Network Infrastructure](#-finding-network-infrastructure)
- [Advanced Filtering](#-advanced-filtering-techniques)
- [Shodan CLI](#-using-shodan-cli-for-advanced-queries)
- [Shodan API](#-shodan-api-usage)
- [Practical Strategies](#-practical-search-strategies)
- [Ethical Considerations](#-ethical-considerations)
- [Responsible Disclosure](#-responsible-vulnerability-disclosure)
- [Resources](#-resources-and-references)

---

## 🎯 Overview

```
> Loading module: Shodan_Recon.dll...
> Initializing global scanner...
> Status: READY
```

**Shodan** is the world's first search engine for Internet-connected devices. Unlike traditional search engines that index web content, Shodan indexes information about devices, including:

| Function | Description | Icon |
|:---------|:------------|:----:|
| **Banner Grabbing** | Captures service banners and metadata | 📡 |
| **Port Scanning** | Indexes open ports and services | 🔌 |
| **Vulnerability Detection** | Identifies known CVEs | 🔓 |
| **SSL/TLS Analysis** | Certificate and encryption info | 🔐 |
| **Geographic Mapping** | Device location tracking | 🌍 |
| **Historical Data** | Track changes over time | 📊 |

### ⚡ Key Capabilities

```
[✓] Real-time internet scanning
[✓] Historical data access
[✓] API integration
[✓] CLI tools
[✓] Network monitoring
[✓] Vulnerability alerts
[✓] Export capabilities
```

---

## 💿 Getting Started

```
> Loading module: Init_Shodan.dll...
> Status: READY
```

### 🔑 Account Setup

1. **Create Account**: Visit [shodan.io](https://www.shodan.io) and register
2. **Get API Key**: Navigate to Account → API Key
3. **Choose Plan**: Free tier available with limitations

### 📊 Plan Comparison

| Feature | Free | Membership | Small Business | Corporate |
|:--------|:----:|:----------:|:--------------:|:---------:|
| Search Results | 10 | Unlimited | Unlimited | Unlimited |
| Query Credits | 0 | 100/month | 10,000/month | Unlimited |
| Scan Credits | 0 | 100/month | 5,000/month | Unlimited |
| Network Monitoring | No | Yes | Yes | Yes |
| API Access | Limited | Full | Full | Full |

### 🛠️ CLI Installation

```bash
# Install via pip
pip install shodan

# Initialize with API key
shodan init YOUR_API_KEY

# Verify installation
shodan info
```

---

## 📖 Query Syntax Reference

```
> Loading module: Query_Engine.dll...
> Status: INDEXED
```

### 🎯 Core Filters

| Filter | Usage | Example |
|:------:|:-----:|:-------:|
| `title:` | Search page title | `title:"Admin Panel"` |
| `product:` | Search product name | `product:"Apache"` |
| `port:` | Search specific port | `port:22` |
| `country:` | Filter by country code | `country:"US"` |
| `city:` | Filter by city | `city:"New York"` |
| `region:` | Filter by state/region | `region:"California"` |
| `org:` | Search by organization | `org:"Google"` |
| `asn:` | Search by ASN | `asn:AS15169` |
| `net:` | Search by IP range | `net:8.8.8.0/24` |
| `geo:` | Search by coordinates | `geo:"40.7128,-74.0060"` |
| `vuln:` | Search by vulnerability | `vuln:CVE-2021-44228` |
| `has_screenshot:` | Include/exclude screenshots | `has_screenshot:true` |
| `html:` | Search in HTML content | `html:"server version"` |
| `header:` | Search HTTP headers | `header:"Server: Nginx"` |
| `ssl:` | Search SSL certificate data | `ssl:"Google"` |
| `ssl.cert.subject.cn:` | SSL Common Name | `ssl.cert.subject.cn:"*.google.com"` |
| `ssl.cert.issuer.cn:` | SSL Issuer | `ssl.cert.issuer.cn:"Let's Encrypt"` |
| `os:` | Operating system | `os:"Windows Server 2019"` |
| `before:` | Results before date | `before:01/01/2024` |
| `after:` | Results after date | `after:01/01/2024` |
| `hostname:` | Search by hostname | `hostname:"example.com"` |
| `isp:` | Internet Service Provider | `isp:"Comcast"` |
| `version:` | Software version | `version:"7.4"` |
| `http.title:` | HTTP page title | `http.title:"Dashboard"` |
| `http.status:` | HTTP status code | `http.status:200` |
| `http.component:` | Web technology | `http.component:"WordPress"` |
| `http.favicon.hash:` | Favicon hash | `http.favicon.hash:116323821` |

### 🔧 Boolean Operators

```
AND  →  Implicit (space between filters)
OR   →  Explicit with "OR" keyword
NOT  →  Minus sign (-) or "NOT" keyword
```

**Examples:**
```shodan
# AND (implicit)
apache port:80 country:US

# OR
title:"Camera" OR title:"Webcam"

# NOT
apache -country:CN
apache NOT country:CN
```

---

## 📹 Finding Cameras

```
> Loading module: Camera_Scanner.dll...
> Status: STREAMING
```

> **`[NOTE]`** Common Ports for IP Cameras
> - **HTTP**: 80, 8080
> - **HTTPS**: 443
> - **RTSP**: 554
> - **Custom Ports**: Vary by manufacturer (e.g., 81, 8888)

### 🎥 Camera Models and Brands

#### Axis Cameras 📹
| Property | Value |
|:---------|:------|
| **Common Ports** | 80, 443 |
| **Search Queries** | `title:"AXIS"` \| `product:"Axis"` |

#### D-Link Cameras 🔗
| Property | Value |
|:---------|:------|
| **Common Ports** | 80, 8080 |
| **Search Queries** | `title:"DCS-930L"` \| `product:"D-Link"` |

#### Foscam Cameras 📷
| Property | Value |
|:---------|:------|
| **Common Ports** | 80, 88, 443 |
| **Search Queries** | `title:"Foscam"` \| `product:"Foscam"` |

#### Hikvision Cameras 🎬
| Property | Value |
|:---------|:------|
| **Common Ports** | 80, 443, 554, 8000, 8080 |
| **Search Queries** | `title:"Hikvision"` \| `product:"Hikvision"` |

#### Dahua Cameras 🔍
| Property | Value |
|:---------|:------|
| **Common Ports** | 80, 443, 554, 8000, 8080, 8081, 8888 |
| **Search Queries** | `title:"Dahua"` \| `product:"Dahua"` \| `html:"Dahua"` |

#### Ubiquiti UniFi Cameras 🌐
| Property | Value |
|:---------|:------|
| **Common Ports** | 80, 443, 8080, 8443, 7080, 7443 |
| **Search Queries** | `title:"UniFi"` \| `product:"Ubiquiti"` \| `"UniFi Video"` |

#### Reolink Cameras 📷
| Property | Value |
|:---------|:------|
| **Common Ports** | 80, 443, 8080 |
| **Search Queries** | `title:"Reolink"` \| `product:"Reolink"` \| `html:"Reolink"` |

#### Additional Brands

| Brand | Common Ports | Search Query |
|:------|:-------------|:-------------|
| **Linksys** | 80, 1024 | `title:"Linksys WVC80N"` |
| **Panasonic** | 80, 443 | `title:"Panasonic Network Camera"` |
| **Sony** | 80, 443 | `title:"Sony Network Camera"` |
| **Trendnet** | 80, 443 | `title:"TV-IP"` |
| **TP-Link** | 80, 8080 | `title:"TP-Link"` |
| **Vivotek** | 80, 443 | `title:"Vivotek"` |
| **AvTech** | 80, 8888 | `title:"AVTech"` |
| **Wansview** | 80, 8080 | `title:"Wansview"` |
| **Wyze** | 80, 443, 8080 | `title:"Wyze"` |
| **Uniview** | 80, 443, 554, 8080 | `title:"Uniview"` |
| **Amcrest** | 80, 8080, 8000 | `title:"Amcrest"` |
| **Lorex** | 80, 443 | `title:"Lorex"` |
| **Mobotix** | 80, 443, 8080 | `title:"Mobotix"` |
| **Avigilon** | 80, 443, 554, 8080 | `title:"Avigilon"` |
| **GoPro** | 8080 | `title:"GoPro"` |
| **FLIR** | 80, 443, 554 | `title:"FLIR"` |

### 💡 Example Camera Queries

<details>
<summary><strong>🇺🇸 Axis Cameras in the United States</strong></summary>

```shodan
title:"AXIS" country:"US"
```
</details>

<details>
<summary><strong>📸 Foscam Cameras with Screenshots</strong></summary>

```shodan
title:"Foscam" has_screenshot:true
```
</details>

<details>
<summary><strong>🏙️ Hikvision Cameras in New York</strong></summary>

```shodan
title:"Hikvision" city:"New York"
```
</details>

<details>
<summary><strong>🔌 TP-Link Cameras on Port 8080</strong></summary>

```shodan
title:"TP-Link" port:8080
```
</details>

<details>
<summary><strong>🌍 Vivotek Cameras Worldwide</strong></summary>

```shodan
title:"Vivotek"
```
</details>

<details>
<summary><strong>⚙️ D-Link Cameras on Custom Ports</strong></summary>

```shodan
title:"DCS-930L" port:8080
```
</details>

<details>
<summary><strong>🌐 Dahua Cameras Worldwide</strong></summary>

```shodan
title:"Dahua"
```
</details>

<details>
<summary><strong>🇪🇺 Reolink Cameras in Europe</strong></summary>

```shodan
title:"Reolink" country:"DE" OR country:"GB" OR country:"FR"
```
</details>

<details>
<summary><strong>📹 UniFi Cameras with Screenshots</strong></summary>

```shodan
"UniFi Video" has_screenshot:true
```
</details>

<details>
<summary><strong>🔗 Amcrest Cameras on Port 8080</strong></summary>

```shodan
title:"Amcrest" port:8080
```
</details>

<details>
<summary><strong>💾 Dahua Cameras with HTML Interface</strong></summary>

```shodan
html:"Dahua" port:80
```
</details>

---

## 💡 Tips and Tricks for Advanced Shodan Searching

```
> Loading module: Advanced_Queries.dll...
> Status: OPTIMIZED
```

> **`[TIP]`** Query Optimization & Best Practices
> Master these techniques to write more effective and efficient Shodan queries.

### ➕ Using Boolean Operators Effectively

**OR Operator** (Find Multiple Brands)
```shodan
title:"Axis" OR title:"Hikvision" OR title:"Dahua"
```

**AND Operator** (Combine Filters)
```shodan
title:"Camera" AND port:8080 AND country:"US"
```

**NOT Operator** (Exclude Results)
```shodan
title:"Camera" NOT "authentication required"
```

### ⚙️ Advanced Query Techniques

**Search by HTTP Header**
```shodan
header:"Server: Boa"
```

**Search by Specific HTTP Status Codes**
```shodan
http.status:200 "admin"
```

**Find Open Web Interfaces**
```shodan
"200 OK" http.title:"Index of"
```

**Search Across Multiple Ports**
```shodan
title:"Camera" (port:80 OR port:8080 OR port:8888)
```

**Combine Product Name with Vulnerable Versions**
```shodan
product:"Apache" "2.2.15"
```

**Find Devices with Outdated Java**
```shodan
java org:*
```

**Search by Favicon Hash**
```shodan
http.favicon.hash:116323821
```

**Find Devices by SSL Certificate**
```shodan
ssl.cert.subject.cn:"*.target.com"
```

### 🔍 Filtering by Response Content

**Find Pages with Specific Keywords**
```shodan
"username" "password" filetype:html
```

**Search for Default Pages**
```shodan
"It works!" "Apache"
```

**Find Administrative Interfaces**
```shodan
title:"Admin" OR title:"Administration" OR title:"Dashboard"
```

**Find Login Pages**
```shodan
http.title:"Login" OR http.title:"Sign In"
```

### 🏢 Organizational & Network Searches

**Find All Devices from a Specific Organization**
```shodan
org:"Company Name"
```

**Search by Autonomous System Number (ASN)**
```shodan
asn:AS12345
```

**Search by IP Range (CIDR Notation)**
```shodan
net:192.168.1.0/24
```

**Search by ISP**
```shodan
isp:"Amazon Technologies"
```

### 🌍 Combining Geographic + Service Filters

**Find a Specific Service in Multiple Countries**
```shodan
title:"Camera" (country:"US" OR country:"CA" OR country:"MX")
```

**Search Specific Regions for Vulnerable Services**
```shodan
"MongoDB Server Information" port:27017 region:"California"
```

**Find Devices in a City with a Specific Product**
```shodan
product:"Cisco" city:"London"
```

### ⚡ Performance Optimization Tips

```
[✓] Use Specific Queries: More specific queries return faster results
[✓] Avoid Common Keywords Alone: Instead of just port:80, combine with product or title
[✓] Use has_screenshot Filter Sparingly: Screenshots slow down queries
[✓] Limit Geographic Scope: Searching worldwide takes longer; narrow by country when possible
[✓] Use Title/Product Filters: These are indexed and search faster than HTML content
[✓] Cache Frequent Searches: Save common searches for quicker access
```

---

## ⚠️ Finding Vulnerable Servers

```
> Loading module: Vuln_Scanner.dll...
> Status: ARMED
```

> **`[IMPORTANT]`** Common Vulnerabilities and Their Shodan Queries
>
> These queries help identify common vulnerabilities. Always use findings responsibly and with proper authorization.

### 🔓 Known Vulnerability Queries

**Heartbleed (CVE-2014-0160)** 🩹
```shodan
vuln:CVE-2014-0160
```

**OpenSSL CCS Injection (CVE-2014-0224)** 🔐
```shodan
vuln:CVE-2014-0224
```

**Shellshock (CVE-2014-6271)** 💥
```shodan
vuln:CVE-2014-6271
```

**EternalBlue (MS17-010)** 🛡️
```shodan
vuln:ms17-010
```

**Log4Shell (CVE-2021-44228)** 🪵
```shodan
vuln:CVE-2021-44228
```

**ProxyLogon (CVE-2021-26855)** 📧
```shodan
vuln:CVE-2021-26855
```

**BlueKeep (CVE-2019-0708)** 💙
```shodan
vuln:CVE-2019-0708
```

**Apache Struts (CVE-2017-5638)** 🌐
```shodan
vuln:CVE-2017-5638
```

**Zerologon (CVE-2020-1472)** 🔢
```shodan
vuln:CVE-2020-1472
```

**PrintNightmare (CVE-2021-34527)** 🖨️
```shodan
vuln:CVE-2021-34527
```

### 🔑 Default Credentials

**FTP with Anonymous Login**
```shodan
"220" "Anonymous FTP login allowed"
```

**Telnet with Default Credentials**
```shodan
"220" "telnet" "default password"
```

**Default Admin Pages**
```shodan
"default password" http.title:"admin"
```

**Cisco Default Login**
```shodan
"cisco" "level 15 access"
```

### 📦 Outdated Software

**Outdated Apache Servers**
```shodan
"Apache/2.2.15"
```

**Outdated IIS Servers**
```shodan
"Microsoft-IIS/6.0"
```

**Outdated OpenSSH**
```shodan
"OpenSSH_5"
```

**Outdated nginx**
```shodan
"nginx/1.4"
```

**Outdated PHP**
```shodan
"PHP/5.2"
```

### 🎯 Example Queries by Service and Vulnerability

**Open MongoDB Instances**
```shodan
"MongoDB Server Information" port:27017
```

**ElasticSearch Instances Without Authentication**
```shodan
"200 OK" "elastic indices" port:9200
```

**Open SMB Shares**
```shodan
port:445 "smb" "NT_STATUS_ACCESS_DENIED"
```

**Exposed RDP Services**
```shodan
port:3389 "Remote Desktop Protocol"
```

**Jenkins Without Auth**
```shodan
"X-Jenkins" "200 OK"
```

**Exposed Kubernetes API**
```shodan
"kube-apiserver" port:6443
```

**CouchDB No Auth**
```shodan
"couchdb" port:5984 "200 OK"
```

### 🗺️ Geographic Vulnerability Filtering

> **`[TIP]`** Combining Vulnerability Queries with Geographic Filters
>
> Narrow down vulnerability searches by location for more targeted research.

**EternalBlue in the United States**
```shodan
vuln:ms17-010 country:"US"
```

**Open MongoDB Instances in Germany**
```shodan
"MongoDB Server Information" port:27017 country:"DE"
```

**Shellshock Vulnerable Servers in California**
```shodan
vuln:CVE-2014-6271 region:"California"
```

**Heartbleed Vulnerable Servers in London**
```shodan
vuln:CVE-2014-0160 city:"London"
```

### 🔗 Combining Multiple Filters for Specific Searches

**Outdated Apache Servers with Screenshots in France**
```shodan
"Apache/2.2.15" country:"FR" has_screenshot:true
```

**Elasticsearch with Screenshots in the UK**
```shodan
"200 OK" "elastic indices" port:9200 country:"GB" has_screenshot:true
```

**FTP Servers with Anonymous Login in Japan**
```shodan
"220" "Anonymous FTP login allowed" country:"JP"
```

---

## 🌍 Searching by Geographic Filters

```
> Loading module: Geo_Mapper.dll...
> Status: LOCATED
```

> **`[NOTE]`** Common Geographic Filters
>
> - **Country**: `country:"<country_code>"`
> - **City**: `city:"<city_name>"`
> - **Region/State**: `region:"<region_name>"`
> - **Coordinates**: `geo:"<latitude>,<longitude>"`

### 🌐 Example Queries by Service and Location

**Web Servers in the United States**
```shodan
http country:"US"
```

**FTP Servers in Germany**
```shodan
ftp country:"DE"
```

**Telnet Servers in London, UK**
```shodan
telnet city:"London"
```

**RDP Servers in California, USA**
```shodan
rdp region:"California"
```

**MySQL Servers in Paris, France**
```shodan
mysql city:"Paris"
```

**Elasticsearch Instances in Berlin, Germany**
```shodan
"elastic indices" port:9200 city:"Berlin"
```

### 🔐 Advanced Geographic Queries

**SSH Servers in Specific Countries**

| Country | Query |
|:--------|:------|
| 🇺🇸 United States | `ssh country:"US"` |
| 🇯🇵 Japan | `ssh country:"JP"` |
| 🇬🇧 United Kingdom | `ssh country:"GB"` |
| 🇩🇪 Germany | `ssh country:"DE"` |
| 🇦🇺 Australia | `ssh country:"AU"` |

**HTTP Servers in Specific Cities**

| City | Query |
|:-----|:------|
| 🗽 New York | `http city:"New York"` |
| 🦘 Sydney | `http city:"Sydney"` |
| 🗼 Tokyo | `http city:"Tokyo"` |
| 🏰 London | `http city:"London"` |

**Database Servers in Specific Regions**
- 📍 California:
  ```shodan
  "database" region:"California"
  ```
- 🍁 Ontario:
  ```shodan
  "database" region:"Ontario"
  ```

**Specific Services in Coordinated Areas**
- 📍 Within proximity of New York City:
  ```shodan
  http geo:"40.7128,-74.0060"
  ```

### 🧩 Combining Geographic Filters

**Apache Servers in Germany with Screenshots**
```shodan
"Apache" country:"DE" has_screenshot:true
```

**Elasticsearch in the UK and Open Ports**
```shodan
"elastic indices" country:"GB" port:9200
```

**FTP Servers in France with Anonymous Login**
```shodan
"FTP" "Anonymous login allowed" country:"FR"
```

### 🎯 Specific Device Searches

**Axis Cameras in the Netherlands**
```shodan
"AXIS" country:"NL"
```

**Hikvision Cameras in Los Angeles**
```shodan
"Hikvision" city:"Los Angeles"
```

**Cisco Devices in California**
```shodan
"cisco" region:"California"
```

---

## 🎬 Finding Plex Media Servers

```
> Loading module: Media_Scanner.dll...
> Status: STREAMING
```

> **`[NOTE]`** Common Ports
>
> - **HTTP**: 32400

### 📡 Search Queries

**Plex Media Servers Worldwide**
```shodan
"X-Plex-Protocol" port:32400
```

**Plex Media Servers in the United States**
```shodan
"X-Plex-Protocol" port:32400 country:"US"
```

**Plex Media Servers in Germany with Screenshots**
```shodan
"X-Plex-Protocol" port:32400 country:"DE" has_screenshot:true
```

**Plex by Version**
```shodan
"X-Plex-Version" port:32400
```

---

## 🍓 Finding Raspberry Pi Devices

```
> Loading module: IoT_Scanner.dll...
> Status: READY
```

> **`[NOTE]`** Common Ports
>
> - **SSH**: 22
> - **HTTP**: 80

### 📡 Search Queries

**Raspberry Pi Devices via SSH**
```shodan
"Raspbian" port:22
```

**Raspberry Pi Devices via HTTP**
```shodan
"Raspberry Pi" port:80
```

**Raspberry Pi Devices in the United States**
```shodan
"Raspbian" port:22 country:"US"
```

**Pi-hole Instances**
```shodan
title:"Pi-hole" http.component:"Pi-hole"
```

**Raspberry Pi with VNC**
```shodan
"RFB" "Raspbian" port:5900
```

---

## 🖥️ Finding Proxmox Servers

```
> Loading module: Virtualization_Scanner.dll...
> Status: READY
```

> **`[NOTE]`** Common Ports
>
> - **HTTPS**: 8006

### 📡 Search Queries

**Proxmox Servers Worldwide**
```shodan
"Proxmox" port:8006
```

**Proxmox Servers in the United States**
```shodan
"Proxmox" port:8006 country:"US"
```

**Proxmox Servers with Screenshots**
```shodan
"Proxmox" port:8006 has_screenshot:true
```

**Proxmox VE Specific**
```shodan
title:"Proxmox Virtual Environment"
```

---

## 🎥 Finding Web Cameras & Video Streaming

```
> Loading module: Stream_Scanner.dll...
> Status: LIVE
```

> **`[NOTE]`** Common Streaming Protocols & Ports
>
> - **MJPEG**: 8081, 8082, 8888
> - **RTSP**: 554, 322
> - **HTTP**: 80, 8080

### 📡 Search Queries

**Webcams with MJPEG Streams**
```shodan
"MJPEG Server" port:8081
```

**Devices Streaming Video Content**
```shodan
"Motion JPEG" port:8888
```

**RTSP Protocol Streams**
```shodan
port:554 "rtsp"
```

**Webcams with No Authentication**
```shodan
"200 OK" "webcam" NOT "password"
```

**IP Webcam Android App**
```shodan
"IP Webcam Server" http.component:"IP Webcam"
```

**Blue Iris DVR**
```shodan
title:"Blue Iris" http.favicon.hash:-1616143106
```

---

## 💡 Finding IoT Devices

```
> Loading module: IoT_Discovery.dll...
> Status: CONNECTED
```

> **`[NOTE]`** Common IoT Protocols & Ports
>
> - **MQTT**: 1883, 8883 (TLS)
> - **CoAP**: 5683
> - **ZigBee**: 6100
> - **Smart Home Hubs**: 8080-8090

### 📡 Search Queries

**IoT Devices via MQTT**
```shodan
port:1883
```

**Smart Home Hubs Worldwide**
```shodan
title:"Home" OR title:"Smart" port:8080
```

**MQTT Brokers with Open Connections**
```shodan
"MQTT" port:1883
```

**IoT Devices from Manufacturers**
```shodan
product:"Arduino" OR product:"Raspberry Pi" OR product:"ESP"
```

**Home Assistant Instances**
```shodan
title:"Home Assistant" port:8123
```

**SmartThings Hubs**
```shodan
"SmartThings" port:39500
```

**Philips Hue Bridges**
```shodan
"Philips hue" port:80
```

**Nest Devices**
```shodan
"Nest" port:443
```

**Ring Doorbells**
```shodan
"Ring" product:"Ring"
```

---

## 🏭 Finding Industrial Control Systems

```
> Loading module: ICS_SCADA_Scanner.dll...
> Status: CRITICAL
```

> **`[IMPORTANT]`** Common ICS/SCADA Protocols
>
> - **Modbus**: 502
> - **Siemens S7**: 102
> - **Profinet**: 34962-34964
> - **OPC UA**: 4840
> - **DNP3**: 20000
> - **BACnet**: 47808
> - **EtherNet/IP**: 44818

### 📡 Search Queries

**Modbus Devices**
```shodan
port:502 "Modbus"
```

**Siemens Control Systems**
```shodan
port:102 "Siemens"
```

**Industrial Devices Worldwide**
```shodan
"Siemens" OR "Modbus" OR "PLC"
```

**Allen-Bradley PLCs**
```shodan
port:44818 "Allen-Bradley"
```

**BACnet Building Automation**
```shodan
port:47808 "BACnet"
```

**DNP3 SCADA Systems**
```shodan
port:20000 "DNP3"
```

**OPC UA Servers**
```shodan
port:4840 "OPC"
```

**Schneider Electric Devices**
```shodan
"Schneider Electric" port:502
```

**GE Automation**
```shodan
"GE" "PLC" OR "PACSystems"
```

---

## 💾 Finding Network Attached Storage (NAS)

```
> Loading module: Storage_Scanner.dll...
> Status: MOUNTED
```

> **`[NOTE]`** Common NAS Brands & Ports
>
> - **QNAP**: 8080, 8443
> - **Synology**: 5000, 5001
> - **WD MyCloud**: 80
> - **Netgear ReadyNAS**: 80, 443

### 📡 Search Queries

**QNAP NAS Devices**
```shodan
title:"QNAP" port:8080
```

**Synology NAS Devices**
```shodan
"Synology" port:5000
```

**Western Digital MyCloud**
```shodan
"WD MyCloud" port:80
```

**Open NAS Shares**
```shodan
"NAS" "sharing" has_screenshot:true
```

**FreeNAS/TrueNAS**
```shodan
title:"FreeNAS" OR title:"TrueNAS"
```

**Netgear ReadyNAS**
```shodan
title:"ReadyNAS"
```

**Buffalo NAS**
```shodan
title:"Buffalo" "NAS"
```

**Drobo Devices**
```shodan
title:"Drobo"
```

---

## 🗄️ Finding Database Servers

```
> Loading module: Database_Scanner.dll...
> Status: QUERYING
```

> **`[NOTE]`** Common Database Ports
>
> - **MySQL**: 3306
> - **PostgreSQL**: 5432
> - **MongoDB**: 27017
> - **Redis**: 6379
> - **Cassandra**: 9042
> - **Elasticsearch**: 9200
> - **CouchDB**: 5984
> - **InfluxDB**: 8086
> - **Neo4j**: 7474

### 📡 Search Queries

**Unprotected MongoDB Instances**
```shodan
"MongoDB Server Information" port:27017 -"authentication"
```

**Redis Servers**
```shodan
"redis_version" port:6379
```

**Open PostgreSQL Servers**
```shodan
port:5432 "PostgreSQL"
```

**Elasticsearch Clusters**
```shodan
"elasticsearch" port:9200
```

**MySQL Servers with Default Credentials**
```shodan
"MySQL" port:3306 -"Access denied"
```

**CouchDB Without Auth**
```shodan
"couchdb" port:5984 "Welcome"
```

**Cassandra Databases**
```shodan
port:9042 "cassandra"
```

**InfluxDB Instances**
```shodan
port:8086 "InfluxDB"
```

**Neo4j Graph Databases**
```shodan
port:7474 "neo4j"
```

**MariaDB Servers**
```shodan
port:3306 "MariaDB"
```

**Oracle Databases**
```shodan
port:1521 "Oracle"
```

**Microsoft SQL Server**
```shodan
port:1433 "SQL Server"
```

---

## 🔒 Finding VPN & Remote Access Services

```
> Loading module: Remote_Access_Scanner.dll...
> Status: TUNNELING
```

> **`[NOTE]`** Common Remote Access Ports
>
> - **OpenVPN**: 1194
> - **WireGuard**: 51820
> - **IPSec VPN**: 500, 4500
> - **RDP**: 3389
> - **VNC**: 5900-5999
> - **SSH**: 22
> - **TeamViewer**: 5938

### 📡 Search Queries

**OpenVPN Servers**
```shodan
"OpenVPN" port:1194
```

**RDP Services with Screenshots**
```shodan
port:3389 has_screenshot:true
```

**VNC Servers**
```shodan
port:5900 "RFB"
```

**Citrix Servers**
```shodan
"Citrix" port:1494
```

**Fortinet VPN**
```shodan
"Fortinet" ssl:"Fortinet"
```

**Palo Alto GlobalProtect**
```shodan
"GlobalProtect" ssl:"Palo Alto"
```

**Pulse Secure VPN**
```shodan
"Pulse Secure" port:443
```

**SonicWall VPN**
```shodan
"SonicWall" port:443
```

**AnyDesk**
```shodan
port:7070 "AnyDesk"
```

**TeamViewer**
```shodan
port:5938 "TeamViewer"
```

**WireGuard VPN**
```shodan
port:51820
```

---

## 🖨️ Finding Printers & Multifunction Devices

```
> Loading module: Printer_Scanner.dll...
> Status: READY
```

> **`[NOTE]`** Common Printer Ports
>
> - **HP/Canon/Xerox**: 80, 443, 9100 (JetDirect)
> - **Ricoh**: 80, 443, 8080
> - **IPP**: 631

### 📡 Search Queries

**HP Printers**
```shodan
"HP" port:9100
```

**Canon Printers**
```shodan
title:"Canon" port:80
```

**Xerox Devices**
```shodan
"Xerox" OR "WorkCentre"
```

**Open Printer Interfaces**
```shodan
"printer" port:80 has_screenshot:true
```

**Brother Printers**
```shodan
title:"Brother" "printer"
```

**Epson Printers**
```shodan
title:"EPSON" port:80
```

**Ricoh Devices**
```shodan
title:"Ricoh" port:80
```

**Kyocera Printers**
```shodan
title:"Kyocera"
```

**IPP Printing Protocol**
```shodan
port:631 "IPP"
```

---

## 🎮 Finding Game Servers

```
> Loading module: Game_Server_Scanner.dll...
> Status: ONLINE
```

> **`[NOTE]`** Common Game Server Ports
>
> - **Minecraft**: 25565
> - **Counter-Strike**: 27015
> - **ARK**: 27015
> - **Rust**: 28015
> - **TeamSpeak**: 9987

### 📡 Search Queries

**Minecraft Servers**
```shodan
"Minecraft Server" port:25565
```

**Counter-Strike Servers**
```shodan
product:"Counter-Strike" port:27015
```

**ARK Survival Servers**
```shodan
"ARK" port:27015
```

**Rust Game Servers**
```shodan
product:"Rust" port:28015
```

**TeamSpeak Servers**
```shodan
product:"TeamSpeak" port:9987
```

**Valheim Servers**
```shodan
"Valheim" port:2456
```

**Garry's Mod Servers**
```shodan
product:"Garry's Mod"
```

**7 Days to Die**
```shodan
"7 Days to Die" port:26900
```

---

## ☁️ Finding Cloud Services & APIs

```
> Loading module: Cloud_Scanner.dll...
> Status: DEPLOYED
```

> **`[NOTE]`** Common Cloud/API Ports
>
> - **HTTP/HTTPS**: 80, 443
> - **API Gateways**: 8080, 8443
> - **Docker**: 2375, 2376

### 📡 Search Queries

**Exposed Docker APIs**
```shodan
port:2375 product:"Docker"
```

**Kubernetes Dashboards**
```shodan
title:"Kubernetes Dashboard"
```

**Jenkins Servers**
```shodan
"X-Jenkins" http.title:"Dashboard"
```

**GitLab Instances**
```shodan
http.title:"GitLab"
```

**Grafana Dashboards**
```shodan
title:"Grafana"
```

**Kibana Dashboards**
```shodan
title:"Kibana"
```

**Prometheus Servers**
```shodan
title:"Prometheus" port:9090
```

**Portainer**
```shodan
title:"Portainer"
```

**Swagger API Docs**
```shodan
title:"Swagger UI"
```

**AWS Metadata Leaks**
```shodan
"169.254.169.254"
```

**Exposed .git Directories**
```shodan
http.title:"Index of /.git"
```

**Exposed .env Files**
```shodan
http.title:"Index of" ".env"
```

---

## 🌐 Finding Network Infrastructure

```
> Loading module: Network_Infra_Scanner.dll...
> Status: ROUTING
```

> **`[NOTE]`** Common Network Device Ports
>
> - **SNMP**: 161
> - **SSH**: 22
> - **Telnet**: 23
> - **BGP**: 179

### 📡 Search Queries

**Cisco Routers**
```shodan
"cisco" product:"Cisco IOS"
```

**Juniper Devices**
```shodan
"Juniper" product:"Juniper"
```

**MikroTik Routers**
```shodan
product:"MikroTik"
```

**Ubiquiti EdgeOS**
```shodan
"EdgeOS" OR "Ubiquiti"
```

**pfSense Firewalls**
```shodan
title:"pfSense"
```

**OPNsense Firewalls**
```shodan
title:"OPNsense"
```

**FortiGate Firewalls**
```shodan
"FortiGate" ssl:"Fortinet"
```

**SonicWall Firewalls**
```shodan
"SonicWall" port:443
```

**F5 BIG-IP**
```shodan
"BIG-IP" product:"BIG-IP"
```

**Netgear Devices**
```shodan
"NETGEAR" product:"NETGEAR"
```

**TP-Link Routers**
```shodan
"TP-LINK" product:"TP-LINK"
```

**SNMP Enabled Devices**
```shodan
port:161 "public"
```

---

## 🔍 Advanced Filtering Techniques

```
> Loading module: Advanced_Filters.dll...
> Status: OPTIMIZED
```

> **`[TIP]`** Pro Tips for Finding Specific Vulnerabilities
>
> Master these techniques for precise vulnerability discovery.

**Finding Weak SSL/TLS Configurations** 🔐
```shodan
ssl:"OpenSSL/1.0" "weak"
```

**Finding Devices with Exposed Information** 📤
```shodan
"Basic realm" port:80
```

**Finding CVE-Specific Vulnerabilities** 🐛
```shodan
vuln:CVE-2021-21315
```

**Finding Devices by HTTP Response Headers** 📋
```shodan
"X-Powered-By: PHP" port:80
```

**Finding Devices Running Custom Applications** ⚙️
```shodan
header:"X-Custom-Header"
```

**Geographic Proximity Search** 📍
```shodan
geo:"40.7128,-74.0060"
```

**Finding Self-Signed Certificates**
```shodan
ssl.cert.issuer.cn:ssl.cert.subject.cn
```

**Finding Expired Certificates**
```shodan
ssl.cert.expired:true
```

**Finding Specific Web Technologies**
```shodan
http.component:"WordPress" http.component_category:"cms"
```

---

## ⌨️ Using Shodan CLI for Advanced Queries

```
> Loading module: CLI_Interface.dll...
> Status: TERMINAL
```

> **`[NOTE]`** Shodan CLI Commands
>
> Command-line interface for powerful Shodan interactions.

### 📥 Installation
```bash
pip install shodan
```

### 🔑 Initialize Shodan CLI
```bash
shodan init YOUR_API_KEY
```

### 🔎 Basic Search
```bash
shodan search "apache"
```

### 📊 Export Results to CSV
```bash
shodan search "title:Camera" --fields ip_str,port,org,country,html
```

### 📥 Download Bulk Data
```bash
shodan download camera-results "title:Camera"
```

### 🖥️ Host Information Lookup
```bash
shodan host 8.8.8.8
```

### 🔄 Stream Shodan Data
```bash
shodan stream
```

### 📈 Query with Count
```bash
shodan count "apache"
```

### 🔍 Get Specific Info
```bash
shodan host --history 8.8.8.8
```

### 📋 Parse Downloaded Data
```bash
shodan parse camera-results.json.gz --fields ip_str,port
```

### 🔗 Domain Info
```bash
shodan domain example.com
```

### 🌐 DNS Lookup
```bash
shodan dns resolve example.com
```

### 🔙 Reverse DNS
```bash
shodan dns reverse 8.8.8.8
```

### 📊 Stats on Query
```bash
shodan stats --facets country apache
```

### 🗃️ Convert Data
```bash
shodan convert results.json.gz csv
```

---

## 🔌 Shodan API Usage

```
> Loading module: API_Interface.dll...
> Status: CONNECTED
```

### 🐍 Python Examples

**Basic Search**
```python
import shodan

api = shodan.Shodan('YOUR_API_KEY')

# Search for Apache servers
results = api.search('apache')

for result in results['matches']:
    print(f"IP: {result['ip_str']}")
    print(f"Port: {result['port']}")
    print(f"Org: {result.get('org', 'N/A')}")
    print("---")
```

**Host Lookup**
```python
import shodan

api = shodan.Shodan('YOUR_API_KEY')

# Get info on specific host
host = api.host('8.8.8.8')

print(f"IP: {host['ip_str']}")
print(f"Organization: {host.get('org', 'N/A')}")
print(f"OS: {host.get('os', 'N/A')}")

for item in host['data']:
    print(f"Port: {item['port']}")
    print(f"Banner: {item['data'][:100]}...")
```

**Streaming API**
```python
import shodan

api = shodan.Shodan('YOUR_API_KEY')

# Subscribe to banners
for banner in api.stream.banners():
    print(banner)
```

**Network Alerts**
```python
import shodan

api = shodan.Shodan('YOUR_API_KEY')

# Create alert for network monitoring
alert = api.create_alert('My Network', '192.168.1.0/24')
print(f"Alert ID: {alert['id']}")
```

---

## 🎯 Practical Search Strategies

```
> Loading module: Strategy_Engine.dll...
> Status: TACTICAL
```

**Strategy 1: Finding Devices by Vulnerability Chain**
```shodan
product:"Cisco" "privilege" "escalation"
```

**Strategy 2: Finding Default Installation Instances**
```shodan
"default username is" OR "default password is"
```

**Strategy 3: Finding Recently Indexed Devices**
```shodan
"200 OK" after:2024-01-01
```

**Strategy 4: Finding Devices with Known CVEs**
```shodan
"Apache/2.4.49" OR "Apache/2.4.50"
```

**Strategy 5: Finding Devices in Critical Infrastructure**
```shodan
"SCADA" OR "HMI" OR "historian"
```

**Strategy 6: Finding Your Organization's Exposure**
```shodan
org:"Your Company Name"
```

**Strategy 7: Finding Shadow IT**
```shodan
ssl.cert.subject.cn:"yourcompany.com" -org:"Your Company"
```

**Strategy 8: Finding Misconfigured Cloud Storage**
```shodan
title:"Index of /backup"
```

**Strategy 9: Finding Exposed Development Environments**
```shodan
http.title:"phpMyAdmin" OR http.title:"Adminer"
```

**Strategy 10: Finding Exposed Admin Panels**
```shodan
http.title:"admin" http.status:200 -http.title:"login"
```

---

## ⚖️ Ethical Considerations

```
> Loading module: Ethics_Protocol.dll...
> Status: CRITICAL
```

> **`[IMPORTANT]`** Ethical Considerations
>
> - **Permission**: Always have explicit permission to search and access devices before attempting any kind of access or testing.
> - **Responsibility**: Use the information to help improve security and report vulnerabilities responsibly.
> - **Legal Compliance**: Ensure compliance with all legal regulations and guidelines (CFAA, GDPR, etc.).
> - **No Malicious Intent**: Do not use Shodan searches for illegal activities, unauthorized access, or data theft.

### ✅ Quick Reference Checklist

> **`[NOTE]`** Pre-Search Checklist
>
> Before conducting any Shodan search, ensure you've covered these items:

- [ ] Do you have authorization to perform this search?
- [ ] Is your search for legitimate security purposes?
- [ ] Have you reviewed the legal implications in your jurisdiction?
- [ ] Are you prepared to responsibly disclose any vulnerabilities?
- [ ] Have you understood Shodan's Terms of Service?
- [ ] Will you report findings to the appropriate parties?

### ❌ Common Mistakes to Avoid

```
[✗] Over-Broad Searches: Refine queries to reduce false positives
[✗] Ignoring False Positives: Not every result is actually vulnerable
[✗] Unauthorized Testing: Never attempt to access systems without permission
[✗] Public Disclosure: Never publicly disclose before the vendor has patched
[✗] Assuming Ownership: Just because a service is exposed doesn't mean you should access it
[✗] Ignoring Honeypots: Some honeypots are intentionally exposed for research
```

---

## 📝 Responsible Vulnerability Disclosure

```
> Loading module: Disclosure_Protocol.dll...
> Status: ETHICAL
```

> **`[TIP]`** Best Practices for Reporting Found Issues
>
> Responsible disclosure is essential for ethical security research.

### ✅ Steps for Responsible Disclosure

**1. Identify the Vulnerability** 🔍
   - Confirm the vulnerability exists
   - Document your findings with evidence
   - Note the affected systems and versions

**2. Find Contact Information** 📧
   - Look for security.txt file at `/.well-known/security.txt`
   - Check the organization's website for security contacts
   - Search for published vulnerability disclosure policies
   - Use reverse DNS or whois for contact details

**3. Report Responsibly** 📨
   - Send detailed technical report to security contact
   - Provide reasonable timeline for patching (typically 90 days)
   - Do not publicly disclose before vendor has patched
   - Offer to assist with verification of the fix

**4. Document Everything** 📋
   - Keep records of all communications
   - Note dates and responses received
   - Track patch releases and updates

### ⏰ Sample Disclosure Timeline

| Timeline | Action |
|:--------:|:-------|
| **Day 1** | Discover and confirm vulnerability |
| **Day 1** | Contact vendor with details |
| **Day 30** | Follow up if no response |
| **Day 60** | Consider escalation |
| **Day 90** | Coordinate public disclosure after patch |

---

## 📚 Resources and References

```
> Loading module: Resources.dll...
> Status: INDEXED
```

### 🔗 Official Documentation
- [Shodan Official Website](https://www.shodan.io)
- [Shodan CLI Documentation](https://cli.shodan.io)
- [Shodan API Documentation](https://developer.shodan.io)
- [Shodan Query Cheat Sheet](https://cheatsheet.shodan.io)

### 🛠️ Related Tools
| Tool | Description |
|:-----|:------------|
| **Shodan Dorking** | Use complex queries to find specific devices |
| **Censys** | Alternative internet scan database |
| **Shodan Scripts** | Official GitHub repository with helpful scripts |
| **Shodan Maps** | Geographic visualization of devices |
| **ZoomEye** | Chinese alternative to Shodan |
| **GreyNoise** | Identifies mass internet scanning |
| **BinaryEdge** | Cybersecurity data platform |

### 📖 Learning Resources
| Purpose | Description |
|:--------|:------------|
| **Security Research** | Use Shodan for vulnerability research |
| **Threat Intelligence** | Monitor for exposed company devices |
| **Network Discovery** | Map your organization's external footprint |
| **Compliance** | Identify devices not in compliance with policies |

---

## 💼 Common Use Cases

```
> Loading module: Use_Cases.dll...
> Status: OPERATIONAL
```

### 🔬 Security Researchers
- Identify vulnerable devices
- Track exploitation of CVEs
- Monitor emerging threats

### 🔐 Penetration Testers
- Recon targets (with authorization)
- Find internet-facing services
- Identify tech stacks

### 🖥️ System Administrators
- Audit exposed services
- Monitor your organization's external presence
- Identify rogue devices

### 🏢 Organizations
- Monitor brand/product mentions
- Identify misconfigurations
- Track shadow IT

---

<div align="center">

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                                                                              ║
║                     [ END OF TRANSMISSION // SHODAN ]                        ║
║                                                                              ║
║              "The future is already here — it's just not                     ║
║                        evenly distributed."                                  ║
║                                                                              ║
║                                        - William Gibson                      ║
║                                                                              ║
║      ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓               ║
║                                                                              ║
║                    With great power comes great responsibility.              ║
║             Always conduct your research ethically and responsibly.          ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

</div>
