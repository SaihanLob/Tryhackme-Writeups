# SOC L1 Alert Reporting 

**Platform:** TryHackMe  
**Room:** [SOC L1 Alert Reporting](https://tryhackme.com/room/socl1alertreporting)  
**Difficulty:** Easy  
**Category:** SOC Analysis | Alert Reporting | Escalation  


## Overview

This builds directly on alert triage fundamentals by focusing on what happens *after* an alert is classified — how to report it clearly and escalate it effectively. It introduces a structured reporting framework and walks through the full escalation workflow from L1 to L2, emphasising that clear communication is just as critical as technical analysis in a SOC environment.


## Objectives

- Understand the importance of structured alert reporting in a SOC
- Apply the Five Ws framework to write clear, actionable analyst comments
- Learn how to escalate alerts to L2 using correct status, verdict, and assignee fields
- Build confidence in SOC communication and documentation practices


## Tools Used

| Tool | Purpose |
|------|---------|
| TryHackMe SIEM SOC Dashboard | Primary platform for alert reporting, status management, and escalation |
| Integrated Ticketing System | Alert tracking and L2 assignment during escalation workflow |


## Methodology & Walkthrough

<img width="1823" height="904" alt="alert-reporting-dashboard" src="https://github.com/user-attachments/assets/e4914be1-7721-46b5-9492-fa1c0148671a" />

### Step 1 — Accessing the SOC Dashboard

The workflow began by opening the SOC dashboard in a browser, being a centralised platform where all alert reporting and escalation tasks are performed. In a real SOC, this is typically the first thing an analyst opens at the start of a shift. Familiarity with the dashboard layout directly impacts how efficiently an analyst can move through their queue.

### Step 2 — Writing the Alert Report Using the Five Ws

The core of this room was learning to document alerts using the **Five Ws** framework; Who, What, When, Where, and Why. This structure ensures that every alert comment contains the essential context an L2 analyst needs to continue the investigation without starting from scratch.

- **Who** — Which user or system is involved?
- **What** — What activity or behaviour triggered the alert?
- **When** — What is the timestamp of the event?
- **Where** — Which system, IP, or location is associated with the alert?
- **Why** — What is the suspected intent or cause behind the activity?

The report was entered into the Analyst Comment field and saved without changing the alert status at this stage. This is an important distinction as saving a report and escalating an alert are separate actions that serve different purposes.

### Step 3 — Escalating the Alert to L2

Once the report was documented, the escalation process followed a specific sequence:

1. Changed the alert **Status** to *In Progress* — signaling active investigation
2. Set the **Verdict** to *True Positive* — confirming the alert represents a genuine threat
3. Assigned the alert to the on-shift L2 analyst (**E. Fleming**) — establishing accountability
4. Saved the escalated alert — triggering notification to L2 for further investigation

Each of these steps serves a distinct operational purpose. Skipping or incorrectly setting any one of them can result in alerts sitting unattended, being routed to the wrong analyst, or being misinterpreted by L2, which slow down incident response.


## Key Takeaways

- **The Five Ws transform vague notes into actionable intelligence.** A comment like "suspicious activity detected" tells L2 nothing. A structured comment covering who was involved, what happened, when it occurred, where it was observed, and why it is considered suspicious gives L2 everything they need to begin a deeper investigation immediately. This framework should become second nature for any L1 analyst.

- **Vague documentation has operational consequences.** One of the most important lessons from this room is that poor comments don't just look unprofessional, but actively slow down incident response. When L2 receives an escalated alert with insufficient context, they must spend time reconstructing what L1 already investigated. In a time-sensitive incident, that delay can be the difference between containment and a full breach.

- **Escalation is a structured handoff, not just a status change.** Setting the correct status, verdict, and assignee fields before saving is what makes an escalation functional. Each field communicates something specific to the receiving analyst and to the broader SOC workflow. Treating escalation as a checklist rather than an afterthought is a habit worth cultivating.

- **Meticulous SOC operations begin at L1.** The quality of an L2 investigation is directly dependent on the quality of the L1 report it receives. This room reinforced that L1 is not a stepping stone to be rushed through and is instead the foundation on which all subsequent response actions are built.


## Key Terminology

| Term | Definition |
|------|-----------|
| Five Ws | A reporting framework covering Who, What, When, Where, and Why. This ensures comprehensive alert documentation |
| Escalation Path | The structured process of passing an alert from L1 to L2 for deeper investigation |
| Alert Status | Reflects the current stage of an alert: New, In Progress, or Closed |
| Verdict | Classification of an alert as True Positive or False Positive. This drives escalation decisions |
| Assignee | The designated analyst responsible for handling a specific alert, ensuring accountability |
| Analyst Comments | Detailed notes documenting context, evidence, and rationale. This is critical for effective escalation |
| Communication Protocols | Best practices for documenting and conveying findings clearly across SOC team members |


## References

- [TryHackMe — SOC L1 Alert Reporting](https://tryhackme.com/room/socl1alertreporting)
