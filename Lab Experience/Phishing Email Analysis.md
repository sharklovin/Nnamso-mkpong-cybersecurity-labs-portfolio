# Phishing Email Analysis — "Opensea" impersonation
**Category:** CySA+  
**Date:** 12/10/2025 (email received)  
**Author:** Nnamso Mkpong

---

## Summary
Received an email with display name "Opensea" sent from `emails.dynamitestyle.com`. The message appears in the spam folder and contains tracking looking addresses in the Reply-To header. Initial header checks and link reputation checks were performed without clicking any links.

---

## Header Observations
<img width="488" height="237" alt="email head summary" src="https://github.com/user-attachments/assets/296d56aa-dfd4-4cf3-9524-ba7969a12d8a" />

**Observations**
- Display name (Opensea) does not match sending domain.
  <img width="488" height="237" alt="email head summary" src="https://github.com/user-attachments/assets/baa2fc52-0680-42a8-b6a7-236f373b72f9" />

- Message is likely from a third-party mailing service or a spoofed sender.  
- Transport encryption present but no indication of legitimate Opensea origin.

---

## Header analysis
- Display name shows it is from *Opensea* but email originates from `emails.dynamitestyle.com`.
- Legit senders generally use their corporate domains (e.g., `@opensea.io`).
- I checked my old emails from when I signed up for OpenSea and found their real website address.
  <img width="533" height="334" alt="real doman email" src="https://github.com/user-attachments/assets/a5aba76f-73cd-4275-a1d0-0adb552c7160" />

- **Conclusion:** The email address did not match. High suspicion of impersonation.

---
### 2. Reply-To and tracking style address 
<img width="484" height="233" alt="reply to" src="https://github.com/user-attachments/assets/d8f2220b-0977-4dfb-b9ff-1459031c9f7b" />


- Reply-To contains a long, tracking-looking address on the same suspicious domain.  
- Reply-To may route responses to attacker-managed inboxes and is commonly used by phishing campaigns to capture replies.  
- What i did was to keep the replies blocked and did not interact.

---

## Body and Link analysis 
<img width="395" height="502" alt="body of text" src="https://github.com/user-attachments/assets/b4e2d69b-3776-4d4f-bb70-1c78a83e0900" />

- The body content exhibits all classic social engineering patterns. Psychological Manipulation like 
  False Urgency: "Hurry", "won't last forever", "limited-time opportunity"

- I proceeded with a deeper investigation by extracting the actual embedded link from the email. I did this by opening the specific email, clicked the three vertical dots in the top-right corner, and selected "Show original" to access the raw email source code.
<img width="488" height="237" alt="email head summary" src="https://github.com/user-attachments/assets/40c85ead-6db9-466e-a1a3-964daabb3c63" />



-   I then located the body of the email and copied the URL hidden behind the "Access Now" button. Finally, I submitted this link to Virus Total for analysis, which confirmed the domain was malicious and flagged by a security vendor.
  <img width="1329" height="515" alt="score" src="https://github.com/user-attachments/assets/0c5f614d-05cf-45f9-9443-e277da7595fd" />



---

## Findings / Conclusion
- The email 'emails.dynamitestyle.com' is suspicious for impersonation and should be treated as phishing.  
- Recommended steps: mark as phishing, block sender domain in mail filters.
- No evidence of successful compromise has been found at this time.





