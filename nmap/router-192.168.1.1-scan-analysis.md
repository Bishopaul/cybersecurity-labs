# **Nmap Scan Analysis — 192.168.1.1 (Home Router)**

## **Overview**
This scan targeted my home router at `192.168.1.1`, the default gateway for all devices on my network.  
The objective was to identify exposed services, evaluate firewall behavior, and understand how the router manages internal network traffic.

The scan revealed four ports of interest, all of which are typical for consumer routers. Most other ports were closed, indicating strong default security.

---

## **Command Used**
```
nmap -sV -Pn 192.168.1.1 -oN router-192.168.1.1-scan.txt
```

- `-sV` → Service/version detection  
- `-Pn` → Skip ping (some routers block ICMP)  
- `-oN` → Save output in normal format  

---

## **Key Findings**

### **1. Port 23/tcp — Telnet (filtered)**
- **State:** filtered  
- **Service:** telnet  
- **Meaning:**  
  - The router is silently dropping Telnet probes.  
  - Telnet is not open, but the firewall is blocking the connection attempts.  
- **Security Notes:**  
  - Telnet is an outdated, insecure protocol that sends credentials in clear text.  
  - Seeing it **filtered** (not open) is a good sign.  
  - Modern routers disable Telnet by default, which aligns with best practices.

---

### **2. Port 53/tcp — DNS (open, running Unbound)**
- **State:** open  
- **Service:** domain (DNS)  
- **Version:** Unbound  
- **Meaning:**  
  - The router is running a DNS resolver for devices on the network.  
  - Unbound is a secure, modern DNS server.  
- **Security Notes:**  
  - This is normal and required for internal DNS resolution.  
  - Safe as long as it is not exposed to the public internet.

---

### **3. Port 80/tcp — HTTP (open)**
- **State:** open  
- **Service:** http  
- **Meaning:**  
  - This is the router’s web-based admin interface.  
  - Nmap captured a large HTML response, including cookies and security headers.  
- **Security Notes:**  
  - Normal for internal access.  
  - Should be password-protected (it is).  
  - Should not be accessible from outside the LAN.

---

### **4. Port 443/tcp — HTTPS (open, tcpwrapped)**
- **State:** open  
- **Service:** tcpwrapped  
- **Meaning:**  
  - The router accepts the connection but immediately closes it.  
  - This behavior is common for HTTPS admin interfaces.  
- **Security Notes:**  
  - Indicates the router supports secure login via HTTPS.  
  - Expected and safe.

---

## **Closed Ports**
```
996 closed tcp ports (conn-refused)
```

- The router actively refused connections on most ports.  
- This is excellent security behavior.  
- Only essential services are exposed.

---

## **Interpretation**
- The router exposes only the services required for internal management (DNS, HTTP, HTTPS).  
- Telnet is filtered, meaning it is not available — a positive security sign.  
- The firewall is functioning correctly, blocking unnecessary ports.  
- The HTML fingerprint from port 80 confirms the presence of a secure login interface with modern headers (XSS protection, CSP, SameSite cookies).  
- No unexpected or suspicious services were detected.

Overall, the router appears **secure**, **well-configured**, and **following best practices** for a home network environment.

---

## **What I Learned**
- How routers expose only essential services to internal networks.  
- How to interpret filtered vs closed ports on network devices.  
- How to analyze router fingerprints and HTML responses from admin interfaces.  
- How DNS, HTTP, and HTTPS behave on consumer networking equipment.  
- How to document router scans in a structured, professional format.
