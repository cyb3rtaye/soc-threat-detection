# Detection Engineering

## Objective

To define the detection logic used to identify suspicious behaviour generated during the threat simulation scenarios.

This phase focuses on turning raw telemetry into meaningful detections that support SOC monitoring and investigation.

---

## Detection Philosophy

Detection engineering is not simply about producing alerts. The goal is to create detections that are:

- relevant to attacker behaviour  
- grounded in available telemetry  
- understandable to analysts  
- useful for investigation and triage  

In this lab, detections are based on endpoint telemetry collected by Sysmon and centralised through Wazuh.

---

## Telemetry Source

### Sysmon

Sysmon provides detailed process execution telemetry, including:

- process creation  
- command-line arguments  
- parent-child process relationships  
- execution context  

This makes it a strong source for detecting suspicious behaviour associated with execution, abuse of native tools, and script-based activity.

---

## Detection 1: PowerShell Execution

### Description

PowerShell is a legitimate administrative tool but is frequently abused by attackers to execute commands, run scripts, and perform post-exploitation activity.

### Detection Logic

Identify process creation events where:

- the image is `powershell.exe`
- PowerShell is launched directly from the command line
- command-line arguments suggest scripted or non-interactive execution

### Why It Matters

PowerShell is one of the most common execution mechanisms used in real-world attacks. Detecting its use is important even when the activity appears basic, because it often forms part of a broader attack chain.

### MITRE ATT&CK Mapping

- Tactic: Execution  
- Technique: Command and Scripting Interpreter: PowerShell (T1059.001)  

---

## Detection 2: Suspicious Parent-Child Process Chain

### Description

Unexpected parent-child relationships can indicate malicious or abnormal behaviour, particularly when one shell launches another process in a way that is inconsistent with normal user behaviour.

### Detection Logic

Identify process creation events where:

- `cmd.exe` launches `powershell.exe`
- the parent-child relationship suggests chained shell execution
- the behaviour indicates possible scripted activity or command proxying

### Why It Matters

Process chains help analysts understand execution context. A suspicious parent-child relationship can be more valuable than a single process event viewed in isolation.

### MITRE ATT&CK Mapping

- Tactic: Execution  
- Technique: Command and Scripting Interpreter (T1059)  

---

## Detection 3: certutil Abuse

### Description

certutil is a legitimate Windows utility, but it is also a well-known LOLBin (Living Off the Land Binary) that attackers abuse for downloading or encoding files.

### Detection Logic

Identify process creation events where:

- the image is `certutil.exe`
- the command line includes file retrieval or URL-related parameters
- the activity suggests non-standard administrative usage

### Why It Matters

LOLBins are particularly important in detection engineering because they blend malicious activity with trusted tools that already exist on the operating system.

### MITRE ATT&CK Mapping

- Tactic: Command and Control  
- Technique: Ingress Tool Transfer (T1105)  

---

## Analyst Considerations

These detections should not be treated as equal in all situations. Context matters.

For example:

- PowerShell may be benign in an administrative environment  
- certutil may have legitimate uses  
- shell-to-shell execution may occur during troubleshooting  

However, these behaviours become more valuable when:
- observed unexpectedly  
- repeated frequently  
- combined with other suspicious activity  
- associated with unusual users or timing  

---

## Outcome

This phase defines a set of detections grounded in realistic attacker behaviour and relevant telemetry. These detections will be used in the next phase to drive controlled attack simulations and validate whether the lab can surface meaningful security signals.