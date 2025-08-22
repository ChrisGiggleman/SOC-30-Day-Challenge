# Day 12: Incident Response Basics – Linux Suspicious Bash Script Execution  

---

## 🎯 Objective
The objective of this lab is to help students understand the core steps of incident response by investigating a suspicious bash script execution on a Linux system. Students will learn how to detect, analyze, and respond to a basic script-based intrusion.  

---

## 📘 What is Incident Response?  
Incident Response (IR) is the process of detecting and managing a cybersecurity incident to minimize impact and restore normal operations.  
This lab introduces a basic Linux scenario where an attacker’s script is executed through user error or misconfiguration.  

---

## 🔁 Incident Response Process (NIST SP 800-61 Rev. 2)  

| **Phase** | **Description** |
|-----------|-----------------|
| Preparation | Ensure logging is enabled, tools are installed, and the system is ready. |
| Detection & Analysis | Identify suspicious activity using logs, running processes, and file checks. |
| Containment, Eradication & Recovery | Isolate the threat, remove the script, and secure the system. |
| Post-Incident Activity | Document the incident and implement prevention steps. |

---

## ⚠️ Scenario: Suspicious Script Found in `/tmp`  
Your monitoring system flagged a suspicious file in `/tmp`.  
Upon inspection, it's a bash script (`payload.sh`) that connects to an unknown IP.  
You are tasked with investigating this incident.  

---

## 🛠️ Lab Setup  

**System Requirements**  
- Ubuntu 20.04/22.04 or Kali Linux  
- Terminal access with `sudo` privileges  

**Simulate the attack:**  

```bash
# 1. Create a test directory
mkdir ~/script-lab && cd ~/script-lab

# 2. Create a fake backup script
nano fakebackup.sh
Paste inside:

bash
Copy
Edit
#!/bin/bash
echo "[*] Simulating backup operation..."
sleep 60
bash
Copy
Edit
# 3. Make the script executable
chmod +x fakebackup.sh

# 4. Run the script in the background
./fakebackup.sh &
🧪 Step-by-Step Investigation
1) Preparation
Install core tools:

bash
Copy
Edit
# Debian/Ubuntu/Kali
sudo apt update
sudo apt install -y curl lsof procps grep coreutils findutils psmisc tree
Optional aliases:

bash
Copy
Edit
alias psa='ps auxww'
alias sst='ss -tupna'
2) Detection & Analysis
A. Find the process

bash
Copy
Edit
ps auxww | grep -i fakebackup.sh | grep -v grep
pgrep -fa 'fakebackup\.sh'
pstree -a -p | grep -i 'fakebackup.sh' -C2
B. Check network activity

bash
Copy
Edit
ss -tupna
sudo lsof -i -n -P | grep -iE 'ssh|curl|wget|nc|python|bash'
C. Hunt for suspicious files

bash
Copy
Edit
sudo find /tmp /var/tmp -type f -mmin -1440 -name '*.sh' -ls
sudo find /tmp /var/tmp -type f -perm -111 -ls
D. Inspect the script safely

bash
Copy
Edit
TARGET=~/script-lab/fakebackup.sh
file "$TARGET"
stat "$TARGET"
sha256sum "$TARGET"
head -n 20 "$TARGET"
tail -n 20 "$TARGET"
strings -n 6 "$TARGET" | head -50
E. Who executed it?

bash
Copy
Edit
sudo grep -E 'session opened|FAILED|Accepted' /var/log/auth.log
last -a | head
F. Check persistence

bash
Copy
Edit
crontab -l
systemctl list-units --type=service --state=running | grep -iE 'tmp|backup|sh'
3) Containment, Eradication & Recovery
A. Capture evidence

bash
Copy
Edit
EVDIR=~/IR-$(date +%F-%H%M%S); mkdir -p "$EVDIR"
cp -a ~/script-lab/fakebackup.sh "$EVDIR"/
sha256sum "$EVDIR"/* > "$EVDIR"/hashes.sha256
ps auxww > "$EVDIR"/ps.txt
B. Kill processes

bash
Copy
Edit
sudo pkill -f 'fakebackup\.sh'
C. Remove artifacts

bash
Copy
Edit
sudo chmod -x ~/script-lab/fakebackup.sh
sudo rm -f /tmp/payload.sh
D. Remove persistence

bash
Copy
Edit
crontab -e    # remove bad lines
sudo systemctl disable --now suspicious.service
4) Post-Incident Activity
A. Document Findings

Trigger: What alerted you?

Behavior: What did the script do?

Who executed it?

Scope: Was lateral movement attempted?

Actions taken.

B. Export logs/evidence

bash
Copy
Edit
sudo cp -a /var/log/auth.log* "$EVDIR"/
tar czf "$EVDIR".tar.gz "$EVDIR"
C. Recommendations

Enable File Integrity Monitoring (AIDE).

Mount /tmp with noexec,nosuid,nodev.

Restrict curl/wget.

Educate users on running unverified scripts.

✅ Lab Checklist
 Simulate a suspicious bash script

 Investigate processes, files, and logs

 Kill malicious process and remove script

 Document findings & export evidence

📸 Submission
Submit screenshots of:

The malicious script content

Process list showing the script

Script deleted from /tmp
