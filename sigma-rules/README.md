# Sigma Detection Rules

## Overview

This repository contains Sigma detection rules designed to identify suspicious activity in system logs and support security monitoring, threat detection, and incident investigation. The rules focus on authentication attacks, suspicious process execution, and potential credential access.

## Detection Rules

### 1. SSH Brute-Force Detection

**Objective:** Identify repeated SSH authentication failures that may indicate a brute-force attack.

- **Log source:** Linux authentication logs
- **Detection logic:** Searches for failed SSH login attempts that may indicate password-guessing activity.
- **MITRE ATT&CK:** [T1110.001 — Password Guessing](https://attack.mitre.org/techniques/T1110/001/)
- **Investigation:** Review source IP addresses, timestamps, targeted accounts, and successful logins following repeated failures.

### 2. Suspicious Office Macro Execution

**Objective:** Identify potentially malicious process activity originating from Microsoft Office applications.

- **Log source:** Windows process creation events, such as Sysmon Event ID 1
- **Detection logic:** Looks for suspicious child processes launched by Office applications, such as `WINWORD.EXE` or `EXCEL.EXE`, including command interpreters.
- **MITRE ATT&CK:** [T1204.002 — Malicious File](https://attack.mitre.org/techniques/T1204/002/) and [T1059 — Command and Scripting Interpreter](https://attack.mitre.org/techniques/T1059/)
- **Investigation:** Review parent-child process relationships, command-line arguments, file paths, and related endpoint activity.

### 3. LSASS Access Detection

**Objective:** Identify potentially suspicious processes attempting to access the Local Security Authority Subsystem Service (LSASS), which handles sensitive Windows authentication information.

- **Log source:** Sysmon Event ID 10 (Process Access), when configured to capture relevant activity
- **Detection logic:** Looks for process-access events involving `lsass.exe` that may indicate attempted credential access.
- **MITRE ATT&CK:** [T1003.001 — LSASS Memory](https://attack.mitre.org/techniques/T1003/001/)
- **Investigation:** Examine the source process, access rights, process path, user context, and related security alerts.

## Technologies & Frameworks

- **Sigma:** Platform-independent detection rule format
- **Linux Authentication Logs:** SSH authentication monitoring
- **Windows Event Logs:** Windows security and process activity
- **Sysmon:** Detailed endpoint telemetry for process creation and access events
- **MITRE ATT&CK:** Classification of adversary tactics and techniques

## Repository Structure

```text
sigma-rules/
├── README.md
└── rules/
    ├── ssh_brute_force.yml
    ├── office_macro_execution.yml
    └── lsass_access.yml
```

## Testing & Validation

Each rule should be evaluated against its intended log source to verify field mappings, detection conditions, and expected results. Testing should include relevant suspicious activity and legitimate behavior to identify potential false positives.

Sigma rules may require backend-specific field mappings or conversion before use in a SIEM such as Splunk or Microsoft Sentinel.

## Limitations

These rules identify patterns associated with potentially suspicious behavior; they do not independently confirm malicious activity. Detection effectiveness depends on available telemetry, rule logic, and logging configuration. Threshold-based detection, exclusions, and alert tuning may be necessary for operational use.

## Purpose

This project demonstrates the development and documentation of detection logic, log analysis, MITRE ATT&CK mapping, and investigation considerations relevant to security operations and detection engineering.

**Disclaimer:** These rules are intended for educational and defensive security purposes. Validate them in a controlled environment before deploying them in production.
