# Elastic Stack: The Basics

**Platform:** TryHackMe  
**Room:** [Elastic Stack: The Basics](https://tryhackme.com/room/investigatingwithelk101)  
**Difficulty:** Easy  
**Category:** SOC Analysis | SIEM | Log Analysis | KQL  


## Overview

This room builds foundational skills in using the Elastic Stack, specifically Kibana, for VPN log analysis and anomaly detection. Participants navigate the Kibana interface, apply index patterns and time filters, construct KQL queries to investigate user activity, and build visualisations and dashboards to summarise findings. The room places particular emphasis on detecting potential misuse of valid accounts — a common and high-priority threat scenario in real SOC environments.


## Objectives

- Understand the core components of the Elastic Stack (Elasticsearch, Kibana, Logstash, Beats)
- Navigate Kibana's Discover tab to search and filter VPN log data
- Construct KQL queries to investigate specific users, IP addresses, and connection statuses
- Create visualisations and dashboards to present findings clearly
- Detect anomalous user activity including post-termination account usage and failed connection attempts


## Tools Used

| Tool | Purpose |
|------|---------|
| Kibana (Live Instance) | Log analysis, KQL querying, visualisation, and dashboard creation |
| VPN Connections Index | Source dataset used for user activity investigation and anomaly detection |


## Methodology & Walkthrough

<img width="1855" height="951" alt="elk1" src="https://github.com/user-attachments/assets/a9f8cb2e-6c8a-42a9-803a-7173da16b351" />

### Step 1 — Connecting to the ELK Instance and Logging into Kibana

The workflow began by connecting to the live ELK instance and accessing Kibana through a web browser. Kibana serves as the front-end interface for the entire Elastic Stack, providing the search, visualisation, and dashboard capabilities that analysts interact with directly. In a real SOC environment, Kibana is typically configured with pre-built dashboards tailored to the organisation's specific log sources and detection use cases.

### Step 2 — Selecting the Index Pattern and Applying a Time Filter

In the Discover tab, the `vpn_connections` index pattern was selected to scope the investigation to the relevant dataset. A time filter was then applied to narrow the data to the period of interest. These two steps are always the starting point of any Kibana investigation. Selecting the correct index pattern ensures queries are running against the right data source, and applying a time filter immediately reduces the volume of events to a manageable and relevant subset.

### Step 3 — Exploring the Fields Pane

The fields pane was used to explore top values for `Source_ip` and `UserName` across the dataset. This exploratory step provides immediate situational awareness before targeted queries are run. Reviewing top values reveals which users and IP addresses are most active within the dataset, which can surface anomalies such as unfamiliar accounts generating unusually high volumes of VPN connections.

### Step 4 — Investigating User Activity with KQL

KQL queries were constructed to investigate specific users and connection patterns. Each query targeted a distinct investigative question.

**Filtering for specific users from a specific country:**

```kql
Source_Country: "United States" AND (UserName: "James" OR UserName: "Albert")
```

This query scoped the investigation to VPN activity from James and Albert originating from the United States. Combining country and username filters is a practical technique for validating whether user activity aligns with expected geographic patterns.

**Checking for post-termination account activity:**

```kql
UserName: "Johny Brown" AND @timestamp: ["2022-01-01" TO "now"]
```

This query checked for any VPN connections from Johny Brown after a specified date, investigating whether a terminated employee's credentials were still being used. Post-termination account activity is a significant insider threat indicator and a scenario SOC analysts encounter regularly.

**Analysing failed connection attempts from a specific IP:**

```kql
Status: "failed" AND Source_IP: "238.163.231.224"
```

Failed connection attempts from a single IP address can indicate a brute force attack or credential stuffing attempt. Isolating failed attempts by IP is a standard technique for quantifying the scale of an attack and determining whether it warrants escalation.

**Excluding a specific state while focusing on an IP:**

```kql
Source_IP: "238.163.231.224" AND NOT State: "New York"
```

This query applied an exclusion filter to strip out connections from New York associated with the target IP, focusing attention on activity from other locations. The use of `NOT` in KQL mirrors the same exclusion logic used in SPL, reinforcing that this technique is a universal investigative habit across SIEM platforms.

### Step 5 — Creating Visualisations and Dashboards

Following the query-based investigation, visualisations were created to summarise key findings and combined into a dashboard. Dashboards serve a different purpose to raw query results — they provide an at-a-glance view of trends and anomalies that would be difficult to spot by reading through individual events. In a real SOC, dashboards are often the first thing an analyst checks at the start of a shift to identify any overnight activity that requires attention.


## Key Takeaways

- **The Elastic Stack offers greater flexibility and scalability compared to Splunk.** Kibana's dashboard and visualisation capabilities are highly customisable, allowing SOC teams to build monitoring interfaces tailored precisely to their environment and threat model. While Splunk's SPL is arguably more powerful for complex querying, Kibana's interface is often considered more intuitive for visualisation-heavy workflows. Having practical experience with both platforms is a genuine differentiator in the job market, as different organisations standardise on different tools.

- **KQL and SPL share the same investigative logic.** Despite the syntactic differences between Kibana Query Language and Splunk's Search Processing Language, the underlying investigative approach is identical. Filtering by user, correlating IPs, applying exclusions, and scoping by time are techniques that translate directly between platforms. Analysts who understand the logic behind the query, rather than just the syntax, can adapt quickly to any SIEM tool they encounter.

- **Post-termination account checks are a high-priority SOC use case.** The query investigating Johny Brown's activity after termination reflects a real and common threat scenario. Offboarding processes do not always result in immediate account deprovisioning, and former employees or attackers using harvested credentials can exploit this window. Knowing how to construct a time-scoped query to detect this activity is a practical skill with direct operational value.

- **Visualisations are an investigative tool, not just a reporting aid.** Building dashboards during an investigation is not purely for presentation purposes. Visualising trends in user activity, connection volumes, and failure rates can surface patterns that raw query results obscure. Developing the habit of visualising data alongside querying it produces more thorough investigations and more communicable findings.


## Key Terminology

| Term | Definition |
|------|-----------|
| Elasticsearch | The search and analytics engine at the core of the Elastic Stack, responsible for storing and indexing data |
| Kibana | The front-end visualisation and exploration tool for Elasticsearch, used for querying, dashboards, and analysis |
| Logstash | A data processing pipeline that ingests, transforms, and forwards data to Elasticsearch |
| Beats | Lightweight data shippers that collect and send log data from endpoints and servers to Logstash or Elasticsearch |
| Index Patterns | Definitions within Kibana that specify which Elasticsearch indices to query during an investigation |
| KQL | Kibana Query Language — the query syntax used in Kibana to search and filter log data |
| Dashboards | Customisable Kibana views that combine visualisations and saved searches into a unified monitoring interface |


## References

- [TryHackMe — Elastic Stack: The Basics](https://tryhackme.com/room/investigatingwithelk101)
