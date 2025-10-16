# Lab 02 — Create GPO: Password Policy
**Category:** CySA+  
**Date:** 16/10/2025  
**Author:** Nnamso Mkpong

---

## Objective
Create a Group Policy Object (GPO) to enforce password requirements across the domain: maximum password age, minimum length, and complexity.

---

## Environment / Tools
- Windows Server 2022 (Domain Controller)  
- Group Policy Management Console (gpmc.msc)  
- gpupdate / rsop.msc for verification

---

## Steps (what I did)
1. Opened **Group Policy Management** → expanded my domain → right-clicked **Group Policy Objects** → **New** → named it `Password Policy`.  <img width="1016" height="609" alt="screenshot_03_gpo_list" src="https://github.com/user-attachments/assets/b0ce8108-4866-4bc4-ba02-4867fc4a0b4a" />

2. Right-clicked the new GPO → **Edit**.  
3. Navigated to **Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy**.
4. Configured:
   - **Maximum password age**: Defined = **90 days**  <img width="1014" height="587" alt="screenshot_03_password_policy_settings" src="https://github.com/user-attachments/assets/357c0e9f-f815-4d65-ade0-a27413bdd118" />

   - **Minimum password length**: Defined = **12**  <img width="1002" height="557" alt="screenshot_03_password_policy_settings_Min_passwrd_Length" src="https://github.com/user-attachments/assets/a4ee05f2-84b3-4aa8-952e-3e04afc1541e" />

   - **Password must meet complexity requirements**: **Enabled**<img width="1003" height="574" alt="screenshot_03_password_policy_settings_complexity_requirement" src="https://github.com/user-attachments/assets/7553e116-dca4-4f10-8164-f9e36f4494c0" />

5. Ran `gpupdate /force` on a test client and opened `rsop.msc` to verify the effective settings.<img width="922" height="289" alt="group policy force update" src="https://github.com/user-attachments/assets/53cd1bb5-82b5-403f-a6e7-946e94d33c16" />


---

## Findings / Results
- GPO `Password Policy` created and settings were applied to the test host. `rsop.msc` confirmed MaxAge=90, MinLength=12, Complexity=Enabled.

---

## Key takeaways
- GPOs enforce consistent security settings across the domain and always verify on a client host to confirm user experience.

---

