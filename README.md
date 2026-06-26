# Home SOC Lab using Splunk

A fully self-built Security Operations Center lab demonstrating end-to-end SIEM detection engineering: log pipeline architecture, attack simulation, SPL detection authoring, and full incident investigation. Built and run on a resource-constrained 8GB RAM, dual-core laptop.

## Architecture

- SIEM: Splunk Enterprise (free tier), running directly on the host machine
- Hypervisor: Oracle VirtualBox 7.2
- Victim host: Windows 11 Enterprise LTSC Evaluation VM, fully offline, local account only
- Endpoint telemetry: Sysmon (SwiftOnSecurity config) plus Splunk Universal Forwarder, sending Security, System, and Sysmon logs
- Attack simulation: Atomic Red Team, mapped to MITRE ATT&CK techniques
- Network isolation: VirtualBox Host-Only network, air-gapped from the physical LAN during testing

A second cloned VM was used to simulate cross-host lateral movement.

## What's in this repo

- detections: 9 SPL detection queries, each mapped to a MITRE ATT&CK technique
- investigations: 5 full incident investigation reports
- investigations/screenshots: Splunk evidence for each investigation
- docs: build documentation

## Detections Built

1. PowerShell Encoded Command - T1059.001 - Medium
2. LSASS Memory Dump via comsvcs.dll - T1003.001 - High
3. Lateral Movement via SMB Admin Shares - T1021.002 - High
4. Registry Run Key Persistence - T1547.001 - Medium
5. Scheduled Task Creation - T1053.005 - Medium
6. Process Injection - T1055 - Medium
7. Download Cradle / Privilege Escalation - T1134.001 - High
8. DNS Exfiltration - T1048 - High
9. LOLBin Abuse via Rundll32 - T1218.011 - Medium

## Investigations Written

1. Credential Access via LSASS Memory Dumping
2. Lateral Movement via SMB Administrative Shares
3. Fileless Privilege Escalation via PowerShell Download Cradle (Empire C2)
4. Process Injection Detected via Anomalous Process Lineage
5. Data Exfiltration Attempt via DNS-over-HTTPS Covert Channel

## Real Engineering Findings From This Build

- Windows applies UAC remote restrictions to local accounts by default, blocking admin share access even with correct credentials, until LocalAccountTokenFilterPolicy is set
- Security log audit events 4698 and 5140 are not logged by default and require explicit Advanced Audit Policy configuration
- Modern Windows 11 builds block the most common process injection technique (CreateRemoteThread) even with Administrator privileges and Defender disabled
- Sysmon's XML-rendered log format does not reliably expose EventCode as a searchable Splunk field, requiring raw text extraction for reliable detection logic

## Author

Israel Ehiabhi Edeaghe