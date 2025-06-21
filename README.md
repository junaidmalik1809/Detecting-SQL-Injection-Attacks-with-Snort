# SQL Injection Detection with Snort IDS – Cybersecurity 

## Overview
This project demonstrates the simulation of a real-world SQL injection attack using `sqlmap` and its detection using a custom rule written in `Snort IDS`. This forms part of my structured Phase 4 cybersecurity lab focusing on offensive security and intrusion detection.

---

## 🖥️ Lab Topology
Kali Linux (Attacker) ───── pfSense (Router) ───── Ubuntu Server (Victim + Snort IDS)
192.168.200.X 192.168.1.10

---

## 📌 Objectives
- Exploit SQL injection vulnerability in DVWA from Kali Linux.
- Detect the attack using Snort on the Ubuntu machine.
- Write and test a custom Snort rule to generate alerts.
- Document the process end-to-end.

---

## 🧰 Tools Used
- **Kali Linux**: Offensive VM (sqlmap)
- **Ubuntu Server**: Victim + Snort monitoring
- **DVWA**: Damn Vulnerable Web App hosted on Apache + MySQL
- **Snort 2.x**: Intrusion Detection System
- **pfSense**: Firewall & Routing
- **Wireshark**: (optional) For packet inspection

---

## 🔧 Snort Configuration

### 1. Install Snort
```bash
sudo apt update
sudo apt install snort
During installation, provide the monitored subnet: 192.168.1.0/24
```
##3 2. Create custom rule
Edit or create /etc/snort/rules/custom.rules:
```
alert tcp any any -> any 80 (msg:"SQLMAP SQL Injection Attempt"; content:"sqlmap"; sid:1000001; rev:1;)
```
### 3. Include it in Snort config
Edit /etc/snort/snort.conf:
```
include $RULE_PATH/custom.rules
```
### 4. Run Snort
```
sudo snort -i enp0s9 -c /etc/snort/snort.conf -A console
```
🎯 Attack Simulation from Kali
```
sqlmap -u "http://192.168.1.10/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=xxxxx; security=low" --batch --dbs
Ensure DVWA security is set to "low" and login session is active.
```
🔍 Snort Output
Successful detection logs a message like:
```
[**] [1:1000001:1] SQLMAP SQL Injection Attempt [**]
[Classification: Attempted Information Leak] [Priority: 2]
```
📸 Screenshots
Snort terminal detection output

sqlmap attack terminal logs

Custom rule file

DVWA vulnerable page

✅ Outcome
Demonstrated end-to-end attack and detection

Validated Snort IDS capability for detecting real-world threats

Enhanced understanding of both Red and Blue team perspectives



