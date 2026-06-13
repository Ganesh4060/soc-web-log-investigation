# MITRE ATT&CK Mapping

## Overview

During the investigation, multiple suspicious activities were identified within the HTTP logs. These activities were analyzed and mapped to relevant MITRE ATT&CK techniques to better understand attacker behavior and the stages of the attack lifecycle.

The observed activity primarily falls within the Reconnaissance, Initial Access, Discovery, Persistence, and Command and Control tactics.

---

# Finding 1: SQL Injection Reconnaissance

## Observed Activity

Detected User Agent:

```text
sqlmap/1.5.1
```

SQLMap is an automated tool used to identify and exploit SQL Injection vulnerabilities.

## MITRE ATT&CK Mapping

| Tactic         | Technique ID | Technique                         |
| -------------- | ------------ | --------------------------------- |
| Initial Access | T1190        | Exploit Public-Facing Application |

## Justification

The attacker used SQLMap to probe the web application for SQL Injection vulnerabilities. Exploiting a vulnerable public-facing application is a common technique used to gain unauthorized access to systems and data.

---

# Finding 2: Directory Traversal Attempt

## Observed Activity

Targeted Resource:

```text
/etc/passwd
```

## MITRE ATT&CK Mapping

| Tactic    | Technique ID | Technique      |
| --------- | ------------ | -------------- |
| Discovery | T1006        | Path Traversal |

## Justification

The request targeting `/etc/passwd` indicates an attempt to access sensitive files outside the application's intended directory structure. This behavior is consistent with Directory Traversal attacks used to gather information about the target system.

---

# Finding 3: Web Shell Access Attempt

## Observed Activity

Targeted Resource:

```text
/shell.php
```

## MITRE ATT&CK Mapping

| Tactic      | Technique ID | Technique                            |
| ----------- | ------------ | ------------------------------------ |
| Persistence | T1505.003    | Server Software Component: Web Shell |
| Execution   | T1505.003    | Server Software Component: Web Shell |

## Justification

Web shells are commonly deployed by attackers after successful exploitation to maintain access and execute commands remotely on compromised web servers.

---

# Finding 4: Automated Reconnaissance Tools

## Observed Activity

Detected User Agents:

```text
curl
python-requests
botnet-checker
```

## MITRE ATT&CK Mapping

| Tactic         | Technique ID | Technique       |
| -------------- | ------------ | --------------- |
| Reconnaissance | T1595        | Active Scanning |

## Justification

These tools are frequently used to automate requests against web applications and collect information about available services, resources, and vulnerabilities.

---

# Finding 5: OPTIONS Method Usage

## Observed Activity

HTTP Method:

```text
OPTIONS
```

## MITRE ATT&CK Mapping

| Tactic         | Technique ID | Technique       |
| -------------- | ------------ | --------------- |
| Reconnaissance | T1595        | Active Scanning |

## Justification

OPTIONS requests are often used to discover supported HTTP methods and identify application capabilities before launching further attacks.

---

# Finding 6: PUT Method Usage

## Observed Activity

HTTP Method:

```text
PUT
```

## MITRE ATT&CK Mapping

| Tactic         | Technique ID | Technique                            |
| -------------- | ------------ | ------------------------------------ |
| Initial Access | T1190        | Exploit Public-Facing Application    |
| Persistence    | T1505.003    | Server Software Component: Web Shell |

## Justification

PUT requests may be used to upload files to vulnerable servers. Attackers frequently abuse misconfigured PUT functionality to deploy malicious scripts and web shells.

---

# Finding 7: CONNECT Method Usage

## Observed Activity

HTTP Method:

```text
CONNECT
```

## MITRE ATT&CK Mapping

| Tactic              | Technique ID | Technique                  |
| ------------------- | ------------ | -------------------------- |
| Command and Control | T1071        | Application Layer Protocol |

## Justification

CONNECT requests can be used to establish communication tunnels and may facilitate covert command-and-control communications.

---

# Finding 8: Large Transfer Events

## Observed Activity

HTTP responses exceeding 1 MB.

## MITRE ATT&CK Mapping

| Tactic       | Technique ID | Technique                    |
| ------------ | ------------ | ---------------------------- |
| Collection   | T1005        | Data from Local System       |
| Exfiltration | T1041        | Exfiltration Over C2 Channel |

## Justification

Large transfers may indicate that files or sensitive information were collected and transferred from the target environment.

---

# ATT&CK Summary Table

| Observed Activity    | ATT&CK ID | Technique                            | Tactic              |
| -------------------- | --------- | ------------------------------------ | ------------------- |
| SQLMap Activity      | T1190     | Exploit Public-Facing Application    | Initial Access      |
| Directory Traversal  | T1006     | Path Traversal                       | Discovery           |
| Web Shell Access     | T1505.003 | Server Software Component: Web Shell | Persistence         |
| Curl/Python Requests | T1595     | Active Scanning                      | Reconnaissance      |
| OPTIONS Requests     | T1595     | Active Scanning                      | Reconnaissance      |
| PUT Requests         | T1190     | Exploit Public-Facing Application    | Initial Access      |
| PUT Requests         | T1505.003 | Server Software Component: Web Shell | Persistence         |
| CONNECT Requests     | T1071     | Application Layer Protocol           | Command and Control |
| Large Transfers      | T1005     | Data from Local System               | Collection          |
| Large Transfers      | T1041     | Exfiltration Over C2 Channel         | Exfiltration        |

---

# ATT&CK Coverage Summary

The investigation identified activity spanning multiple phases of the MITRE ATT&CK framework:

* Reconnaissance
* Initial Access
* Discovery
* Persistence
* Collection
* Exfiltration
* Command and Control

The presence of techniques across several ATT&CK tactics suggests a structured attack sequence involving reconnaissance, vulnerability assessment, potential exploitation, and possible post-compromise activity.

This ATT&CK mapping provides additional context for understanding attacker objectives and assists security teams in developing detection and mitigation strategies.

