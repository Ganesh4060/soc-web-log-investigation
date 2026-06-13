# # 08 - Large Transfer Analysis

## Objective

The objective of this phase of the investigation is to identify unusually large HTTP responses that may indicate data exfiltration, unauthorized downloads, sensitive information disclosure, or abnormal file transfer activity.

Large data transfers are not always malicious; however, they often warrant investigation because attackers commonly transfer significant amounts of data after gaining access to a system. Monitoring response sizes helps analysts detect potential data theft and abnormal application behavior.

---

## Splunk Query

```spl
index=web_practicing
event_type="Large Transfer"
| table _time id.orig_h id.resp_h method uri resp_body_len status_code user_agent
| sort - resp_body_len
```

---

## Alternative Detection Query

```spl
index=web_practicing
resp_body_len > 1000000
| table _time id.orig_h uri resp_body_len status_code user_agent
| sort - resp_body_len
```

This query identifies responses larger than 1 MB, which may represent significant file transfers.

---

## Statistical Overview Query

```spl
index=web_practicing
| stats max(resp_body_len) as Largest_Response
        avg(resp_body_len) as Average_Response
        count by event_type
```

This query provides an overview of response sizes across the dataset.

---

## Background

Large HTTP responses can occur during legitimate business operations such as:

* Software downloads
* Image transfers
* Video streaming
* Backup downloads
* Document retrieval

However, attackers may also generate large transfers when:

* Downloading sensitive files
* Exfiltrating data
* Retrieving database exports
* Accessing backup archives
* Downloading application source code

For this reason, unusually large transfers should always be reviewed within their broader context.

---

## Investigation Methodology

The investigation focused on identifying requests with unusually large response sizes and determining whether the activity appeared legitimate or suspicious.

The following factors were examined:

* Response size
* Requested resource
* Source IP address
* User agent
* HTTP response code
* Correlation with other attack indicators

---

## Observed Activity

Analysis identified several responses with significantly larger payload sizes than typical web requests.

Examples included responses exceeding:

```text
1.5 MB
1.8 MB
1.9 MB
```

These transfers were classified as "Large Transfer" events within the dataset.

---

## Security Analysis

Large transfers may indicate the movement of substantial amounts of information between the server and a client.

Potential explanations include:

### Legitimate Activity

* File downloads
* Application updates
* Media content delivery
* Report generation

### Suspicious Activity

* Data exfiltration
* Database exports
* Backup retrieval
* Source code downloads
* Sensitive file disclosure

Additional context is required to determine whether the observed transfers were authorized.

---

## Potential Attack Scenario

A common attack sequence involving large transfers may follow these stages:

### Stage 1 – Initial Access

The attacker gains access through:

* SQL Injection
* Vulnerable web application
* File upload vulnerability
* Stolen credentials

### Stage 2 – Data Discovery

The attacker identifies valuable resources such as:

* Customer databases
* Configuration files
* Internal documents
* Backup archives

### Stage 3 – Data Collection

Sensitive information is gathered and prepared for transfer.

### Stage 4 – Data Exfiltration

Large HTTP responses are generated as the attacker downloads:

* Database dumps
* Configuration files
* Compressed archives
* Sensitive records

---

## Correlation with Other Findings

The large transfer events occurred within an environment where multiple indicators of suspicious activity were also observed.

Related findings include:

* SQLMap activity
* Directory Traversal attempts
* Web Shell access attempts
* Suspicious User Agents
* Unexpected HTTP Methods

The presence of these indicators increases the importance of investigating large transfer events because attackers often retrieve data after successful reconnaissance or exploitation.

---

## MITRE ATT&CK Mapping

### T1041 – Exfiltration Over C2 Channel

Attackers may exfiltrate data through existing communication channels.

### T1020 – Automated Exfiltration

Automated tools may be used to collect and transfer large amounts of information.

### T1005 – Data from Local System

Attackers may access and retrieve files from compromised systems.

---

## Risk Assessment

| Category     | Rating |
| ------------ | ------ |
| Likelihood   | Medium |
| Impact       | High   |
| Overall Risk | High   |

The impact of large transfers can be severe if sensitive information is involved.

---

## Threat Hunting Recommendations

The following actions are recommended:

1. Identify the source IP addresses responsible for large transfers.
2. Review the requested URIs and file types.
3. Investigate user agents associated with the transfers.
4. Determine whether sensitive files were involved.
5. Correlate transfer events with suspicious activity from the same hosts.
6. Review server-side logs for evidence of data access.
7. Implement alerts for unusually large HTTP responses.

---

## Key Findings

The investigation identified multiple HTTP responses significantly larger than normal web traffic.

Key observations include:

* Several responses exceeded 1 MB in size.
* Large transfers may represent legitimate file downloads or potential data exfiltration.
* The activity occurred alongside multiple attack indicators identified throughout the investigation.
* Additional investigation is required to determine the sensitivity of transferred content.
* Large response sizes should be treated as potential indicators of data exposure.

---

## Conclusion

Analysis of HTTP response sizes revealed multiple large transfer events within the dataset. While large file transfers can occur during normal business operations, the presence of other suspicious indicators—including SQLMap activity, Directory Traversal attempts, Web Shell access attempts, and unusual HTTP methods—raises concerns regarding potential data exposure or exfiltration.

Further investigation should focus on identifying the content transferred, the systems involved, and whether the activity was authorized. Continuous monitoring and alerting for abnormal transfer sizes are recommended to improve visibility into potential data loss events.
