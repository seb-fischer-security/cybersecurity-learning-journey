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

---

## 📌 Key Takeaways

- Nmap is a powerful and flexible tool for network reconnaissance  
- Host discovery (`-sn`) is the first step in scanning  
- Local and remote scans behave differently  
- Target definition is flexible and supports multiple formats  
- Enumeration begins with identifying live hosts before deeper analysis  
