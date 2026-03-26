# Detection Tuning

## Objective

To refine and improve the quality of detections generated during the attack simulation.

This involves reducing noise, increasing contextual awareness, and improving the usefulness of alerts for SOC analysts.

---

## Why Detection Tuning Matters

Raw detections often generate a high volume of alerts, many of which may be benign. Without tuning, this can lead to alert fatigue and reduced effectiveness of monitoring.

Detection tuning ensures that alerts:

- provide meaningful context  
- highlight genuinely suspicious behaviour  
- support efficient investigation  

---

## Tuning Approach

The following strategies were applied to improve detection quality:

- analysing command-line arguments  
- evaluating parent-child process relationships  
- distinguishing between normal and abnormal usage  
- focusing on behaviour rather than single events  

---

## Tuned Detection 1: PowerShell Execution Context

### Original Detection

- Any execution of `powershell.exe`

### Limitation

- PowerShell is widely used for legitimate administrative tasks  
- High potential for false positives  

### Tuning Strategy

Focus on suspicious characteristics:

- non-interactive execution  
- unusual command-line arguments  
- execution outside expected administrative context  

### Improved Detection Logic

Identify PowerShell execution where:

- command-line arguments indicate scripted activity  
- execution is not initiated by typical user behaviour  
- activity appears automated or chained  

### Analyst Insight

Not all PowerShell usage is suspicious. Context is critical. Analysts should prioritise PowerShell activity that deviates from normal operational patterns.

---

## Tuned Detection 2: Parent-Child Process Relationship

### Original Detection

- `cmd.exe` launching `powershell.exe`

### Limitation

- This behaviour can occur during legitimate troubleshooting  

### Tuning Strategy

Enhance detection by combining:

- parent-child relationship  
- execution frequency  
- timing and user context  

### Improved Detection Logic

Flag events where:

- `cmd.exe` launches `powershell.exe` repeatedly  
- execution occurs in quick succession  
- behaviour is inconsistent with normal user activity  

### Analyst Insight

Process chains provide valuable context. Repeated or automated chaining is more indicative of suspicious behaviour than a single occurrence.

---

## Tuned Detection 3: certutil Usage

### Original Detection

- Execution of `certutil.exe`

### Limitation

- certutil is a legitimate Windows utility  
- occasional use may be benign  

### Tuning Strategy

Focus on command-line indicators:

- URL usage  
- file download behaviour  
- non-standard usage patterns  

### Improved Detection Logic

Identify certutil execution where:

- command line includes external URLs  
- file retrieval behaviour is present  
- activity is not part of routine system administration  

### Analyst Insight

LOLBins are powerful detection opportunities. When used outside normal context, they often indicate attacker activity attempting to blend in with legitimate tools.

---

## MITRE ATT&CK Alignment

The tuned detections align with the following techniques:

- T1059.001 — PowerShell  
- T1059 — Command and Scripting Interpreter  
- T1105 — Ingress Tool Transfer  

This mapping ensures that detections are aligned with recognised adversary behaviours.

---

## Outcome

Detection tuning improved the overall quality of alerts by reducing noise and increasing contextual relevance.

The tuned detections are more aligned with realistic attacker behaviour and provide stronger signals for SOC investigation.

This demonstrates the ability to move beyond basic alerting and towards effective detection engineering practices.