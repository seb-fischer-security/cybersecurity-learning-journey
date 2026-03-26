# 🧠 TryHackMe Writeup – Nmap: The Basics

📅 Date: 2026-03-26  
🖥️ Environment: Kali Linux / TryHackMe AttackBox  
🛠️ Tool: nmap  

---

## 🎯 Objective

The goal of this room is to build a solid understanding of **Nmap fundamentals** and how it is used during reconnaissance.

Key learning objectives:

- Discover live hosts on a network  
- Identify running services  
- Understand different scan techniques  
- Learn how to structure reconnaissance workflows  

---

## 🌐 Task 1 – Introduction

Nmap is an **open-source network scanner** first released in 1997.  
It is widely used in cybersecurity for **network discovery and service enumeration**.

### 🔍 Why Nmap?

Manual methods such as:

- `ping`
- `arp-scan`
- `telnet`

are inefficient because:

- they are slow  
- they require manual interaction  
- they may fail depending on firewall configurations  

Nmap solves these problems by providing:

- fast scanning  
- multiple discovery techniques  
- flexible targeting options  

---

## 🛰️ Task 2 – Host Discovery

One of the first steps in reconnaissance is identifying **which hosts are online**.

### 🔧 Basic Command

```bash
nmap -sn 192.168.0.0/24
```

### 📌 Explanation

- `-sn` → **Host discovery only**
- No port scanning is performed
- Used to identify **live systems in a network**

---

### 🎯 Target Specification

Nmap allows flexible ways to define targets:

| Method | Example | Description |
|------|--------|-------------|
| Range | `192.168.0.1-10` | Scan specific IP range |
| Subnet | `192.168.0.0/24` | Scan entire network |
| Hostname | `example.thm` | Resolve and scan domain |

---

### 🧠 Local vs Remote Network Scanning

#### 🏠 Local Network

When scanning a network you are directly connected to:

- Nmap uses **ARP requests**
- MAC addresses can be discovered
- Vendor information can be identified

Example insight:

```text
MAC Address → Device vendor → possible device type
```

---

#### 🌍 Remote Network

When scanning across routers:

- ARP is **not possible**
- Nmap uses alternative techniques:

  - ICMP (ping)
  - TCP SYN
  - TCP ACK

- Results depend on firewall behavior

---

### 🔎 List Scan (No Interaction)

```bash
nmap -sL 192.168.0.0/24
```

### 📌 Explanation

- Lists targets without scanning them
- Useful to verify scan scope before execution


## 📌 Key Takeaways

- Nmap is a powerful and flexible tool for network reconnaissance  
- Host discovery (`-sn`) is the first step in scanning  
- Local and remote scans behave differently  
- Target definition is flexible and supports multiple formats  
- Enumeration begins with identifying live hosts before deeper analysis

---

## 🚪 Task 3 – Port Scanning

After identifying live hosts, the next step is determining **which services are listening** on the target system.

A listening service means that a process is bound to a **TCP or UDP port** and is waiting for incoming connections.

Examples include:

- Web servers → TCP 80 / 443
- DNS → UDP/TCP 53
- SSH → TCP 22

---

### 🔌 TCP Connect Scan

The most basic TCP scan is the **Connect Scan**.

```bash
nmap -sT 10.114.181.209
```

### 📌 Explanation

- `-sT` → **TCP Connect Scan**
- Attempts to complete the **full TCP three-way handshake**
- If the port is open, Nmap establishes and then closes the connection

This is the most straightforward TCP scan, but it is generally considered **less stealthy** because the full connection is completed.

---

### 🕵️ SYN Scan (Stealth Scan)

A more common scan in pentesting is the **SYN Scan**.

```bash
nmap -sS 10.114.181.209
```

### 📌 Explanation

- `-sS` → **TCP SYN Scan**
- Sends only the initial **SYN packet**
- If the port is open, the target responds with **SYN-ACK**
- Nmap then replies with **RST** instead of completing the handshake

This scan is often referred to as:

- **Stealth Scan**
- **Half-Open Scan**

It is considered more stealthy because the full TCP connection is **never established**.

---

### 📡 UDP Scan

Some services use **UDP instead of TCP**, so scanning UDP ports is also important.

