Day#12: Incident Response Basics – Linux Suspicious Bash Script Execution
🎯 Objective:
The objective of this lab is to help students understand the core steps of incident response by investigating a suspicious bash script execution on a Linux system. Students will learn how to detect, analyze, and respond to a basic script-based intrusion.

📘 What is Incident Response?
Incident Response (IR) is the process of detecting and managing a cybersecurity incident to minimize impact and restore normal operations. This lab introduces a basic Linux scenario where an attacker’s script is executed through user error or misconfiguration.

🔁 Incident Response Process (NIST SP 800-61 Rev. 2)
Phase	Description
1. Preparation	Ensure logging is enabled, tools are installed, and the system is ready.
2. Detection and Analysis	Identify suspicious activity using logs, running processes, and file checks.
3. Containment, Eradication, and Recovery	Isolate the threat, remove the script, and secure the system.
4. Post-Incident Activity	Document the incident and implement prevention steps.
⚠️ Scenario: Suspicious Script Found in /tmp
Your monitoring system flagged a suspicious file in /tmp. Upon inspection, it's a bash script (payload.sh) that connects to an unknown IP. You are tasked with investigating this incident.

🛠️ Lab Setup
System Requirements:
Ubuntu 20.04/22.04 or Kali Linux
Terminal access with sudo privileges
Simulate teh attack:
Create a test directory:
mkdir ~/script-lab && cd ~/script-lab
Create a Bash script:
nano fakebackup.sh
Paste the following content:
#!/bin/bash
echo "[*] Simulating backup operation..."
sleep 60
Make the script executable:
chmod +x fakebackup.sh
Simulate the Attack / Execution Run the script in the background:
./fakebackup.sh &
🧪 Step-by-Step Investigation
1. Preparation
Install curl, lsof, ps, and grep (usually pre-installed).
2. Detection and Analysis
Check running processes:
ps aux | grep fakebackup.sh
Search for suspicious files:
find /tmp -name "*.sh"
3. Containment, Eradication, and Recovery
Kill any related processes:
pkill curl
Remove the malicious script:
rm -f /tmp/payload.sh
Clear the crontab if persistence was found:
crontab -e
Restart services if needed and log out inactive sessions.
4. Post-Incident Activity
Document:

What triggered the alert?

What was the script doing?

Which user executed it?

Recommendations:

Enable file integrity monitoring (e.g., AIDE).

Restrict /tmp execution using mount options (noexec).

Educate users about unknown script execution.

Lab Checklist
✅ Simulate Script Create and execute a suspicious bash script
✅ Investigate Logs Use commands to analyze the event
✅ Kill and Delete Contain and remove the malicious file
✅ Document Findings Note IPs, users, and recommendations

📸 Submission
Submit screenshots of:

The malicious script content
Process list showing the script or curl
Script deleted from /tmp
