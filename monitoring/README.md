# Capstone 2: monitoring Robot Shop

## What is here
- `kube-prometheus-values.yaml`: settings for Prometheus, Alertmanager and Grafana
- `probe.yaml`: blackbox probe that checks web, catalogue, cart and payment every 15 seconds
- `dashboard-robot-shop.yaml`: the "Robot Shop Health" dashboard, stored as a ConfigMap
- `loki-values.yaml` and `fluent-bit-values.yaml`: log collection into Loki
- `jaeger.yaml` and `otel-collector.yaml`: tracing (shipping is instrumented with the Java agent)
- `slo.md`: availability SLO, error budget and policy
- `slo-alert.yaml` and `alertmanager-slack.yaml`: the alert and its routing to Slack
- `runbook-availability.md`: what to do when the alert fires
- `postmortem-template.md`: blameless postmortem template

## Open the tools
    kubectl port-forward -n monitoring svc/monitoring-grafana 3000:80
    kubectl port-forward -n tracing svc/jaeger 16686:16686

Grafana: http://localhost:3000, Jaeger: http://localhost:16686
