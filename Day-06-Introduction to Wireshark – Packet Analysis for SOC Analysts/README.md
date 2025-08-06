---

# 📡 Day 6: Introduction to Wireshark – Packet Analysis for SOC Analysts

> 🎯 **Objective:**  
> This lab introduces Wireshark, a powerful packet analysis tool used by SOC analysts to investigate network traffic.  
> You will learn to create a custom Wireshark profile, apply display and capture filters, and identify ICMP packets.

---

## 🛠️ Lab Setup

### System Requirements
- Operating System: Windows, Linux, or MacOS  
- Network Adapter: Required for packet capture  
- Administrative rights for capturing packets  

### Software Required
- **Wireshark** (latest version recommended)  
- **Sample PCAP file** (optional for offline practice)  

---

## 📺 Tutorial Video
If you are new to Wireshark, check out this video tutorial:  
**[Wireshark for Beginners – Hands-On Walkthrough](#)**  
*(Duration: 15 minutes)*

---

# 🧪 Lab Task – Wireshark Profile & ICMP Filters

## **Objectives**
- Create a custom Wireshark profile named **“SOC Analyst”**  
- Use a **Display Filter** to view only ICMP traffic  
- Use a **Capture Filter** to only capture ICMP traffic  

---

## Step 1: Create a New Profile – "SOC Analyst"
1. Open **Wireshark**.  
2. In the **Menu bar**, go to:  
   `Edit → Configuration Profiles`  
3. Click **“New”** and name it:  
SOC Analyst

4. Click **OK** to create the profile.  
5. Switch to the new profile from the bottom-right corner or via:  
`Edit → Configuration Profiles → SOC Analyst → OK`  

### 📸 Snapshot to Share
- Screenshot of Wireshark showing the **“SOC Analyst” profile active** (bottom right should say "SOC Analyst").

<img width="938" height="694" alt="image" src="https://github.com/user-attachments/assets/e81473c8-d901-4174-941f-1d46532ffbf9" />

---

## Step 2: Create a Display Filter for ICMP Traffic
1. In the **Display Filter** bar at the top, type:  
icmp


2. Press **Enter** (or click the green checkmark).  
3. Generate ICMP traffic (from a terminal):  

ping 8.8.8.8
Wireshark should now display only ICMP packets (echo request & echo reply).

📸 Snapshot to Share
Screenshot showing only ICMP packets visible in Wireshark with the display filter applied.

---

### Step 3: Create a Capture Filter for ICMP Traffic
On the main Wireshark capture screen, select your active network interface.

In the Capture Filter box (next to the shark fin icon), type:

icmp
Click Start to begin capturing.

Generate ICMP traffic again:

ping 8.8.8.8
Wireshark will now capture only ICMP packets.

📸 Snapshot to Share
Screenshot showing only ICMP packets captured with the capture filter applied.

<img width="975" height="727" alt="image" src="https://github.com/user-attachments/assets/198d5b90-bb16-4bad-b2bd-1faaeba13e3f" />

---

📤 Submission
Submit three screenshots:

“SOC Analyst” profile active

ICMP Display filter applied

ICMP Capture filter applied

