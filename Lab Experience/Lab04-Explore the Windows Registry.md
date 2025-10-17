## Explore the Windows Registry
**Category:** CySA+  
**Date:** 16/10/2025  
**Author: Nnamso Mkpong:**

---

## Objective
To explore the Windows Registry structure, identify key system settings, and understand how security configurations such as password policies are stored in the system registry.

---

## Environment / Tools
- secpol.msc
- gpedit.msc    

---

## Steps (what I did)
## Opened Registry Editor 
**- Launched regedit using the Windows search bar.**
  <img width="749" height="311" alt="Opened Registry Editor" src="https://github.com/user-attachments/assets/37e65f50-d673-4979-b751-1e7ec7cc5256" />
- Gave administrator permission when prompted.

---

## Explored the Main Registry Hives
**- Expanded and reviewed the following hives:**
- HKEY_CLASSES_ROOT
- HKEY_CURRENT_USER
- HKEY_LOCAL_MACHINE
- HKEY_USERS
- HKEY_CURRENT_CONFIG
  <img width="906" height="591" alt="Explored the Main Registry Hives" src="https://github.com/user-attachments/assets/d3e278b7-dc85-4e03-b647-da41d5a07789" />

- Observed how each hive stores system, user, and configuration data.
- No changes were made to avoid impacting system stability.

---

## Located Maximum Password Age Setting
- Navigated to:
- HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SeCEdit\Reg Values\MACHINE/System/CurrentControlSet/Services/Netlogon/Parameters/
- Found and noted the value controlling maximum machine account password age.
- <img width="1001" height="594" alt="Located Maximum Password Age Setting" src="https://github.com/user-attachments/assets/47bfebb9-fa0d-416d-812f-51241553f1a3" />

---

## Viewed Security Policy Setting via secpol.msc
Opened **Local Security Policy (secpol.msc) as Administrator.**<img width="749" height="677" alt="Security Policy" src="https://github.com/user-attachments/assets/9f873bda-6d38-4289-898b-d9ff32eee4df" />

**Navigated to:**
- Security Settings → Local Policies → Security Options → Domain Member: Maximum Machine Account Password Age
- Recorded the value (default: 30 days).
  <img width="860" height="604" alt="Recorded the value " src="https://github.com/user-attachments/assets/177c7c4d-6c7e-4f32-8a6c-9efbd99f6a4a" />

---

## Lastly i observed Policy Change Effect
- Changed the maximum password age in secpol.msc.<img width="858" height="600" alt="Changed the maximum password age in secpol ms" src="https://github.com/user-attachments/assets/6bb8df05-ea41-46c8-98b3-41da00b196c6" />

- Reopened regedit to confirm that the value updated under the same registry path.
- This confirmed the link between Group Policy settings and Registry configurations.

## Reflection
This activity helped me understand how deeply integrated Windows security policies are with the registry.
Even though most users manage security through graphical tools like secpol.msc or gpedit.msc, the registry is where the actual configuration values live. 

Making manual edits here could impact system behavior, so administrative caution is essential.



