# SOC Project (Elastic Stack) — Metasploitable3 Attacks, Detections, Incident Report

I built an Elastic Stack pipeline to collect host logs, create detections, generate alerts, and document incidents like a SOC analyst.

This repository demonstrates a SOC workflow: telemetry collection, detection engineering, alerting, investigation, and incident documentation using controlled attack simulations.

---

## Environment (this lab)
- **Attacker:** Kali Linux
- **Victim:** Metasploitable3 (Ubuntu)

---

## Architecture
Metasploitable3 → rsyslog → Logstash (syslog/UDP 5514) → Elasticsearch → Kibana (Discover + Security Alerts)

- Index pattern: `metasploitable-*`

---

## What I did
- Using Kali Linux, I simulated various attacks against Metasploitable3
- Verified the events in Kibana Discover 
- Created detection rules in Elastic Security and validated that alerts trigger
- Wrote a short incident report with timeline, evidence, and response recommendations
