# Splunk: The Basics

**Platform:** TryHackMe  
**Room:** [Splunk: The Basics](https://tryhackme.com/room/splunk101)  
**Difficulty:** Easy  
**Category:** SOC Analysis | SIEM | Log Analysis | SPL  


## Overview

This room builds foundational Splunk skills through hands-on log ingestion and analysis using a live Splunk instance. Participants navigate the Splunk interface, ingest VPN log data into a dedicated index, and use Search Processing Language (SPL) to query, filter, and aggregate events. The practical emphasis on SPL query writing makes this room directly applicable to day-to-day SOC analyst work, where Splunk is one of the most widely deployed SIEM platforms in enterprise environments.


## Objectives

- Understand the core components of the Splunk interface
- Ingest log data into a dedicated Splunk index
- Write SPL queries to count, filter, and correlate events
- Associate IP addresses with users and apply exclusion filters
- Build confidence navigating Splunk as a primary SOC investigation tool


## Tools Used

| Tool | Purpose |
|------|---------|
| Splunk (Live Instance) | Log ingestion, SPL querying, and event analysis |
| VPN_Logs Dataset | Source data used for ingestion and investigation exercises |


## Methodology & Walkthrough

### Step 1 — Navigating the Splunk Interface

The workflow began by accessing the Splunk home page and familiarising with the interface layout. Splunk's UI is organised around Apps, Search, and Settings. Understanding where each function lives is essential before attempting any investigation. In a real SOC environment, analysts spend significant time in the Search & Reporting app, so navigating it efficiently directly impacts response times.

### Step 2 — Ingesting Log Data

Log ingestion was initiated by selecting **Add Data** from the Splunk home page, then choosing the **Upload** option to import the `VPN_logs` file. During the upload wizard, the index was set to `VPN_Logs` to ensure the data was stored in a dedicated, searchable location separate from other data sources.

This step highlights an important operational concept. In Splunk, an **index** is not just a storage location, it is the primary organisational unit for data retrieval. Scoping queries to a specific index significantly improves search performance and reduces noise from unrelated data sources. Successful ingestion was validated by running an initial event count query.

### Step 3 — Validating Ingestion and Counting Events

With the data ingested, the first SPL query confirmed the total number of events loaded into the index:

```spl
index=VPN_Logs | stats count
```

**Result:** 2,862 events

This baseline count is a standard first step in any Splunk investigation. It confirms that the expected volume of data is present and searchable before more targeted queries are run.

### Step 4 — Filtering by User Activity

To investigate activity associated with a specific user, the following query filtered events by username:

```spl
index=VPN_Logs Maleena | stats count
```

**Result:** 60 events

This query demonstrates one of Splunk's most practical capabilities: the ability to rapidly scope an investigation to a single user's activity across an entire log dataset. In a real SOC scenario, this technique would be used immediately after an alert fires on a specific account to understand the full scope of that user's behaviour.

### Step 5 — Correlating an IP Address to a Username

A key investigative task involved identifying the user associated with a suspicious IP address:

```spl
index=VPN_Logs 107.14.182.38 | stats values(User)
```

**Result:** `Smith`

IP-to-user correlation is a fundamental SOC technique. When an alert fires on a suspicious IP, one of the first questions an analyst must answer is whose activity is this. This query demonstrates how Splunk enables that attribution quickly and precisely using the `stats values()` function to extract unique field values associated with the search term.

### Step 6 — Applying Exclusion Filters

To focus on events from all countries except France, an exclusion filter was applied:

```spl
index=VPN_Logs NOT France | stats count
```

**Result:** 2,814 events

Exclusion filters are a powerful technique for reducing noise in large datasets. In practice, analysts use `NOT` filters to strip out known-good traffic such as activity from trusted geographic regions or internal IP ranges, so that investigations can focus on the data that actually matters.

### Step 7 — Investigating a Specific IP Address

A final query investigated the volume of VPN activity associated with a second IP address:

```spl
index=VPN_Logs 107.3.206.58 | stats count
```

**Result:** 14 events

Quantifying the event volume associated with a specific IP provides immediate context. 14 events from a single IP within a VPN log dataset may indicate targeted activity, whereas thousands of events from the same IP might suggest automated or scripted behaviour. Volume alone does not determine severity, but it is always a relevant data point.


## Key Takeaways

- **SPL is the most important skill a Splunk-based SOC analyst can develop.** The queries in this room are foundational. `stats count`, `stats values()`, and `NOT` filters are building blocks that appear in virtually every Splunk investigation. Mastering these simple commands and understanding how to chain them with pipes creates the foundation for writing more complex detection queries and correlation searches.

- **Dedicated indexes are not optional, they are operational discipline.** Storing data in a named, purpose-specific index like `VPN_Logs` makes queries faster, reduces cross-contamination with unrelated data, and makes it significantly easier to scope investigations accurately. In environments with dozens of data sources, index hygiene is what keeps Splunk performant and investigations manageable.

- **IP-to-user correlation is a core SOC technique.** The ability to answer whose activity is this within seconds of receiving an alert is what separates reactive analysts from effective ones. Splunk's `stats values()` function makes this correlation immediate, and understanding how to use it instinctively is a skill that pays dividends across almost every investigation type.

- **Exclusion filters are as valuable as inclusion filters.** Knowing what to ignore is just as important as knowing what to look for. Stripping out known-good activity using `NOT` allows analysts to work with a refined, high-signal dataset rather than sifting through noise. This is a critical habit in high-volume SOC environments where alert fatigue is a real operational risk.


## Key Terminology

| Term | Definition |
|------|-----------|
| SPL | Search Processing Language — the query language used in Splunk to search, filter, and analyse log data |
| Index | A dedicated storage location in Splunk that organises data for efficient retrieval and scoped searching |
| Sourcetype | A designation that tells Splunk how to parse incoming data, ensuring it is structured correctly for analysis |
| Fields | Attributes extracted from events such as user, IP address, and country, used to filter and correlate data |
| Events | Individual records ingested into Splunk, each representing a discrete occurrence within the log data |
| Time Range Picker | A Splunk UI control that scopes searches to a specific time window, essential for incident timeline analysis |
| Wildcards | Special characters used in SPL queries to match variable strings or sequences within field values |
| Pipes | SPL operators that chain commands together, passing the output of one command as the input to the next |


## References

- [TryHackMe — Splunk: The Basics](https://tryhackme.com/room/splunk101)
