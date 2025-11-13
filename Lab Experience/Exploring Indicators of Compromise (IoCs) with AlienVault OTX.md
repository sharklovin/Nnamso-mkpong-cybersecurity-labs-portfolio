## Exploring Indicators of Compromise (IoCs) with AlienVault OTX

**Category:** CySA+  
**Date:** 06/11/2025 
**Author:** Nnamso Mkpong


## Objective
The goal of this lab was to gain practical experience using AlienVault’s Open Threat Exchange (OTX), a community-driven threat intelligence platform. The exercise focused on understanding pulses, indicators of compromise (IoCs), and malware families, and how these can strengthen an organization’s overall security posture.


##Overview
This lab provided a hands-on look at how security analysts can use shared threat intelligence for proactive defense. By exploring OTX, I learned how to analyze community “pulses,” investigate IoCs, and study malware behaviors just as a real-world SOC analyst would when tracking and mitigating threats.


## Tools Used
`AlienVault OTX (Open Threat Exchange)`

## Procedures and Observations
##Step 1: Account Setup and Dashboard Familiarization**
(add pics)
- Created an account on `https://otx.alienvault.com` and logged in.

- Reviewed the main dashboard, which summarizes global threat activity and community-shared data known as pulses.

##Step 2: Analyzing Community Pulses**
(add pics)
- Navigated to the “Browse” section to explore publicly available pulses.

- Opened the pulse titled “CiscoASA → Attacker IPs – Australia – November 2025.”

* Key Findings:

- The pulse listed several IPv4 addresses identified as malicious through honeypot data.

- Each indicator included metadata such as the date added, related pulses, and number of detections.

* Analysis:

- Practical use: The IPs can be blocklisted at the firewall or monitored through intrusion detection systems to prevent potential attacks.

- Why subscribe: Subscribing to trusted pulses ensures automatic updates whenever new IoCs are published, keeping defenses current.

- Assessing author credibility: Reputation can be judged based on the author’s organization, level of detail in their submissions, number of followers, and overall community reputation.

## Step 3: Investigating Indicators of Compromise (IoCs)
(add pics)
- Opened the “Indicators” tab to explore different IoC types.

- Domain explored: lombard-ulushin.kz — analysis revealed its IP address, hosting country (Kazakhstan), name servers, and HTTP scan data.

- IPv4 explored: 141.98.83.188 — associated with ASN AS209588, and tagged with keywords like tpot and honeypot.

**Key Insights:

**Useful IoC types:

- IPv4 / Domain: For blocking or monitoring malicious traffic.

- File Hash (MD5/SHA1): Useful in EDR or antivirus systems for detecting known malware.

- Email: Helps filter phishing attempts and malicious senders.

- Additional useful info: Historical data (first/last seen), linked malware families, and confidence ratings from multiple intelligence sources help prioritize real threats.

## Step 4: Examining Malware Families

- Explored the malware visualization dashboard showing related families (e.g., Gamason, Berbzov).

- Selected one major family to analyze.

**Key Observations:

- The malware page detailed its technical features, such as:

- “Unknown PE section name”

- “Contains PE overlay”

- “Accesses Recycle Bin”

- The Related Pulses section showed other analysts’ research tied to the same malware family.

**Analysis:**

**What i learn:** Technical behavior and indicators that can be turned into YARA or Sigma detection rules.

**Why related pulses matter:** They provide real-world context, showing how often and where the malware has been observed.

**Practical use:** These behaviors can inform SIEM queries, improve EDR detection logic, or guide proactive threat hunting.



## Conclusion

This lab provided practical experience in leveraging an open-source threat intelligence platform. By learning how to navigate OTX, interpret IoCs, and analyze malware characteristics, I gained a clearer understanding of how threat intelligence supports modern cybersecurity operations. The exercise demonstrated how community-shared data can turn raw indicators into actionable defense strategies.