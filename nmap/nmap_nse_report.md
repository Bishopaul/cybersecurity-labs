Absolutely, Paul — here is your **GitHub‑ready README.md** for **Day 6**, cleanly formatted, professional, and portfolio‑worthy.  
You can paste this directly into your `Day6/README.md` file.

---

# **Day 6 – Nmap NSE Enumeration & Vulnerability Assessment**

## **1. Executive Summary**
This assessment focused on enumerating and analyzing a Python SimpleHTTPServer instance running on port 8080 within a local Kali Linux environment. Using Nmap’s NSE (Nmap Scripting Engine), multiple HTTP‑focused scripts were executed to identify service behavior, configuration weaknesses, and potential vulnerabilities.  
The scan revealed directory listing exposure and a **Slowloris Denial‑of‑Service vulnerability (CVE‑2007‑6750)**.

---

## **2. Scope**
**Target:**  
- Host: `127.0.0.1`  
- Port: `8080/tcp`  
- Service: Python SimpleHTTPServer 0.6 (Python 3.13.7)

**Tools Used:**  
- Nmap 7.95  
- NSE scripts:  
  - `http-title`  
  - `http-enum`  
  - `http-methods`  
  - `vuln`

---

## **3. Methodology**

### **3.1 Service Title Enumeration**
```
nmap --script http-title -p 8080 127.0.0.1
```
**Result:**  
- Returned: **“Directory listing for /”**  
- Confirms directory listing is enabled.

---

### **3.2 Directory Enumeration**
```
nmap --script http-enum -p 8080 127.0.0.1
```
**Result:**  
- No common directories discovered.  
- Indicates a minimal or empty served directory.

---

### **3.3 HTTP Methods Enumeration**
```
nmap --script http-methods -p 8080 127.0.0.1
```
**Result:**  
- Supported Methods: **GET, HEAD**  
- No dangerous methods (PUT, DELETE, TRACE, OPTIONS) enabled.

---

### **3.4 Vulnerability Scan**
```
nmap --script vuln -p 8080 127.0.0.1
```
**Result:**  
- **Likely Vulnerable to Slowloris DoS attack**  
- Identified CVE: **CVE‑2007‑6750**  
- Server fails to close partial HTTP requests, enabling resource exhaustion.

---

## **4. Findings**

### **Finding 1 — Directory Listing Enabled**
**Evidence:**  
- `http-title` script returned “Directory listing for /”.

**Impact:**  
- Exposes all files in the served directory.  
- Potential leakage of sensitive information.

**Risk Level:** Medium

---

### **Finding 2 — Limited HTTP Methods**
**Evidence:**  
- Only GET and HEAD supported.

**Impact:**  
- Server is read‑only; no upload or modification possible.  
- Low exploitation potential.

**Risk Level:** Low

---

### **Finding 3 — Slowloris DoS Vulnerability (CVE‑2007‑6750)**
**Evidence:**  
- `http-slowloris-check` flagged the service as *Likely Vulnerable*.  
- Python SimpleHTTPServer lacks connection timeout protections.

**Impact:**  
- Attackers can exhaust server resources.  
- Service becomes unresponsive to legitimate users.  
- High risk if exposed beyond localhost.

**Risk Level:** High

---

## **5. Recommendations**
- Avoid using Python SimpleHTTPServer for production environments.  
- Restrict access to localhost only (already implemented).  
- Replace with hardened servers such as **Nginx**, **Apache**, or **Caddy**.  
- Implement rate limiting and connection timeout controls.  
- Disable the server when not actively needed.  
- Avoid serving sensitive directories.

---

## **6. Conclusion**
The Day 6 enumeration revealed that while the Python SimpleHTTPServer is minimal and exposes only safe HTTP methods, it remains unsuitable for production use due to directory listing exposure and susceptibility to the Slowloris DoS attack (CVE‑2007‑6750).  
The service should be used strictly for testing and should remain accessible only from localhost. 
