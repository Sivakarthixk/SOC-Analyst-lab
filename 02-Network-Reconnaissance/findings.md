# Network Reconnaissance — Nmap

## 1. Investigation Overview

**Investigation ID:** SOC-002

**Tool:** Nmap

**Objective:**
Identify open TCP ports and running services on an authorized local workstation.

## 2. Target

**Target:** My own Windows workstation

**IP Address:** 10.236.182.192

## 3. Basic Port Scan

**Command:**

`nmap 10.236.182.192`

The scan was performed against my own workstation to identify accessible TCP ports.

## 4. Port Findings

| Port | Protocol | State | Service    | 
|------|----------|-------|------------|
| 135  |   tcp    | open  |msrpc       |
| 139  |   tcp    | open  |netbios-ssn |
| 445  |   tcp    | open  |microsoft-ds|

## 5. Service Detection

**Command:**

`nmap -sV 10.236.182.192`

Service detection was performed to obtain additional information about services associated with the identified open ports.

## 6. Security Analysis

Open ports represent network services that may accept incoming connections. Their security significance depends on whether the service is required, properly configured, and protected.

The identified services should be reviewed to determine whether they are necessary and whether they are exposed beyond the intended network.

## 7. Recommendations

* Disable unnecessary network services.
* Restrict access to required services using firewall rules.
* Keep installed services and operating systems updated.
* Monitor unexpected port-scanning activity.
* Regularly review exposed services.

## 8. Conclusion

The Nmap investigation identified accessible TCP ports and associated services on the authorized workstation. Service enumeration provided additional information that can be used by a security analyst to assess the workstation's network exposure.
