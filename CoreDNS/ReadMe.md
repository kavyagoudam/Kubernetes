What is CoreDNS?

    Part of the cloud native Foundation. Mostly used for service Discovery within Kubernetes.
    Highly Flexible Due to its plugin system


# Kubernetes CoreDNS Custom Domains
    CoreDNS integrates with Kubernetes via the Kubernetes plugin, or with etcd with the etcd plugin.
    CoreDNS is already installed in Kubernetes
    There are 2 Core-DNS available in k8s.

    1. coredns - it will be managed by provider
    2. coredns-custom - this is available for user customization

# How DNS works in Kubernetes?

    1. DNS is a built-in Kubernetes service launched automatically using the addon manager
    2. Kubernetes creates DNS records for services and pods automatically
    3. kubelet configures pods DNS entry, so that running containers can lookup services by name rather than IP
    4. DNS queries may be expanded using the Pod’s /etc/resolv.conf. kubelet configures this file on each pod
    5. Kubernetes DNS add-ons currently support forward lookups (A records), port lookups (SRV records), reverse IP address (PTR records)

# How to deploy CoreDNS in kubernetes?

    1. CoreDNS is deployed in a simple way
    2. It runs as a cluster-level Pod and handles DNS queries within the cluster from there.
    3. By default, the service is named kube-dns and 1 or more pods are created

command

    kubectl get all -n kubesystem -o wide |grep core
    kubectl -n kube-system get service
    # cluster ip is used to resolve the dns query
    kubectl -n kubesystem get configmap
    # coredns configmap is bind with the deployment, in future if any  pods go down, replicaset with attach this configmap to the pods


Read More About CoreDNS 

    https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns

# How Kubernetes works?

The kube-dns service listens for service and endpoint events from Kubernetes API and updates its DNS records as needed.
These events are triggered when you create, update or delete Kubernetes services and their associated pods
A pod would have record 

```jsx
POD-IP.namespace.pod.cluster.local.

```

A service record
```jsx
SERVICE-IP.namespace.svc.cluster.local.
```


Demo


# ndots
Number of dots in a query name to consider as a FQDN
Whenever a query is forwarded to pods it will search in “search” if not found forwarded to root server

# How to login into DNS pods?
Demo

    kubectl -n kube-system get pods -o wide |grep core
    kubectl get pods/<podname> -n kube-system -o=jsonpath="{range.items[*]}{.spec.containers[].name}";echo
    kubectl -n kube-system debug -it <pod name> --image=busybox:1.28 --target=coredns

![alt text](image.png)