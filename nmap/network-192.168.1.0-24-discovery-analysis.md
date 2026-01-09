# **Network Discovery Scan Analysis — 192.168.1.0/24**

## **Overview**
This scan performed host discovery across the entire local network range `192.168.1.0/24`.  
The goal was to identify all active devices, understand network population, and observe latency differences between hosts.

Nmap detected **6 active devices** out of 256 possible IP addresses, which is typical for a small home network.

---

## **Command Used**
nmap -sn 192.168.1.0/24 -oN network-192.168.1.0-24-discovery.txt


- `-sn` → Host discovery only (no port scanning)  
- Scans all 256 IPs in the subnet  
- Identifies devices that respond to ARP/ICMP/TCP probes  

---

## **Discovered Hosts**

### **1. 192.168.1.1 — Router**
- **Latency:** 0.0085s  
- Typically the fastest response on the network  
- Confirms the router is online and reachable  
- Matches earlier router scan results  

### **2. 192.168.1.15 — Unknown Device**
- **Latency:** 0.19s  
- Higher latency suggests:  
  - A mobile device  
  - A smart TV  
  - An IoT device  
- Will require MAC address lookup (future ARP scan) to identify  

### **3. 192.168.1.29 — Windows Machine (My PC)**
- **Latency:** 0.0011s  
- Fastest response on the network  
- Matches earlier Windows scan results  
- Confirms the host is active and reachable  

### **4. 192.168.1.49 — Unknown Device**
- **Latency:** 0.38s  
- Slowest response in the scan  
- Likely a low‑power IoT device (camera, bulb, smart plug)  
- Could also be a phone in sleep mode  

### **5. 192.168.1.56 — Unknown Device**
- **Latency:** 0.0084s  
- Very fast response  
- Likely a device connected via Ethernet or strong Wi‑Fi  

### **6. 192.168.1.61 — Unknown Device**
- **Latency:** 0.090s  
- Moderate latency  
- Could be a phone, tablet, or smart home device  

### 192.168.1.15 — Likely VoIP or Embedded Network Device

I performed a targeted scan and OS detection against 192.168.1.15:

- Only port 5060/tcp (SIP) appeared, and it was filtered.
- SIP is commonly used for VoIP (IP phones, PBX systems, VoIP gateways) and sometimes IP cameras.
- Nmap’s aggressive OS guesses all pointed toward small embedded network devices such as routers, wireless access points, IP cameras, or VoIP-related hardware.
- Because there is no open port and only one filtered SIP port, OS detection is unreliable, but the behavior strongly suggests this is a specialized network/telecom device rather than a general-purpose PC or phone.

Conclusion: 192.168.1.15 is most likely a VoIP-related or embedded network device (e.g., IP camera, VoIP adapter, or similar).
---

## **Interpretation**
- The network contains **6 active devices**, which is normal for a home environment.  
- Latency differences help infer device types (PCs respond faster than IoT devices).  
- No unexpected or suspicious hosts appear in the scan.  
- The router and Windows machine match previous scans, confirming consistency.  
- Unknown devices can be identified later using:  
  arp -a
  or  
  sudo nmap -O <IP> 
  (OS detection)

---

## **What I Learned**
- How to perform a full subnet discovery using Nmap.  
- How ARP/ICMP host discovery works on local networks.  
- How to interpret latency to infer device characteristics.  
- How to identify active hosts without scanning ports.  
- How to document network reconnaissance results professionally.
