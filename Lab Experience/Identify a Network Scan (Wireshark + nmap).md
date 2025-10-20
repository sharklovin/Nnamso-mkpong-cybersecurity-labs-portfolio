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
1. Determined target IP on Metasploitable: `ifconfig -a` → **192.168.119.133**.
2. Started Wireshark capture on Kali (interface: eth0).
3. Ran nmap on Kali:
   - `nmap -p 1-65535 192.168.119.133
4. Noted a random open port: **512** 
5. Visited the target web server in Firefox:`http://http://192.168.119.133/
6. Stopped capture and saved.
7. Reran an nmap scan using a SYN scan to compare behavior:
   - `nmap -sS -p 1-65535 192.168.119.133
8. In Wireshark, applied a filter for the discovered port: `tcp.port == 512`
9. Reviewed HTTP traffic with this filter: `tcp.port == 80`

---

## Analysis / Observations
- `tcp.port == 512` returned no matches in the capture. That indicates either:
- The port reported by nmap was not used during the exact capture window, or nmap reported it as open but the probe traffic was not captured on the interface selected, or the open service on that port was UDP or otherwise not visible as TCP on that capture.
- Repeating the scan with -sS and rechecking `tcp.port == 512` still returned no results in the saved capture.

- Port 80 analysis: 
- Wireshark shows a complete TCP 3-way handshake followed by an HTTP GET and a 200 OK response. This indicates the web server served content successfully and the browser visit generated full application-layer traffic (large payloads).

---


## Scan vs browser traffic differences:

- Scan traffic (nmap) appears as many SYN probes to many destination ports in a short time window. For closed ports you might see RST replies; for open ports you might see SYN/ACK responses.
- Browser traffic follows a full handshake (SYN → SYN/ACK → ACK) and carries HTTP request/response payloads (GET, HTML body).

- Effect of nmap flags:
  - `-sS` (SYN scan) produced half-open probes (SYNs), often followed by RSTs.

## Conclusion
The scan signature is visible as many SYN probes from one source to many destination ports, which is distinguishable from legitimate browser traffic that shows complete handshakes and application-layer data.