---

# 🔹 Day 8: Wireshark Basics – TCP Protocol Analysis

🎯 **Objective:**  
Introduce students to analyzing TCP (Transmission Control Protocol) traffic using Wireshark.  
Learn how TCP establishes connections (3-way handshake) and how to interpret TCP fields and flags.

---

## 🛠️ Lab Setup

### System Requirements
- Operating System: Windows 10/11, Linux, or macOS  
- Software: Wireshark (latest version)  

---

## 📘 TCP Packet Structure and Fields

**TCP (Transmission Control Protocol)** is a **Layer 4** (Transport Layer) protocol ensuring reliable, ordered, and error-checked delivery of data between applications.

### **Key TCP Fields**
| Field Name        | Description                           |
|-------------------|---------------------------------------|
| Source Port       | Sender’s port number                 |
| Destination Port  | Receiver’s port number               |
| Sequence Number   | Number of the first byte in segment   |
| Acknowledgment No | Confirms received data                |
| Flags             | Control bits (SYN, ACK, FIN, RST, PSH, URG) |
| Window Size       | Buffer size available                 |
| Checksum          | Error-checking field                  |

---

## 🔍 Most Common TCP Display Filters
| Filter                  | Description                         |
|-------------------------|-------------------------------------|
| `tcp`                   | Show all TCP packets                |
| `tcp.flags.syn == 1`    | Show SYN packets (start of connection) |
| `tcp.flags.fin == 1`    | Show FIN packets (end of connection)  |
| `tcp.port == 80`        | Show TCP packets on port 80 (HTTP)    |
| `ip.addr == 192.168.1.1`| TCP traffic to/from a specific host    |

---

## 🧪 Lab Steps – TCP Filters & Flags

### **Step 1: Open Wireshark**
1. Launch **Wireshark** on your system.  
2. Select the **active network interface** (Ethernet or Wi-Fi).

---

### **Step 2: Show All TCP Packets**
1. In the **Display Filter bar**, type:  
tcp

2. Press **Enter**.

  <img width="975" height="693" alt="image" src="https://github.com/user-attachments/assets/d4e630a4-adea-434f-ae59-a2720cd767f8" />

3. **Generate TCP traffic** (examples):  
- Open a website in your browser  
- Or run:  
  ```bash
  curl http://example.com
  ```
4. Verify you see TCP packets in Wireshark.

<img width="975" height="681" alt="image" src="https://github.com/user-attachments/assets/fa94e29a-1f2a-4934-a482-98083fb190d6" />

---

### **Step 3: Show Only SYN Packets (Connection Start)**
1. In the **Display Filter bar**, type:  
tcp.flags.syn == 1 && tcp.flags.ack == 0

2. Press **Enter**.  
*This filter isolates the initial SYN packets (connection initiation).*


<img width="975" height="684" alt="image" src="https://github.com/user-attachments/assets/4cad923b-3032-4913-a926-9f221e82562e" />

---

### **Step 4: Show Only FIN Packets (Connection End)**
1. In the **Display Filter bar**, type:  
tcp.flags.fin == 1

2. Press **Enter**.  
*This filter isolates TCP connection termination packets.*


<img width="975" height="688" alt="image" src="https://github.com/user-attachments/assets/0d91eb93-04bc-45ad-9739-20ee72fc295a" />

---

### **Step 5: Show TCP Traffic To/From a Specific Host**
1. In the **Display Filter bar**, type:  
tcp && ip.addr == <Host-IP>

Replace `<Host-IP>` with the IP address you want to filter (e.g., `192.168.1.100`).  
2. Press **Enter**.  
*This filter isolates TCP traffic for a single host.*

<img width="975" height="683" alt="image" src="https://github.com/user-attachments/assets/597f08fc-4ea3-4edf-bf1c-cdf6eb96243f" />

---

## 📤 Submission
Submit **four screenshots**:
1. **All TCP packets** (`tcp`)  
2. **Only SYN packets** (`tcp.flags.syn == 1 && tcp.flags.ack == 0`)  
3. **Only FIN packets** (`tcp.flags.fin == 1`)  
4. **TCP traffic to/from a specific host** (`tcp && ip.addr == <Host-IP>`)

---

## ✅ Conclusion
- **TCP ensures reliable and ordered data delivery** through its **3-way handshake** and **flow control**.  
- Understanding **TCP flags** helps with:  
- Connection state tracking  
- Troubleshooting dropped or reset connections  
- Detecting scanning and abnormal behavior (e.g., RST floods)

---
