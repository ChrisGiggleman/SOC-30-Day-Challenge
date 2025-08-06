---

# 🌐 Day 9: Wireshark Basics – HTTP Protocol Analysis

🎯 **Objective:**  
 Analyze HTTP (Hypertext Transfer Protocol) packets using Wireshark.  
Explore HTTP request/response headers, understand web communications, and detect common HTTP-based attacks or data leaks.
---

## 🛠️ Lab Setup

### System Requirements
- Operating System: Windows 10/11, Linux, or macOS  
- Software: Wireshark (latest version)  
---

## 📘 HTTP Packet Structure and Fields

HTTP is an **application-layer** protocol commonly running on **TCP port 80**. It is used for client (browser) ↔ server communication.

### **Key HTTP Fields**
| Field Name     | Description                                 |
|----------------|---------------------------------------------|
| Request Method | HTTP verbs (GET, POST, HEAD, etc.)          |
| Host           | The website being accessed                  |
| User-Agent     | Information about the client/browser        |
| URI            | Resource path on the server                 |
| Status Code    | Server's response status (e.g., 200 OK)     |
| Content-Type   | MIME type of the response (e.g., text/html) |
| Cookie/Header  | Session or tracking information             |

---

## 🔍 Most Common HTTP Display Filters
| Filter                          | Description                          |
|--------------------------------|--------------------------------------|
| `http`                         | Show all HTTP traffic                |
| `tcp.port == 80`               | HTTP traffic by default port         |
| `http.request.method == "GET"` | Show all GET requests                |
| `http.request.uri`             | View requested resources             |
| `http.set_cookie`              | Show cookies in HTTP responses       |
| `ip.addr == 192.168.1.10`      | HTTP traffic to/from specific host   |

---

## 🧪 Lab Steps – HTTP Packet Analysis

### **Step 1: Open Wireshark & Load Capture**
1. Launch **Wireshark**.  
---

### **Step 2: Apply HTTP Filter**
1. In the **Display Filter bar**, type:  
http

2. Press **Enter**.  
*All HTTP traffic is now visible.*

 <img width="975" height="735" alt="image" src="https://github.com/user-attachments/assets/864ee9b9-db4d-42b8-a4ce-6521bc755859" />

---

### **Step 3: Analyze HTTP Requests**
1. Look for **GET** and **POST** methods in the **Info** column.  
2. Right-click an HTTP request → **Follow → HTTP Stream** to view full client-server conversation.  
3. Note:
- **Host header**
- **Request URI**
- **User-Agent**

 <img width="975" height="926" alt="image" src="https://github.com/user-attachments/assets/c1696050-9a0e-4e88-b9c7-d8cc5ab66ca8" />

---

### **Step 4: Check HTTP Response**
1. Select any **HTTP 200 OK** packet.  
2. Expand:
- **Hypertext Transfer Protocol** section  
- Review:
  - **Status Code**
  - **Content-Type**
  - Any **Set-Cookie** headers

---

### **Step 5: Detect Sensitive Data**
1. Look for:
- **Credentials in clear text** (common in basic auth or query parameters)
- **Session cookies**
- **Suspicious file downloads**

---

## 📤 Submission
Submit **three screenshots**:
1. **All HTTP traffic** with `http` filter applied  
2. **An HTTP GET request** with headers visible (Host, URI, User-Agent)  
3. **An HTTP response** showing Status Code and Content-Type (expand HTTP section)

---

## ✅ Conclusion
- **HTTP traffic is readable and easy to analyze** in Wireshark.  
- Analyzing HTTP helps detect:
- Sensitive data exposure in URLs or headers  
- Malware beaconing to C2 servers  
- Suspicious file downloads or unauthorized access

---
