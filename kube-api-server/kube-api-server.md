# Kube API server

## Life cycle of a Kubernetes API request(via API server)

1. Send API request(from user or controller)
2. AUthentication and Authorization(permission)
3. Mutating admission(ex: add nabels)
4. Object schema validation(by API server)
5. Validating Admission(Final checks no modification)
6. Persisted to etcd(Storing the data)

What happend after that?

Normally. a controller picks up and takes some action to get to the desired state.

## Kubernetes API Aggregation Layer & Extension apiservers ⚡

Kubernetes API server contains an [aggregation layer](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/apiserver-aggregation/?ref=blog.techiescamp.com) which allows you to extend kubernetes API (almost inifinite flexibility) to create custom APIs resources which are not natively available to kubernetes.

Why would you need a custom API?

Custom APIs allow you to add new features and resources to Kubernetes without modifying its core code. This enables you to tailer kubernetes to your specific needs.

To enable Custom APIs, you need to build and deploy a extension API server.

The aggregation layer sits in front of the main kubernetes API server, routing requests to either the core API server or to extension API servers you've set up.

Examples:-
A classic example of using the aggregation layer in kubernetes is the implementation of the Metrics server. The Metrics server is usually deployed as an add-on component in kubernetes clusters.

```importent
The Metrics Server is widely used in Kubernetes clusters to collect CPU and memory metrics, which are important for implementing Horizontal Pod Autoscalers (HPA) & VPA to retrieve metrics data and make scaling decisions based on them.
```

Metrics server implements the Metrics API as an add-on API server, meaning it extends the Kubernetes API using the aggregation layer we discussed earlier.

Here how it works:

1. The metrics server collects resource metrics from kubelets on each node in your cluster.
2. it then serves these metrics via the metrics API, which is exposed through kubernetes API aggregation.
3. This allows the kubernetes API server to serve these metrics alongside other core APIs

You can view the API services in a cluster that has metrics server deployed by running the followed command

```command
 kubectl get apiservices

v1.storage.k8s.io                      Local                        
v1beta1.metallb.io                     Local                        
v1beta1.metrics.k8s.io                 kube-system/metrics-server 
```

Local - means the API is served directly by the main Kubernetes API server.
kube-system/metrics-server - indicates that this API is served by the metrics-server running in the kube-system namespace.


This means that even though the Metrics Server is a separate service, its API can be accessed as if it were a native part of the Kubernetes API.

So when the Metrics Server is deployed in your cluster, it acts as an add-on API server that collects and serves resource metrics (like CPU and memory usage) through the Metrics API, which is accessible via the Kubernetes API server.

```importent
Another example is Prometheus Adapter. It Exposes arbitrary Prometheus metrics to Kubernetes' custom metrics API. It issed for autoscaling based on custom application metrics and more complex monitoring scenarios. Whereas metrics server provides only basic resource-based autoscaling. This API is served under the path /apis/custom.metrics.k8s.io/v1beta1.
```

To build an extension API server you can make use of the [apiserver-builder](https://github.com/kubernetes-sigs/apiserver-builder-alpha/?ref=blog.techiescamp.com) repo.

apiserver-builder is a tool designed to simplify the process of creating Kubernetes extension API servers.

It automates much of the boilerplate code generation needed to create a Kubernetes API server. For example,

It generates API definitions
It creates controller scaffolding
It handles common tasks like setting up CRUD operations


Here is an [example api-server implementation](https://github.com/kubernetes/sample-apiserver/?ref=blog.techiescamp.com)

When to use Aggregation Layer

In practice, it's common to start with CRDs and move to the Aggregation Layer if you encounter limitations.

For the majority of scenarios where you need to extend Kubernetes, such as defining application-specific resources or operational patterns, CRDs provide all the necessary functionality.

Many projects, including major Kubernetes extensions like Istio and Knative, use a combination of both approaches.

https://kubernetes.io/docs/tasks/extend-kubernetes/configure-aggregation-layer/?ref=blog.techiescamp.com

https://kubernetes.io/docs/tasks/extend-kubernetes/configure-aggregation-layer/?ref=blog.techiescamp.com