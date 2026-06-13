# 03 - Suspicious User Agents Analysis

## Objective

The objective of this phase of the investigation is to identify and analyze suspicious user agents present within the HTTP traffic logs. User agents provide information about the client software initiating requests to a web server and can help distinguish legitimate user activity from automated tools, scanners, and potentially malicious applications.

Threat actors frequently use automated tools during reconnaissance, vulnerability scanning, exploitation attempts, and post-exploitation activities. Identifying these tools provides valuable insight into attacker behavior and intent.

---

## Splunk Query

```spl
index=web_practicing
event_type="Suspicious Agent"
| table _time id.orig_h method uri user_agent status_code
```

---

## Alternative Query

```spl
index=web_practicing
| stats count by user_agent
| sort - count
```

This query helps identify the most frequently observed user agents within the dataset.

---

## Methodology

User agent values were extracted and reviewed to identify tools commonly associated with security testing, automation, and malicious activity.

The investigation focused on:

* Automated scanning tools
* Scripting frameworks
* Command-line utilities
* Unusual or custom user agents
* Indicators of attacker reconnaissance

---

## Suspicious User Agents Identified

The analysis revealed several user agents that are not typically associated with normal web browsing activity.

### 1. sqlmap/1.5.1

#### Description

SQLMap is a widely used open-source penetration testing tool designed to automate the detection and exploitation of SQL Injection vulnerabilities.

#### Security Significance

The presence of SQLMap strongly suggests that an entity attempted to identify or exploit SQL Injection vulnerabilities within the target web application.

Common SQLMap activities include:

* Database fingerprinting
* SQL Injection testing
* Data extraction attempts
* Database enumeration

#### Risk Level

🔴 High

#### Potential ATT&CK Mapping

* T1190 – Exploit Public-Facing Application

---

### 2. curl/7.68.0

#### Description

Curl is a command-line utility used to transfer data using various protocols, including HTTP and HTTPS.

#### Security Significance

While curl is a legitimate administrative tool, it is frequently used by attackers for:

* Manual reconnaissance
* API testing
* Payload delivery
* Exploitation attempts

Attackers often use curl because it allows direct interaction with web servers without a browser.

#### Risk Level

🟡 Medium

#### Potential ATT&CK Mapping

* T1595 – Active Scanning

---

### 3. python-requests/2.25.1

#### Description

Python Requests is a popular HTTP library used within Python scripts and automation frameworks.

#### Security Significance

The presence of python-requests indicates automated interactions with the target application.

Possible activities include:

* Vulnerability scanning
* Data collection
* Enumeration scripts
* Custom attack automation

Because Python-based tools are highly customizable, identifying this user agent warrants further investigation.

#### Risk Level

🟠 Medium to High

#### Potential ATT&CK Mapping

* T1595 – Active Scanning
* T1587 – Develop Capabilities

---

### 4. botnet-checker/1.0

#### Description

The user agent identified as "botnet-checker" does not correspond to common browser software and appears to represent a custom or specialized tool.

#### Security Significance

Custom user agents are often used by:

* Malware
* Botnets
* Automated reconnaissance tools
* Internal testing frameworks

Due to the unusual naming convention and lack of association with standard software, this activity should be considered suspicious.

#### Risk Level

🔴 High

#### Potential ATT&CK Mapping

* T1071 – Application Layer Protocol
* T1595 – Active Scanning

---

## Behavioral Analysis

Several characteristics indicate automated activity rather than legitimate user behavior:

### Indicators Observed

* Non-browser user agents
* Security testing tools
* Automated scripting frameworks
* Repeated requests from the same hosts
* Requests targeting sensitive resources

These characteristics are commonly observed during the reconnaissance and vulnerability discovery stages of an attack.

---

## Correlation with Other Findings

The suspicious user agents were observed alongside other indicators identified during the investigation, including:

* Unusual HTTP methods
* Suspicious URIs
* Client-side errors (404)
* Server-side errors (500)
* Administrative endpoint access attempts

The combination of these indicators increases confidence that the observed activity was not accidental and may represent deliberate security testing or malicious reconnaissance.

---

## Threat Hunting Recommendations

The following investigative actions are recommended:

1. Identify source IP addresses associated with each suspicious user agent.
2. Review request frequency and timing patterns.
3. Examine accessed URIs for signs of enumeration.
4. Correlate user agents with HTTP status codes.
5. Investigate any successful responses generated by suspicious tools.
6. Monitor future traffic for similar user agent strings.

---

## Key Findings

The user agent analysis identified multiple indicators of automated and potentially malicious activity:

* SQLMap was detected, indicating possible SQL Injection testing.
* Curl was used to interact directly with the web application.
* Python Requests suggested automated scripting activity.
* A custom botnet-related user agent was identified.
* Multiple suspicious tools were observed interacting with the environment.
* Findings are consistent with reconnaissance and vulnerability assessment activity.

---

## Conclusion

Analysis of user agent data revealed evidence of automated scanning and security testing activity targeting the web application. Several user agents commonly associated with penetration testing and attack automation were identified, including SQLMap, Curl, and Python Requests.

These findings suggest that external entities were actively probing the application for weaknesses and attempting to gather information about available resources. The presence of these tools, combined with other suspicious indicators observed throughout the investigation, supports the assessment that the environment experienced reconnaissance and potential exploitation attempts.
