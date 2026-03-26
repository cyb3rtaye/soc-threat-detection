# Architecture

## Overview

This project is built around an endpoint-focused detection workflow using Sysmon and Wazuh.

---

## Components

### Windows Endpoint

- Generates telemetry  
- Runs attack simulations  

### Sysmon

- Captures process creation  
- Logs command-line activity  
- Records parent-child relationships  

### Wazuh

- Ingests logs  
- Provides visibility  
- Enables analysis  

---

## Data Flow

Simulation → Sysmon Event ID 1 → Wazuh → Analyst

---

## Purpose

The architecture supports detection of:

- PowerShell execution  
- process chains  
- certutil usage  

---

## Outcome

Provides a structured detection pipeline for analysis and validation of attacker behaviour.