```bash
nmap -sU 10.114.181.209
```

### 📌 Explanation

- `-sU` → **UDP Scan**
- Used to identify services listening on UDP ports
- Common UDP services include:

  - DNS
  - DHCP
  - NTP
  - SNMP

UDP scanning is often slower and less reliable than TCP scanning because UDP does not use a connection-based handshake.

---

### 🎯 Limiting Target Ports

By default, Nmap scans the **top 1000 most common ports**.

However, scans can be adjusted depending on the goal.

#### Fast Mode

```bash
nmap -F 10.114.181.209
```

- `-F` → scans the **100 most common ports**

#### Specific Port Range

```bash
nmap -p10-1024 10.114.181.209
```

- scans ports **10 through 1024**

#### Full Port Scan

```bash
nmap -p- 10.114.181.209
```

- scans **all 65,535 TCP ports**

This is the most thorough option and useful for identifying services running on **non-standard ports**.

---

### 🧪 Practical Findings

During scanning of the target system:

- **6 TCP ports** were identified as open
- A **web server** was discovered on the target
- Accessing the web service in the browser revealed the following flag:

```text
THM{SECRET_PAGE_38B9P6}
```

This demonstrates how port scanning can directly lead to the discovery of **accessible services and hidden content**.


## 📌 Key Takeaways

- Port scanning identifies which services are available on a target  
- `-sT` performs a full TCP connection  
- `-sS` is more stealthy and commonly used in pentesting  
- `-sU` is required to discover UDP services  
- `-F` and `-p` help control scan scope  
- Full port scans (`-p-`) are useful for discovering hidden or uncommon services  

---

## 🧬 Task 4 – Version Detection

After discovering open ports, the next step is identifying **which operating system and services are running** on the target.

This provides much more context for further enumeration and possible vulnerability research.


### 🖥️ OS Detection

Nmap can attempt to identify the target operating system.

```bash
nmap -O 10.114.181.209
```

### 📌 Explanation

- `-O` → **Operating System Detection**
- Nmap analyzes different characteristics of the target host
- It then makes an **educated guess** about the operating system

This can help identify whether the target is running:

- Linux
- Windows
- network appliances
- embedded systems

⚠️ Important:  
OS detection is **not always 100% accurate**.  
It should be treated as a **strong indicator**, not absolute proof.

---

### 🔎 Service and Version Detection

To identify the software running behind open ports, Nmap provides version detection.

```bash
nmap -sV 10.114.181.209
```

### 📌 Explanation

- `-sV` → **Service / Version Detection**
- Attempts to identify:

  - service name
  - software version
  - sometimes additional platform information

This is especially useful for:

- web servers
- SSH services
- FTP services
- mail servers

Version detection is critical because it can reveal **outdated or vulnerable software**.

---

### ⚡ Aggressive Scan

Nmap also provides a more advanced all-in-one scan option.

```bash
nmap -A 10.114.181.209
```

### 📌 Explanation

- `-A` enables:

  - OS detection
  - version detection
  - traceroute
  - additional default script scanning

This is a powerful option for fast enumeration, but it is also:

- louder
- more detectable
- more intrusive than basic scans

For that reason, it is useful in labs and internal testing, but should be used carefully in real environments.

---

### 🚫 Skipping Host Discovery

Sometimes a target does not respond during the host discovery phase, even though it is actually online.

To force Nmap to scan anyway:

```bash
nmap -Pn 10.114.181.209
```

### 📌 Explanation

- `-Pn` → Treat the target as **online**
- Skips host discovery
- Useful when ICMP or other discovery traffic is blocked

This is especially relevant when scanning hosts behind firewalls or restrictive filtering rules.

---

### 🧪 Practical Findings

Using version detection against the target revealed the following web server:

```text
lighttpd 1.4.74
```

This means:

- the web service was successfully identified
- software enumeration provided a **specific product and version**
- this information could later be used for:

  - vulnerability research
  - exploit verification
  - service fingerprinting


## 📌 Key Takeaways

- `-O` helps estimate the target operating system  
- `-sV` reveals service and software version details  
- `-A` combines multiple advanced enumeration features  
- `-Pn` forces scans against hosts that appear offline  
- Version detection is essential for identifying potentially outdated services  
