# **Nmap Network Scanning Portfolio**

This folder contains my hands‑on Nmap reconnaissance work as part of my cybersecurity learning journey. Each scan includes:

- The **raw Nmap output** (`.txt`)
- A **detailed analysis** (`.md`)
- Clear explanations of what each result means in a real‑world security context

The goal is to demonstrate practical skills in host discovery, service enumeration, OS fingerprinting, and network mapping — all essential capabilities for a SOC Analyst or penetration tester.

---

## **Scans Included**

### **1. Windows Host Scan — 192.168.1.29**
- **File:** `windows-192.168.1.29-scan.txt`  
- **Analysis:** `windows-192.168.1.29-scan-analysis.md`  
- **Summary:**  
  Identifies open Windows networking ports (RPC and SMB), confirms firewall filtering, and analyzes expected internal Windows behavior.

---

### **2. Router Scan — 192.168.1.1**
- **File:** `router-192.168.1.1-scan.txt`  
- **Analysis:** `router-192.168.1.1-scan-analysis.md`  
- **Summary:**  
  Enumerates DNS, HTTP, and HTTPS services on the home router. Shows strong firewall behavior and explains why Telnet being filtered is a good security sign.

---

### **3. Network Discovery Scan — 192.168.1.0/24**
- **File:** `network-192.168.1.0-24-discovery.txt`  
- **Analysis:** `network-192.168.1.0-24-discovery-analysis.md`  
- **Summary:**  
  Maps all active devices on the network, identifies hosts using ARP, and classifies unknown devices based on MAC vendors and scan behavior.

---

### **4. Targeted Device Fingerprinting (Example: 192.168.1.15)**
- **File:** `192.168.1.15-os-scan.txt` (optional if you saved it)  
- **Summary:**  
  Attempts OS detection on a device exposing only a filtered SIP port. Demonstrates how to interpret uncertain OS guesses and identify embedded network devices.

---

## **Tools & Techniques Used**

### **Nmap Flags**
- `-sV` → Service/version detection  
- `-Pn` → Skip ping (useful for Windows hosts)  
- `-sn` → Host discovery only  
- `-O` → OS fingerprinting  
- `-oN` → Save output in normal format  

### **ARP Analysis**
Used `arp -a` on Windows to map IP addresses to MAC addresses and identify device vendors. This is a standard technique for network asset discovery.

---

## **What I Learned**
- How to perform structured reconnaissance on a local network  
- How Windows, routers, and IoT devices behave under Nmap scans  
- How to interpret open, closed, and filtered ports  
- How to identify devices using MAC vendor lookup  
- How to document scans in a clean, professional format  
- How to compare results across multiple network interfaces (WSL, VirtualBox, Wi‑Fi)  

---

## **Purpose of This Folder**
This Nmap portfolio demonstrates:

- Practical, real‑world scanning experience  
- Ability to analyze and interpret network behavior  
- Strong documentation skills  
- A methodical approach to cybersecurity learning  

It forms part of my broader SOC Analyst and cybersecurity development roadmap.
