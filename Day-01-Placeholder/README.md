# Day 1: Simulating and Detecting Windows PowerShell Events

## Lab Environment
- **Windows 11** (Target system)
- **Kali Linux** (Attacker/monitoring system)

---

## Objective
The goal of this lab is to:
1. Configure PowerShell logging for enhanced visibility.
2. Simulate a potentially suspicious PowerShell command.
3. Detect and verify the event logs in Windows Event Viewer.

---

## Example logs
🐧Linux auth.log Example

Apr  7 10:42:15 ubuntu sshd[12345]: Failed password for invalid user admin from 192.168.1.100 port 54321 ssh2
Explanation:

Apr 7 10:42:15 — Timestamp
ubuntu — Hostname
sshd[12345] — Service name and process ID
Failed password for invalid user admin — Failed login attempt
from 192.168.1.100 — Source IP address
port 54321 — Source port
ssh2 — Protocol version

---

## Log Sources on Linux:

System Logs: Stored in /var/log/, including files like syslog (system messages), auth.log (authentication attempts), and kern.log (kernel-related logs).
Application Logs: Application-specific logs like Apache (/var/log/apache2/access.log) or MySQL (/var/log/mysql/error.log).

## Log Sources on Windows:

Event Viewer: Provides access to logs such as:
System Logs: Logs related to operating system events.
Security Logs: Logs related to login attempts, permission changes, etc.
Application Logs: Logs for system applications.
PowerShell Logs: Logs for PowerShell commands, including suspicious execution.

---

## Step 1: Environment Setup

### Install Group Policy Editor (If Not Pre-installed)
1. Open **Command Prompt** as Administrator.
2. Run the following commands to install the Group Policy Editor:
   ```cmd
   FOR %F IN ("%SystemRoot%\servicing\Packages\Microsoft-Windows-GroupPolicy-ClientTools-Package~*.mum") DO (
       DISM /Online /NoRestart /Add-Package:"%F"
   )

   FOR %F IN ("%SystemRoot%\servicing\Packages\Microsoft-Windows-GroupPolicy-ClientExtensions-Package~*.mum") DO (
       DISM /Online /NoRestart /Add-Package:"%F"
   )
Restart the computer/VM.

Confirm installation by opening gpedit.msc:
<img width="975" height="696" alt="image" src="https://github.com/user-attachments/assets/f44a903c-0c10-442b-a8c1-cafd9b80f447" />


Create a shortcut:

Right-click desktop → New → Shortcut

Type: %systemroot%\system32\gpedit.msc

<img width="272" height="469" alt="image" src="https://github.com/user-attachments/assets/83eaa79b-e335-42ff-b489-274dce7f2580" />

## Step 2: Enable PowerShell Logging
Force Group Policy Update
Open PowerShell as Administrator and run:

gpupdate /force
Verify Logging Settings
Check if logging is enabled:

Get-ItemProperty -Path HKLM:\Software\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging
Get-ItemProperty -Path HKLM:\Software\Policies\Microsoft\Windows\PowerShell\ModuleLogging
Get-ExecutionPolicy -List
If keys do not exist, create and enable logging:

# Script Block Logging
New-Item -Path "HKLM:\Software\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging" -Force
Set-ItemProperty -Path "HKLM:\Software\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging" `
    -Name "EnableScriptBlockLogging" -Value 1

# Module Logging
New-Item -Path "HKLM:\Software\Policies\Microsoft\Windows\PowerShell\ModuleLogging" -Force
Set-ItemProperty -Path "HKLM:\Software\Policies\Microsoft\Windows\PowerShell\ModuleLogging" `
    -Name "EnableModuleLogging" -Value 1

# Transcription Logging
New-Item -Path "HKLM:\Software\Policies\Microsoft\Windows\PowerShell\Transcription" -Force
Set-ItemProperty -Path "HKLM:\Software\Policies\Microsoft\Windows\PowerShell\Transcription" `
    -Name "EnableTranscripting" -Value 1
## Step 3: Simulate Suspicious PowerShell Activity
Run the following elevated PowerShell command:

Get-LocalUser | Select-Object Name, Enabled
Why suspicious?
Attackers may enumerate local accounts after gaining access to a host. This command could indicate reconnaissance.

## Step 4: Detect Suspicious Activity in Windows Event Viewer
Press Win + R, type:

eventvwr.msc
and press Enter.

Navigate to:

Applications and Services Logs → Microsoft → Windows → PowerShell → Operational
Filter the log for Event ID 4104 (PowerShell script execution).

Locate the log entry for the executed Get-LocalUser command.

Take a screenshot of the event details for documentation.

<img width="975" height="720" alt="image" src="https://github.com/user-attachments/assets/ecfaf029-218f-4bb1-a5d6-f5f30c172347" />


## *Notes*
Event ID Reference
4103 → Module Logging

4104 → Script Block Logging (command content)

4688 → Process Creation (if Security Logging is enabled)

## Popular Tools for Log Analysis:

ELK Stack (Elasticsearch, Logstash, Kibana): A powerful suite for log collection, storage, and visualization.
Splunk: A leading tool for searching, analyzing, and visualizing machine-generated data.
Graylog: An open-source log management solution.
Wazuh: An open-source security monitoring platform that integrates well with ELK for log analysis and threat detection.

## Outcome
Successfully enabled PowerShell logging.

Simulated a suspicious PowerShell command.

Verified detection through Event Viewer.

Screenshot captured for reference.
