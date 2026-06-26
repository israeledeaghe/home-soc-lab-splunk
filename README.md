# \# Home SOC Lab using Splunk

# 

# A fully self-built Security Operations Center lab demonstrating end-to-end SIEM

# detection engineering: log pipeline architecture, attack simulation, SPL

# detection authoring, and full incident investigation — built and run on a

# resource-constrained 8GB RAM / dual-core laptop.

# 

# \## Architecture

# 

# \- \*\*SIEM:\*\* Splunk Enterprise (free tier), running directly on the host machine

# \- \*\*Hypervisor:\*\* Oracle VirtualBox 7.2

# \- \*\*Victim host:\*\* Windows 11 Enterprise LTSC Evaluation VM, fully offline/local

# &#x20; account (no Microsoft account, no domain ties)

# \- \*\*Endpoint telemetry:\*\* Sysmon (SwiftOnSecurity config) + Splunk Universal

# &#x20; Forwarder, forwarding Security, System, and Sysmon Operational logs

# \- \*\*Attack simulation:\*\* Atomic Red Team (MITRE ATT\&CK-mapped test execution,

# &#x20; no external C2 infrastructure required)

# \- \*\*Network isolation:\*\* VirtualBox Host-Only network, air-gapped from the

# &#x20; physical LAN during attack simulation

# 

# A second cloned VM (`victim-win11-target`) was used to simulate cross-host

# lateral movement for Detection 3 / Investigation 2.

# 

# \## What's in this repo

# 

# | Folder | Contents |

# |---|---|

# | `/detections` | 9 SPL detection queries, each mapped to a MITRE ATT\&CK technique, documented with severity, validation method, and known limitations/false-positive notes |

# | `/investigations` | 5 full incident investigation reports (Word format), each covering triage, investigation steps, timeline reconstruction, MITRE mapping, root cause, and remediation |

# | `/investigations/screenshots` | Splunk search result evidence for each investigation |

# | `/docs` | Build documentation |

# 

# \## Detections Built

# 

# | # | Technique | MITRE ID | Severity |

# |---|---|---|---|

# | 1 | PowerShell Encoded Command | T1059.001 | Medium |

# | 2 | LSASS Memory Dump via comsvcs.dll | T1003.001 | High |

# | 3 | Lateral Movement via SMB Admin Shares | T1021.002 | High |

# | 4 | Registry Run Key Persistence | T1547.001 | Medium |

# | 5 | Scheduled Task Creation | T1053.005 | Medium |

# | 6 | Process Injection | T1055 | Medium |

# | 7 | Download Cradle / Privilege Escalation | T1134.001 | High |

# | 8 | DNS Exfiltration | T1048 | High |

# | 9 | LOLBin Abuse via Rundll32 | T1218.011 | Medium |

# 

# \## Investigations Written

# 

# 1\. Credential Access via LSASS Memory Dumping

# 2\. Lateral Movement via SMB Administrative Shares

# 3\. Fileless Privilege Escalation via PowerShell Download Cradle (Empire C2)

# 4\. Process Injection Detected via Anomalous Process Lineage

# 5\. Data Exfiltration Attempt via DNS-over-HTTPS Covert Channel

# 

# \## Real Engineering Findings From This Build

# 

# This lab surfaced several genuine, non-obvious technical issues along the

# way — documented here because working through them is the actual point:

# 

# \- Windows applies UAC remote restrictions to local accounts by default,

# &#x20; blocking admin share access even with correct credentials, until

# &#x20; `LocalAccountTokenFilterPolicy` is set

# \- Several Security log audit events (4698, 5140) are \*\*not logged by

# &#x20; default\*\* and require explicit Advanced Audit Policy configuration

# \- Modern Windows 11 builds block the most common process injection

# &#x20; technique (`CreateRemoteThread`) even with Administrator privileges and

# &#x20; Defender disabled — a less common injection primitive was required to

# &#x20; generate valid telemetry

# \- Sysmon's XML-rendered log format does not reliably expose `EventCode` as

# &#x20; a directly searchable Splunk field, requiring raw-text regex extraction

# &#x20; for reliable detection logic across this entire project

# 

# \## Author

# 

# Israel Ehiabhi Edeaghe

