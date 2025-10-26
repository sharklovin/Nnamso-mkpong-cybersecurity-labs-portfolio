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
<img width="1020" height="463" alt="nmap ip " src="https://github.com/user-attachments/assets/5c5cab0d-5a7a-4ac7-85f6-963072b69f8b" />

- Note the IP address assigned to Metasploitable for targeting which is 192.168.119.133 
<img width="1020" height="463" alt="ip address" src="https://github.com/user-attachments/assets/72839402-0e56-4561-9e4d-120b26abda55" />


**Port Scanning with Nmap**
- From Kali Linux terminal: type **nmap 192.168.119.133**
  <img width="1147" height="565" alt="kali port scan" src="https://github.com/user-attachments/assets/d01c6687-5203-4adf-a24f-77766398813d" />

- The scan identified 22 open TCP ports running various services including FTP, SSH, Telnet, SMTP, DNS, HTTP, multiple RPC services, NetBIOS, database services (MySQL, PostgreSQL), VNC, and several legacy Unix services (exec, login, shell).
- To identify the operating system and check for all possible ports, I ran: nmap –O 192.168.119.133 –p 1-65535 
<img width="1015" height="484" alt="OS scan results" src="https://github.com/user-attachments/assets/3a796026-1b93-4213-af6d-283de0d93a34" />

The scan result showed the Metasploitable is running OS Linux kernel version 2.6.9 to 2.6.33
The comprehensive scan discovered 5 new ports (3632, 6697, 8787, 47786, 49404, 58122, 59063) running services like distccd and IRCS that the basic scan missed.

---

### Metasploit Framework Vulnerability Scanning
**WMAP Web Vulnerability Assessment Process**
- Launched Metasploit Framework using msfconsole
- Loaded WMAP module with *load wmap* command
<img width="1015" height="484" alt="OS scan results" src="https://github.com/user-attachments/assets/1d6373a2-3f2c-432b-975e-2af20de0b3b1" />

- Added target site using wmap_sites -a http://192.168.119.133
- <img width="477" height="69" alt="telling wmap" src="https://github.com/user-attachments/assets/02835f79-8e91-41b9-90db-6d3c93983282" />

- Set scan target with wmap_targets -t http://192.168.119.133
- Executed vulnerability scan via *wmap_run -e*
  <img width="1134" height="413" alt="scan for vuln" src="https://github.com/user-attachments/assets/22a07e23-92ce-4a34-9f61-8f8569ff236b" />

- Reviewed discovered vulnerabilities using wmap_vulns -l
<img width="712" height="373" alt="show vulns" src="https://github.com/user-attachments/assets/6209a5c1-1194-475c-bbbb-cf88b8cdfcbf" />


**Key Findings**
- Web Server: Apache 2.2.8 (Ubuntu) with PHP/5.2.4
<img width="1134" height="413" alt="scan for vuln" src="https://github.com/user-attachments/assets/65717a44-7f4a-4570-8322-1a3a194cdf3a" />

 Reveals exact Apache/PHP versions, allowing attackers to target known exploits for these specific software versions.


- Exposed Directories: /dav/, /doc/, /phpMyAdmin/, /test/, /icons/, /index/
- <img width="712" height="373" alt="show vulns" src="https://github.com/user-attachments/assets/745c37ad-2bdb-47e2-b660-7df5d7307628" />
/phpMyAdmin/ exposure - Allows attackers to attempt database authentication bypass or SQL injection attacks on the MySQL administration interface.
/dav/ WebDAV directory - Could enable unauthorized file uploads, downloads, or modification of web content if misconfigured.
/doc/ directory exposure - May reveal sensitive documentation, configuration files, or application details to attackers.
/test/ directory access - Often contains testing scripts or data that could expose application vulnerabilities or sensitive information.


## Conclusion
- The port scanning and fingerprinting exercises confirmed that Metasploitable exposes multiple open services. The scan successfully identified multiple exposed directories and services, with /phpMyAdmin/ access posing the most significant security risk due to potential database compromise vectors.
