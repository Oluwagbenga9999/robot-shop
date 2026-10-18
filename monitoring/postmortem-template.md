# Postmortem: <short title of the incident>

Blameless: this document is about how the system and our process failed, not about who made a mistake.

## Summary
One or two sentences: what broke, for how long, and who or what was affected.

## Impact
- Which services were affected:
- How long (start to end):
- What users could not do:

## Timeline (all times in the same timezone)
| Time | What happened |
|---|---|
|  | Incident starts (from the dashboard or logs) |
|  | Alert fires in Slack |
|  | We start investigating |
|  | Mitigation applied |
|  | Service recovers, alert resolves |

## Detection
How did we find out? Was it the alert, the dashboard or a person? How long after the start?

## Root cause
What actually caused it? Use the logs (Loki), the trace (Jaeger) and `kubectl describe` to back it up.

## Mitigation
What did we do to restore service? Did we follow the runbook?

## What went well

## What went badly

## Where we got lucky

## Action items
| Action | Owner | Due |
|---|---|---|
|  |  |  |
