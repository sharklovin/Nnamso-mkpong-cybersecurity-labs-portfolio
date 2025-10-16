## PENETRATION TESTING PLAN 
Do not run any tests without written authorization.

## Target organization: CyberSafe Solutions Inc. - A fictional cybersecurity consulting firm

**Purpose / Objective:** Test the security posture of external web applications and network infrastructure, identify exploitable weaknesses, and provide remediation guidance to improve overall security.

## Authorization:
- Ahman Dickson (CEO, CyberSafe Solutions Inc.) – ahmaddickson1@cybersafesolutions.com
- Maria Lyons (CTO, CyberSafe Solutions Inc.) – maria.lyons002@cybersafesolutions.com
- [Reference: Authorization Letter #CS-PT-2025-001 attached]

## Timing (Testing Window):
- Start: 2025-16-05 22:00 (UTC+01:00)
- End: 2025-16-05 06:00 (UTC+01:00)
- Blackout Windows: Mon-Fri 08:00-18:00 (business hours), system maintenance windows every Sunday 02:00-04:00
- Emergency Stop: Immediate cessation if critical production impact detected

## Scope:
**- In-scope:**
  - IP range: 10.0.1.0/24
  - Web application: portal.cybersafesolutions.com
  - DNS: www.cybersafesolutions.com
    
**- Out-of-scope:**
  - Payment processing system (pay.cybersafesolutions.com)
  - Customer database servers
  - Third-party vendor systems
  - Employee workstations
    
**- Testing Allowed:**
  - External network scanning
  - Web application testing (OWASP Top 10, limited to 5 main pages)
  - Vulnerability assessment
  - NO social engineering
  - NO denial-of-service testing

## Rules of Engagement:
- No destructive testing or data modification
- Tools: Nmap, Nikto, Burp Suite Community, Nessus
- NO Metasploit or exploitation tools
- Daily status reports via email to security-team@cybersafesolutions.com
- Immediate phone escalation for critical findings: +2348062794979
- All data encrypted using PGP key provided
  
## Data handling & Privacy: All findings treated as confidential; encrypted delivery of reports.

## Deliverables:
- Preliminary findings report within 24 hours of critical issue discovery
- Final comprehensive report by 2025-16-05
- Report to include: Executive summary, technical details, CVSS scoring, remediation steps

## Emergency contacts & rollback plan:
- Primary: Maria Lyons - +2348062974979
- Secondary: Ahmad Dickson - +23387654412
- Rollback Plan: Immediate testing cessation and system restoration procedures documented in Appendix A

**Signatures:**
## Tester: Nnamso Mkpong
## Approver: Ahmad Dickson
## Date: 2025-16-05

