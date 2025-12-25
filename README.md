# SOC Project (Elastic Stack) — Metasploitable3 Attacks, Detections, Incident Report

I built an Elastic Stack pipeline to collect host logs, create detections, generate alerts, and document incidents like a SOC analyst.

This repo shows the workflow: **telemetry → detection → alert → investigation → documentation**.

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
- Simulated SSH attack activity from **Kali** against **Metasploitable3**
- Verified the events in Kibana Discover 
- Created detection rules in Elastic Security and validated that alerts trigger
- Wrote a short incident report with timeline, evidence, and response recommendations
