# GhostCOM

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

## Disclaimer
This project is for **educational and research** purposes only. Do not deploy it on unauthorized systems. The author is not responsible for any misuse.

## Future Plans
- Add shell functionality to beacon
- Encrypt beacon-C2 communication
- More functionality to beacon
