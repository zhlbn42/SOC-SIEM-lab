# Detection: SSH Brute Force Attempt (Threshold)

## Idea
Too many `Failed password` events in a short time = brute-force behavior.

## Data source
- index: `metasploitable-*`
- program: `sshd`

## KQL
```kql
msg : "Failed password"
```

(optional)
```kql
msg : "Failed password" and program : "sshd"
```

## Rule
- Type: Threshold
- Example: >= 10 events within 1 minute
- Look-back: at least 1 minute (recommended by Elastic docs)

## Tuning notes
- If the rule is noisy, add suppression by `source.ip` or `user.name` (once parsed).
- Consider a higher threshold for internet-facing hosts.
