# Common node Problems

## Memory Pressure

Memory Pressure is encountered when a nodes's memory is nearly exhausted, leading to potential pod eviction. You can dignose this with

```code
kubectl top nodes
```

## Disk Pressure
Disk pressure is trigger when available disk space is low, leading to pod eviction or performance degradation. Dignose this by describing the node:

```code
kubestl describe node
Conditions:
Type              Status
----              ------
DiskPressure      True
MemoryPressure    False
```

## Node Not Ready States
A 'Not Ready' state means the node cannot host pods. in Particular, this could be due to network issues, kubelet crashes or resource exhaustion. to chck the status.

```code
kubectl get nodes
NAME               STATUS   ROLES                       AGE   VERSION
rke2-agent1        Ready    <none>                      70d   v1.27.10+rke2r1
rke2-agent2        NotReady <none>                      70d   v1.27.10+rke2r1
rke2-server1       Ready    control-plane,etcd,master   70d   v1.27.10+rke2r1
```

Further inspection can be done with

```code
kubectl describe node rke2-agent2
kubectl logs <kubelet-pod-name> -n kube-system
```
Restarting the node, fixing network issues, or addressing kubelet errors often restores the node to a "Ready" state

Tools & Techniques

* Kubectl
    The primary tool for managing kubernetes nodes is kubectl. It Provides commands like kubectl top nodes for resource monitoring and kubectl describe node for detailed status and condition information.
* Prometheus & Grafana
    For Advanced monitoring prometheus and grafana are powerful tools. Prometheus collects metrics from your nodes and cluster, while grafaba visualizes these metrics in dashboards, making it easier to identify trends and issues. Setting up alerts in prometheus can notify you of node problems before the impact workloads.
* Node Problem Detector
    The health of nodes is monitored by the Node Problem Detector, a daemon set that reports problems to the kubernetes control plane. it helps in detecting issues like kernal problems, disk IO errors and network failures, allowing for more proactive management.
* Kubenetes Dashboard
    The kubernetes Dashboard provides a web-based UI that gives a graphical overview of node health and resource usage. It can be uick and easy way to monitor node status without diving into command.

[ pod troubleshoot](https://techinik.com/troubleshooting-pod-failures-in-kubernetes-a-comprehensive-guide/)
[Service discovery problem](https://techinik.com/resolving-service-discovery-problems-in-kubernetes/)