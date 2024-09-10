Debugging in kubernetes is so much easier with the following techniques and tools

In your K8s journey, you will be spending more than 60% of your time debugging, so lets get good at it. Apart from the kubectl logs, describe and events you need to know the following.

Node Resource Analysis: Pinpoint resource bottlenecks and critical events with kubectl describe node <node-name>

Init Container Logs: 90% startup issues come with init and by inspecting logs of init containers using kubectl logs <pod-name> -c <init-container-name> --previous you will get a very good understanding to what went wrong.

Network Traffic Monitoring: Debug service connectivity directly by forwarding ports: kubectl port-forward svc/<service-name> <local-port>:<service-port>. This will be tricky to do in production envirornments.

Real-time Log Capture: stream and save pod logs locally for detailed analysis: kubectl logs -f <pod-name> >local-output.log

Direct Cluster queries: Access raw internal data by querying the kubelet or API server: kubectl get --raw "/api/v1/nodes"


👉 Apart from kubectl, consider to have the following tool kits in your arsenal

Lens: its a kubernetes IDE that provides a user-friendly graphical interface for managing Kubernetes clusters.

Kube-ops-view: Provides a read-only system dashboards for kubernetes metrics or just use the 100's of default ones

Prometheus and Grafana: create visual dashboards for Kubernetes metrics or just use the 100's of default ones

Istio: use this or any open-source service mesh that provides a way to control how microservices share data
