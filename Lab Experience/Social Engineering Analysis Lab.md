## Social Engineering Analysis Lab

**Category:** SOC awareness

**Date:** 02/12/2026

**Author:** Nnamso Mkpong


## Lab Objective

The objective of this lab is to investigate a suspected fraudulent job recruitment email using standard Security Operations Center methodologies. The analysis focuses on identifying indicators of compromise, evaluating social engineering tactics, and mapping attacker behavior to the MITRE ATT&CK framework.

This lab simulates a real world SOC investigation scenario in which a user reports a suspicious email for security review.

## Scenario Overview

A user received an unsolicited email claiming to represent a medical technology company offering remote job opportunities. The message instructed the recipient to contact a hiring manager through Microsoft Teams using a personal Outlook email address and included a verification code for the interview process.

Due to multiple suspicious characteristics, the user reported the email to the security team for analysis.

## Investigation Methodology
The investigation followed a structured SOC workflow:

• Evidence collection and review

• Indicator of compromise extraction

• Behavioral threat analysis

• Social engineering assessment

• MITRE ATT and CK mapping

• Risk severity evaluation

• Response recommendations

## Evidence Collection

## Evidence 1: Full Email View
**Screenshot showing the complete email message.**
<img width="1051" height="512" alt="E!" src="https://github.com/user-attachments/assets/a9e801a2-5199-4113-a70a-86c1070727b2" />
<img width="1038" height="501" alt="E2" src="https://github.com/user-attachments/assets/e4326bd7-34d3-4c2b-8055-23b83cc75906" />


Figure 1: Original email containing recruitment message.

## Evidence 2: Suspicious Contact Information 
The email instructs the recipient to contact a hiring manager using a personal Outlook email address.
<img width="1038" height="501" alt="suspicious contact" src="https://github.com/user-attachments/assets/8bdd74df-9c56-405c-a94c-65872f7e2b82" />

`Tiffanycampbell001@outlook.com`

Figure 2: Use of personal email domain instead of official corporate domain.
**Security Concern:** Legitimate organizations use corporate domains for recruitment communications. The use of a free email service strongly indicates impersonation.

## Evidence 3: External Communication Link
The email contains a Microsoft Teams invitation link directing communication outside official corporate channels.
<img width="1038" height="501" alt="suspicous teams link" src="https://github.com/user-attachments/assets/fabb1af4-9648-462c-bdb5-b1c700803e0b" />


Figure 3: External Teams link used to bypass official recruitment channels.

Security Concern: Attackers frequently use external platforms to avoid enterprise security monitoring.

## Evidence 3: Unrealistic Salary Offer
The email advertises a high hourly rate for entry level roles.
<img width="1038" height="501" alt="hourly rate" src="https://github.com/user-attachments/assets/b27bbf37-8621-476e-ab82-f44d93c45a9a" />

`$25.75 per hour`

Security Concern: Unrealistic compensation is a known psychological tactic used in recruitment scams.

## Evidence 5: Eligibility Contradiction
The email states the opportunity is only available to United States citizens despite contacting an international recipient.
<img width="1038" height="501" alt="us citizen" src="https://github.com/user-attachments/assets/3d5abf25-5bba-4536-bc7c-0f9f4fe36d2f" />

Figure 5: Logical inconsistency indicating mass targeting.

Security Concern: Contradictions suggest automated bulk distribution rather than legitimate recruitment.

## Indicator of Compromise Extraction
-------------------------------------------------------------
| Indicator Type       | Indicator Value                     | Description                                         |
|----------------------|--------------------------------------|-----------------------------------------------------|
| Email Domain         | outlook.com                         | Personal email used for hiring communication        |
| External Link        | Teams invitation URL                | Redirects victim to external communication channel  |
| Behavioral Pattern   | Generic recruitment message         | Matches known scam templates                        |
| Process Indicator    | Absence of formal hiring workflow   | No official application process                     |

## Behavioral Threat Indicators

The following behavioral red flags were identified:

• Unsolicited job offer

• Immediate request to move communication to chat platform

• Use of verification codes to simulate legitimacy

• Lack of formal interview process

• Eligibility inconsistencies

## Social Engineering Tactics Identified

**The attacker used multiple persuasion techniques commonly observed in recruitment scams.**

**Authority Impersonation:**

• The email claims affiliation with a legitimate medical technology company to establish credibility.

**Urgency and Direction:**

• The message provides step by step instructions designed to quickly move the recipient into engagement.

**Financial Incentive:**

• High hourly pay is presented to create emotional motivation and reduce skepticism.

**Legitimacy Simulation:**

• Use of professional terminology such as Human Resources Department, structured training, and verification codes to mimic real hiring processes.

## MITRE ATT&CK Mapping

**Initial Access:**

T1566.002 Spearphishing via Service

The attacker initiated contact using an email and attempted to transition communication to an external service.

**Social Engineering Techniques:**

T1598 Phishing for Information

The attacker aimed to gather victim information through a fraudulent recruitment interaction.

**Credential Access Risk:**

T1056.003 Credential Harvesting

Potential future attacker actions may involve requests for personal identity or financial information.

## Risk Assessment

Risk Level: High

**Severity Justification**

This threat is classified as High due to the following factors:

• High probability of user engagement due to financial incentive

• Direct attempt to move communication outside monitored channels

• Strong indicators of impersonation

• Known association with financial fraud patterns

• Potential for identity theft or monetary loss

## Recommended Response Actions

The following actions would be recommended:

• Block sender email address
• Report to organizational phishing awareness program
• Educate users on recruitment scam tactics
• Monitor for similar campaign indicators
• Update threat intelligence database

## Key Learning Outcomes

This lab demonstrates how SOC analysts detect social engineering threats by analyzing behavioral patterns rather than relying solely on technical malware indicators.

**Skills reinforced include:**

• Phishing investigation

• Threat intelligence documentation

• MITRE ATT&CK mapping

• Risk assessment evaluation

• Security awareness analysis

The investigation confirms that the email represents a social engineering attempt designed to exploit job seekers through a fraudulent recruitment process. The attacker relies primarily on psychological manipulation to achieve initial access rather than technical exploitation. Understanding these tactics is essential for SOC analysts because social engineering remains one of the most common entry points for cyber attacks.


