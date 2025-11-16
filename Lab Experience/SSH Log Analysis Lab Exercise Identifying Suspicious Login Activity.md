## SSH Log Analysis Lab Exercise: Identifying Suspicious Login Activity

**Category:** CySA+  
**Date:** 06/11/2025 
**Author:** Nnamso Mkpong

## Overview 
In this security lab exercise, I performed a structured investigation of SSH authentication logs to identify suspicious login activity. The objective was to detect unauthorized access attempts, determine which authentications were successful, and understand attacker behaviour using basic Linux command-line tools.

## Exercise Objectives
- Download and analyze authentication logs from Elastic’s security examples
- Identify brute-force attempts and successful compromises
- Investigate attacker behaviour across multiple IP addresses
- Understand “preauth” noise vs. actual authenticated sessions
- Document findings and reconstruct the attack pattern
- Recommend appropriate remediation steps

## Tools Used
- Ubuntu
- GitHub
- grep for pattern matching
- awk for data extraction
- Basic Linux CLI utilities
- SSH authentication log analysis


## Step by Step Investigation Process
**Step 1: Accessing the Data Source**
- I began by navigating to Elastic's Machine Learning Security Analytics Recipes repository on GitHub: 

**Step 2: Locating the Exercise Files**
Within the repository, I found the suspicious_login_activity folder containing:
`The auth.log data file for analysis`

<img width="1207" height="703" alt="Github start" src="https://github.com/user-attachments/assets/37f1b351-9299-46cd-a005-2a655383e0f8" />

<img width="1203" height="705" alt="suspicious login activity" src="https://github.com/user-attachments/assets/8ae23ac6-306a-4046-99ac-ed27b69efe8b" />

<img width="1219" height="695" alt="navigate to the data folder" src="https://github.com/user-attachments/assets/eac6f47c-f6f6-44d7-b14d-fe82c60cddfd" />

---

**Step 3: Downloading the Log File**
- I downloaded the auth.log file from the data directory and saved it to my local Ubuntu system's Downloads folder for analysis.
  
<img width="1217" height="700" alt="opening and download auth log" src="https://github.com/user-attachments/assets/a338f026-57d5-4618-838d-d8bde6481f08" />

---

**Step 4: Searching for Connection Patterns:**
- I searched for connection patterns: `grep -i "connection" auth.log`

<img width="1215" height="716" alt="grep" src="https://github.com/user-attachments/assets/59eb3ea2-29af-481d-8db8-d325a49750e2" />
<img width="1224" height="721" alt="port scanning uncertainty" src="https://github.com/user-attachments/assets/110aba53-44a7-4051-ac5f-616a455ebc5c" />


- auth.log output shows a classic pattern of automated SSH scanning and brute-force probing from multiple external IP addresses. None of these connections actually authenticate as they all terminate [preauth], meaning the attacker disconnected before completing the SSH handshake.

*Key Indicators You’re Seeing*

- High-frequency connection attempts hitting the server within seconds.
- Multiple random high ports on the attacker side (e.g. 53756, 58980, 45525).
- Repeating attacker IPs such as 91.197.232.109, 85.245.107.41, 24.151.103.17).
- All ending with “[preauth]”, which means they didn’t get to password or key validation.
- Several attempts hitting non-default SSH ports (222, 2222), which means the attacker is scanning all ports, not just 22.

* To determine what happened, a check for a repeative attacker with the ip `85.245.107.41` was conducted to determined what exaclty happened : `grep -A 3 "85.245.107.41" auth.log | grep -E "Accepted|Failed|authentication"`



<img width="1218" height="714" alt="Check what happened after 8524510741 connections" src="https://github.com/user-attachments/assets/70d8c665-d91f-48da-8dd7-571fa5ae901d" />


- Repeated successful public-key logins to the ubuntu account
These appear frequently over multiple days, always from the same IP. The pattern suggests that whoever controls 85.245.107.41 has a valid SSH key for the ubuntu user and is logging in consistently.

---

- Brute-force–style password login attempts on multiple “elastic_user_X” accounts

<img width="1297" height="668" alt="passowrd" src="https://github.com/user-attachments/assets/4e79a49c-170b-4022-99df-285d03acefbe" />
<img width="1298" height="688" alt="password 3" src="https://github.com/user-attachments/assets/cb794efd-a734-44a8-86bf-d4fe1c2795c6" />
<img width="1301" height="684" alt="passowrd 4" src="https://github.com/user-attachments/assets/eba1fffc-c859-4945-b54a-ca1523562bde" />


- The same IP repeatedly tries various numbered accounts. Many attempts fail with: `Failed publickey, pam_unix(...) authentication failure, Failed password`
But several succeed:
- Accepted password for elastic_user_X from 85.245.107.41
- This behaviour looks like an automated script cycling through usernames and passwords until it finds valid combos.


---
Evidence of two attacker IPs (not one)

