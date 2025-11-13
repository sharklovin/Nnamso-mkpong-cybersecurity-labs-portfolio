## Exploring Indicators of Compromise (IoCs) with AlienVault OTX

**Category:** CySA+  
**Date:** 06/11/2025 
**Author:** Nnamso Mkpong


## Objective
The goal of this lab was to gain practical experience using AlienVault’s Open Threat Exchange (OTX), a community-driven threat intelligence platform. The exercise focused on understanding pulses, indicators of compromise (IoCs), and malware families, and how these can strengthen an organization’s overall security posture.


## Overview
This lab provided a hands-on look at how security analysts can use shared threat intelligence for proactive defense. By exploring OTX, I learned how to analyze community “pulses,” investigate IoCs, and study malware behaviors just as a real-world SOC analyst would when tracking and mitigating threats.


## Tools Used
`AlienVault OTX (Open Threat Exchange)`

## Procedures and Observations
## Step 1: Account Setup and Dashboard Familiarization
<img width="1357" height="611" alt="Logging in Alien Vault" src="https://github.com/user-attachments/assets/35076d4a-1165-4b55-82ba-3afc19577482" />

- Created an account on `https://otx.alienvault.com` and logged in.

- Reviewed the main dashboard, which summarizes global threat activity and community-shared data known as pulses.

---

## Step 2: Analyzing Community Pulses
**- Navigated to the “Browse” section to explore publicly available pulses.**

<img width="1358" height="605" alt="SELECT BROWSE" src="https://github.com/user-attachments/assets/14fafb9c-cef1-4f24-bc47-54357c3b2732" />


---

**Opened the pulse titled “CiscoASA → Attacker IPs – Australia – November 2025.”**

<img width="1365" height="678" alt="SELECT CISCO" src="https://github.com/user-attachments/assets/5b4dc024-7dd7-49f9-8f32-b0f5faa5dc3f" />


<img width="1356" height="689" alt="cisco asa1" src="https://github.com/user-attachments/assets/04ad86ee-97e3-462b-ba7c-0774e45c0511" />
<img width="1365" height="520" alt="cisco asa2" src="https://github.com/user-attachments/assets/b8706fb4-f642-4d71-8001-88d0d7700589" />


*Key Findings:*

- The pulse listed several IPv4 addresses identified as malicious through honeypot data.

- Each indicator included metadata such as the date added, related pulses, and number of detections.

*Analysis:*

- Practical use: The IPs can be blocklisted at the firewall or monitored through intrusion detection systems to prevent potential attacks.

- Why subscribe: Subscribing to trusted pulses ensures automatic updates whenever new IoCs are published, keeping defenses current.

- Assessing author credibility: Reputation can be judged based on the author’s organization, level of detail in their submissions, number of followers, and overall community reputation.

---

## Step 3: Investigating Indicators of Compromise (IoCs)

**Opened the “Indicators” tab to explore different IoC types.**

<img width="1365" height="609" alt="i went back to browse and selected Indicators" src="https://github.com/user-attachments/assets/9d48e07f-2908-42c5-9fab-8b9122d09a9a" />

---
**Domain explored: lombard-ulushin.kz — analysis revealed its IP address, hosting country (Kazakhstan), name servers, and HTTP scan data.**

<img width="1365" height="648" alt="Selected DOmain in Indicators" src="https://github.com/user-attachments/assets/8a7c6026-edd3-4d0c-9e5e-b848405e8304" />

---

**IPv4 explored: 141.98.83.188 — associated with ASN AS209588, and tagged with keywords like tpot and honeypot.**

<img width="1364" height="658" alt="LOMBARD DOMAIN 1" src="https://github.com/user-attachments/assets/2cde9d78-4113-4e47-a809-57d64db4eade" /><img width="1363" height="678" alt="LOMBARD DOMAIN 2" src="https://github.com/user-attachments/assets/62f36045-cecf-450d-b2b6-00cc100d5562" /><img width="1365" height="666" alt="LOMBARD DOMAIN 3" src="https://github.com/user-attachments/assets/b3f1cae2-3fcd-4ebe-a288-6e44c083eb18" /><img width="1363" height="577" alt="LOMBARD DOMAIN 4" src="https://github.com/user-attachments/assets/20dfc392-8fb1-4228-8c96-c95633aa28e6" /><img width="1364" height="510" alt="LOMBARD DOMAIN 5" src="https://github.com/user-attachments/assets/00b9c976-b134-4874-80ea-ba51c0e48512" /><img width="1360" height="663" alt="LOMBARD 6" src="https://github.com/user-attachments/assets/5073de45-b770-4a91-afd9-08f01036b0cf" />




**Key Insights:**
*Useful IoC types:*

- IPv4 / Domain: For blocking or monitoring malicious traffic.

- File Hash (MD5/SHA1): Useful in EDR or antivirus systems for detecting known malware.

- Email: Helps filter phishing attempts and malicious senders.

- Additional useful info: Historical data (first/last seen), linked malware families, and confidence ratings from multiple intelligence sources help prioritize real threats.

---

## Step 4: Examining Malware Families
<img width="692" height="665" alt="SELECTING G3" src="https://github.com/user-attachments/assets/c666b0ce-c29e-481a-acb5-93b3b85cfd83" />

- Explored the malware visualization dashboard showing related families.

- Selected one major family to analyze which was `G3nasom`

**Key Observations:**

<img width="677" height="714" alt="FEATURES OF F3" src="https://github.com/user-attachments/assets/4b809317-d8e4-4c8c-89c5-486112dcd6e2" />

- The malware page detailed its technical features, such as:

- “Unknown PE section name”

- “Contains PE overlay”

- “Accesses Recycle Bin”

<img width="1363" height="716" alt="G3 RELATED PULSES" src="https://github.com/user-attachments/assets/de7e9a68-b7d3-4ea5-bc7e-2b3962f415ec" />

- The Related Pulses section showed other analysts’ research tied to the same malware family.

**Analysis:**

**What i learn:** Technical behavior and indicators that can be turned into YARA or Sigma detection rules.

**Why related pulses matter:** They provide real-world context, showing how often and where the malware has been observed.

**Practical use:** These behaviors can inform SIEM queries, improve EDR detection logic, or guide proactive threat hunting.



## Conclusion


This lab provided practical experience in leveraging an open-source threat intelligence platform. By learning how to navigate OTX, interpret IoCs, and analyze malware characteristics, I gained a clearer understanding of how threat intelligence supports modern cybersecurity operations. The exercise demonstrated how community-shared data can turn raw indicators into actionable defense strategies.









