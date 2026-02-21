# SOC L1 Alert Triage

**Platform:** TryHackMe  
**Room:** [SOC L1 Alert Triage](https://tryhackme.com/room/socl1alerttriage)  
**Difficulty:** Easy  
**Category:** SOC Analysis | Alert Management  


## Overview

This room introduces the foundational concepts behind SOC alert triage from the perspective of an L1 analyst. It covers how alerts are structured, classified, and prioritised within a SOC environment, with hands-on practice using a simulated SIEM SOC dashboard. The focus is on building a systematic and repeatable approach to alert handling.


## Objectives

- Understand what SOC alerts are and why proper triage matters
- Familiarise with alert fields, statuses, and classification types
- Perform alert triage using a simulated SOC dashboard
- Practice documenting findings and closing alerts with appropriate verdicts


## Tools Used

| Tool | Purpose |
|------|---------|
| TryHackMe SIEM SOC Dashboard | Simulated environment for reviewing, assigning, and triaging alerts |
| Web Browser | Accessing the dashboard and performing alert management tasks |


## Methodology & Walkthrough

<img width="1573" height="693" alt="alert-triage-dashboard" src="https://github.com/user-attachments/assets/537705c1-f847-4539-b7a2-5f12076e6f8e" />

### Step 1 — Situational Awareness

The first task involved accessing the SOC dashboard to get an overview of the alert landscape. This included counting the total number of active alerts and identifying the most recent alert. In a real SOC environment, this step establishes situational awareness before diving into individual investigations (you need to understand the volume and recency of alerts before prioritising).

### Step 2 — Alert Examination

Individual alerts were opened to examine their details, including the verdict fields and the users involved. This step highlights something critical in real SOC work, being that an alert is not just a notification, it's a data point. Understanding who is affected, what triggered the alert, and what the initial verdict suggests are all part of building an accurate picture before taking action.

### Step 3 — Filtering and Prioritization

Alerts were filtered to display only those with a **New/Unassigned** status, then sorted by severity and time. This mirrors real triage workflow where analysts work through the highest severity, most recent alerts first. Letting lower-priority alerts sit while critical ones are unaddressed is a common failure point in under-resourced SOC teams.

### Step 4 — Assignment, Documentation, and Closure

Priority alerts were assigned to the analyst, investigated thoroughly, and documented with detailed comments before a verdict was set and the status changed to **Closed**. The documentation step stood out as particularly important. In a real SOC, your comments are the record of your reasoning. If an incident is escalated or revisited, that documentation is what allows the next analyst to pick up where you left off without losing context.


## Key Takeaways

- **Triage is a process, not a single action.** The room reinforced that effective triage follows a structured sequence; awareness, examination, prioritisation, investigation, documentation, and closure. Skipping steps creates gaps that can be exploited or cause incidents to go unresolved.

- **Documentation is as important as the investigation itself.** Adding detailed comments and clear rationale for each verdict is not bureaucratic overhead; it is the foundation of institutional memory in a SOC. A well-documented alert enables faster escalation, better post-incident reviews, and more accurate reporting.

- **Alert classification directly impacts response.** Understanding the difference between a **True Positive** (a genuine threat correctly identified) and a **False Positive** (a benign event incorrectly flagged) determines whether resources are deployed appropriately. Misclassification in either direction has consequences — missed threats or wasted analyst time.

- **Severity scales drive prioritisation.** The alert severity scale ranging from Low to Critical is the primary tool an L1 analyst uses to decide what to work on first. Internalising this hierarchy early builds good habits that carry into real-world SOC work.


## Key Terminology

| Term | Definition |
|------|-----------|
| True Positive | An alert that correctly identifies a real threat or incident |
| False Positive | An alert that flags a benign event as a threat — no real incident occurred |
| Alert Triage | The process of assessing, prioritising, and responding to alerts based on severity and urgency |
| Alert Status | Indicates the current state of an alert: New/Unassigned, In Progress, or Closed |
| Severity Scale | Ranges from Low to Critical. This is used to prioritize analyst attention |
| Documentation | Detailed comments and verdict rationale recorded during and after an investigation |


## References

- [TryHackMe — SOC L1 Alert Triage](https://tryhackme.com/room/socl1alerttriage)