Aside from 85.245.107.41, there are entries from 24.151.103.17, also successfully authenticating users like:

<img width="1203" height="355" alt="elastic 7" src="https://github.com/user-attachments/assets/7bcea3cd-34fc-47ff-96ca-38fbbca468c5" />

- elastic_user_0
- elastic_user_7
- elastic_user_6

This shows that multiple external hosts have credentials or have brute-forced access.


---
*Summary*

- Persistent public-key access into the ubuntu account from 85.245.107.41.
- Large-scale password attacks on multiple elastic_user_X accounts.
- Multiple successful logins, suggesting account compromise.
- Activity coming from two external IP addresses, not one.
- Each SSHD PID represents a distinct connection that you can annotate as either successful, failed, or brute-force.



---

Since i found 2 compromised IPs, there could be a lot more so digged deeper.
Using the grep `grep "Accepted" auth.log | awk '{print $11}' | sort -u` I Looked for ALL IPs that had successful logins

<img width="1211" height="135" alt="Find ALL IPs that had successful logins" src="https://github.com/user-attachments/assets/550b5fb2-561f-4e3b-84ae-4b5b2479abb4" />

- It confirmed 2 malicious IPs with successful unauthorized access which we found out ealier (85.245.107.41, 24.151.103.17 )
- Also 1 new unknown IP (95.93.96.191) and 1 local IP (127.0.0.1)

- Unsure if the new unknown ip 95.93.96.191 could be another attacker, or potentially a legitimate user. 
- I decided to run checks  on 95.93.96.191

*Analysis of 95.93.96.191:*
 <img width="1210" height="266" alt="Detailed analysis of 95 93 96 191" src="https://github.com/user-attachments/assets/74d4a6b3-b454-4fda-bfa6-95c4c1d001d7" />


IP 95.93.96.191 is not scanning or brute forcing as it is successfully logging in.
The logs show multiple password-based logins to elastic_user_X accounts and a public-key login to the ubuntu account. Each line has a distinct SSHD PID indicating a separate authenticated session. The presence of accepted-password and accepted-publickey messages confirms that the attacker has valid credentials or has previously compromised the server.


---

Out of curiosity, i ran checks on the local host IP (127.0.0.1)
<img width="1286" height="247" alt="127" src="https://github.com/user-attachments/assets/3768a89e-c604-453b-8eb5-dad69182ad7b" />


*Analysis of 127.0.0.1: 
- Attacker gained shell access externally first
- The compromised system initiated SSH connections back into itself via localhost, indicating internal pivoting, tunneling, or automated credential testing.
- Successfully accessed elastic_user_7 account internally
- Quick session suggests automated tool or data exfiltration
- The compromised system initiated SSH connections back into itself via localhost, indicating internal pivoting, tunneling, or automated credential testing.



## Final Exercise Summary
*What occurred with connections to 10.77.20.248?*
- External brute force compromising multiple accounts
- SSH key theft enabling persistent access
- Coordinated campaign across 3+ external IPs + internal localhost


## Are these connections suspicious, and why?
- Yes highly suspicious

*Why it’s suspicious:*
- Multiple successful logins from the same external IP into different accounts
- Use of both password authentication and public key authentication
- Localhost SSH activity consistent with tunneling, scripts, or automated post-exploitation
- Rapid connect→authenticate→disconnect patterns
- Activity over multiple days (indicating persistence)

## How would you determine what occurred once the connection was made?
*What you i confirm from logs:*

- External accounts were compromised
- Successful authentication into multiple users
- Localhost session creation
- Shell sessions were opened

## What other information do these log entries provide?

- Timeline of external and internal SSH activity
- Authentication methods (password + publickey)
- Repeated access to multiple user accounts
- Shell session creation and termination
- Potential use of automated tools (based on timing)


## Recommended Remediation

1. Isolate the host
-	Prevent further unauthorized access.

2. Rotate all credentials
-	Both passwords and SSH keys.

3. Perform full forensic analysis
-	Check logs, processes, binaries, and user activity.

4. Rebuild the system if compromise is deep
-	Often the safest option.

5. Implement hardening
-	Disable password-based SSH authentication
-	Enforce MFA or hardware-backed keys
-	Restrict SSH by IP allowlists
-	Monitor for brute-force attempts

## Conclusion
This exercise successfully identified a sophisticated multi-vector attack campaign with evidence of credential theft, brute force attacks, and internal lateral movement. The analysis demonstrated how basic log examination tools can reveal complex security breaches when used systematically.
The investigation highlighted the importance of regular security monitoring, proper credential management, and the need for defense in depth strategies to detect and prevent similar attacks in production environments.

## Lessons Learned
- Multiple attack vectors can originate from the same threat actor
- Stolen credentials can be reused across different geographical locations
- Internal lateral movement is a key indicator of successful compromise
- Regular log analysis is crucial for early breach detection
- Comprehensive security requires both prevention and detection mechanisms



