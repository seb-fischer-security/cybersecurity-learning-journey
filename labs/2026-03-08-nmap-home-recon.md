# Lab 02: Nmap Network Reconnaissance
**Date:** Mar 08, 2026 | **Author:** S. Fischer | **Tool:** nmap on Kali Linux

## 🎯 Objectives
- Complete home router enumeration (Host Discovery, TCP/UDP ports, service versions)
- Build pentest reporting skills
- Foundation for vulnerability scanning

## 🔍 Target Overview
| Property | Value |
|----------|-------|
| IP | 192.168.1.1 |
| MAC | 60:32:4F |
| Network | 192.168.1.0/24 |

## 📊 Scan Results

### 1. Host Discovery (`-sn`)
**Command:** `nmap -sn 192.168.1.0/24`  
**Duration:** 1.5s | **Result:** 256/256 hosts up  

<img width="728" height="582" alt="Screenshot 2026-03-08 011705" src="https://github.com/user-attachments/assets/53b92cc4-db0a-4db2-9e2d-83d8b3bb350b" />

### 2. TCP SYN Scan (Default)
**Command:** `nmap 192.168.1.1`  
**Duration:** 4.8s | **Ports:** 80/tcp http, 53/tcp filtered  

<img width="712" height="572" alt="Screenshot 2026-03-08 011928" src="https://github.com/user-attachments/assets/dc682f7a-6d6b-4332-ba11-9c1d2744a3fb" />

### 3. Service Version (`-sV`)
**Command:** `nmap -sV 192.168.1.1`  
**Duration:** 36.1s | **Service:** lighttpd 1.4.35 (80/tcp)  

<img width="683" height="536" alt="Screenshot 2026-03-08 013226" src="https://github.com/user-attachments/assets/77442246-7976-4d0d-ab0b-de6b87d3e526" />

### 4. UDP Scan (`-sU`)
**Command:** `nmap -sU 192.168.1.1`  
**Duration:** 4.5s | **Port:** 53/udp domain  

<img width="684" height="550" alt="Screenshot 2026-03-08 013511" src="https://github.com/user-attachments/assets/784a912f-f3f7-407f-9b04-b71052772bac" />

## 📈 Summary Table
| Scan Type | Command | Open Ports | Duration |
|-----------|---------|------------|----------|
| Host Discovery | `nmap -sn 192.168.1.0/24` | 256 hosts | 1.5s |
| TCP | `nmap 192.168.1.1` | 80/tcp, 53/tcp filtered | 4.8s |
| Service Version | `nmap -sV 192.168.1.1` | lighttpd 1.4.35 | 36.1s |
| UDP | `nmap -sU 192.168.1.1` | 53/udp | 4.5s |

## 🔒 Security Assessment
- ✅ **No default credentials** (admin/admin failed)
- ⚠️ **lighttpd 1.4.35** → CVE research needed
- ✅ **Minimal attack surface** (only expected services)

## 💻 All Commands
```bash
nmap -sn 192.168.1.0/24     # Find all hosts
nmap 192.168.1.1           # TCP ports
nmap -sV 192.168.1.1       # Service versions
nmap -sU 192.168.1.1       # UDP ports
