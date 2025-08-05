---

### 🔍 Day 4: Log Analysis Basics – Network-Based Attack Detection Using UFW

🎯 **Objective:**  
Simulate a network-based port scan attack and detect it using `ufw.log` logs on a Linux system.  
You will launch an HTTP scan probe from a Kali Linux attacker machine and detect it on the Ubuntu victim machine.

---

## 🧰 Lab Setup

**System Requirements:**
1. **Attacking Machine:** Kali Linux  
2. **Target Machine:** Ubuntu Linux  

**Tools Needed:**
- **Nmap** (on Kali / attacker machine)  
- **UFW** (on Ubuntu / target machine)  

**Log Files:**
- `/var/log/ufw.log` → captures firewall events and blocked connections

---

## 🌐 What is Nmap?
- **Nmap (Network Mapper)** is an open-source network scanning tool.  
- Used to discover hosts and services on a network.  
- Identifies open ports, running services, and even operating systems.  
- Commonly used for **network inventory**, **vulnerability scanning**, and **security assessments**.

---

### 🔥 Nmap Popular Scan Types
- **SYN Scan (-sS):** Fast and stealthy port scan.  
- **TCP Connect Scan (-sT):** Full TCP connection, less stealthy.  
- **UDP Scan (-sU):** Scans UDP ports for services.  
- **Ping Scan (-sn):** Host discovery without port scanning.

---

## 🔐 What is UFW?
- **UFW (Uncomplicated Firewall)** is a frontend for `iptables` on Linux.  
- Simplifies firewall management for non-experts.  
- Manages allow/deny rules easily.  
- Logs firewall events to `/var/log/ufw.log`.  
- Main rules file: `/etc/ufw/before.rules`.  

**Useful Commands:**
- Check firewall status: `sudo ufw status`  
- Check rules with numbers: `sudo ufw status numbered`  
- Delete rule: `sudo ufw delete allow`  

---

## 🧾 UFW Rule Syntax Examples
- Allow a port: `sudo ufw allow 22`  
- Deny a port: `sudo ufw deny 80`  
- Allow by service: `sudo ufw allow ssh`  
- Allow by IP: `sudo ufw allow from 192.168.1.10`  
- Allow specific port from IP: `sudo ufw allow from 192.168.1.10 to any port 22`  

---

# 🧪 Lab Task: Detecting a Network Scan

---

## Step 1: Attack Simulation
Run an Nmap scan from the attacker (Kali) machine targeting port 80 of the Ubuntu system:

nmap -p80 <Target-IP>
Step 2: Detection & Analysis
1. Install UFW on the target (Ubuntu) machine:

<img width="975" height="454" alt="image" src="https://github.com/user-attachments/assets/07fe8ebe-8385-4ddd-ab43-f79d00824fd5" />

sudo apt install ufw
sudo ufw enable
sudo ufw logging on
sudo ufw logging high

<img width="814" height="202" alt="image" src="https://github.com/user-attachments/assets/6953104f-56fd-42ef-907c-182a8aeef2cc" />


2. Block HTTP traffic from attacker IP:

sudo ufw deny from <Attacker-IP> to any port 80 proto tcp

3. Reload firewall rules:

sudo ufw reload

4. Detect scanning traffic in logs:

sudo tail -f /var/log/ufw.log | grep <Attacker-IP>

📊 Example Output
A successful detection log might look like:

ini
Copy
Edit
[UFW BLOCK] IN=eth0 OUT= MAC=00:0c:29:9f:xx:xx:00:0c:29:xx:xx:xx:08:00 SRC=<Attacker-IP> DST=<Target-IP> LEN=60 TOS=0x00 PREC=0x00 TTL=64 ID=12345 DF PROTO=TCP SPT=45678 DPT=80 WINDOW=29200 RES=0x00 SYN URGP=0

---

✅ Conclusion
UFW logs, combined with firewall rules, provide early detection of network reconnaissance.

Port scanning is often the first stage of an attack.

Detecting & blocking IPs performing scans is a critical proactive defense measure.

---

📸 Submission Requirement
🎯 Submit a screenshot of a blocked scan event from /var/log/ufw.log showing the attacker’s IP.
Example screenshot placeholder:
<img width="975" height="582" alt="image" src="https://github.com/user-attachments/assets/bf35d893-0b8b-41ee-bb4b-3e6ced836a91" />

