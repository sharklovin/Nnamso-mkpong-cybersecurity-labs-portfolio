Lab 01 – Verifying Windows Defender Firewall and Configuring File and Printer Sharing

Category: CySA+
Date: October 16, 2025
Author: Nnamso Mkpong

Objective

The goal of this lab was to confirm that Windows Defender Firewall is active on a Windows Server 2022 system and to adjust inbound settings for File and Printer Sharing. This helps control how systems on a private network communicate while limiting exposure to public networks.

Environment and Tools Used

Windows Server 2022 (test system)

Control Panel (Windows Defender Firewall settings)

PowerShell commands:

Get-NetFirewallProfile

Get-NetFirewallRule

What I Did

Opened Control Panel → System and Security → Windows Defender Firewall → Check Firewall Status to confirm the firewall was turned on.

The Private network profile was active and set to On.

Opened PowerShell as Administrator and ran the command below to check each firewall profile’s configuration:

Get-NetFirewallProfile | Format-List Name, Enabled, DefaultInboundAction, DefaultOutboundAction


This helped confirm that inbound connections were blocked by default and outbound ones were allowed.

Went back to the Control Panel and selected Allow an app or feature through Windows Defender Firewall.

Found File and Printer Sharing in the list.

Made sure the Private box was checked and Public was left unchecked.

Saved the changes.

Verified the settings again using PowerShell:

Get-NetFirewallRule -DisplayGroup "File and Printer Sharing" | Select-Object DisplayName, Enabled, Profile


The rule showed that File and Printer Sharing was allowed only on the Private profile, matching the expected result.

Evidence Collected

Screenshot showing the firewall turned on under the Private profile.

Screenshot of the “Allowed apps” window with File and Printer Sharing adjusted.

PowerShell output saved as text files for both commands.

Reflection

This lab reinforced how important it is to verify firewall settings, especially when enabling features that share files or printers. Restricting these services to a private network reduces the chance of unwanted access from public or external systems. It’s a small configuration step that supports the principle of least privilege in network defense.
