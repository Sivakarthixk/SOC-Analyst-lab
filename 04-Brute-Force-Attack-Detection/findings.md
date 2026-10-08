# SOC Investigation 04 — Brute-Force Attack Detection

## Investigation ID

**SOC-004**

## Title

**Brute-Force Attack Detection Using Windows Security Logs**

## Objective

The objective of this investigation was to simulate repeated failed authentication attempts on a Windows system and analyze the resulting Security Event Logs.

The investigation focused on identifying multiple **Event ID 4625** events occurring within a short period and understanding how a SOC analyst can use authentication logs to detect potential brute-force or password-guessing activity.

## Environment

| Category           | Details                           |
| ------------------ | --------------------------------- |
| Operating System   | Windows 11                        |
| Tool               | Windows Event Viewer              |
| Log Analyzed       | Windows Security Log              |
| Event ID           | 4625 — Failed Logon               |
| Investigation Type | Authentication / Threat Detection |
| Activity Type      | Controlled Lab Simulation         |

---

## 1. Brute-Force Simulation

Four incorrect authentication attempts were intentionally generated on the Windows workstation to simulate repeated failed login activity.

Windows recorded each failed authentication attempt as **Event ID 4625**.

The four events occurred within approximately **11 seconds**, creating a clear cluster of failed authentication events.

### Observed Events

| Event | Account   | Logon Type | Source    | Timestamp           |
| ----- | --------- | ---------: | --------- | ------------------- |
| 1     | SIVA0315$ |          2 | 127.0.0.1 | 08-10-2026 23:50:05 |
| 2     | SIVA0315$ |          2 | 127.0.0.1 | 08-10-2026 23:50:08 |
| 3     | SIVA0315$ |          2 | 127.0.0.1 | 08-10-2026 23:50:13 |
| 4     | SIVA0315$ |          2 | 127.0.0.1 | 08-10-2026 23:50:16 |

### Screenshot

**Screenshot:** `01-failed-login-events.png`

This screenshot shows the multiple Event ID 4625 entries recorded during the controlled simulation.

---

## 2. Event Details Analysis

A representative Event ID 4625 event was examined in detail.

### Observed Details

| Field                  | Value                           |
| ---------------------- | ------------------------------- |
| Event ID               | 4625                            |
| Account Name           | SIVA0315$                       |
| Failure Reason         | An Error occurred during Logon. |
| Logon Type             | 2                               |
| Workstation Name       | -                               |
| Source Network Address | 127.0.0.1                       |

### Analysis

The events indicate unsuccessful authentication attempts.

The account name **SIVA0315$** ends with a `$`, which indicates a Windows computer account rather than a typical human user account.

The **Logon Type 2** represents an interactive logon attempt.

The source address **127.0.0.1** is the local loopback address, indicating that the activity originated locally from the same system rather than from a remote network address.

### Screenshot

**Screenshot:** `02-event-details.png`

This screenshot shows the details of the Event ID 4625 authentication failure.

---

## 3. Attack Pattern Analysis

The four failed authentication events occurred between:

**23:50:05 and 23:50:16**

This represents four failed logon events within approximately **11 seconds**.

The close timing creates a recognizable authentication-failure pattern that a SOC monitoring system could detect.

### Pattern

```text
23:50:05 → 4625
23:50:08 → 4625
23:50:13 → 4625
23:50:16 → 4625
```

The repeated events demonstrate how multiple authentication failures can be identified from Windows Security logs.

### Important Finding

Because these failed logons were **intentionally generated as part of this lab**, they should not be classified as an actual brute-force attack.

Instead, they represent a **controlled brute-force simulation** used to demonstrate how a SOC analyst could detect and investigate this type of activity.

### Screenshot

**Screenshot:** `03-attack-pattern.png`

This screenshot shows the timestamps of the repeated failed authentication events.

---

## 4. Security Analysis

The investigation demonstrated that Windows Security Event Logs can provide useful evidence for detecting repeated authentication failures.

The main observations were:

1. Four **Event ID 4625** events were successfully generated.
2. All four events occurred within approximately **11 seconds**.
3. All events involved the same account: **SIVA0315$**.
4. All events used **Logon Type 2**.
5. The source address was **127.0.0.1**, indicating local activity.
6. The repeated events demonstrate a pattern that could be used to create a brute-force detection rule.
7. Because the activity was intentionally generated, it was classified as a controlled security-lab simulation rather than a real attack.

---

## 5. SOC Analyst Perspective

A SOC analyst can use authentication logs to identify patterns that may indicate password guessing or brute-force activity.

Important indicators include:

* Multiple failed logons within a short period
* Repeated attempts against the same account
* Attempts against multiple accounts
* Unusual source IP addresses
* Repeated failures followed by a successful logon
* Unusual authentication times
* Remote logon attempts from unexpected systems

A typical detection workflow is:

**Detect → Validate → Analyze → Correlate → Determine Severity → Respond → Document**

For a real-world investigation, the analyst should correlate the failed logon events with other endpoint, network, and authentication logs before determining whether the activity is malicious.

---

## 6. Recommendations

The following security practices can help detect and reduce brute-force attacks:

* Monitor repeated Event ID 4625 failures.
* Configure alerts for excessive authentication failures.
* Investigate unusual authentication patterns.
* Use strong and unique passwords.
* Enable multi-factor authentication where available.
* Implement account lockout policies appropriately.
* Monitor privileged accounts closely.
* Correlate authentication events with network and endpoint logs.
* Use a SIEM platform to centralize authentication logs and generate alerts.

---

## 7. Conclusion

This investigation successfully demonstrated how repeated failed authentication attempts can be detected using Windows Security Event Logs.

Four Event ID 4625 events were generated within approximately 11 seconds during a controlled lab simulation. The events were analyzed based on their account, logon type, source address, failure reason, and timestamps.

The investigation provided practical experience with an important SOC Analyst task: identifying authentication-failure patterns and distinguishing between a controlled test and potentially malicious activity.

## Evidence

### Screenshots

* `01-failed-login-events.png` — Multiple Event ID 4625 events
* `02-event-details.png` — Event ID 4625 details
* `03-attack-pattern.png` — Authentication failure timeline
