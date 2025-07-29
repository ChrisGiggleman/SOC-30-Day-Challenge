
Day 1: Simulating and Detecting Windows PowerShell Events
Lab Environment
Windows 11

Kali Linux

Lab Setup
Installed Group Policy Editor on Windows 11 using DISM commands.

Enabled Module Logging, Script Block Logging, and Script Execution Policies.

Simulation
Ran elevated PowerShell command to simulate suspicious activity:

pgsql
Copy
Edit
Get-LocalUser | Select-Object Name, Enabled
Detection
Verified PowerShell events in Event Viewer under Applications and Services Logs → Microsoft → Windows → PowerShell → Operational.

Filtered for Event ID 4104 to observe the suspicious command execution.

