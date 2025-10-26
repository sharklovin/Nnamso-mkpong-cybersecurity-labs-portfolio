# Port Scanning, Device Fingerprinting, and Metasploit Scan
**Category:** CySA+  
**Date:** 25/10/2025
**Author:** Nnamso Mkpong

---

## Objective
- Perform comprehensive network reconnaissance with nmap
- Conduct operating system detection via device fingerprinting
- Execute web vulnerability scanning using Metasploit Framework
- Analyze and interpret security scanning results

---

## Environment / Tools
- Attacker: Kali Linux VM (username: `kali`, password: `kali`)  
- Target: Metasploitable VM (username: `msfadmin`, password: `msfadmin`)  
- Tools: `nmap`, `Metasploit Framework (msfconsole)` VM ware 17 pro

---
## Steps
**Boot both virtual machines:**
- Kali Linux (attacker machine) - credentials: kali/kali
- Metasploitable (target machine) - credentials: msfadmin/msfadmin

**Network Discovery:**
- On Metasploitable VM: **ifconfig** 
(add pic)
- Note the IP address assigned to Metasploitable for targeting which is 192.168.119.133 

**Port Scanning with Nmap**
- From Kali Linux terminal: type **nmap 192.168.119.133** (add pic)
- The scan identified 22 open TCP ports running various services including FTP, SSH, Telnet, SMTP, DNS, HTTP, multiple RPC services, NetBIOS, database services (MySQL, PostgreSQL), VNC, and several legacy Unix services (exec, login, shell).
- To identify the operating system and check for all possible ports, I ran: nmap –O 192.168.119.133 –p 1-65535 
(add pics)
The scan result showed the Metasploitable is running OS Linux kernel version 2.6.9 to 2.6.33
The comprehensive scan discovered 5 new ports (3632, 6697, 8787, 47786, 49404, 58122, 59063) running services like distccd and IRCS that the basic scan missed.

---

### Metasploit Framework Vulnerability Scanning
**WMAP Web Vulnerability Assessment Process**
- Launched Metasploit Framework using msfconsole
- Loaded WMAP module with *load wmap* command
- Added target site using wmap_sites -a http://192.168.119.133
- Set scan target with wmap_targets -t http://192.168.119.133
- Executed vulnerability scan via *wmap_run -e*
- Reviewed discovered vulnerabilities using wmap_vulns -l


**Key Findings**
- Web Server: Apache 2.2.8 (Ubuntu) with PHP/5.2.4
- Exposed Directories: /dav/, /doc/, /phpMyAdmin/, /test/, /icons/, /index/
- Access Control: /cgi-bin/ returns 403 (Forbidden)
- Information Disclosure: Server banner reveals detailed version information**
   
## Conclusion
- The port scanning and fingerprinting exercises confirmed that Metasploitable exposes multiple open services. The scan successfully identified multiple exposed directories and services, with /phpMyAdmin/ access posing the most significant security risk due to potential database compromise vectors.