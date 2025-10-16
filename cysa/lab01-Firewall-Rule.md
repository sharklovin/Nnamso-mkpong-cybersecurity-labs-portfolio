# Lab 01 — Verify Windows Defender Firewall & Configure File and Printer Sharing
**Category:** CySA+  
**Date:** 16/10/2025  
**Author:** Nnamso Mkpong

---

## Objective
The goal of this lab was to confirm that Windows Defender Firewall is active on a Windows Server 2022 system and to adjust inbound settings for File and Printer Sharing. This helps control how systems on a private network communicate while limiting exposure to public networks.

---

## Environment / Tools
- Windows Server 2022 (test system)  
- Control Panel → Windows Defender Firewall  
- PowerShell (Get-NetFirewallProfile, Get-NetFirewallRule)

---

## Steps (what I did)
- Opened Control Panel → System and Security → Windows Defender Firewall → Check Firewall Status to confirm the firewall was turned on.
- The Private network profile was active and set to On.
- <img width="1010" height="559" alt="screenshot_01_firewall_status" src="https://github.com/user-attachments/assets/5843556e-963a-4033-8cd4-4cdb502c2caf" />

- Opened PowerShell as Administrator and ran the command below to check each firewall profile’s configuration:
  **Get-NetFirewallProfile | Format-List Name, Enabled, DefaultInboundAction, DefaultOutboundAction**
  
  <img width="962" height="381" alt="screenshot_01_powershell" src="https://github.com/user-attachments/assets/53a827ff-ee8f-42b2-8f99-2ac710a93eac" />

- This helped confirm that inbound connections were blocked by default and outbound ones were allowed.
- Went back to the Control Panel and selected Allow an app or feature through Windows Defender Firewall. 
- Found File and Printer Sharing in the list.
- Made sure the Private box was checked and Public was left unchecked.
  
  <img width="1022" height="592" alt="screenshot_02_allow_app" src="https://github.com/user-attachments/assets/5c5038b3-0a40-4b44-8f49-7acf2faa7bfd" />

- Lastly i saved the changes.

Evidence Collected
---
Screenshot showing the firewall turned on under the Private profile.
Screenshot of the “Allowed apps” window with File and Printer Sharing adjusted.
PowerShell output saved as text files for both commands.

---

Reflection
---
This lab reinforced how important it is to verify firewall settings, especially when enabling features that share files or printers. Restricting these services to a private network reduces the chance of unwanted access from public or external systems. It’s a small configuration step that supports the principle of least privilege in network defense.

---
