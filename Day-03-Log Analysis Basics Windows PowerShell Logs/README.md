
---

# ⚡️ Day 3: Log Analysis Basics – Windows PowerShell Logs

> 🔍 Learn to enable logging, generate PowerShell activity, and analyze critical Event IDs for threat detection.

---

## 🧰 Lab Setup

**Requirements:**
- 🖥️ System: Windows 10/11 or Windows Server 2019/2022
- 🛠️ Tools:
  - Windows Event Viewer (pre-installed)
  - PowerShell (pre-installed)
- 🔐 Administrative Privileges required

---

## 🔧 Preparation: Enabling PowerShell Logging

1. Press `Win + R`, type `gpedit.msc`, and press **Enter**.
2. Navigate to:  
   `Computer Configuration → Administrative Templates → Windows Components → Windows PowerShell`
3. Enable the following:
   - ✅ **Module Logging**
   - ✅ **Script Block Logging**
   - ✅ **Script Execution**
4. Apply the settings and close Group Policy Editor.
<img width="975" height="727" alt="image" src="https://github.com/user-attachments/assets/25c4f264-fd07-45ae-8029-ce45063cffc8" />

---

## 📝 Module Logging Configuration

When enabling Module Logging, you must specify module names.

### 🛡️ Microsoft’s Default Recommendations
Microsoft.PowerShell.*
Microsoft.WSMan.Management

### 🔒 Highly Recommended for Blue Team Monitoring
Microsoft.PowerShell.*
Microsoft.WSMan.Management
Microsoft.PowerShell.Security
Microsoft.PowerShell.Utility
Microsoft.PowerShell.Management
CimCmdlets
NetAdapter
NetTCPIP
NetSecurity
ScheduledTasks

> 💡 Tip: Use `*` to log **everything** (high visibility, more noise).
<img width="769" height="509" alt="image" src="https://github.com/user-attachments/assets/4e2c998e-59a9-42ad-acfe-d9a8c16ea4e6" />

---

## 🛠️ Script Execution Policy

You’ll be prompted to select an execution policy. Here are the options:

| Option                                       | Execution Policy | Security Level |
|---------------------------------------------|------------------|----------------|
| Allow only signed scripts                   | AllSigned        | 🔒 High        |
| Allow local scripts and remote signed ones  | RemoteSigned ✅   | 🟡 Medium      |
| Allow all scripts                           | Unrestricted     | 🔓 Low         |

### ✅ My Choice: `RemoteSigned`
- Local scripts run without issues.
- Downloaded scripts must be signed.
- Good balance for learning labs and real-world use.
<img width="975" height="897" alt="image" src="https://github.com/user-attachments/assets/f900aeb0-e477-4041-ae7c-b0aaf59fdfcf" />

---

## 🧪 Lab Task: Generate & Analyze PowerShell Logs

### Step 1: Run a PowerShell Command
Open PowerShell as Administrator and run:

```powershell
Start-Process "notepad.exe" -ArgumentList "C:\Windows\System32\drivers\etc\hosts"
What it does:

Launches Notepad.

Opens the system's hosts file.

Triggers PowerShell logging.
<img width="975" height="676" alt="image" src="https://github.com/user-attachments/assets/57bfcb40-9ca9-4e5e-9ee7-d3d4c151614f" />

---

### Step 2: Check Event Viewer Logs
Open Event Viewer

Navigate to:
Applications and Services Logs → Microsoft → Windows → PowerShell → Operational

Look for:

Event ID 4103 (detailed command execution)

Optionally check for 4104, 4101, 4698

🖼️ Example Screenshot:

🧠 Blue Team Relevance: Why This Matters
PowerShell logging helps detect:

⚠️ LOLBAS (Living Off The Land Binaries) attacks

🎯 Obfuscated payloads and suspicious scripts

🔁 Persistence mechanisms via tasks or startup scripts

🛠️ Common LOLBAS Tools
🛠️ Tool	📌 Path	🚩 Abuse Technique
powershell.exe	C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe	Execute payloads, bypass AV
certutil.exe	C:\Windows\System32\certutil.exe	Download files (certutil -urlcache -f)
mshta.exe	C:\Windows\System32\mshta.exe	Run remote malicious scripts
regsvr32.exe	C:\Windows\System32\regsvr32.exe	Load/execute DLLs silently
rundll32.exe	C:\Windows\System32\rundll32.exe	Load DLLs for stealthy code execution
wmic.exe	C:\Windows\System32\wbem\wmic.exe	System info & remote command execution
bitsadmin.exe	C:\Windows\System32\bitsadmin.exe	Background file transfers
msbuild.exe	C:\Windows\Microsoft.NET\Framework\v4.0.30319\msbuild.exe	Execute C# payloads in project files
installutil.exe	C:\Windows\Microsoft.NET\Framework\v4.0.30319\installutil.exe	Code execution during assembly registration
schtasks.exe	C:\Windows\System32\schtasks.exe	Persistence via scheduled tasks

✅ Conclusion
🔍 PowerShell logs are critical for detecting malicious actions.

🛡️ SOC analysts use Event IDs like 4103 & 4104 for post-exploitation analysis.

🚨 Threat detection begins with good visibility and logging practices.

📸 Submission Requirement
🎯 Submit a screenshot of an Event ID 4103 showing PowerShell command execution.
<img width="1027" height="780" alt="image" src="https://github.com/user-attachments/assets/d3c86359-13ff-4f9d-a47f-ddd4346fa2f1" />
