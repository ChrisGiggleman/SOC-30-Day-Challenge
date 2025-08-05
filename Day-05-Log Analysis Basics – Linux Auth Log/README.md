# 🔐 Day 5: Log Analysis Basics – Linux Auth Log

> 🎯 **Objective:**  
> Simulate an SSH brute force attack and detect it by analyzing Linux authentication logs (`/var/log/auth.log`).  
> Learn to identify multiple failed login attempts and analyze patterns revealing brute force activity.

---

## 🧰 Lab Requirements

**Systems:**
1. **Attacking Machine:** Kali Linux (or any Linux system with Hydra installed)
2. **Target Machine:** Ubuntu Linux Server

**Tools Needed:**
- Hydra (on attacker machine)
- OpenSSH Server (on target machine)
- Rsyslog (default logging service)

**Log Files:**
- Ubuntu/Debian: `/var/log/auth.log`
- CentOS/RHEL: `/var/log/secure`

---

## 🧠 What is an SSH Brute Force Attack?

A brute force attack attempts to guess a user’s SSH password by trying many combinations quickly using automated tools like Hydra.

### Why It’s Dangerous:
- **Full Shell Access:** A successful attack gives an attacker complete system access.  
- **Post-Exploitation:** Attackers can pivot, install malware, or exfiltrate data.  
- **Stealth:** Without monitoring, attacks can go unnoticed.

---

## 🛡️ Attack Patterns Detectable via Auth Logs
- Multiple failed password attempts from one IP  
- Repeated login attempts to root/admin accounts  
- A successful login after multiple failures (brute force success)  
- Logins from unknown or foreign IP addresses  

---

## ⚡ What is Hydra?
- **Hydra** is a fast, open-source password-cracking tool for brute force attacks.  
- Supports **50+ protocols** including SSH, FTP, HTTP, SMB, and more.  
- Commonly used for **penetration testing** and **weak password audits**.

**Syntax:**

hydra -l <username> -p <password> <target_ip> <protocol>
hydra -L users.txt -P passlist.txt <target_ip> ssh
Options:

-l/-p → single username/password

-L/-P → files with usernames/passwords

-vV → verbose output

-t 4 → set number of threads

<img width="975" height="354" alt="image" src="https://github.com/user-attachments/assets/ec12cb33-954c-4a70-b0a7-6bff1e65229a" />

---

###🧪 Lab Task: Detecting SSH Brute Force via Auth Logs
Step 1: Attack Simulation – Brute Force SSH using Hydra
Run the brute force attack from the attacking (Kali) machine:

hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://<Target-IP>
Ensure SSH is enabled on the target machine:

sudo systemctl status ssh
If SSH is not installed, install and enable it:

sudo apt update
sudo apt install openssh-server -y
sudo systemctl enable ssh
sudo systemctl start ssh
Allow SSH through UFW if enabled:

sudo ufw allow ssh
sudo ufw reload

Step 2: Detection and Analysis

1. Check for failed login attempts:

sudo grep "Failed password" /var/log/auth.log

2. Find usernames that were attempted:

sudo grep "Failed password" /var/log/auth.log | awk '{print $(NF-5)}' | sort | uniq -c | sort -nr

<img width="975" height="149" alt="image" src="https://github.com/user-attachments/assets/49748c58-4544-412f-b478-d677e50125bc" />

3. Watch live log activity:

sudo tail -f /var/log/auth.log

<img width="975" height="537" alt="image" src="https://github.com/user-attachments/assets/5f96391c-2aff-476b-9e84-342e031ba0c4" />

🔍 What to Look For:
20+ failed attempts from the same IP in under 5 minutes

Attempts on sensitive accounts (root, admin)

A sudden success after multiple failures → likely brute force success

✅ Conclusion
Authentication logs are critical for detecting brute force attacks.

Multiple failures from a single IP is a strong indicator of malicious activity.

Combine log analysis with tools like Fail2ban to block repeat offenders automatically.

📸 Submission Requirement
🎯 Submit a screenshot showing failed SSH login attempts detected in /var/log/auth.log.
Example placeholder screenshot:
