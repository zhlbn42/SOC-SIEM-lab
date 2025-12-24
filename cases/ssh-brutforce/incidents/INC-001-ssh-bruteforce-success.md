# INC-001 — SSH brute force + successful login

## Summary
I noticed a lot of failed SSH logins on Metasploitable3 and then a successful login to `vagrant`.
To me, this looks like either the attacker actually guessed or brute-forced the password, or they used already valid credentials after generating noise with failed attempts.

## Environment
- Victim: metasploitable3-ub1404 (192.168.56.102)
- Attacker IP: 192.168.56.104
- User: vagrant
- Index: metasploitable-*
- Pipeline: rsyslog → Logstash (udp/5514) → Elasticsearch → Kibana (Discover + Security Alerts)

## What happened (timeline)
- Burst of `Failed password` events for user `vagrant`
- Medium alert fired (`SSH Brute Force Attempt`)
- Then I saw `Accepted password` for the same user
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
If this was a real server, a success right after a lot of failures can mean the account got compromised.
After that the next риск — попытки sudo / создание нового юзера / persistence.

## What I would do as a SOC analyst (simple)
1) Check if the attacker IP is expected (admin/VPN range) or unknown.
2) If it’s unknown + there is a success:
   - treat as high priority
   - block the IP (temporary)
   - reset/rotate the `vagrant` creds (or disable the account)
3) Look for post-login activity:
   - sudo usage
   - new users
   - changes to ssh keys / cron
4) Create a short case/ticket with the evidence and escalate if I see any persistence or privilege escalation.

## Improvements (next step)
- Parse `source.ip` and `user.name` into real fields (right now IP/user is mostly inside the message).
- Tune the brute-force rule (suppression/grouping) so it doesn’t spam alerts.
- Later: make a real correlation rule “fail burst → success” by same IP + user within a short time window.
