## SSH Log Analysis Lab Exercise: Identifying Suspicious Login Activity

**Category:** CySA+  
**Date:** 06/11/2025 
**Author:** Nnamso Mkpong

## Overview 
- In this security lab exercise, I performed a structured investigation of SSH authentication logs to identify suspicious login activity. The objective was to detect unauthorized access attempts, determine which authentications were successful, and understand attacker behaviour using basic Linux command-line tools.

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
`https://github.com/elastic/examples/tree/master/Machine%20Learning/Security%20Analytics%20Recipes`

**Step 2: Locating the Exercise Files**
Within the repository, I found the suspicious_login_activity folder containing:
`The auth.log data file for analysis`


**Step 3: Downloading the Log File**
- I downloaded the auth.log file from the data directory and saved it to my local Ubuntu system's Downloads folder for analysis.

**Step 4: Searching for Connection Patterns:
- I searched for connection patterns: `grep -i "connection" auth.log`
(Add pics))

- auth.log output shows a classic pattern of automated SSH scanning and brute-force probing from multiple external IP addresses. None of these connections actually authenticate as they all terminate [preauth], meaning the attacker disconnected before completing the SSH handshake.

*Key Indicators You’re Seeing*

- High-frequency connection attempts hitting the server within seconds.
- Multiple random high ports on the attacker side (e.g. 53756, 58980, 45525).
- Repeating attacker IPs such as 91.197.232.109, 85.245.107.41, 24.151.103.17).
- All ending with “[preauth]”, which means they didn’t get to password or key validation.
- Several attempts hitting non-default SSH ports (222, 2222), which means the attacker is scanning all ports, not just 22.

* To determine what happened, a check for a repeative attacker with the ip `85.245.107.41` was conducted to determined what exaclty happened : `grep -A 3 "85.245.107.41" auth.log | grep -E "Accepted|Failed|authentication"`
(add pics Check what happened after 8524510741 connections 1,2,3)

-Repeated successful public-key logins to the ubuntu account
These appear frequently over multiple days, always from the same IP. The pattern suggests that whoever controls 85.245.107.41 has a valid SSH key for the ubuntu user and is logging in consistently.

---

- Brute-force–style password login attempts on multiple “elastic_user_X” accounts
(Add pics password 1,3)

- The same IP repeatedly tries various numbered accounts. Many attempts fail with: `Failed publickey, pam_unix(...) authentication failure, Failed password`
But several succeed:
- Accepted password for elastic_user_X from 85.245.107.41
- This behaviour looks like an automated script cycling through usernames and passwords until it finds valid combos.


---
Evidence of two attacker IPs (not one)

Aside from 85.245.107.41, there are entries from 24.151.103.17, also successfully authenticating users like:

(Add elatic pic 7, )
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

(Add pics)
- It confirmed 2 malicious IPs with successful unauthorized access which we found out ealier (85.245.107.41, 24.151.103.17 )
- Also 1 new unknown IP (95.93.96.191) and 1 local IP (127.0.0.1)

- Unsure if the new unknown ip 95.93.96.191 could be another attacker, or potentially a legitimate user. 
- I decided to run checks  on 95.93.96.191

*Analysis of 95.93.96.191:*
(add pics Detailed analysis of 95.93.96.191) 

IP 95.93.96.191 is not scanning or brute forcing as it is successfully logging in.
The logs show multiple password-based logins to elastic_user_X accounts and a public-key login to the ubuntu account. Each line has a distinct SSHD PID indicating a separate authenticated session. The presence of accepted-password and accepted-publickey messages confirms that the attacker has valid credentials or has previously compromised the server.


---

Out of curiosity, i ran checks on the local host IP (127.0.0.1)
(add pics)

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
