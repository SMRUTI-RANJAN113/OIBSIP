# Task 1 — Basic Network Scanning with Nmap

## Objective
Perform a network scan to identify open ports and services running on a
local machine, and document the findings with a security analysis of
each discovered port.

## Tech Stack / Tools
- **Nmap** 7.99
- **Windows** (Command Prompt / PowerShell — `C:\Program Files (x86)>nmap`)
- Target: `127.0.0.1` (localhost — the local machine itself)

---

## What is Nmap?
Nmap ("Network Mapper") is a free, open-source tool used to discover
hosts and services on a computer network. It works by sending
specially crafted packets to a target and analyzing the responses.
Nmap can determine:
- Which hosts are up on a network
- Which ports are open, closed, or filtered on those hosts
- What services and versions are running on open ports
- What operating system a host is likely running

It is one of the most widely used tools in cybersecurity, used by
network administrators for auditing and by penetration testers /
security researchers for reconnaissance.

## Why Network Scanning Matters
Network scanning is a foundational security practice because:
- **Visibility** — you can't secure what you don't know exists. Scanning
  reveals every open door (port/service) on a system.
- **Attack surface reduction** — many services are enabled by default
  and left running unnecessarily. Identifying them lets admins close
  ports that don't need to be open.
- **Vulnerability awareness** — some services (like older SMB versions)
  have known, actively-exploited vulnerabilities. Knowing a port is
  open is the first step to patching or restricting it.
- **Compliance & auditing** — regular scanning helps organizations
  verify their systems match their intended security configuration.

## Ethical Use Guidelines
- **Only scan systems you own or have explicit written permission to
  scan.** Scanning networks or hosts without authorization is illegal
  in most jurisdictions (e.g., under the U.S. Computer Fraud and Abuse
  Act, and the UK Computer Misuse Act).
- This task was performed exclusively against **127.0.0.1 (localhost)**
  — i.e., the scanner's own machine — so no external system or network
  was touched.
- Never use scanning techniques against production systems, third-party
  infrastructure, or any network without a clear scope of engagement.
- Always document scans and keep results confidential/secure, since
  they reveal information that could be misused by an attacker.

---

## Installation Steps (Windows)
1. Go to the official Nmap website: [https://nmap.org/download.html](https://nmap.org/download.html)
2. Download the **Windows self-installer** (e.g., `nmap-7.99-setup.exe`).
3. Run the installer with administrator privileges.
4. Accept the license agreement and keep the default components
   selected (this includes Npcap, which Nmap needs for raw packet
   capture on Windows).
5. Complete the installation — by default Nmap installs to
   `C:\Program Files (x86)\Nmap`.
6. Verify installation by opening Command Prompt and running:
   ```
   nmap -version
   ```
7. Nmap can then be run directly from `C:\Program Files (x86)>nmap`
   as shown in the scan screenshots for this task.

---

## Scans Performed

| # | Command | Purpose |
|---|---------|---------|
| 1 | `nmap 127.0.0.1` | Basic scan — identify open ports |
| 2 | `nmap -sV 127.0.0.1` | Service version scan — identify service/version details |
| 3 | `nmap -O 127.0.0.1` | OS detection scan — identify the operating system |

Full terminal output for all three scans is recorded in
[`nmap_scan_results.txt`](./nmap_scan_results.txt), and terminal
screenshots for each scan are included in the [`screenshots/`](./screenshots)
folder:
- `screenshots/basic_scan.png`
- `screenshots/service_version_scan.png`
- `screenshots/os_detection_scan.png`

## Key Findings (Summary)
Two open TCP ports were found on the target (127.0.0.1):

| Port | Service | Risk Level |
|------|---------|------------|
| 135/tcp | msrpc (Microsoft RPC Endpoint Mapper) | High — historically exploited (e.g., Blaster worm) |
| 445/tcp | microsoft-ds (SMB) | Very High — historically exploited (e.g., EternalBlue/WannaCry) |

The `-O` scan identified the host as **Microsoft Windows 11 (24H2 - 25H2)**.

Full explanations of what each service does and the security risk it
poses are documented in `nmap_scan_results.txt`.

---
