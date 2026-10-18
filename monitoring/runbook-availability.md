# Runbook: RobotShopAvailabilityLow

**Meaning:** a Robot Shop service is failing its health checks. Customers may be unable to browse, add to cart or pay.

## 1. Mitigate first
Get service back before looking for the cause.

1. See what is broken:
       kubectl get pods -n robot-shop
2. If a pod is crashing or stuck, restart it:
       kubectl rollout restart deploy/<service> -n robot-shop
3. If it broke after a release, roll back:
       helm history robot-shop -n robot-shop
       helm rollback robot-shop <last-good-revision> -n robot-shop --wait
4. If a service was scaled to zero, scale it back to 1:
       kubectl scale deploy/<service> -n robot-shop --replicas=1

## 2. Find the cause
- Grafana, Robot Shop Health dashboard: which service, since when?
- Grafana Explore with Loki: `{service="<service>"} |~ "(?i)error"`
- Jaeger: search the service for slow or failing traces.
- `kubectl describe pod <pod> -n robot-shop` and read the Events at the bottom.

## 3. After it is fixed
Confirm the alert resolves in Slack, then write a blameless postmortem.
