# SOC Web Log Investigation Using Splunk

## Project Overview

This project demonstrates a SOC (Security Operations Center) style investigation of HTTP web server logs using Splunk. The objective was to identify suspicious activities, investigate potential attacks, and document findings following real-world SOC analyst methodologies.

The investigation focused on identifying attack patterns, analyzing web traffic, detecting malicious requests, and mapping findings to the MITRE ATT&CK Framework.

---

## Objectives

- Analyze HTTP web server logs
- Detect suspicious and malicious activity
- Identify Indicators of Compromise (IOCs)
- Investigate attack patterns
- Perform threat hunting using Splunk
- Map findings to MITRE ATT&CK techniques
- Document findings in a SOC-style report

---

## Environment

| Component           | Details                      |
|---------------------|------------------------------|
| SIEM Platform       | Splunk                       |
| Data Source         | HTTP Web Logs                |
| Log Format          | JSON                         |
| Analysis Type       | Threat Hunting               |
| Analyst Role        | SOC Analyst L1               |
| Investigation Focus | Web Attacks & Reconnaissance |

---

## Investigation Methodology

### Step 1: Log Review

Reviewed HTTP logs to understand:

- Source IP addresses
- Request methods
- Requested URLs
- User agents
- HTTP status codes

### Step 2: Threat Hunting

Performed targeted searches for:

- Directory Traversal attempts
- SQL Injection scanning activity
- Suspicious User Agents
- High volume error responses
- Web application reconnaissance

### Step 3: Incident Analysis

Correlated events to identify:

- Malicious requests
- Automated attack tools
- Indicators of compromise
- Attack patterns

### Step 4: Documentation

Documented findings, attack techniques, and recommendations.

---

# Investigation Findings

## Finding 1: Directory Traversal Attempt

### Description

Multiple requests attempted to access sensitive system files using directory traversal techniques.

### Evidence

Example target:

```text
/etc/passwd
```

### Example Splunk Query

```spl
index=web_logs uri_path="*/etc/passwd*"
```

### Security Impact

Successful exploitation could allow attackers to access sensitive operating system files and configuration data.

### MITRE ATT&CK Mapping

- T1006 – Path Traversal

---

## Finding 2: SQL Injection Scanner Activity

### Description

Detected automated SQL injection reconnaissance activity originating from a scanning tool.

### Evidence

User-Agent identified:

```text
sqlmap
```

### Example Splunk Query

```spl
index=web_logs user_agent="*sqlmap*"
```

### Observations

- Automated requests
- High request frequency
- SQL injection testing behavior

### MITRE ATT&CK Mapping

- T1190 – Exploit Public-Facing Application

---

## Finding 3: Suspicious User Agents

### Description

Several requests originated from tools commonly used for testing, automation, and reconnaissance.

### Example Splunk Query

```spl
index=web_logs
| stats count by user_agent
| sort -count
```

### Observed User Agents

- sqlmap
- curl
- python-requests

### Risk

These tools are frequently used by attackers during reconnaissance and exploitation attempts.

---

## Finding 4: HTTP Status Code Analysis

### Description

Analyzed server responses to identify unusual activity.

### Example Splunk Query

```spl
index=web_logs
| stats count by status
```

### Findings

- Elevated 404 responses
- Multiple failed requests
- Indicators of reconnaissance activity

### Security Impact

Repeated error responses often indicate scanning or enumeration attempts.

---

# Indicators of Compromise (IOCs)

## Suspicious User Agents

- sqlmap
- curl
- python-requests

## Targeted Resources

- /etc/passwd
- Login pages
- Administrative endpoints

## Attack Categories

- Directory Traversal
- SQL Injection Reconnaissance
- Web Application Enumeration

---

# MITRE ATT&CK Mapping

| Observed Activity              | ATT&CK Technique |
|--------------------------------|------------------|
| Directory Traversal            | T1006            |
| SQL Injection Attempts         | T1190            |
| Web Application Reconnaissance | T1595            |

---


# Recommendations

- Implement Web Application Firewall (WAF)
- Restrict access to sensitive system files
- Monitor suspicious user agents
- Enable detailed logging and alerting
- Validate user input to prevent injection attacks
- Regularly review web server logs
- Block known malicious IP addresses

---

# Skills Demonstrated

- Splunk SIEM
- Log Analysis
- Threat Hunting
- Incident Investigation
- SOC Operations
- Cybersecurity Monitoring
- MITRE ATT&CK Mapping
- Security Event Analysis

---

# Conclusion

This investigation demonstrates the process of analyzing web server logs using Splunk to identify suspicious activity, detect attack attempts, and document findings using SOC analyst methodologies. The analysis revealed evidence of directory traversal attempts, SQL injection reconnaissance, suspicious user agents, and web application probing activity.

---

# Author

Ganesh Kumar

GitHub: https://github.com/Ganesh4060

LinkedIn: https://www.linkedin.com/in/ganesh-kumar-063a5b265/
