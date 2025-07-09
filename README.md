# 🔍 Network Scan Detection Lab with Zeek, Wireshark, and Docker

This project demonstrates how to detect **Nmap scan activity** using **Zeek** (formerly Bro IDS) and **Wireshark** in a simulated environment running on Docker. My MacBook acted as the **attacker**, **victim**, and **sniffer** simultaneously — all on a public network.

> ✅ Ideal for anyone learning network analysis, intrusion detection, or building a blue team lab setup.

---

## 📦 Tools & Technologies

- 🐳 Docker & Docker Compose  
- 🧠 [Zeek (Bro IDS)](https://zeek.org)  
- 🦈 [Wireshark](https://www.wireshark.org)  
- ⚔️ [Nmap](https://nmap.org)  
- 💻 macOS (can be adapted to Linux/Windows)

---

## 🧠 Project Goal

> Simulate a port scan using `nmap` on a public network and detect it using Zeek’s behavioral analysis and Wireshark's packet-level inspection.

---

## 🛠️ Lab Architecture

+-----------------------------+

Host: MacBook (me)
- Nmap Attacker
- Docker Containers:
• Zeek IDS
• Target Web Service
- Wireshark (for pcap)
+-----------------------------+

yaml
Copy
Edit

---

## ⚙️ Setup Instructions

### 1. Clone the Project

```bash
git clone https://github.com/your-username/zeek-nmap-detection-lab.git
cd zeek-nmap-detection-lab
```
### 2. Start the Environment

```bash
docker-compose up -d
```
This spins up:
- A zeek container (in sniffing mode)

- A simple nginx container as the scan target


### 3. Start Packet Capture (Optional)
```bash

docker exec -it zeek tcpdump -i eth0 -w scan.pcap
```
### 4. Run the Nmap Scan from Host
```bash
nmap -sS <target-ip> -p 80,443
Use docker inspect <container_name> to get the container IP of the target.
```

### 5. Stop the Capture
```bash

docker exec -it zeek pkill tcpdump
```
## 📂 Analyze with Zeek
Zeek logs are saved inside the container. To view them:

```bash

docker exec -it zeek cat /opt/zeek/logs/current/conn.log
docker exec -it zeek cat /opt/zeek/logs/current/notice.log
```

### 🧠 Logs to Check:
- conn.log: Connection attempts and TCP handshakes

- notice.log: Detected anomalies like port scans or suspicious patterns

### 🦈 Analyze with Wireshark
- Copy .pcap file from container to host:

```bash
docker cp zeek:/scan.pcap .
```
- Open in Wireshark:

```bash

open scan.pcap
```
- Filter SYN scans:

```bash
tcp.flags.syn==1 && tcp.flags.ack==0
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

