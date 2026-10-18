
```
# Unattended System: Monitor, Alert, Log, and Recover

**Course:** EGN321 - Module 7 Assignment 7.1  
**Author:** Faustino Fernandez  
**Repository:** https://github.com/faustinofernandezatlantis/EGN321-Module7  
**Previous Modules:**  
- [Module 6.1 Repository](https://github.com/faustinofernandezatlantis/EGN321-Module6.1.git)  
- [Module 6.2 Repository](https://github.com/faustinofernandezatlantis/EGN321-Module6.2.git)  

---

## Overview

This application functions as an unattended monitoring service designed to process continuous sensor data streams, detect input degradation or staleness, raise noise-controlled threshold alerts, maintain historical records, and automatically recover from unexpected process failures without manual intervention.

```text
Sensor / Replay Source
        ↓
   Validation
        ↓
  SQLite History
        ↓
   Alert Logic
        ↓
 Flask Dashboard
        ↓
 Logs + Recovery

```

---

## How to Start the System

### Prerequisites

Install the required dependencies:

```bash
pip install Flask

```

### Local / Standard Execution

Start the system using the process manager or individual background services:

```bash
python3 app.py &
python3 monitor_engine.py &
python3 sensor_generator.py &

```

### Unattended Recovery Execution (Process Supervision)

When using Supervisord as the process supervisor:

```bash
supervisord -c supervisord.conf

```

Access the Flask web interface at `http://localhost:5000` (or via the proxy URL when running in hosted environments like Google Colab).

---

## Automatic Restart & Recovery Mechanism

The application achieves unattended recovery through process segregation and state tracking:

* **Process Supervisor:** `supervisord` (or background supervisor scripts) monitors the execution of `app.py`, `monitor_engine.py`, and `sensor_generator.py`.
* **Auto-Restart Policy:** If any process terminates unexpectedly (`kill -9`), the process supervisor automatically re-spawns the terminated script.
* **State Preservation:** Upon restart, the system transitions briefly through the `RECOVERING` state while reading the existing SQLite database (`sensor_history.db`), preserving historical sensor data and audit logs across process crashes.

---

## System Specifications & Threshold Rules

| Parameter | Specification |
| --- | --- |
| **Input Data Source** | Synthetic Sensor Stream Simulator (`sensor_generator.py`) writing to SQLite. |
| **Sampling Interval** | **5 seconds** |
| **Stale Data Rule** | Sensor input is flagged as `STALE` if no new valid reading arrives for **> 15 seconds**. |
| **Alert Threshold** | Triggered when temperature reaches or exceeds **75.0 °C**. |
| **Alert Control Technique** | **Persistence:** Requires **3 consecutive readings** above threshold before triggering.<br>

<br>**Cooldown:** Enforces a **30-second window** between consecutive alert events to eliminate noise. |
| **System Health States** | `HEALTHY`: System operational, fresh sensor readings flowing.<br>

<br>`DEGRADED`: Active high-temperature alert or stale/missing sensor inputs.<br>

<br>`FAILED`: Database connection loss or process initialization failure.<br>

<br>`RECOVERING`: Process startup and state re-initialization phase. |
| **Log Storage** | Plaintext log file (`app_activity.log`) and structured SQLite audit table (`system_logs`). |

---

## Auto-Recovery Capabilities Matrix

### Automatically Recoverable Conditions

* Process termination or unhandled exceptions (`kill -9` interventions).
* Temporary sensor input pauses (detected as `STALE`, auto-restores to `HEALTHY` when data resumes).
* Server reboots when configured with an auto-restart supervisor policy.

### Human Intervention Required

* Physical disk space exhaustion or permission access failures.
* Unrecoverable SQLite database file corruption.
* Code syntax errors or missing system environment dependencies.

---

## Repository Structure

```text
EGN321_Module7_Assignment7_1/
├── app.py                  # Flask Web Server & API Endpoints
├── database.py             # SQLite Schema & Query Interface
├── monitor_engine.py       # Health Engine, Stale Logic & Alert Logic
├── sensor_generator.py     # Sensor Replay / Stream Simulator
├── config.py               # System Thresholds & Parameters
├── supervisord.conf        # Process Manager / Auto-Restart Configuration
├── requirements.txt        # Python Dependencies
├── README.md               # System Documentation
├── AI_LOG.md               # AI Disclosure Log
└── screenshots/            # Evidence Screenshots
    ├── dashboard_healthy.png
    ├── alert_active.png
    ├── alert_cleared.png
    ├── stale_data.png
    └── system_logs_recovery.png

```
