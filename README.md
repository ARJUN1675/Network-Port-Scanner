# Network-Port-Scanner
## Port Scan Analysis Report

**Target IP:** 172.26.239.6  
**Port Range:** 1–1024  
**Status:** Completed

---

### 1. Executive Summary
The port scan identified three open ports within the specified range. All discovered ports (135, 139, 445) are consistently associated with **Microsoft Windows networking services**. The presence of these services suggests the target is likely a Windows-based workstation or server utilizing Remote Procedure Call (RPC) and Server Message Block (SMB) protocols for communication and file sharing.

### 2. Open Ports Detail

| Port | Protocol | Service | Description |
| :--- | :--- | :--- | :--- |
| **135** | TCP | MSRPC | Microsoft RPC Endpoint Mapper; used to locate RPC services. |
| **139** | TCP | NetBIOS-SSN | NetBIOS Session Service; used for legacy Windows file and printer sharing. |
| **445** | TCP | Microsoft-DS | SMB over TCP; the modern standard for Windows file sharing and IPC. |

---

### 3. Technical Analysis

#### Windows Networking Stack
The combination of these three ports is a classic signature of a Windows environment. 

* **Port 135 (MSRPC):** This service acts as a "directory" for other RPC services. When a client wants to communicate with a specific service on the host, it first queries port 135 to find out which port that service is actually listening on.
* **Port 139 & 445 (SMB/NetBIOS):** While Port 139 is the older NetBIOS implementation, Port 445 is the modern standard for SMB. These are used for authenticated access to files, printers, and "Named Pipes," which allow for remote administration and communication between processes.



### 4. Security Implications & Vulnerabilities
The exposure of these ports to an untrusted network presents several security risks:

* **Information Leakage:** Services on Port 135 and 139 can often be queried (sometimes without authentication) to reveal sensitive system information, such as computer names, domain details, user accounts, and active network shares.
* **Legacy Exploits:** Port 139 is frequently targeted due to vulnerabilities in older versions of the SMB protocol (SMBv1).
* **High-Impact Exploits:** Port 445 is historically significant as the primary vector for major exploits like **EternalBlue (MS17-010)** and various ransomware strains that move laterally through a network using SMB.

