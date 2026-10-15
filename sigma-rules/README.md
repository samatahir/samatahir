# Sigma Detection Rules

## Overview

In this repository contains Sigma rules for identifying suspicious activity in logs, including login attacks, unusual process activity, and potential credential access.

## Detection Rules

### 1. SSH Brute-Force Detection

**Objective:** Identify repeated SSH authentication failures that may indicate a brute-force attack.

- **Log source:** Linux authentication logs.
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

- **Sigma:** It is a standard format for writing security detection rules that can be used across different SIEM platforms.
- **Linux Authentication Logs:** SSH authentication monitoring.
- **Windows Event Logs:** Windows security and process activity.
- **Sysmon:** It records activity on Windows systems to help detect suspicious behavior.
- **MITRE ATT&CK:** Classification of adversary tactics & techniques.

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

Rules should be tested with relevant logs to make sure they detect suspicious activity and avoid unnecessary alerts.

Sigma rules may need adjustments or conversion to work properly in SIEM tools like Splunk or Microsoft Sentinel.

## Limitations


These rules help flag suspicious activity but don't automatically confirm an attack. Their effectiveness depends on the logs available and how each rule is configured. Some rules may also need adjustments to reduce false positives and improve detection accuracy.

## Purpose

In this project I am focusing on creating detection rules, analyzing logs, mapping suspicious activity to MITRE ATT&CK, and exploring how these techniques are used to detect and investigate potential threats.
