# Introduction to SIEM

**Platform:** TryHackMe  
**Room:** [Introduction to SIEM](https://tryhackme.com/room/introtosiem)  
**Difficulty:** Easy  
**Category:** SOC Analysis | SIEM | Log Analysis  


## Overview

This room introduces Security Information and Event Management (SIEM) systems as a cornerstone of modern SOC operations. It covers the different types of log sources a SIEM ingests, how correlation rules generate actionable alerts, and how analysts use SIEM dashboards to investigate and respond to security events. Through a simulated SIEM console, participants triage a real alert; tracing suspicious activity back to a specific process, user, and hostname, thus building the foundational skills required for effective security monitoring.


## Objectives

- Understand what a SIEM is and why it is essential in a SOC environment
- Identify the different types of log sources that feed into a SIEM
- Explore core SIEM features including dashboards, correlation rules, and alerting
- Perform alert triage within a simulated SIEM console
- Learn how log normalisation enables consistent analysis across diverse data sources


## Tools Used

| Tool | Purpose |
|------|---------|
| Simulated SIEM Console | Hands-on alert triage, correlation rule examination, and event investigation |


## Methodology & Walkthrough

<img width="702" height="633" alt="siemintro" src="https://github.com/user-attachments/assets/6aa37f0b-248b-4a45-b376-44ede8f0eaf1" />

### Step 1 — Accessing the SIEM Dashboard

The investigation began by accessing the embedded lab environment and opening the SIEM dashboard. In a real SOC, the SIEM dashboard is the central monitoring hub as it aggregates data from across the environment and surfaces alerts, trends, and anomalies in a single interface. Familiarity with navigating this dashboard efficiently is one of the most practical skills an L1 analyst can develop.

### Step 2 — Triggering Suspicious Activity and Generating Alerts

A suspicious activity was triggered within the lab to generate an alert within the SIEM. This step illustrates an important operational concept — SIEM alerts do not appear in isolation. They are the product of underlying activity on endpoints, networks, or applications that has matched a predefined correlation rule. Understanding that an alert is a *signal* derived from raw log data, rather than the raw data itself, is fundamental to effective investigation.

### Step 3 — Triaging the Alert

The alert details panel was opened to begin triage, focusing on three critical fields that define the scope of the incident:

- **Process Name:** `cudominer.exe` — immediately indicative of cryptomining activity
- **User:** `chris` — the account associated with the suspicious process
- **Hostname:** `HR_02` — the affected endpoint within the network

These three data points alone establish the who, what, and where of the incident before any deeper investigation begins. In a real SOC environment, this information would immediately inform the next steps; isolating the endpoint, reviewing the user's recent activity, and determining whether the infection is isolated or part of a broader campaign.

### Step 4 — Examining the Correlation Rule

The detection rule that triggered the alert was examined to identify the matched term, in this case, **miner**. This highlights how SIEM correlation rules work in practice: the system continuously compares incoming log data against a library of rules, and when a match is found, an alert is generated. Understanding the rule behind an alert is essential for accurate classification as it tells the analyst what behaviour the system was designed to catch and whether the alert represents a genuine threat or a potential false positive.

### Step 5 — Resolving the Alert

The appropriate action (isolation) was selected to resolve the alert, completing the triage workflow. This final step reinforces the importance of decisive action following investigation as an alert left unresolved creates noise in the queue and can mask subsequent alerts from the same source.


## Key Takeaways

- **SIEM provides a holistic view that individual tools cannot.** Unlike EDR, which focuses on endpoint telemetry, or a firewall, which monitors network traffic, a SIEM aggregates data from all of these sources simultaneously. This breadth of visibility is what enables analysts to correlate events across different parts of the environment and identify threats that would be invisible when looking at any single data source in isolation. The investigation of `cudominer.exe` on `HR_02` is a straightforward example. In a more complex incident, the same process of correlating user, process, and hostname data across multiple log sources would be far more revealing.

- **Correlation rules are the intelligence layer of a SIEM.** Raw logs are voluminous and largely unactionable on their own. Correlation rules transform that raw data into meaningful signals by defining the conditions under which an alert should be generated. A rule that triggers on the keyword *miner* within process names is a simple but effective example. In mature SOC environments, these rules are continuously refined based on emerging threat intelligence and lessons learned from past incidents.

- **Log normalisation is what makes cross-source correlation possible.** Different systems produce logs in different formats; Windows Event Logs look nothing like Linux syslogs or firewall logs. Normalisation standardises this data into a consistent format before it enters the SIEM, ensuring that correlation rules can operate across all data sources without format-specific exceptions. Without normalisation, the aggregation that makes SIEM powerful would be significantly undermined.

- **Alert context determines response speed.** The three fields that defined this investigation; process name, user, and hostname demonstrate how much investigative value is embedded in a well-structured alert. Analysts who know how to extract and act on this context quickly are significantly more effective than those who treat every alert as a blank canvas requiring investigation from scratch.


## Key Terminology

| Term | Definition |
|------|-----------|
| SIEM | Security Information and Event Management. A platform that aggregates, normalises, and analyses log data from across an environment to detect and respond to threats |
| Log Ingestion | The process of collecting and importing log data from various sources into the SIEM for analysis |
| Normalisation | Standardising log data from different sources into a consistent format to enable reliable correlation |
| Correlation Rules | Predefined conditions that the SIEM uses to match patterns across log data and generate alerts |
| Data Sources | The various systems that feed logs into the SIEM. This includes endpoints, firewalls, applications, and network devices |
| Dashboard | A visual interface within the SIEM that displays alerts, trends, and metrics for analyst monitoring |
| SIEM Use Cases | Specific threat detection and response scenarios that the SIEM is configured to identify and alert on |


## References

- [TryHackMe — Introduction to SIEM](https://tryhackme.com/room/introtosiem)
