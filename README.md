# SOC Analyst L1 Lab Portfolio — Elastic Stack Detections & Incident Reports

I built an Elastic Stack pipeline to collect host logs, create detections, generate alerts, and document incidents like a SOC analyst.

This repo shows the workflow: **telemetry → detection → alert → investigation → documentation**.

---

## Environment (lab)
- **Attacker:** Kali Linux
- **Victim:** Metasploitable3 (Ubuntu)

---

## Architecture
Metasploitable3 → rsyslog → Logstash (syslog/UDP 5514) → Elasticsearch → Kibana (Discover + Security Alerts)

- Index pattern: `metasploitable-*`

---

## Skills demonstrated
- Log collection and normalization (rsyslog → Logstash → Elasticsearch)
- KQL searches and field extraction ideas
- Detection engineering (threshold + correlation-style logic)
- Alert triage and basic incident response workflow
- Documentation: incident summaries, evidence, and recommendations

---

## How to navigate this repo
- **Cases:** `cases/<scenario>/`
  - `detections/` — rules and KQL queries
  - `incidents/` — incident write-ups
  - `screenshots/` — Kibana evidence

### Included cases
- **SSH brute force + success**
  - Detections: `cases/ssh-brutforce/detections/`
  - Incident report: `cases/ssh-brutforce/incidents/INC-001-ssh-bruteforce-success.md`

- **Privilege escalation (sudo to root)**
  - Detections: `cases/privilege-escalation/detections/`
  - Incident report: `cases/privilege-escalation/incidents/INC-002-privilege-escalation.md`

---

## What I did
- Simulated attacks from Kali against Metasploitable3
- Verified events in Kibana Discover
- Created detection rules in Elastic Security and validated alerting
- Wrote short incident reports with timelines, evidence, and recommendations
