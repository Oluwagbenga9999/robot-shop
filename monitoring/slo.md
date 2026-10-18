# Robot Shop availability SLO

## What we protect
The shop's four key services: web, catalogue, cart and payment.

## SLI (what we measure)
The share of HTTP health checks that succeed. A blackbox probe checks each service every 15 seconds (`probe_success` in Prometheus).

## SLO (the target)
99% of checks succeed over a rolling 30 days.

## Error budget
The SLO allows 1% failure. Over 30 days (43,200 minutes) that is **432 minutes, or 7 hours 12 minutes**, of failed checks per service.

## Error budget policy
| Budget used | What we do |
|---|---|
| Under 50% | Normal work. Ship changes as usual. |
| 50% to 100% | Slow down. Every release needs a rollback plan, and risky changes wait. |
| 100% (spent) | Freeze feature releases. Only reliability fixes ship until the budget recovers. Write a blameless postmortem. |

## Alerting
`RobotShopAvailabilityLow` pages Slack when a service's success rate stays under 90% for 1 minute. The alert links to the runbook.

## Review
Check the budget on the Robot Shop Health dashboard every week, and after every incident.
