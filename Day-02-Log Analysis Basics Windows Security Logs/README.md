# Day 02 – Log Analysis Basics: Windows Security Logs

## Objective
Understand and analyze key Windows Security Log Event IDs, simulate failed login attempts, and detect them in Event Viewer.

---

## Key Event IDs
- **4624** – Successful logon  
- **4625** – Failed logon  
- **4740** – Account lockout  
- **4732** – A user was added to a security-enabled local group  
- **4672** – Special privileges assigned to a new logon (Privilege escalation)  

---

## Lab Requirements
- Windows 10/11 system  
- Administrative privileges  
- Notepad (for note-taking)

---

## Lab Steps

### Step 1: Create Test User Accounts
1. Open **Settings → Accounts → Other Users**.  
2. Create three new accounts:
   - `Test1`
   - `Test2`
   - `Test3`
     
<img width="975" height="738" alt="image" src="https://github.com/user-attachments/assets/ff64c74f-2fab-4dbd-8526-48becfd31193" />

---

### Step 2: Simulate Failed Login Attempts
Open **PowerShell (Run as Administrator)** and execute:

net use \\127.0.0.1\IPC$ /user:Test1 IncorrectPassword

net use \\127.0.0.1\IPC$ /user:Test2 BillyBob

net use \\127.0.0.1\IPC$ /user:Test3 Password

> **Explanation:**  
> - **net use** – Connects to network shares or printers.  
> - **\\127.0.0.1\IPC$** – Hidden administrative share on localhost.  
> - **/user:TestX** – Specifies a username for authentication.  
> - **IncorrectPassword** – Purposely incorrect credentials to trigger failed login events.

<img width="975" height="553" alt="image" src="https://github.com/user-attachments/assets/472ca459-7a2d-4269-870b-a64236b4271b" />


---

### Step 3: Analyze Security Logs
1. Open **Event Viewer** (`eventvwr.msc`).  
2. Navigate to **Windows Logs → Security**.  
3. Use **Filter Current Log…** and filter for:
   - **Event ID: 4625** (Failed logon attempts)  
4. Review log details:
   - Identify username, source IP, and failure reason. 
---

## Conclusion
Windows Security Logs provide critical insight into login activity, privilege escalation, and account changes.

SOC Analysts leverage these logs to detect unauthorized access attempts and respond to threats quickly.

Threat Detection Tip: Monitor for:

Multiple failed logins (4625)

Account lockouts (4740)

Privilege escalation attempts (4672)

## Submission Requirements
Screenshot of Event ID 4624 (Successful login).
<img width="1023" height="762" alt="image" src="https://github.com/user-attachments/assets/cbfa91cc-9b8a-4340-8424-273023d8a82a" />


Screenshot of Event ID 4625 (Failed login attempt).
<img width="975" height="737" alt="image" src="https://github.com/user-attachments/assets/9e669c19-1c31-4d7b-a8a8-f30ad2ea83d0" />
