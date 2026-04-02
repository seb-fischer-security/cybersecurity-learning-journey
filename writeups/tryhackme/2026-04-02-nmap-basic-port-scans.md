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
