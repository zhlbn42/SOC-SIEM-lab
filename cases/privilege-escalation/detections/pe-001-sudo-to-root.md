# PE-001 — Privilege escalation: sudo to root (Linux)

**Type:** Custom query (Elastic Security)  
**Severity:** Critical (lab)  
**Goal:** Alert when a user runs commands as root via sudo.

## Why this matters
If someone gets an SSH account and can run `sudo` as root, it's basically game over for that host.
In this lab the host allows NOPASSWD sudo, so the risk is high.

## Data source
- index: `metasploitable-*`
- program: `sudo`

## Rule query (KQL)
```kql
program:"sudo" and message:"USER=root" and message:"COMMAND="
```

### Optional (only if you want to narrow it)
Only vagrant:
```kql
program:"sudo" and message:"USER=root" and message:"COMMAND=" and message:" vagrant "
```

## What I expect to see in logs
Examples (from Discover):
- `vagrant : ... USER=root ; COMMAND=/usr/bin/id`
- `vagrant : ... USER=root ; COMMAND=/usr/bin/touch /tmp/...`

## Tuning notes
- If it gets noisy, add suppression (for example 5 minutes) so you don't get spam.
- Better parsing later: extract `user.name`, `process.command_line`, `source.ip` (right now it's inside `message`).
