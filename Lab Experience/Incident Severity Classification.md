## Incident Severity Classification
**Category:** CySA+  
**Date:** 06/11/2025 
**Author:** Nnamso Mkpong


## Scenario

You are the leader of a cybersecurity incident response team for a large company experiencing a Denial-of-Service (DoS) attack on its e-commerce website.
The attack is preventing customers from making purchases, causing an estimated $2 million in daily revenue loss.
Internal mitigation efforts have been exhausted, and external assistance is being considered.

---

## Scope
This lab focuses on analyzing the severity of a denial-of-service incident affecting the organization’s online sales systems.  No active testing or data collection was performed.

---

## Incident Classification

- **Functional Impact:** High
- **Justification:** The primary business function which is the online product sales is completely disrupted. Customers cannot make purchases, halting core operations and damaging the company’s reputation.

- **Economic Impact:** High
-**Justification:** The organization faces at least $2 million in losses per day, with potential long-term effects on customer trust and brand value. This qualifies as a severe economic impact.

-**Recoverability Effort:** Extended
-**Justification:** The attack originates from multiple distributed sources, overwhelming in-house mitigation capabilities. Also with the need for an external partner indicates that the recovery process will not be quick or simple. 

-**Information Impact:** Low
-**Justification:** There is no evidence of data theft or alteration as this is a pure availability attack targeting service uptime rather than data confidentiality or integrity. So at this point there is no customer data, financial information, or proprietary data has been compromised.

## Overall Incident Severity

**Classification:** High / Critical Severity Incident

---


## Reason Behind the Overall Incident Severity
This incident has a severe functional and financial impact on the organization, completely halting business operations and requiring external support for recovery. While information integrity remains intact, the combination of lost revenue, service downtime, and complex remediation makes this a `Category 1 – Critical Incident under standard incident response frameworks.`


---


## Possible Recommended Actions
- Engage a DDoS mitigation service provider such as `Cloudflare, Akamai, AWS Shield `

- Develop a DDoS response playbook and integrate it into the organization’s Incident Response Plan (IRP).

- Conduct post-incident review and update business continuity strategies.