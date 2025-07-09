# 🔍 Network Scan Detection Lab with Zeek, Wireshark, and Docker

This project demonstrates how to detect **Nmap scan activity** using **Zeek** (formerly Bro IDS) and **Wireshark** in a simulated environment running on Docker. My MacBook acted as the **attacker**, **victim**, and **sniffer** simultaneously — all on a public network.

> ✅ Ideal for anyone learning network analysis, intrusion detection, or building a blue team lab setup.

---

## 📦 Tools & Technologies

- 🐳 Docker 
- 🧠 [Zeek (Bro IDS)](https://zeek.org)  
- 🦈 [Wireshark](https://www.wireshark.org)  
- ⚔️ [Nmap](https://nmap.org)  
- 💻 macOS (can be adapted to Linux/Windows)

---

## 🧠 Project Goal

> Simulate a port scan using `nmap` on a public network and detect it using Zeek’s behavioral analysis and Wireshark's packet-level inspection.

---

## 🛠️ Lab Architecture

+------------------------------------------------+
|                   Host: MacBook                |
|------------------------------------------------|
|  Attacker:     Nmap CLI (on localhost)         |
|  Victim:       nginx container (Docker)        |
|  Sniffer:      Zeek running on host interface  |
|  Packet view:  Wireshark analyzing .pcap       |
+------------------------------------------------+

yaml
Copy
Edit

---

## ⚙️ Setup Instructions

### 1. Run the Victim Web Server (NGINX)

```bash
docker run -d --name victim-nginx -p 8080:80 nginx
```
This container acts as the scan target for Nmap.
<img src="assets/Screenshot 2025-04-30 at 09.01.28.png" alt="Analytics Rules" width="800"/>
### 2. Start Zeek on Your Network Interface

```bash
ifconfig
```

Then start Zeek:
```bash
sudo zeek -i en0 local
```
Zeek will begin monitoring live traffic and output logs to the current directory (conn.log, notice.log, etc.).

### 3. Run the Nmap Scan (From Host)
```bash

sudo nmap -sS -p 8080 127.0.0.1

```
### 4. Analyze Zeek Logs

```bash
cat conn.log | grep 8080
cat notice.log

```

### 5. Capture Packets with Wireshark 
Open Wireshark and look for these interfaces:

- en0

Start capturing before scanning.

Optional Wireshark filter:
```bash

ip.addr == 172.17.0.2

```
## 📂 Run Nmap Scan (From Host)
In a new terminal window:
```bash
nmap -sS 172.17.0.2

```


📸 Screenshots
<details> <summary>🔍 Click to expand</summary>
✅ Wireshark showing SYN scan traffic

✅ Zeek conn.log entries with Nmap behavior

✅ Zeek notice.log detecting scan pattern

✅ Terminal logs showing container interaction

</details>

###  📚 Key Learning Outcomes
- How TCP SYN scans look at packet level

- How Zeek detects scanning behavior behaviorally

- How to build a mini NIDS lab with Docker

- How to use Wireshark filters to inspect low-level traffic

### 🧱 Project Use Cases
- Blue team detection simulations

- OSCP/PNPT lab enrichment

- SOC alert tuning and training

- Cybersecurity homelab setups

🔐 Author
Onuora Adiorah
LinkedIn | GitHub

