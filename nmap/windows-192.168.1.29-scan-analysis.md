# Nmap Scan Analysis — 192.168.1.29

## Overview
This scan targeted my actual Windows machine on my home network (`192.168.1.29`).  
The goal was to identify exposed services, understand firewall behavior, and compare results with other internal scans (e.g., VirtualBox host-only interface).

The scan revealed two open ports commonly associated with Windows networking, while the firewall filtered the remaining ports. This behavior is expected for a properly secured Windows host on a private LAN.


## Command Used
```
nmap -sV -Pn 192.168.1.29 -oN windows-192.168.1.29-scan.txt
```

- `-sV` → Service/version detection  
- `-Pn` → Skip ping (Windows often blocks ICMP)  
- `-oN` → Save output in normal format  



## Key Findings

### 1. Port 135/tcp — MSRPC (Microsoft Windows RPC)
- **State:** open  
- **Service:** msrpc  
- **Purpose:**  
  - Core Windows Remote Procedure Call service  
  - Used for DCOM, service control, and internal OS communication  
- **Notes:**  
  - Always open on Windows systems  
  - Safe inside a private network  
  - Historically targeted by worms (e.g., Blaster), but not dangerous when behind a firewall  


### 2. Port 445/tcp — SMB (microsoft-ds)
- **State:** open  
- **Service:** SMB (Server Message Block)  
- **Purpose:**  
  - File sharing  
  - Printer sharing  
  - Windows authentication  
  - Active Directory communication  
- **Notes:**  
  - Critical Windows networking port  
  - Should never be exposed to the public internet  
  - Internally, this is normal and expected  
  - This port was exploited by EternalBlue (WannaCry), but home routers block external access  


### 3. 998 Filtered Ports
- **State:** filtered (no response)  
- **Meaning:**  
  - Windows Firewall blocked Nmap’s probes  
  - Nmap cannot determine whether the ports are open or closed  
- **Interpretation:**  
  - Firewall is active and functioning correctly  
  - Only essential Windows services are exposed internally  


## Interpretation
- The host is clearly identified as a Windows machine, confirmed by service signatures and port behavior.  
- Only two core Windows networking ports (135 and 445) are open, which is normal for a Windows system on a private LAN.  
- The firewall is filtering the majority of ports, indicating a secure configuration.  
- No unexpected or suspicious services were detected.
- The results closely match the VirtualBox host-only scan, reinforcing the consistency of Windows networking behavior across interfaces.


## What I Learned
- How to scan a Windows host that blocks ping using `-Pn`.  
- How to interpret filtered vs open ports in the context of firewall behavior.  
- How Windows exposes RPC and SMB services internally.  
- How to compare scans across different network interfaces (WSL, VirtualBox, Wi-Fi).  
- How to document Nmap results in a structured, professional format suitable for a cybersecurity portfolio.
