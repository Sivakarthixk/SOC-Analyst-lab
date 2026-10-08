# SOC Investigation 03 — Windows Security Log Analysis

## Investigation ID

**SOC-003**

## Title

**Windows Security Event Log Analysis**

## Objective

The objective of this investigation was to analyze Windows Security Event Logs using Windows Event Viewer and identify successful and failed authentication activities.

The investigation focused mainly on:

* Event ID **4624** — Successful Logon
* Event ID **4625** — Failed Logon
* Logon type and authentication information
* Possible indicators of suspicious authentication activity

## Environment

| Category           | Details                     |
| ------------------ | --------------------------- |
| Operating System   | Windows 11                  |
| Tool               | Windows Event Viewer        |
| Log Analyzed       | Windows Security Log        |
| Investigation Type | Authentication Log Analysis |

---

## 1. Successful Logon Analysis — Event ID 4624

Event ID **4624** indicates that a user account was successfully logged on.

A 4624 event was identified in the Windows Security Log and analyzed to understand the authentication activity.

### Observed Details

| Field                  | Observed Value                |
| ---------------------- | ----------------------------- |
| Event ID               | 4624                          |
| Account Name           | SIVA0315$ 			               |
| Logon Type             | 5                             |
| Workstation Name       | -                             |
| Source Network Address | -                             |
| Authentication Package | Negotiate                     |
| Date and Time          | 08-10-2026 22:48:06           |

### Analysis

The successful logon event confirms that authentication was successfully completed for the identified account.

The **Logon Type** provides additional information about how the authentication occurred. The source address and workstation information can help a SOC analyst determine whether the authentication originated locally or from another system.

Based on the reviewed event, the activity should be compared with expected user activity and normal authentication behavior.

### Screenshot

**Screenshot:** `01-successful-login.png`

This screenshot shows the Event ID 4624 successful authentication event.

---

## 2. Failed Logon Analysis — Event ID 4625

Event ID **4625** indicates that a logon attempt failed.

A 4625 event was identified in the Windows Security Log and analyzed to understand the reason for the failed authentication.

### Observed Details

| Field                  | Observed Value                  |
| ---------------------- | --------------------------------|
| Event ID               | 4625                            |
| Account Name           | SIVA0315$ 			                 |
| Failure Reason         | Unknown username or bad password|
| Logon Type             | 2                               |
| Workstation Name       | -                               |
| Source Network Address | -                               |
| Date and Time          | 08-10-2026 20:56:02             |

### Analysis

The failed logon event indicates that an authentication attempt was unsuccessful.

A single failed authentication attempt does not automatically indicate malicious activity. It may occur because of an incorrect password, an expired credential, a mistyped username, or another legitimate reason.

However, repeated 4625 events occurring within a short period, particularly from an unexpected source or against multiple accounts, could indicate possible password-guessing or brute-force activity.

The event should therefore be correlated with additional authentication and network logs before determining whether the activity is malicious.

### Screenshot

**Screenshot:** `03-failed-login.png`

This screenshot shows the Event ID 4625 failed authentication event.

---

## 3. Security Analysis

The investigation demonstrated how Windows Security logs can be used to monitor authentication activity.

The main observations were:

1. **Event ID 4624** confirmed successful authentication activity.
2. **Event ID 4625** confirmed that at least one authentication attempt failed.
3. Authentication events contain useful information such as logon type, account name, workstation, and source information.
4. Repeated failed authentication events can be investigated as potential indicators of brute-force or password-guessing activity.
5. Authentication events should be correlated with additional security logs before classifying an event as malicious.

---

## 4. SOC Analyst Perspective

From a SOC analyst perspective, authentication logs are important for detecting suspicious account activity.

A typical investigation workflow would be:

**Detect → Validate → Analyze → Correlate → Determine Severity → Respond → Document**

For a suspicious 4625 event, a SOC analyst could investigate:

* Number of failed attempts
* Time interval between attempts
* Targeted username
* Source IP address
* Logon type
* Whether a successful 4624 occurred after multiple 4625 events
* Other activity from the same source

This helps determine whether the activity is normal user behavior or a possible security incident.

---

## 5. Recommendations

The following security practices are recommended:

* Monitor repeated failed authentication attempts.
* Investigate unusual login times and locations.
* Use strong and unique passwords.
* Enable multi-factor authentication where available.
* Monitor privileged account authentication.
* Correlate authentication logs with endpoint and network security logs.
* Use a SIEM platform to centralize and alert on suspicious authentication activity.

---

## 6. Conclusion

This investigation provided hands-on experience analyzing Windows Security Event Logs using Event Viewer.

Event IDs **4624** and **4625** were successfully identified and analyzed to understand successful and failed authentication activity.

The investigation demonstrated an important SOC Analyst skill: examining authentication logs, identifying potentially suspicious behavior, and using contextual information before determining whether an event represen
