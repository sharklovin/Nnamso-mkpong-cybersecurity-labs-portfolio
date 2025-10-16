# Lab 01 — Verify Windows Defender Firewall & Configure File and Printer Sharing
**Category:** CySA+  
**Date:** 16/10/2025  
**Author:** Nnamso Mkpong

---

## Objective
Verify that Windows Defender Firewall is enabled and configure inbound settings to control File and Printer Sharing access.

---

##  Environment / Tools
- Windows Server 2022 (test system)  
- Control Panel → Windows Defender Firewall  
- PowerShell (Get-NetFirewallProfile, Get-NetFirewallRule)

---

## Steps (what I did)
1. Opened **Control Panel → System and Security → Windows Defender Firewall → Check Firewall Status**.  
   → Verified **Private** profile shows **On**.
2. Verified firewall status in PowerShell:
   ```powershell
   Get-NetFirewallProfile | Format-List Name, Enabled, DefaultInboundAction, DefaultOutboundAction

