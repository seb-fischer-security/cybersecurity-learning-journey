# 🧠 TryHackMe Writeup – Nmap: The Basics

📅 Date: 2026-03-26  
🖥️ Environment: Kali Linux / TryHackMe AttackBox  
🎯 Target: `10.114.181.209`  
🛠️ Tool: nmap  



<img width="1482" height="529" alt="Screenshot 2026-03-26 082133" src="https://github.com/user-attachments/assets/fed1ae5d-b655-41e7-ba23-aa18116e36a3" />

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

---

## ⏱️ Task 5 – Timing Control

Nmap provides several options to control **how fast or slow** a scan should run.

This is important because scan speed can affect:

- detection by security tools
- scan reliability
- overall scan duration


### 🚀 Timing Templates

Nmap includes six built-in timing templates:

| Template | Name |
|--------|------|
| `-T0` | paranoid |
| `-T1` | sneaky |
| `-T2` | polite |
| `-T3` | normal |
| `-T4` | aggressive |
| `-T5` | insane |

Example:

```bash
nmap -sS -T4 10.114.181.209
```

### 📌 Explanation

- `-T0` to `-T2` → slower and quieter scans
- `-T3` → default timing
- `-T4` and `-T5` → much faster, but more noisy

The non-numeric equivalent of `-T4` is:

```text
-T aggressive
```

---

### ⚠️ Why Timing Matters

Scan timing can influence how a target reacts.

Slower scans may help with:

- avoiding detection
- reducing traffic spikes
- working around unstable networks

Faster scans may help with:

- saving time
- broad reconnaissance
- lab environments and internal testing

In real-world environments, timing should always be adjusted based on:

- scan objective
- network stability
- detection risk

---

### 🔧 Additional Timing Controls

Nmap also provides more advanced tuning options.

#### Packet Rate Control

```bash
nmap --min-rate 100 --max-rate 500 10.114.181.209
```

- `--min-rate` → minimum number of packets per second
- `--max-rate` → maximum number of packets per second

These options allow more direct control over scan speed.

---

#### Host Timeout

```bash
nmap --host-timeout 30s 10.114.181.209
```

- `--host-timeout` → maximum time Nmap should wait for a host

This is useful when a target is:

- very slow
- unstable
- not responding reliably


## 📌 Key Takeaways

- Nmap timing can be adjusted depending on speed and stealth requirements  
- `-T4` is equivalent to **aggressive timing**  
- Slower scans may reduce detection risk  
- Faster scans are useful in trusted or lab environments  
- Additional options like `--min-rate` and `--host-timeout` provide more precise control  

---

## 💾 Task 6 – Output Control

Nmap provides several options to control **how much information is shown during a scan** and **how scan results are saved**.

This is especially useful for:

- long-running scans
- troubleshooting
- documentation
- reporting


### 🔊 Verbose Output

To display more information while a scan is running:

```bash
nmap -v 10.114.181.209
```

### 📌 Explanation

- `-v` → **Verbose mode**
- Shows additional real-time scan progress
- Useful for understanding what Nmap is doing during each phase

This can include information such as:

- host discovery progress
- DNS resolution
- port scan stages
- discovered open ports

Verbosity can also be increased further:

```bash
nmap -vv 10.114.181.209
```

This is helpful when learning or troubleshooting scan behavior.

---

### 🐞 Debugging Output

For even more detailed internal information:

```bash
nmap -d 10.114.181.209
```

### 📌 Explanation

- `-d` → **Debugging mode**
- Displays internal scan logic and detailed troubleshooting information
- Useful when analyzing unexpected behavior or understanding how Nmap processes a scan

The answer to the room question was:

```text
-d
```

⚠️ Debug output can become extremely noisy, especially at higher levels.

---

### 📝 Saving Scan Results

Saving scan output is a best practice because it allows results to be:

- reviewed later
- reused for reporting
- parsed with other tools
- preserved without rerunning scans

Nmap supports multiple output formats.

---

### 📄 Human-Readable Output

```bash
nmap -oN scan.txt 10.114.181.209
```

- `-oN` → **Normal output**
- Best for manual reading and documentation

---

### 🧾 XML Output

```bash
nmap -oX scan.xml 10.114.181.209
```

- `-oX` → **XML output**
- Useful for automation and importing into tools

---

### 🔎 Grepable Output

```bash
nmap -oG scan.gnmap 10.114.181.209
```

- `-oG` → **grepable output**
- Useful for command-line filtering with tools like:

  - `grep`
  - `awk`

---

### 📦 All Major Formats at Once

```bash
nmap -oA scan 10.114.181.209
```

### 📌 Explanation

- `-oA` → saves results in all major formats:

  - `scan.nmap`
  - `scan.xml`
  - `scan.gnmap`

This is one of the most practical options because it provides:

- readable output
- machine-readable output
- grep-friendly output

all in a single scan.


## 📌 Key Takeaways

- `-v` enables verbose scan output  
- `-d` enables debugging information  
- Saving scan results is a core reconnaissance best practice  
- `-oA` is especially useful because it stores results in multiple formats at once  
- Proper output handling improves analysis, reporting, and repeatability  

---

## ✅ Task 7 – Conclusion and Summary

This room provided a strong foundation in using **Nmap for reconnaissance and service enumeration**.

The key topics covered throughout the room included:

- host discovery
- port scanning
- service and version detection
- timing control
- output formatting and reporting


### 🧠 Key Learning Outcome

One of the most important takeaways from this room is that **Nmap behaves differently depending on user privileges**.

When Nmap is run with **sudo / root privileges**, it can use more advanced scan types such as:

```bash
sudo nmap -sS 10.114.181.209
```

This enables a **SYN Scan**, which requires the ability to craft raw packets.

---

### 👤 Running Nmap as a Local User

If Nmap is run **without elevated privileges**:

```bash
nmap 10.114.181.209
```

it will **not** use a SYN scan by default.

Instead, it will fall back to:

```text
Connect Scan
```

### 📌 Explanation

This means Nmap will use:

```text
-sT
```

instead of:

```text
-sS
```

because a normal local user cannot create the raw packets required for a SYN scan.

---

### 🔁 Practical Difference

| Privilege Level | Default Scan Type |
|----------------|------------------|
| Root / sudo | SYN Scan (`-sS`) |
| Local user | Connect Scan (`-sT`) |

This is an important operational detail because scan results, stealth, and behavior can differ depending on how Nmap is executed.


## 📌 Final Takeaways

- Nmap is a powerful and flexible reconnaissance tool  
- Running Nmap with `sudo` unlocks more advanced scanning capabilities  
- A local user scan defaults to a **Connect Scan (`-sT`)**  
- Understanding scan behavior is just as important as memorizing commands  
- This room covered the essential building blocks needed for more advanced Nmap usage  
