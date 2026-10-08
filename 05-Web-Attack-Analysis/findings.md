# Web Attack Analysis

## Objective

The objective of this investigation was to analyze web server logs and identify suspicious HTTP request patterns that may indicate common web application attacks.

A local Python HTTP server was used to safely simulate web traffic in a controlled environment.

## Lab Environment

* Operating System: Windows 11
* Web Server: Python `http.server`
* Server Port: `8000`
* Server Address: `127.0.0.1`
* Tools Used:

  * Python
  * Windows Command Prompt
  * cURL
  * Web Browser

## Lab Setup

A simple web page was created using:

```cmd
mkdir C:\SOC-Web-Lab
cd C:\SOC-Web-Lab
echo SOC Analyst Web Attack Analysis Lab > index.html
```

The local web server was started using:

```cmd
python -m http.server 8000
```

The server was accessed through:

```text
http://127.0.0.1:8000
```

## Investigation 1 — Normal Web Request

A normal HTTP request was generated:

```cmd
curl "http://127.0.0.1:8000/"
```

The server recorded a normal request for the root page.

### Analysis

This request represents normal web activity where a client requests the default page from the web server.

**Security Assessment:** Normal / Benign

**Screenshot:** `01-web-server.png`

---

## Investigation 2 — SQL Injection-Style Request

The following request was generated:

```cmd
curl "http://127.0.0.1:8000/?id=1%27%20OR%20%271%27%3D%271"
```

The encoded request contains the SQL injection-style pattern:

```text
id=1' OR '1'='1
```

### Analysis

The request contains characters and syntax commonly associated with SQL injection attempts.

An attacker may attempt to manipulate application-generated SQL queries by inserting SQL syntax into parameters such as `id`.

In this lab, the Python HTTP server does not execute SQL queries. Therefore, the request demonstrates **detection of a suspicious pattern**, not successful SQL injection.

**Security Assessment:** Suspicious / Potential SQL Injection Attempt

**Screenshot:** `02-sql-injection.png`

---

## Investigation 3 — Directory Traversal-Style Request

The following request was generated:

```cmd
curl "http://127.0.0.1:8000/../../../../Windows/System32/drivers/etc/hosts"
```

The server logged the normalized request path:

```text
GET /Windows/System32/drivers/etc/hosts HTTP/1.1
```

### Analysis

The requested path references the Windows `System32` directory and the `hosts` file, which is outside the intended web content directory.

The original request attempted to use directory traversal sequences (`../`) to move outside the web server's normal document directory.

The Python HTTP server normalized the path before logging it, so the raw traversal sequence was not preserved in the server log.

This activity is therefore considered a **directory traversal-style request**.

There is no evidence that the Windows `hosts` file was successfully accessed.

**Security Assessment:** Suspicious / Potential Directory Traversal Attempt

**Screenshot:** `03-directory-traversal.png`

---

## Investigation 4 — Web Log Analysis

A normal baseline request was generated:

```cmd
curl "http://127.0.0.1:8000/"
```

The resulting server logs were compared with the suspicious requests.

### Observed Activity

| Request                                   | Type                              | Assessment |
| ----------------------------------------- | --------------------------------- | ---------- |
| `GET /`                                   | Normal web request                | Benign     |
| `GET /?id=1%27%20OR%20%271%27%3D%271`     | SQL injection-style request       | Suspicious |
| `GET /Windows/System32/drivers/etc/hosts` | Directory traversal-style request | Suspicious |

### SOC Analyst Perspective

A SOC analyst reviewing web logs should look for:

* SQL keywords or SQL syntax in URL parameters
* Directory traversal sequences such as `../`
* Requests for sensitive system files
* Repeated suspicious requests from the same source
* Unusual HTTP methods or paths
* High volumes of failed or abnormal requests
* Multiple attack patterns against the same web application

These indicators can be correlated with other security logs to determine whether an attack was successful.

**Screenshot:** `04-log-analysis.png`

---

## Findings

The investigation successfully demonstrated how suspicious HTTP requests can be identified through web server logs.

Two suspicious patterns were observed:

1. SQL injection-style input
2. Directory traversal-style input

However, the presence of a suspicious request alone does not prove that exploitation was successful.

The activity was generated locally as part of a controlled SOC training environment.

## SOC Detection Workflow

The investigation followed a basic SOC workflow:

```text
Web Traffic
    ↓
Log Collection
    ↓
Identify Suspicious Requests
    ↓
Analyze Request Pattern
    ↓
Determine Potential Attack Type
    ↓
Document Findings
    ↓
Recommend Response
```

## Recommendations

For a production web application:

* Validate and sanitize user input.
* Use parameterized SQL queries.
* Implement secure file access controls.
* Prevent access outside the web application's intended directory.
* Monitor web server logs for repeated attack patterns.
* Configure alerting for common web attack signatures.
* Correlate web logs with firewall, endpoint, and authentication logs.
* Use a Web Application Firewall (WAF) where appropriate.

## Conclusion

This investigation provided practical experience in analyzing HTTP requests from a SOC analyst perspective.

The lab demonstrated how normal web traffic can be compared against suspicious request patterns to identify potential SQL injection and directory traversal activity.

The exercise also highlighted an important SOC principle:

> A suspicious request is an indicator that requires investigation; it is not automatically proof of a successful attack.

All activity in this investigation was performed locally in a controlled lab environment.
