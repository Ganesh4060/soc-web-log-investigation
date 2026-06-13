
# 01 - Traffic Overview

## Objective

The objective of this phase of the investigation is to establish a baseline understanding of the web traffic contained within the dataset. Traffic analysis helps identify normal and abnormal patterns, determine the volume of requests, identify the most active hosts, and uncover indicators of suspicious or malicious activity that may require further investigation.


## Methodology

The HTTP log dataset was ingested into Splunk and analyzed using various SPL (Search Processing Language) queries. The analysis focused on identifying request distribution, HTTP methods, source and destination hosts, user agents, and event classifications generated from the logs.

The following areas were examined:

* Total request volume
* HTTP method distribution
* Source IP activity
* Destination server activity
* Traffic trends over time
* User agent analysis
* Event type distribution

## Traffic Summary

Initial analysis revealed a mixture of normal web traffic and suspicious activity. The majority of requests consisted of standard HTTP GET and POST methods, which are commonly observed during normal web browsing and application interactions.

However, additional HTTP methods such as PUT, DELETE, OPTIONS, and CONNECT were also identified. These methods are less common in typical user activity and may indicate reconnaissance, unauthorized resource manipulation attempts, or automated scanning activity.

The presence of these uncommon methods suggests that the environment was subjected to probing or testing by external actors.

## Source Host Analysis

Source IP analysis was conducted to identify the most active clients within the dataset. Several hosts generated significantly more requests than others, indicating potential automated activity or scanning behavior.

High-frequency requests originating from a small number of hosts may represent:

* Vulnerability scanning
* Automated reconnaissance
* Scripted enumeration attempts
* Application testing activities

These hosts were flagged for deeper investigation during subsequent phases of analysis.

## Destination Host Analysis

Traffic was directed toward multiple web servers within the environment. Certain destination hosts received substantially higher volumes of requests, making them likely targets for reconnaissance or exploitation attempts.

Understanding which assets receive the most traffic helps prioritize investigation efforts and identify systems that may require additional monitoring or hardening.

## User Agent Analysis

User agent inspection revealed both legitimate browser traffic and suspicious tools commonly associated with security testing and attack activity.

Several notable user agents were identified, including:

* sqlmap
* curl
* python-requests
* botnet-checker

The presence of these tools suggests automated interaction with the web application. In particular, sqlmap is frequently used to identify and exploit SQL Injection vulnerabilities, while curl and python-requests are commonly used for scripting and automated testing.

These findings increase confidence that portions of the traffic represent malicious reconnaissance activity rather than normal user behavior.

## Event Classification Review

The dataset contained multiple event categories generated during preprocessing and enrichment. The most significant event types observed included:

* Suspicious Agent
* Suspicious URI
* Unexpected Method
* Client Error
* Server Error
* Large Transfer

These classifications indicate that the environment experienced multiple forms of suspicious activity, ranging from reconnaissance attempts to potential exploitation efforts.

The existence of suspicious URIs and uncommon HTTP methods warrants further investigation into possible attack techniques used against the target systems.


## Key Findings

The following observations were identified during the traffic overview phase:

1. Standard GET and POST requests accounted for the majority of traffic.
2. Multiple uncommon HTTP methods were detected, including PUT, DELETE, OPTIONS, and CONNECT.
3. Several source hosts generated unusually high request volumes.
4. Suspicious user agents associated with penetration testing and automation tools were observed.
5. Multiple event types indicated potential malicious activity.
6. Traffic patterns suggest reconnaissance and vulnerability assessment activity against web-facing systems.

## Conclusion

The traffic overview analysis established an initial understanding of the environment and identified several indicators of suspicious behavior. While normal web activity was present, the dataset also contained evidence of automated scanning, suspicious user agents, abnormal HTTP methods, and potentially malicious requests.

Based on these findings, further investigation was conducted focusing on HTTP status codes, suspicious user agents, URI analysis, unexpected HTTP methods, and potential attack indicators to determine the scope and nature of the observed activity.
