Monitoring stack for your kubernetes cluster? Here's my take:

1. Prometheus for metrics
2. Grafana for visualization
3. Alertmanager for alerting
4. Elasticsearch/Opensearch for logging
5. Kibana for logging visualization

I think it's a powerful combo. You can pretty much do most of the monitoring with it. But..

To make things even interesting you can also include:
- Jaeger for tracing
- NewRelic for APM

Why this stack?

• Prometheus is a CNCF graduated project
• Grafana is widely used (my fav).
• Alertmanager integrates seamlessly.
• Elasticsearch/Opensearch is a powerful search & analytics engine.
• Kibana gives beautiful visualizations.

https://www.linkedin.com/pulse/monitoring-spring-boot-project-grafana-cadvisor-node-chaudhari--i5fmf/?trackingId=UiifPdNgRXebUc8n9VIoJQ%3D%3D
