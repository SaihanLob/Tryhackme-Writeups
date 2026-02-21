# Introduction to EDR

**Platform:** TryHackMe  
**Room:** [Introduction to EDR](https://tryhackme.com/room/introductiontoedrs)  
**Difficulty:** Easy  
**Category:** SOC Analysis | Endpoint Security | Threat Detection  


## Overview

This room introduces Endpoint Detection and Response (EDR) as a foundational SOC security solution, distinguishing it from traditional antivirus approaches and exploring its architecture, telemetry collection, and detection capabilities. Through a simulated EDR dashboard, participants investigate realistic alerts, trace process lineage, and extract key indicators of compromise, thus building the practical skills needed to handle endpoint-based threats in a real SOC environment.


## Objectives

- Differentiate EDR from traditional antivirus solutions
- Understand EDR architecture and how telemetry is collected from endpoints
- Analyze endpoint telemetry to investigate suspicious activity
- Extract Indicators of Compromise (IOCs) including malware paths, suspicious executables, and exfiltration URLs
- Understand detection and response capabilities within an EDR console


## Tools Used

| Tool | Purpose |
|------|---------|
| Simulated EDR Dashboard | Hands-on investigation of endpoint alerts, telemetry analysis, and IOC extraction |


## Methodology & Walkthrough

<img width="1847" height="983" alt="introduction-to-edr-dashboard" src="https://github.com/user-attachments/assets/ee37e376-0e25-4a57-a702-4825fa0bc23e" />

### Step 1 — Accessing the EDR Dashboard and Triaging Detections

The investigation began by accessing the simulated EDR console and reviewing active detections. Unlike a SIEM which aggregates logs from across the environment, an EDR dashboard surfaces endpoint-specific telemetry; process execution chains, file system changes, network connections, and registry modifications. The first priority was triaging detections to identify which alerts warranted deeper investigation.

### Step 2 — Identifying Payload Download Tools and Malware Paths

Within the flagged detections, the investigation focused on identifying the tools used to download malicious payloads onto the endpoint. This is a critical step in understanding the initial access and execution phases of an attack. Once the download tool was identified, the absolute path of the malware on the file system was extracted. This is a key IOC that can be used for containment and threat hunting across other endpoints in the environment.

### Step 3 — Analysing Suspicious Executables and Process Lineage

Suspicious executables identified during triage were examined in detail, with particular attention paid to process lineage; the parent-child relationships between processes that reveal how an attack unfolded. Tracing process lineage is one of the most powerful capabilities EDR provides over traditional antivirus. Where antivirus sees a file, EDR sees the full chain of events that led to its execution, making it far easier to attribute malicious activity to specific tools or threat actors.

### Step 4 — Capturing Exfiltration URLs

The final investigative step involved identifying the URLs used to exfiltrate data from the compromised endpoint. Exfiltration URLs are high-value IOCs as they can be used to block outbound connections at the firewall level, identify other potentially compromised endpoints communicating with the same destination, and attribute the attack to known threat infrastructure. Capturing and documenting these artifacts is a core deliverable of any endpoint investigation.


## Key Takeaways

- **EDR and antivirus solve fundamentally different problems.** Traditional antivirus relies on signature-based detection, where it looks for known malicious files and blocks them. EDR takes a behavioural approach, monitoring what endpoints do rather than just what files they contain. This distinction matters operationally because sophisticated attackers routinely bypass signature-based defenses using fileless malware, living-off-the-land techniques, and custom tooling. EDR catches what antivirus misses.

- **Telemetry is the backbone of endpoint investigation.** The depth of data an EDR collects; process executions, network connections, file operations, registry changes is what makes it possible to reconstruct an attack timeline with precision. Without this telemetry, investigations rely on incomplete logs and educated guesswork. With it, analysts can trace exactly what happened, when, and in what order.

- **Process lineage reveals attacker methodology.** One of the most valuable investigative techniques EDR enables is tracing the parent-child relationships between processes. A suspicious executable running under a legitimate process like `explorer.exe` or `winword.exe` is a significant red flag that signature-based tools would likely miss entirely. Understanding how to read and interpret process trees is a skill that directly translates to real-world SOC work.

- **IOCs extracted during investigation feed broader defense.** Malware paths, suspicious executables, and exfiltration URLs are not just evidence; they are actionable intelligence. Each IOC documented during an endpoint investigation can be operationalised: blocked at the firewall, added to detection rules, or used to hunt for lateral movement across the environment. This is what separates reactive incident response from proactive defense.


## Key Terminology

| Term | Definition |
|------|-----------|
| EDR | Endpoint Detection and Response. A security solution that monitors endpoint behavior and collects telemetry for threat detection and investigation |
| Telemetry | Detailed data collected from endpoints covering process execution, network activity, file changes, and registry modifications |
| Behavioural Detection | Identifying threats based on abnormal activity patterns rather than known malicious signatures |
| Process Lineage | The parent-child chain of process relationships that reveals how an attack unfolded on an endpoint |
| Endpoint Isolation | Disconnecting a compromised endpoint from the network to prevent further attacker access or lateral movement |
| Threat Hunting | Proactively searching for threats within the environment that automated detection may have missed |
| Indicators of Compromise (IOCs) | Artifacts that indicate a security breach; including file paths, hashes, IPs, and URLs associated with malicious activity |
| Exfiltration URL | A destination URL used by an attacker to transfer stolen data out of the compromised environment |


## References

- [TryHackMe — Introduction to EDR](https://tryhackme.com/room/introductiontoedrs)
