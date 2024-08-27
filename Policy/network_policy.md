# Default-deny-egress network policy

```yaml
kind: NetworkPolicy
metadata:
 name: default-deny-egress
spec:
 podSelector: {}
 policyTypes:
 - Egress
```
First things first, the packet is dropped by CNI plugin. Where it's dropped depends on network plugin you select.

For example: In Calico, rules are enforced by iptables. So that's where the packets would be dropped.

In Cilium, the packets are dropped by eBPF program in the Kernel. Not the iptables.