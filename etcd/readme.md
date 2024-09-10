# Understanding etcd - The Brain of Kubernetes

Kubernetes is a complex distributed system that requires a robust and efficient distributed database to function smoothly. This is where etcd comes into play.

## What is etcd?

etcd is the backbone of Kubernetes, acting both as a backend service discovery tool and a key-value database. Often referred to as the "brain" of the Kubernetes cluster, etcd is an open-source, strongly consistent, distributed key-value store.

But what exactly does that mean?

Strong Consistency: In a distributed system, when an update is made to one node, strong consistency ensures that all other nodes in the cluster are updated immediately. This guarantees that all nodes reflect the same data at any given time.
Distributed Nature: etcd is designed to operate across multiple nodes as a cluster, without compromising on consistency. This distributed architecture ensures that etcd remains highly available and resilient.
Key-Value Store: etcd is a non-relational database that stores data as simple key-value pairs. It also provides a key-value API, making it easy to interact with the stored data. The data store is built on top of BboltDB, a fork of BoltDB, known for its high performance and reliability.

etcd achieves strong consistency and availability using the Raft consensus algorithm.



https://blog.techiescamp.com/docs/understanding-etcd/