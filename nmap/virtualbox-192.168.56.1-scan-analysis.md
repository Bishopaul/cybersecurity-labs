# Nmap Scan Analysis — 192.168.56.1

## Overview
This scan targeted the VirtualBox Host-Only Network interface on my Windows machine.  
The goal was to identify open ports, understand firewall behavior, and analyze exposed services.

## Command Used
nmap -sV -Pn 192.168.56.1 -oN virtualbox-192.168.56.1-scan.txt

## Key Findings
### Port 135 — MSRPC (Microsoft RPC)
- Used for Windows Remote Procedure Calls.
- Supports DCOM, service control, and internal Windows communication.
- Normal for Windows systems on private networks.

### Port 445 — SMB (Server Message Block)
- Used for file sharing, printer sharing, and Windows authentication.
- Critical internal service for Windows networking.
- Historically targeted by malware (e.g., EternalBlue), but safe inside a LAN.

### 998 Filtered Ports
- Indicates firewall filtering.
- Windows Firewall blocks most inbound probes.
- Expected behavior for a secure Windows host.

## Interpretation
- The host is running Windows, confirmed by service signatures.
- Only essential Windows networking ports are exposed.
- Firewall is active and filtering most traffic.
- No unexpected or suspicious services detected.

## What I Learned
- How to scan a Windows host that blocks ping using `-Pn`.
- How to interpret open vs filtered ports.
- How Windows exposes RPC and SMB internally.
- How to document and present Nmap results professionally.
