# Detection: Brute Force → Success (High)

## Idea
A successful login right after a burst of failed logins is more suspicious than a normal login.

## Data source
- index: `metasploitable-*`
- program: `sshd`

## KQL
```kql
msg : "Accepted password"
```

## Analyst note
When this fires, I immediately check the previous 5–10 minutes for `Failed password`
from the same source IP and user.

## Follow-up improvement
- Create a correlation rule that links fail bursts to a success within a short window.
