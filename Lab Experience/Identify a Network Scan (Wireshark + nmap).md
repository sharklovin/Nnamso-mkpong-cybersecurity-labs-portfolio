## Review System Hardening Guidelines
**Category:** CySA+  
**Date:** 20/10/2025  
**Author: Nnamso Mkpong**

---

## Objective
Capture and identify a network scan from Kali to a Metasploitable target using Wireshark, compare scan traffic with normal browser traffic, and document findings.

---

## Environment / Tools
- Kali Linux (attacker)
- Metasploitable (target)
- Wireshark and nmap
- Firefox

---

## Steps (what I did)
1. I started up both my Kali Linux and Metasploitable virtual machines and logged into both of them. 
<img width="1163" height="581" alt="Kali  start up" src="https://github.com/user-attachments/assets/2d078e24-0530-426c-9b2b-9cfccc6dd66a" />
<img width="1134" height="570" alt="metaspliot start up" src="https://github.com/user-attachments/assets/6dbedba7-b392-47b7-a38f-c2e673fbf6a7" />

---

2. Determined target IP on Metasploitable: `ifconfig -a` → **192.168.119.133**.
<img width="930" height="383" alt="metasploit ip confirmation" src="https://github.com/user-attachments/assets/c986a2d9-a8a3-4f11-b5b9-3147fa6382a2" />

---

3. Started Wireshark capture on Kali (interface: eth0).
<img width="1164" height="514" alt="opened wire shark on Kali" src="https://github.com/user-attachments/assets/03edeca1-5410-4e02-9881-1fe6af6ccdb3" />

---

4. Ran nmap on Kali:
   - **nmap -p 1-65535 192.168.119.133**

---

5. Noted a random open port: **512** 
<img width="1093" height="519" alt="nmap port scan" src="https://github.com/user-attachments/assets/892676e9-a760-4266-acce-b2741b1c0318" />

---

6. Visited the target web server in Firefox:**http://http://192.168.119.133/**
<img width="1149" height="496" alt="started firefox and navigate to the metaspliot ip" src="https://github.com/user-attachments/assets/8c016793-568b-4f0d-8e8f-929de06cf4a0" />

---

7. Stopped capture and saved.

---

8. Reran an nmap scan using a SYN scan to compare behavior:
   - **nmap -sS -p 1-65535 192.168.119.133**
   <img width="1136" height="552" alt="-sS scan" src="https://github.com/user-attachments/assets/1b77eb01-c220-48ef-8999-725b88097468" />

---

9. In Wireshark, applied a filter for the discovered port: **tcp.port == 512**
<img width="1154" height="515" alt="i searched th opened port 512 and nothing displayed" src="https://github.com/user-attachments/assets/88930950-53ab-4496-970b-ae5ae78a8d82" />

---

10. Reviewed HTTP traffic with this filter: **tcp.port == 80**

---

## Analysis / Observations
- **tcp.port == 512** returned no matches in the capture. That indicates either:
- The port reported by nmap was not used during the exact capture window, or nmap reported it as open but the probe traffic was not captured on the interface selected, or the open service on that port was UDP or otherwise not visible as TCP on that capture.
<img width="1154" height="515" alt="i searched th opened port 512 and nothing displayed" src="https://github.com/user-attachments/assets/b4924f02-6981-457c-b0ba-b9223d7d05a9" />

---

- Repeating the scan with -sS and rechecking **tcp.port == 512** still returned no results in the saved capture.
<img width="1154" height="515" alt="i searched th opened port 512 and nothing displayed" src="https://github.com/user-attachments/assets/8a084941-b71d-4374-944e-cf4f4746893a" />

---

- Port 80 analysis: 
- Wireshark shows a complete TCP 3-way handshake followed by an HTTP GET and a 200 OK response. This indicates the web server served content successfully and the browser visit generated full application-layer traffic (large payloads).
<img width="1154" height="585" alt="Review traffic for port 80" src="https://github.com/user-attachments/assets/43cc7150-e58d-4202-b4ec-ac3834901c8d" />


---


## Scan vs browser traffic differences:

- Scan traffic (nmap) appears as many SYN probes to many destination ports in a short time window. For closed ports you might see RST replies; for open ports you might see SYN/ACK responses.
- Browser traffic follows a full handshake (SYN → SYN/ACK → ACK) and carries HTTP request/response payloads (GET, HTML body).

- Effect of nmap flags:
  - `-sS` (SYN scan) produced half-open probes (SYNs), often followed by RSTs.

## Conclusion

The scan signature is visible as many SYN probes from one source to many destination ports, which is distinguishable from legitimate browser traffic that shows complete handshakes and application-layer data.








