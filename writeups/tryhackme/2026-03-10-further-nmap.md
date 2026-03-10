# 🧠 TryHackMe Writeup: Further Nmap

📅 Date: 2026-03-10  
🌐 Platform: TryHackMe  
📚 Room: Further Nmap  
🔗 https://tryhackme.com/room/furthernmap  

<img width="1482" height="564" alt="Screenshot 2026-03-10 211824" src="https://github.com/user-attachments/assets/6c94404e-d863-4c91-87ed-959f381c82b6" />

---

# 🎯 Objective

The **Further Nmap** room expands on the basic usage of Nmap and introduces more advanced scanning concepts.

The room focuses on:

- different Nmap scan techniques
- how TCP responses indicate port states
- scanning specific ports
- scanning all ports
- aggressive scanning
- using the Nmap Scripting Engine (NSE)
- saving scan results

These concepts are essential for **network reconnaissance during penetration testing**.

---

# 🔎 SYN Scan (Stealth Scan)

One of the most commonly used Nmap scans is the **SYN scan**.

Example command:

```
nmap -sS <target>
```

The SYN scan is also known as:

- **Stealth Scan**
- **Half-Open Scan**

Instead of completing the full TCP handshake, Nmap sends a SYN packet and observes the response.

### TCP Responses

| Response | Meaning |
|--------|--------|
| SYN-ACK | Port is open |
| RST | Port is closed |
| No response | Port is filtered |

If a port is closed, the server sends a **RST (Reset)** packet.

This behaviour is defined in **RFC 793**, which specifies the TCP protocol.

---

# 🎯 Scanning Specific Ports

Nmap allows scanning of specific ports using the `-p` switch.

Example:

```
nmap -p 80 <target>
```

This command tells Nmap to scan **only port 80**.

---

# 🌐 Scanning All Ports

By default, Nmap scans the **top 1000 most common ports**.

To scan all ports:

```
nmap -p- <target>
```

This scans the entire port range:

```
1–65535
```

---

# ⚡ Aggressive Scan

Nmap provides an aggressive scan option.

Example:

```
nmap -A <target>
```

This enables several features simultaneously:

- OS detection
- service version detection
- traceroute
- script scanning

Aggressive scans provide more information but are **much louder and easier to detect**.

---

# 🧠 Nmap Scripting Engine (NSE)

Nmap includes a scripting system called the **Nmap Scripting Engine (NSE)**.

Scripts can be executed using:

```
nmap --script=<script-name> <target>
```

To run vulnerability scripts:

```
nmap --script=vuln <target>
```

These scripts help identify **known vulnerabilities and service misconfigurations**.

---

# 💾 Saving Scan Results

Nmap allows saving scan results for later analysis.

Example:

```
nmap -oA scan <target>
```

This saves the results in three formats:

```
scan.nmap
scan.xml
scan.gnmap
```

Saving scan results is useful for **reporting and documentation**.

---

# 📌 Key Takeaways

Important lessons from this room:

- SYN scans are a stealthier method of port scanning.
- TCP responses help determine the state of ports.
- Specific ports can be scanned using `-p`.
- All ports can be scanned using `-p-`.
- Aggressive scans enable multiple detection techniques.
- NSE scripts extend Nmap’s functionality.
- Saving scan output is useful for documentation and analysis.
