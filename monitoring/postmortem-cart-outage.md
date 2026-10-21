# Postmortem: Cart service outage (self-run incident drill)

Blameless: this document is about how the system and our process behaved, not about who made a mistake.

## Summary
During a self-run incident drill on 18 October 2026, the cart service was scaled to zero replicas on purpose. The availability alert fired in Slack, the runbook was followed, and the service was restored.

## Impact
- Services affected: cart only. Web, catalogue and payment kept passing their checks.
- Duration: from about 07:38 until 07:42.
- Customers could not add items to the cart or view it during that time.

## Timeline (local time, taken from Slack)
| Time | What happened |
|---|---|
| 07:38 | Cart scaled to 0 replicas with `kubectl scale deploy/cart -n robot-shop --replicas=0` |
| 07:40 | Alert RobotShopAvailabilityLow fires in Slack for the cart health check. Success rate shown: 60% |
| 07:41 | Runbook opened from the Slack message and followed: step 4, scale the service back to 1 |
| 07:42 | Cart running again, health checks pass |
| 07:46 | Alert resolves in Slack |

## Detection
The alert in Slack, about 3 minutes after the outage began. The Robot Shop Health dashboard also showed the cart line for failed checks rising and its availability dropping.

## Root cause
The cart deployment had zero replicas, so no pod answered the cart health check. This was a deliberate change made to test the alert. In real life the same symptom could come from a crashing pod, a bad release or a failed node.

Evidence: the Slack alert shows `http://cart.robot-shop.svc:8080/health` at 60% success over 5 minutes, and `kubectl get pods -n robot-shop` showed no cart pod while the deployment was at zero.

## Mitigation
Followed the runbook: `kubectl scale deploy/cart -n robot-shop --replicas=1`. Service returned within a minute.

## What went well
- The alert fired by itself and included the failing address and a link to the runbook.
- The "resolved" message arrived automatically.
- The dashboard, runbook and Slack alert pointed at the same service and time window, so the problem was easy to find.

## What went badly
- Detection took about 3 minutes, because the rule averages the last 5 minutes and then waits 1 more.
- The "resolved" message came about 6 minutes after the alert, because of the same 5-minute window.
- The cart runs as a single pod, so losing it means a full outage for that service.

## Where we got lucky
The test was planned and short, so no real customer was affected.

## Action items
| Action | Owner | Due |
|---|---|---|
| Run cart with 2 replicas so one lost pod is not an outage | me | before next demo |
| Add a faster alert using a shorter window, plus a burn-rate alert for the SLO | me | next iteration |
| Add a note in the runbook on how to tell "scaled to zero" from "crashing" | me | next iteration |
