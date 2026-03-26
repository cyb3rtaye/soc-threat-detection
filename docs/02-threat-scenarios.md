# Threat Scenarios

## Objective

This phase defines the simulated attacker behaviours used in this lab. The goal is to replicate realistic techniques that generate meaningful telemetry for detection and analysis.

---

## Scenario 1: PowerShell Execution

### Description

PowerShell is frequently used by attackers due to its flexibility and ability to execute commands and scripts.

### Technique

- PowerShell execution via command line  
- Potential use of encoded commands  

### MITRE ATT&CK Mapping

- Tactic: Execution  
- Technique: Command and Scripting Interpreter – PowerShell (T1059.001)  

---

## Scenario 2: Suspicious Process Chain

### Description

Unusual parent-child relationships can indicate malicious activity, particularly when one process launches another unexpectedly.

### Technique

- Command shell launching PowerShell  

### MITRE ATT&CK Mapping

- Tactic: Execution  
- Technique: Command and Scripting Interpreter (T1059)  

---

## Scenario 3: Living Off The Land (certutil)

### Description

Attackers often use legitimate system binaries to perform malicious actions. certutil is commonly abused for downloading files.

### Technique

- Use of certutil for file retrieval  

### MITRE ATT&CK Mapping

- Tactic: Command and Control  
- Technique: Ingress Tool Transfer (T1105)  

---

## Purpose

These scenarios are designed to generate realistic activity that can be detected using Sysmon and analysed within Wazuh.

They provide a foundation for detection engineering and tuning in subsequent phases.