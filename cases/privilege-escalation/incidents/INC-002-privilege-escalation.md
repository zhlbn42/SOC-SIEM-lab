# INC-002 — Privilege escalation (sudo to root)

## Summary
I saw `vagrant` running commands as `root` via sudo (USER=root; COMMAND=...).
On this host sudo is configured as NOPASSWD, so if someone gets access to `vagrant`,
becoming root is very easy.

## Environment
- Host: metasploitable3-ub1404 (192.168.56.102)
- User: vagrant → root
- Index: metasploitable-*
- Pipeline: rsyslog → Logstash (udp/5514) → Elasticsearch → Kibana

## Evidence (Kibana / Discover)
KQL used:
`program:"sudo" and message:"USER=root" and message:"COMMAND="`

What I found:
- sudo session opened/closed for user root
- commands executed as root, examples:
  - `/usr/bin/id`
  - `/usr/bin/touch /tmp/pe_test`

Screenshots (put them in this folder):
- screenshots/pe-01-discover-sudo-list.jpg
- screenshots/pe-02-discover-command.jpg
- screenshots/pe-03-alert-critical.jpg

## Detection
Rule name: `Privilege-escalation`  
Query:
`program:"sudo" and message:"USER=root" and message:"COMMAND="`

Severity: Critical (lab)  
Reason: root commands via sudo = high impact, and here it's NOPASSWD.

## What I would do as SOC (simple)
- First, check if it’s expected admin activity or not (known IP / change window).
- If it looks suspicious: treat it as high priority because it's already root.
- Containment: block the source IP (temporary) and rotate/reset the affected creds.
- Quick follow-up checks:
  - more sudo commands
  - new users
  - ssh keys changes
  - cron jobs / persistence

## Fix / hardening ideas
- Remove `NOPASSWD: ALL` or limit sudo to specific commands.
- Prefer SSH keys (no password) + rate limit (fail2ban).
- Keep monitoring for post-auth activity (sudo/user changes).
