---

# 🛰️ Day 7: Wireshark Basics – ICMP Protocol Analysis

🎯 **Objective:**  
Understand and analyze ICMP (Internet Control Message Protocol) packets using Wireshark.  
Identify echo requests/replies, interpret ICMP packet fields, and apply filters for investigation.

---

## 🛠️ Lab Setup

### System Requirements
- Operating System: Windows 10/11, Linux, or macOS  
- Software: Wireshark (latest version)  

---

## 🧪 Lab Steps – Capture ICMP Echo Request & Echo Reply

---

### Step 1: Open Wireshark
1. Launch **Wireshark** on your system.  
2. Select the **active network interface** (Ethernet or Wi-Fi).  

---

### Step 2: Apply a Display Filter
1. In the **Display Filter bar**, type:  
icmp

2. Press **Enter** to apply the filter.  
*This will show only ICMP packets.*

---

### Step 3: Generate ICMP Traffic
1. Open a terminal (or Command Prompt) on your machine.  
2. Run the ping command:  
- **Windows:**
  ```cmd
  ping 8.8.8.8
  ```
- **Linux/Mac:**
  ```bash
  ping -c 4 8.8.8.8
  ```
3. This sends **ICMP Echo Request (Type 8)** packets and receives **ICMP Echo Reply (Type 0)** packets.

---

### Step 4: Verify in Wireshark
- You should see packets labeled as:
- **Echo (ping) request**  
- **Echo (ping) reply**

---

### Step 5: Screenshot for Submission
- Capture a screenshot of the Wireshark window showing:
- At least one **Echo Request**
- At least one **Echo Reply**
- Make sure the **“icmp”** filter is visible in the filter bar.

---

## 📤 Submission
Submit **one screenshot** showing:
- **ICMP Echo Request packet**  
- **ICMP Echo Reply packet**  
- **The active `icmp` filter**
<img width="975" height="733" alt="image" src="https://github.com/user-attachments/assets/30b7eccc-ed25-4496-aa78-4e675cd7b5ea" />
