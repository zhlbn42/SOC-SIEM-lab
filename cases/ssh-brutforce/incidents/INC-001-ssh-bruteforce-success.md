# INC-001 — SSH brute force + successful login

## Summary
I observed a burst of failed SSH logins on Metasploitable3 followed by a successful login to `vagrant`.
This pattern suggests a password was guessed, brute-forced, or already known after noise was generated.

## Environment
- Victim: metasploitable3-ub1404 (192.168.56.102)
- Attacker IP: 192.168.56.104
- User: vagrant
- Index: metasploitable-*
- Pipeline: rsyslog → Logstash (udp/5514) → Elasticsearch → Kibana (Discover + Security Alerts)

## What happened (timeline)
- Burst of `Failed password` events for user `vagrant`
- Medium alert fired (`SSH Brute Force Attempt`)
- Successful login observed for the same user
- High alert fired (`Brute Force → Success`)

## Evidence
### Failed logins
KQL:
`msg:"Failed password"`

Screenshots:
- screenshots/ssh-bruteforce.jpg

### Successful login
KQL:
`msg:"Accepted password"`

Screenshots:
- screenshots/brutoforce-success.jpg

### Alerts
- screenshots/bruteforce-alert-rules.jpg

## Why it matters
A success right after many failures can indicate account compromise. The next risk is post-login activity such as sudo usage, new users, or persistence.

## What I would do as a SOC analyst (simple)
1) Check whether the source IP is expected (admin/VPN range) or unknown.
2) If unknown + success:
   - treat as high priority
   - block the IP (temporary)
   - reset/rotate the `vagrant` creds (or disable the account)
3) Look for post-login activity:
   - sudo usage
   - new users
   - changes to SSH keys / cron
4) Create a short case/ticket with evidence and escalate if I see persistence or privilege escalation.

## Improvements (next step)
- Parse `source.ip` and `user.name` into fields (right now IP/user is mostly inside `message`).
- Tune the brute-force rule (suppression/grouping) to reduce alert spam.
- Add a correlation rule: “fail burst → success” by same IP + user within a short time window.
