# Day 10: Wireshark Basics – TLS Protocol Analysis

---

## 🎯 Objective
Learn how to analyze **TLS (Transport Layer Security)** traffic using Wireshark.  
Students will:
- Understand how TLS secures network communication.
- Explore handshake messages and metadata (like server names and certificates).
- Identify TLS versions in captured traffic.

---

## 🛠️ Lab Setup

### System Requirements
- Operating System: Windows 10/11, Linux, or macOS
- Software: Wireshark (latest version)
- Files: Sample TLS PCAP file (`tls_traffic.pcap`)

---

## 📘 TLS Packet Structure and Fields

**TLS (Transport Layer Security)** is a cryptographic protocol providing secure communication over TCP (commonly port 443).  
It is widely used in **HTTPS, FTPS, SMTPS**, and many secure applications.

### Key TLS Handshake Messages
| Message Type  | Description                                   |
|---------------|-----------------------------------------------|
| Client Hello  | Client initiates connection, offers cipher suites |
| Server Hello  | Server selects cipher and provides certificate    |
| Certificate   | Server sends its X.509 certificate                |
| Key Exchange  | Exchange of keys for encryption                    |
| Finished      | Secure session begins                             |

---

## 🔍 Common TLS Display Filters
| Filter                        | Description                     |
|-------------------------------|---------------------------------|
| `tls`                         | Show all TLS traffic            |
| `tcp.port == 443`             | TLS over HTTPS                  |
| `tls.handshake.type == 1`     | Client Hello messages            |
| `tls.handshake.type == 2`     | Server Hello messages            |
| `tls.record.version == 0x0303`| TLS 1.2 traffic                  |
| `tls.record.version == 0x0304`| TLS 1.3 traffic                  |

---

## 🧪 Lab Steps – TLS Traffic Analysis

### Step 1: Open Wireshark & Load PCAP
1. Launch **Wireshark**.

---

### Step 2: Show All TLS Traffic
1. In the **Display Filter bar**, type:  
tls

2. Press **Enter**.
3. Verify only **TLS packets** are displayed.

**📸 Screenshot #1:** Show all TLS traffic (with `tls` filter applied).

<img width="1030" height="723" alt="image" src="https://github.com/user-attachments/assets/c850b03d-6145-40a2-b3dd-a1326ae58173" />

---

### Step 3: Show Only Client Hello Messages
1. In the **Display Filter bar**, type:  
tls.handshake.type == 1

2. Press **Enter**.
3. Click on one of the **Client Hello** packets.
4. Expand the **Secure Sockets Layer / Transport Layer Security** section:
- Locate **Version**, **Cipher Suites**, and **Server Name (SNI)**.

**📸 Screenshot #2:** Show Client Hello messages (with filter applied).

<img width="1030" height="768" alt="image" src="https://github.com/user-attachments/assets/5cba89ff-febc-4d41-8634-991fb3c88b12" />

---

### Step 4: Show Only TLS 1.2 Traffic
1. In the **Display Filter bar**, type:  
tls.record.version == 0x0303

2. Press **Enter**.
3. Confirm that the displayed packets are using **TLS 1.2**.

**📸 Screenshot #3:** Show TLS 1.2 traffic (with filter applied).

<img width="1027" height="723" alt="image" src="https://github.com/user-attachments/assets/347beb4c-fd1b-4a92-804d-33fa5454ad70" />

---

## 📤 Submission
Submit **three screenshots**:
1. **All TLS traffic** using `tls` filter.
2. **Client Hello messages** using `tls.handshake.type == 1` filter.
3. **TLS 1.2 traffic** using `tls.record.version == 0x0303` filter.

---

## ✅ Conclusion
- TLS provides **encryption and security** for network traffic.  
- While Wireshark can’t decrypt encrypted traffic without keys, it can still reveal:
- **Server names (SNI)**
- **Certificate chain**
- **TLS version and cipher suites**
- Analyzing TLS metadata helps detect:
- **Outdated TLS versions** (e.g., TLS 1.0, TLS 1.1)
- **Suspicious/self-signed certificates**
- **Malicious encrypted domain usage**
