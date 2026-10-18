# AI Assistance Disclosure Log (AI_LOG.md)

**Course:** EGN321 - Module 7 Assignment 7.1  
**Project:** Unattended System: Monitor, Alert, Log, and Recover  
**Author:** Faustino Fernandez  
**Repository:** https://github.com/faustinofernandezatlantis/EGN321-Module7  

---

## Overview

In accordance with course academic integrity guidelines, this document discloses the extent and nature of AI assistance utilized during the development, testing, and documentation of this project.

---

## Summary of AI Contributions

AI tools were engaged as technical collaborators for the following tasks:
1. **Architecture & Schema Design:** Guidance on modularizing multi-process applications (Flask Dashboard, SQLite DB Manager, Sensor Generator, and Monitor Engine).
2. **Noise-Control Logic Implementation:** Writing Python algorithms for persistence counters and cooldown timers.
3. **Frontend Visualization:** Structuring responsive Bootstrap 5 templates and Chart.js dynamic polling scripts.
4. **Environment & Supervisor Configuration:** Creating `supervisord.conf` and background process management scripts for unattended startup and auto-recovery.
5. **Documentation & Testing:** Structuring `README.md` and defining simulation steps to capture required assignment evidence.

---

## Prompt & Interaction Audit Log

### Interaction 1: Architecture & Unattended Design
- **User Prompt:** "How do I structure an unattended system with Flask, SQLite, a sensor simulator, and automatic recovery for Module 7 assignment?"
- **AI Output:** Recommended modular separation of concerns (`app.py`, `database.py`, `monitor_engine.py`, `sensor_generator.py`) and provided a `supervisord.conf` configuration with `autorestart=true`.
- **Student Verification:** Implemented UTC timestamp handling in SQLite and verified schema consistency with Module 6 specifications.

### Interaction 2: Threshold Alert & Stale Data Logic
- **User Prompt:** "Write Python code for threshold alerts using persistence (3 consecutive readings) and cooldown (30s) along with 15s stale data detection."
- **AI Output:** Provided the `MonitorEngine` class implementation containing time-difference calculations and state management (`HEALTHY`, `DEGRADED`, `RECOVERING`).
- **Student Verification:** Tested threshold values (75.0 °C) and verified correct state transitions in the SQLite `system_logs` table.

### Interaction 3: Cloud Deployment & Recovery Verification
- **User Prompt:** "How can I execute and test process recovery in Google Colab while exposing the Flask dashboard?"
- **AI Output:** Provided background execution commands using `nohup` / `supervisord` and port exposure via proxy utilities (`google.colab.output.eval_js`).
- **Student Verification:** Executed process termination commands (`pkill`), verified automatic process recovery, and captured screenshot evidence.

---

## Declaration

All code, configurations, and documentation provided by AI assistance were independently reviewed, executed, and validated by the student on the target runtime environment prior to final submission.