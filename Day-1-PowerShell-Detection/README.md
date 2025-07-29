Day 1: Simulating and Detecting Windows PowerShell Events
markdown
Copy
Edit
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

Create a shortcut:

Right-click desktop → New → Shortcut

Target:

shell
Copy
Edit
%systemroot%\system32\gpedit.msc
Step 2: Enable PowerShell Logging
Force Group Policy Update
Open PowerShell as Administrator and run:

powershell
Copy
Edit
gpupdate /force
Verify Logging Settings
Check if logging is enabled:

powershell
Copy
Edit
Get-ItemProperty -Path HKLM:\Software\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging
Get-ItemProperty -Path HKLM:\Software\Policies\Microsoft\Windows\PowerShell\ModuleLogging
Get-ExecutionPolicy -List
If keys do not exist, create and enable logging:

powershell
Copy
Edit
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
Step 3: Simulate Suspicious PowerShell Activity
Run the following elevated PowerShell command:

powershell
Copy
Edit
Get-LocalUser | Select-Object Name, Enabled
Why suspicious?
Attackers may enumerate local accounts after gaining access to a host. This command could indicate reconnaissance.

Step 4: Detect Suspicious Activity in Windows Event Viewer
Press Win + R, type:

Copy
Edit
eventvwr.msc
and press Enter.

Navigate to:

nginx
Copy
Edit
Applications and Services Logs → Microsoft → Windows → PowerShell → Operational
Filter the log for Event ID 4104 (PowerShell script execution).

Locate the log entry for the executed Get-LocalUser command.

Take a screenshot of the event details for documentation.

## *Notes*
Event ID Reference
4103 → Module Logging

4104 → Script Block Logging (command content)

4688 → Process Creation (if Security Logging is enabled)

## Outcome
Successfully enabled PowerShell logging.

Simulated a suspicious PowerShell command.

Verified detection through Event Viewer.

Screenshot captured for reference.
