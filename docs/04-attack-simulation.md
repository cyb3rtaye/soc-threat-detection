# Attack Simulation

## Objective

To simulate realistic attacker behaviours based on the defined threat scenarios and validate whether the configured telemetry and detections capture the activity.

---

## Simulation 1: PowerShell Execution

### Description

PowerShell was executed to simulate command-based activity commonly observed in attacker behaviour.

### Command Used

```powershell
powershell.exe -Command "Get-Process"

## Expected Behaviour

Sysmon generates Event ID 1
Wazuh ingests the process creation event

### Evidence

[View full image](../assets/screenshots/powershell-execution.png)

![PowerShell Execution](../assets/screenshots/powershots/powershell-execution.png)

Simulation 2: Suspicious Process Chain
Description

A command shell was used to launch PowerShell, creating a parent-child process chain.

Command Used
cmd.exe /c powershell.exe

## Expected Behaviour

Sysmon logs process creation
Parent process: cmd.exe
Child process: powershell.exe

### Evidence

[View full image](../assets/screenshots/process-chain.png)

![Process Chain cmd to PowerShell](../assets/screenshots/process-chain.png)

## Simulation 3: certutil Abuse

## Description

certutil was used to simulate Living Off the Land behaviour.

## Command Used
certutil.exe -urlcache -split -f http://example.com file.txt

## Expected Behaviour
Sysmon logs execution of certutil
Command-line arguments visible in event data

### Evidence

[View full image](../assets/screenshots/certutil-activity.png)

![certutil Activity](../assets/screenshots/certutil-activity.png)

Observation Points
Endpoint (Sysmon)

Event Viewer:
Applications and Services Logs → Microsoft → Windows → Sysmon → Operational

SIEM (Wazuh)
sudo tail -f /var/ossec/logs/alerts/alerts.log

## Outcome

All simulated behaviours were successfully executed and captured by Sysmon as process creation events.

The generated telemetry was ingested into Wazuh, confirming visibility across the endpoint and SIEM layers.

The simulations demonstrate that the lab environment is capable of capturing:

- direct command execution  
- chained process behaviour  
- use of native Windows utilities (LOLBins)  

This establishes a reliable foundation for detection tuning and validation in subsequent phases.