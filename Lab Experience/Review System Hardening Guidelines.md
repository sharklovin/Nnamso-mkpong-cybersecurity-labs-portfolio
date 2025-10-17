## Review System Hardening Guidelines
**Category:** CySA+  
**Date:** 17/10/2025  
**Author: Nnamso Mkpong**

---

## Objective
To explore the Center for Internet Security (CIS) benchmarks for Windows 11 and understand how security standards guide system configuration and hardening.

---

## Environment / Tools
- CIS Benchmark PDF  

---

## Steps (what I did)
**Accessed CIS Benchmarks**
- Visited [cisecurity.org/cis-benchmarks](https://www.cisecurity.org/insights/webinar/effective-implementation-of-the-cis-benchmarks-and-cis-controls)
- Signed up and downloaded CIS_Microsoft_Windows_11_Stand-alone_Benchmark_v2.0.0 PDF
  <img width="1277" height="585" alt="Signed up and downloaded the Windows 11 Stand-alone Benchmark PDF" src="https://github.com/user-attachments/assets/a833426e-afa1-4885-9347-50f2566dd42f" />

---

## Reviewed Benchmark Structure
- Opened the document and examined the index to understand how it’s organized by system area (Account Policies, Network Settings, Services, etc.).

---

## Examined Account Lockout Policy (Page 32)
- I read through the rules for account and password policies.
  It really showed me that you have to balance security with usability. If the rules are too strict, they annoy users. But if they are too lenient, they make brute force attacks easy.
  In a real company, I would suggest a moderate setting that fits the specific threats the business actually faces.

---

## Reviewed Wi-Fi and Network Sharing Settings
- I spent some time reviewing the policies that control automatic connections to open or shared Wi Fi networks. The main thing I learned was understanding the serious security risk of letting a device connect to untrusted networks on its own. It is a major vulnerability that could easily lead to an attack.
  For that reason, I would strongly recommend turning off automatic connections. This is especially important in a corporate setting or any environment that handles sensitive information.
  I also explored several other parts of the benchmark to get a broader understanding. I looked into settings for Remote Desktop, Audit Policy, and Password History. Seeing how each of these works helped me understand how they all play a part in reducing a system's overall attack surface. Each one is a different layer of defense that makes the entire system much more secure.

---

## Reflection

This exercise provided real-world insight into how security standards like CIS benchmarks guide administrators in securing systems. The document showed not only what to change but also why, which helps justify configuration decisions when working with management or compliance teams.

