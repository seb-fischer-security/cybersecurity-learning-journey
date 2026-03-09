# Lab 02: Nmap NSE Vulnerability Scan
**Date:** Mar 09, 2026 | **Author:** S. Fischer | **Tool:** nmap on Kali Linux

## 🎯 Objectives
- Run NSE vulnerability scripts against the home router
- Identify possible web-related issues on exposed services
- Extend basic reconnaissance with first vulnerability checks

## 🔍 Target Overview
| Property | Value |
|----------|-------|
| IP | 192.168.1.1 |
| MAC | 60:32:4F |
| Services | 53/tcp, 80/tcp, 443/tcp |

## 📊 Scan Results

### 1. NSE Vuln Scan (`--script vuln`)
**Command:** `nmap -sV --script vuln 192.168.1.1`  
**Duration:** 150.87s | **Result:** No confirmed vulnerabilities identified  

<img width="728" height="582" alt="Screenshot 2026-03-09 163943" src="https://github.com/user-attachments/assets/e76b5b74-cd82-4280-8d13-b247a08aa394" />

## 📈 Summary Table
| Scan Type | Command | Open Ports / Findings | Duration |
|-----------|---------|-----------------------|----------|
| NSE Vuln Scan | `nmap -sV --script vuln 192.168.1.1` | 53/tcp DNS, 80/tcp HTTP, 443/tcp HTTPS | 150.87s |

## 🔒 Security Assessment
- ✅ **No CSRF vulnerabilities found**
- ✅ **No DOM-based XSS found**
- ✅ **No stored XSS found**
- ⚠️ **Script execution errors** on `http-vuln-cve2014-3704` and `http-aspnet-debug`
- ⚠️ **ZTE web server 1.0 (2015)** identified on ports 80 and 443
- ℹ️ **3 services unrecognized** despite returning data

## 💻 All Commands
```bash
nmap -sV --script vuln 192.168.1.1   # Run NSE vulnerability scripts
```
