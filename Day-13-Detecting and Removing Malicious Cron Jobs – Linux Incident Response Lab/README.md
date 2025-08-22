Day #13: Detecting and Removing Malicious Cron Jobs – Linux Incident Response Lab
🎯 Objective

The objective of this lab is to investigate and respond to a malicious cron job used by an attacker to maintain persistence on a Linux system. Students will:

Simulate the attack

Detect the malicious scheduled task

Analyze the script

Remove the threat

Apply the incident response lifecycle (NIST SP 800-61).

📘 What is a Cron Job?

A cron job is a scheduled task that runs automatically at defined intervals on Unix/Linux systems.

🔑 Attackers abuse cron to:

Re-execute payloads

Reconnect to C2 servers

Maintain persistence after reboot

🧾 Format of a crontab entry:

*  *  *  *  *  command-to-run
│  │  │  │  │
│  │  │  │  └─ Day of the week (0-7, Sun = 0 or 7)
│  │  │  └──── Month (1 - 12)
│  │  └─────── Day of month (1 - 31)
│  └────────── Hour (0 - 23)
└───────────── Minute (0 - 59)

🔁 Incident Response Process (NIST SP 800-61 Rev. 2)
Phase	Description
1. Preparation	Ensure logging is active and cron activity is monitored.
2. Detection & Analysis	Identify unauthorized cron entries and review their behavior.
3. Containment, Eradication & Recovery	Stop malicious cron activity, delete the script, and restore configuration.
4. Post-Incident Activity	Document the incident and set up monitoring for future cron modifications.
⚠️ Scenario

Your monitoring system flagged unusual log activity in /tmp. Upon investigation, you find a cron job running every minute, executing /tmp/malicious.sh.

🛠️ Lab Setup

System Requirements:

Ubuntu 20.04/22.04 or Kali Linux

Terminal with sudo privileges

Simulate the Attack:

# Step 1: Create a fake malicious script
echo -e '#!/bin/bash\necho "Ping from attacker server" >> /tmp/.cron.log' > /tmp/malicious.sh
chmod +x /tmp/malicious.sh

# Step 2: Add a cron job (root persistence simulation)
echo "* * * * * /tmp/malicious.sh" | sudo tee -a /var/spool/cron/root


✅ At this point, the malicious script runs every minute and appends to /tmp/.cron.log.

🧪 Step-by-Step Investigation
1) Preparation
# Ensure cron is running
sudo systemctl status cron


✅ Example output:

Active: active (running) since ...

# Verify logging location
grep CRON /var/log/syslog | tail -5

2) Detection & Analysis

A. Check for suspicious cron entries

crontab -l


Example output:

* * * * * /tmp/malicious.sh


B. Search cron directories for jobs referencing /tmp/

grep -r "/tmp/" /etc/cron* /var/spool/cron/crontabs


Example output:

/var/spool/cron/root:* * * * * /tmp/malicious.sh


C. Confirm execution from logs

cat /tmp/.cron.log | tail -5


Example output:

Ping from attacker server
Ping from attacker server


D. Analyze the script content

cat /tmp/malicious.sh


Example output:

#!/bin/bash
echo "Ping from attacker server" >> /tmp/.cron.log

3) Containment, Eradication & Recovery

A. Remove the malicious cron job

crontab -l | grep -v "malicious.sh" | crontab -


B. Delete the script + log

sudo rm -f /tmp/malicious.sh /tmp/.cron.log


C. Restart cron service

sudo systemctl restart cron


✅ Example verification:

crontab -l


Output:

(no entries)

4) Post-Incident Activity

Document findings:

Trigger: Suspicious /tmp activity in logs

Behavior: Cron job executed script every minute, appending messages to /tmp/.cron.log

Impact: Persistence mechanism, potential for malicious payload execution

Actions taken: Removed cron entry, deleted script, restarted cron

Recommendations:

Restrict cron access to authorized users

Monitor /etc/cron* and /var/spool/cron/* for changes

Use auditd or inotify to alert on new cron jobs

Train users/admins to recognize persistence techniques

✅ Lab Checklist

 Simulate cron job (script + scheduled entry)

 Detect malicious cron job with crontab -l

 Analyze script & logs

 Remove cron job & artifacts

 Document incident & recommend hardening

📸 Submission Requirements

Provide screenshots of:

crontab -l showing malicious entry

Contents of /tmp/malicious.sh

Logs from /tmp/.cron.log

Clean cron job list (after removal)
