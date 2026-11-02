# TryHackMe Writeups and Investigations

A documented collection of SOC-focused TryHackMe room writeups and incident investigations, covering log analysis, threat detection, incident response, and security tooling. Each writeup details the methodology, the tools used and the key takeaways, to demonstrate practical, hands-on cybersecurity skills.


## Investigations

Investigations work through evidence from start to finish without room instructions, producing an incident report with a timeline, MITRE ATT&CK mapping, indicators of compromise and recommendations.

| Investigation | Evidence | Techniques |
|---------------|----------|------------|
| [SSH Brute Force to Root Backdoor](Investigations/ssh-brute-force-to-backdoor.md) | Linux `auth.log` (simulated) | Password spraying, valid accounts, sudo abuse, setuid binary, backdoor account, reverse shell |


## SOC Core Rooms

| Room | Difficulty | Topics |
|------|------------|--------|
| [Introduction to SIEM](SOC-Core/introduction-to-siem.md) | Easy | Log sources, correlation rules, SIEM dashboards |
| [Splunk: The Basics](SOC-Core/splunk-basics.md) | Easy | Log ingestion, SPL queries, VPN log analysis |
| [Elastic Stack: The Basics](SOC-Core/elastic-stack-basics.md) | Easy | Kibana, KQL queries, visualisations, VPN log anomalies |
| [Introduction to EDR](SOC-Core/introduction-to-edr.md) | Easy | EDR vs antivirus, telemetry, simulated EDR investigation |
| [SOC L1 Alert Triage](SOC-Core/soc-l1-alert-triage.md) | Easy | Alert prioritisation, classification, documentation |
| [SOC L1 Alert Reporting](SOC-Core/soc-l1-alert-reporting.md) | Easy | Structured alert reports, L1 to L2 escalation |


## Related Tools

- [Log Analyser](https://github.com/SaihanLob/Log-Analyser): Python tool for detecting brute force, privilege escalation and suspicious processes in Linux and Windows logs. Used in the investigation above.
- [File Integrity Monitor](https://github.com/SaihanLob/File-Integrity-Monitor): Python tool for detecting unauthorised file changes, with a signed baseline.
