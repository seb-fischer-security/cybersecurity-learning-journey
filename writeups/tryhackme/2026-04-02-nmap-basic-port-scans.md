# 🔎 TryHackMe Writeup – Nmap Basic Port Scans

📅 Date: 2026-04-02  
🖥️ Environment: TryHackMe AttackBox  
🛠️ Tool: `nmap`

---

## 🎯 Objective

The goal of this room was to build a stronger understanding of **basic Nmap port scanning techniques** and how different scan types reveal open, closed, or filtered services.

This room focused on:

- TCP Connect Scans
- TCP SYN Scans
- UDP Scans
- Port selection and scan scope
- Scan rate tuning
- Parallel probe behavior


## 🌐 Room Context

This room continues the Nmap learning path after **host discovery**.

The previous step focused on identifying **live systems**.  
This room builds on that by answering the next important question:

> Which ports are open, and which services are listening?

This is a core part of reconnaissance because open ports reveal the **available attack surface** of a target.

---

## 🚪 Task 2 – TCP and UDP Ports

In network scanning, a **port** identifies a specific service running on a host.

Just like an IP address identifies a device on a network, a TCP or UDP port identifies **which service is listening** on that device.

Common examples include:

- HTTP → TCP `80`
- HTTPS → TCP `443`
- SSH → TCP `22`
- DNS → UDP `53`

A service usually binds to a default port, although administrators may configure custom ports if needed.


### 🧠 Port States in Nmap

In simple terms, ports can often be described as either:

- **Open** → a service is listening
- **Closed** → no service is listening

However, in real environments, firewalls and filtering devices make things more complex.

Because of this, Nmap uses **six possible port states**:

| State | Meaning |
|------|---------|
| **Open** | A service is listening on the port |
| **Closed** | No service is listening, but the port is reachable |
| **Filtered** | Nmap cannot determine the state because access is blocked |
| **Unfiltered** | The port is reachable, but Nmap cannot determine if it is open or closed |
| **Open\|Filtered** | Nmap cannot determine whether the port is open or filtered |
| **Closed\|Filtered** | Nmap cannot determine whether the port is closed or filtered |

---

### 🎯 Practical Relevance

From a pentesting perspective, the most interesting state is:

```text
Open
```

because an open port indicates that a service is available for:

- enumeration
- fingerprinting
- possible exploitation

---

### 🧪 Quick Reference

Some important default ports covered in this task:

| Service | Protocol / Port |
|--------|------------------|
| DNS | UDP `53` |
| SSH | TCP `22` |

---

## 🧬 Task 3 – TCP Flags

To understand how Nmap performs different scan types, it is important to understand the **TCP flags** used inside the TCP header.

These flags control how TCP connections are started, maintained, and terminated.


### 🚩 Common TCP Flags

The following TCP flags are especially relevant for Nmap scanning:

| Flag | Meaning |
|------|---------|
| **URG** | Urgent data |
| **ACK** | Acknowledgement |
| **PSH** | Push data to application |
| **RST** | Reset the connection |
| **SYN** | Initiate a TCP connection |
| **FIN** | Finish / close a connection |

---

### 🔁 Most Important Flags for Port Scanning

#### **SYN**
Used to **start a TCP connection**.

This is the first packet sent during the **TCP three-way handshake**.

#### **RST**
Used to **reset or reject a connection**.

This is especially important during scanning because it can indicate:

- a **closed port**
- or that the connection is being terminated intentionally

---

### 🧠 Why This Matters for Nmap

Nmap relies heavily on TCP flag behavior to determine whether a port is:

- open
- closed
- filtered

For example:

- **SYN** → used in SYN scans (`-sS`)
- **RST** → often returned when a port is closed

Understanding these flags makes scan behavior much easier to interpret.

---

## 📌 Key Takeaways

- Ports identify **specific services** on a host  
- Nmap recognizes **6 different port states**  
- The most important state for a pentester is **Open**  
- TCP flags help define how communication behaves  
- `SYN` starts a connection, while `RST` resets it  
- Understanding TCP flags is essential for interpreting Nmap scan results  

---

## 🔌 Task 4 – TCP Connect Scan

A **TCP Connect Scan** works by completing the full **TCP three-way handshake**.

This is the most basic method of checking whether a TCP port is open.


### 🔁 How It Works

The scan follows the normal TCP connection process:

1. Client sends **SYN**
2. Server replies with **SYN/ACK** (if the port is open)
3. Client sends **ACK**
4. Nmap then tears the connection down with **RST/ACK**

This means the connection is actually established briefly before being closed again.

---

### 🔧 Basic Command

```bash
nmap -sT 10.81.174.63
```

### 📌 Explanation

- `-sT` → **TCP Connect Scan**
- Completes the full TCP handshake
- Used to identify **open TCP ports**

This scan is especially important because it is the **default fallback scan** when Nmap is run **without elevated privileges**.

In other words:

- **local user** → `-sT`
- **root / sudo** → `-sS` usually possible

---

### 🧠 Why It Matters

A TCP Connect Scan is:

- simple
- reliable
- easy to understand

However, compared to a SYN scan, it is also:

- more visible
- less stealthy
- more likely to generate logs

This is because the target system sees a **real completed TCP connection attempt**.

---

### 🚀 Useful Additional Options

#### Fast Mode

```bash
nmap -sT -F 10.81.174.63
```

- `-F` → scans only the **100 most common ports**
- useful for quicker checks

#### Sequential Port Order

```bash
nmap -sT -r 10.81.174.63
```

- `-r` → scans ports in **consecutive order**
- instead of Nmap’s default randomized order

This can be useful when testing how services behave during startup or when checking port consistency.

---

### 🧪 Practical Findings

During the room scan, a **newly opened port** was identified:

```text
110/tcp
```

Nmap’s service guess for this port was:

```text
POP3
```

This indicates that a **mail-related service** had been added since the previous scan.

This is a strong example of why repeat scanning matters:

- new services can appear
- attack surface can change over time
- previously closed ports may become available later

---

## 📌 Key Takeaways

- `-sT` performs a **TCP Connect Scan**  
- It completes the full **TCP three-way handshake**  
- This scan is the default when running Nmap without elevated privileges  
- It is reliable, but less stealthy than a SYN scan  
- Port `110/tcp` was discovered as newly open and identified as **POP3**  
