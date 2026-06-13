# 02 - Status Code Analysis

## Objective

The objective of this phase of the investigation is to analyze HTTP response status codes and identify abnormal behavior within the web application environment. HTTP status codes provide valuable insight into application performance, user activity, and potential malicious behavior.

By examining response codes, analysts can identify failed requests, application errors, reconnaissance activity, and potential exploitation attempts.

---

## Splunk Query

```spl
index=web_practicing
| stats count by status_code
| sort - count
```

---

## Methodology

HTTP response codes were analyzed to determine how the web server responded to client requests. Particular attention was given to error-related responses because they often reveal indicators of scanning activity, invalid requests, application misconfigurations, or attempted attacks.

The analysis focused on the following categories:

* Successful requests (200)
* Client-side errors (4xx)
* Server-side errors (5xx)

---

## Status Code Distribution

The dataset contained a combination of successful and unsuccessful requests. While the majority of requests were processed successfully, several error responses were identified that warrant further investigation.

Observed status codes included:

| Status Code | Description           |
| ----------- | --------------------- |
| 200         | OK                    |
| 400         | Bad Request           |
| 404         | Not Found             |
| 500         | Internal Server Error |
| 503         | Service Unavailable   |

---

## Analysis of Successful Requests (200 OK)

HTTP 200 responses indicate that the server successfully processed and returned the requested resource.

A high volume of 200 responses is generally expected in normal web traffic and demonstrates that the web application remained accessible for most requests.

However, successful responses should still be correlated with suspicious user agents and URIs because malicious requests can also receive successful responses from the server.

### Security Observation

* Normal application functionality appears operational.
* Successful responses may still include attacker reconnaissance or exploitation attempts.
* Further analysis of associated requests is required.

---

## Analysis of Client Errors (400 Bad Request)

HTTP 400 responses occur when the server cannot process a request due to malformed syntax or invalid parameters.

Multiple 400 responses may indicate:

* Invalid requests
* Automated scanning activity
* Fuzzing attempts
* Malformed payload submissions

Attackers frequently generate 400 errors while probing applications for vulnerabilities or attempting to discover valid endpoints.

### Security Observation

The presence of repeated 400 responses suggests that certain requests were intentionally crafted or malformed, potentially as part of a reconnaissance or vulnerability assessment campaign.

---

## Analysis of Client Errors (404 Not Found)

HTTP 404 responses indicate that the requested resource does not exist on the server.

These responses are commonly associated with:

* Directory enumeration
* File discovery attempts
* Vulnerability scanning
* Manual reconnaissance

Attackers often request numerous files and directories in an attempt to identify hidden administrative panels, backup files, configuration files, or vulnerable endpoints.

### Security Observation

The frequency of 404 responses suggests active resource enumeration against the web application. This behavior is consistent with the early stages of an attack lifecycle, where adversaries attempt to map the target environment.

---

## Analysis of Server Errors (500 Internal Server Error)

HTTP 500 responses indicate that the server encountered an unexpected condition while processing a request.

Potential causes include:

* Application crashes
* Backend failures
* Improper input validation
* Exploitation attempts

Attackers frequently trigger server errors while testing payloads designed to exploit application vulnerabilities.

### Security Observation

The presence of multiple 500 responses may indicate that specific requests caused abnormal application behavior. These events should be correlated with suspicious user agents and request URIs to determine whether exploitation attempts were responsible.

---

## Analysis of Server Errors (503 Service Unavailable)

HTTP 503 responses occur when the server is temporarily unable to process requests.

Common causes include:

* Resource exhaustion
* Maintenance activities
* High traffic volume
* Denial-of-Service (DoS) conditions

In security investigations, repeated 503 responses may indicate that the application was overwhelmed by excessive requests or affected by malicious activity.

### Security Observation

The occurrence of 503 responses suggests temporary service degradation and may indicate increased server load or potential attack-related resource exhaustion.

---

## Threat Hunting Considerations

The following conditions should be investigated further:

1. Source IP addresses generating large numbers of 400 or 404 responses.
2. Requests associated with suspicious user agents such as sqlmap, curl, or python-requests.
3. URIs linked to 500 server errors.
4. Time periods containing spikes in 503 responses.
5. Correlation between server errors and unusual HTTP methods.

These indicators may help identify attack campaigns targeting the application.

---

## Key Findings

The status code analysis revealed several noteworthy observations:

* The majority of requests were successfully processed (HTTP 200).
* Multiple client-side errors (400 and 404) suggest reconnaissance and resource discovery activity.
* Server-side errors (500 and 503) indicate application instability or potential attack impact.
* Error responses were distributed across multiple requests and require correlation with other indicators such as suspicious URIs and user agents.
* The observed error patterns are consistent with web application scanning and vulnerability assessment activity.

---

## Conclusion

Analysis of HTTP response codes revealed evidence of both normal application usage and suspicious activity. While most requests were processed successfully, the presence of client-side and server-side errors suggests active reconnaissance, invalid request generation, and possible exploitation attempts.

These findings provide a foundation for deeper investigation into suspicious user agents, abnormal URIs, unexpected HTTP methods, and potential attack techniques observed within the dataset.
