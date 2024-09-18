# Persistent Storage in Kubernetes: A Comprehensive Guide

Eventhough kubernetes ensures the effifient management of containers, it also introduces challenges when it comes to storing data persistently. This is where persistent storage comes into play.

Persistent storage in kubernetes provides a way to store data beyond the lifecycle of a pod, ensuring that data is not lost when pods are recreated or rescheduled. This is crucial for applications that requires stateful data, such as databases, message queues and file systems.

In this blog post, we will delve into the world of persistent storage in Kubernetes, exploring its importance, different types, and considerations for choosing the right solution.

## Why Do we need persistent storage?

Kuberenetes is designed to be stateless, meaning that containers are ephemeral and can be recreated at any time. while this provides flexibility and scalability, it also poses challenges for applications that require persistent data.

Here are some key reasons why persistent storage is essential in kubernetes:

Data retention: persistent storage ensures that data is retained even when pods are restarted or rescheduled, preventing data loss.

Stateful Apllications: Many applications, such as databases and message queues, rely on persistent storage to maintain their state and function correctly.

Data consistency: persistent storage can be used to guarantee data consistancy across multiple pods or replicas of an application.

Data Sharing: Persistent storage can be shared between multiple pods, enabling collaboration and data sharing between different components of an application.

## Understanding Persistent Storage

In kubernetes, persistent storage ensures that data persists beyond the lifecycle of a pod. This is crucial for stateful applications like databases. message queues and file systems. Kubernetes provides two primary mechanisms for managing persistent storage: Persistent Volumes(PVs) and Persistent Volume Claims(PVCs).

### Persistent Volumes (PVs)

Abstract representation: A PV represents a storage resource in the cluster.
Provisioning: Can be created manually or dynamically provisioned by storage classes.
Attributes: Includes capacity, access modes, and reclaimer policy.

### Persistent Volume Claims (PVCs)

Requests: A PVC is a request for storage by a pod.
Binding: It is bound to a PV that meets its requirements.
Attributes: Includes access modes and storage requests.

### Storage Classes

Abstraction: StorageClasses provide a way to define different types of storage available in the cluster, such as high-performance SSDs, low-cost HDDs, or cloud-based storage.

Dynamic Provisioning: They enable automatic creation of PVs based on PVC requests, simplifying the storage management process.

### Access Modes

ReadWriteOnce (RWO): Allows exclusive read-write access to a single Pod.
ReadOnlyMany (ROX): Allows read-only access to multiple Pods.
ReadWriteMany (RWX): Allows read-write access to multiple Pods, requiring a shared storage system.

### Volume Binding Modes

WaitForFirstConsumer: Delays binding the PV to a PVC until a Pod using the PVC is scheduled.
Immediate: Binds the PV to a PVC as soon as it’s created, regardless of whether a Pod is using it.

### Reclaim Policies
Retain: Retains the PV after the PVC is deleted, allowing manual reclamation.
Delete: Deletes the PV after the PVC is deleted.
Recycle: Recycles the PV for reuse, potentially cleaning up data before rebinding.

## How Persistent Storage Works

PVC Creation: A pod specifies a PVC in its spec.

PV Binding: The Kubernetes scheduler finds a suitable PV that matches the PVC’s requirements.

Mount Point: The PV is mounted to a specific path within the pod’s container.

Data Access: The pod can now access the persistent data through the mounted path.


## Example: Using a Dynamically Provisioned PV

```yaml
# StorageClass
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: my-storageclass
provisioner: <storage-provisioner>
parameters:
  # Provisioner-specific parameters
---
# PVC
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
  storageClassName:   
 my-storageclass
---
# Pod
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-app
    image: my-image
    volumeMounts:
    - mountPath: /data
      name: my-pv-claim
  volumes:
  - name: my-pv-claim
    persistentVolumeClaim:
      claimName: my-pvc

```
A storage class my-storageclass is defined, using a specific storage provisioner.
A PVC my-pvc is created, requesting 10Gi of storage and using the my-storageclass.
A pod my-pod is defined, mounting the PVC to the /data directory.

## Container Storage Interface (CSI)

Container Storage Interface (CSI) is a standard for interfacing between container orchestration systems like Kubernetes and storage systems. It provides a consistent API for Kubernetes to interact with various storage technologies, making it easier to manage and provision persistent storage in the cluster.


Here’s how CSI drivers work in Kubernetes:

Driver Registration: A CSI driver is deployed as a containerized application within the Kubernetes cluster. It registers itself with the Kubernetes control plane, indicating the types of storage it supports and the operations it can perform.
PV Provisioning: When a Persistent Volume Claim (PVC) is created, Kubernetes checks the available StorageClasses to find a suitable match. Based on the StorageClass, Kubernetes invokes the appropriate CSI driver to provision a Persistent Volume (PV) that meets the requirements of the PVC.
PV Binding: Once the PV is provisioned, Kubernetes binds it to the PVC. This establishes a connection between the PVC and the underlying storage resource managed by the CSI driver.
Volume Mounting: When a Pod is scheduled and needs to access the persistent storage, Kubernetes uses the CSI driver to mount the PV to the Pod’s filesystem. The driver handles the necessary operations to make the storage accessible to the Pod’s applications.

Volume Unmounting: When a Pod is terminated or the PVC is deleted, Kubernetes uses the CSI driver to unmount the PV from the Pod’s filesystem.

PV Deletion: If the PVC is deleted and the reclaim policy for the PV is set to “Delete,” the CSI driver is responsible for deleting the underlying storage resource.

Key benefits of using CSI drivers in Kubernetes:

Standardization: CSI provides a unified interface for various storage systems, simplifying integration and management.

Flexibility: Kubernetes can support a wide range of storage technologies through CSI drivers, offering flexibility in choosing the right storage solution for different workloads.

Extensibility: New storage systems can be easily integrated into Kubernetes by developing custom CSI drivers.

Efficiency: CSI drivers can optimize storage operations, improving performance and resource utilization.

## Considerations When Choosing a Specific Type of Persistent Storage


When selecting a persistent storage solution for your Kubernetes environment, several factors need to be considered:

Performance Requirements: Different storage types offer varying levels of performance. For applications that require high I/O operations per second (IOPS), iSCSI or CephFS might be suitable. For less demanding workloads, NFS or HostPath volumes could suffice.
Scalability: Consider how your storage needs might grow over time. CephFS and cloud-based storage solutions offer better scalability compared to local storage options like HostPath.
Cost: The cost of persistent storage varies depending on the type and provider. Cloud-based storage solutions often have pay-as-you-go pricing models, while on-premises solutions might involve upfront hardware costs.
Data Consistency: Some applications require strong data consistency guarantees. Distributed file systems like CephFS and GlusterFS can provide stronger consistency levels compared to NFS.
Availability: Consider the availability requirements of your application. High-availability features like replication and redundancy can be provided by some storage solutions.
Integration with Kubernetes: The storage solution should integrate seamlessly with Kubernetes. This includes support for PVs, PVCs, and storage classes.
Security: Ensure that the storage solution has adequate security measures in place, such as encryption and access controls.

## Best Practices in Using Persistent Storage
Use PVCs: Always use Persistent Volume Claims (PVCs) to abstract the underlying storage details from your pods. This makes your applications more portable and easier to manage.
Choose Appropriate Access Modes: Select the appropriate access mode for your PVC (ReadWriteOnce, ReadWriteMany, ReadOnlyMany) based on your application’s requirements.
Consider Storage Classes: Use storage classes to define default storage parameters and provision storage dynamically.
Monitor Storage Usage: Regularly monitor storage usage to avoid running out of capacity. Set up alerts to notify you when storage utilization reaches critical levels.
Back Up Regularly: Implement a robust backup and restore strategy for your persistent data. This includes backing up both the PVC and the underlying PV.
Use Snapshots: Consider using snapshots to create point-in-time copies of your persistent volumes for quick recovery in case of data corruption or accidental deletion.

Backup and Restore Options for Kubernetes Persistent Storage

There are several ways to back up and restore persistent storage in Kubernetes:

Manual Backups: Manually copy data from persistent volumes to external storage systems. This approach can be time-consuming and error-prone.
Backup Tools: Use specialized backup tools designed for Kubernetes, such as Velero, Ark, or Kasten K10. These tools automate the backup process and provide features like incremental backups, snapshot management, and disaster recovery.
Cloud-Native Backup Solutions: If you’re using cloud-based storage, leverage the backup capabilities provided by your cloud provider. For example, AWS EBS volumes can be backed up using EBS snapshots.

## Open Source Storage Solutions for Kubernetes

Open source storage solutions offer a flexible and cost-effective way to manage persistent data in Kubernetes environments. Here are some popular options:

1. Ceph
Ceph is a highly scalable, distributed object storage system that can be used to provide persistent storage for Kubernetes. It offers several advantages, including:

Scalability: Ceph can scale horizontally to accommodate large amounts of data.
Durability: Data is replicated across multiple nodes for redundancy and fault tolerance.
Performance: Ceph can deliver high performance, even for large datasets.
Flexibility: It supports various storage use cases, such as block storage, object storage, and file systems.
To use Ceph with Kubernetes, you typically need to deploy a Ceph cluster and then configure Kubernetes to use it as a storage backend. This involves creating StorageClasses that define the characteristics of the Ceph storage, such as performance tiers and access modes. When a Persistent Volume Claim (PVC) is created, Kubernetes will provision a Persistent Volume (PV) using the Ceph storage and bind it to the PVC.

Ceph’s integration with Kubernetes provides a flexible and scalable solution for managing persistent storage in containerized environments. It is a popular choice for organizations that require high availability, durability, and scalability for their data.

2. GlusterFS
GlusterFS is a scalable, distributed file system that can be used to provide persistent storage for Kubernetes. It offers several advantages, including:

Scalability: GlusterFS can scale horizontally by adding more nodes to the cluster, providing high availability and performance for demanding workloads.
Flexibility: It supports various storage protocols, such as NFS and iSCSI, allowing it to integrate seamlessly with different Kubernetes environments.
Efficiency: GlusterFS optimizes data distribution and access, ensuring high performance and low latency.
Durability: It provides data redundancy and fault tolerance, protecting against data loss in case of hardware failures.
To use GlusterFS with Kubernetes, you need to create Persistent Volumes (PVs) and Persistent Volume Claims (PVCs). The PVs represent units of storage in the cluster, while the PVCs are requests for storage by applications. GlusterFS can be used to provision these PVs dynamically or manually.

Once the PVs and PVCs are configured, Kubernetes can mount GlusterFS volumes to Pods, allowing applications to access and persist data. This provides a reliable and scalable solution for storing and managing data in Kubernetes environments.

3. Rook
Rook offers several specific advantages:

Operator-Based Management:

Simplicity: Rook is managed using an operator, which simplifies the deployment, configuration, and management of storage systems.
Automation: The operator automates many common tasks, such as scaling, upgrades, and failure recovery.
Multi-Cluster Support:

Flexibility: Rook can manage storage across multiple Kubernetes clusters, providing a centralized solution for managing storage resources.
Integration with Other Kubernetes Components:

Seamless Integration: Rook integrates seamlessly with other Kubernetes components, such as the Scheduler and Controller Manager, ensuring efficient resource allocation and management.
Modular Architecture:

Flexibility: Rook’s modular architecture allows for easy integration with different storage backends, such as Ceph, GlusterFS, and external storage systems.
Community Support and Development:

Active Community: Rook has a strong and active community, providing ongoing development, support, and documentation.
4. OpenEBS
OpenEBS is a cloud-native, open-source persistent storage solution for Kubernetes. It offers several unique and prominent features that make it a compelling choice for managing storage in containerized environments:

Dynamically Provisioned Local Persistent Volumes (LVPVs):

Performance: LVPVs provide high performance and low latency, making them ideal for applications that require fast data access.
Flexibility: LVPVs can be dynamically provisioned based on application needs, ensuring efficient resource utilization.
Hybrid Cloud Support:

Portability: OpenEBS supports both on-premises and cloud-native environments, making it easy to migrate applications between different infrastructure.
Flexibility: Users can choose from various storage backends, such as local storage, NFS, and iSCSI, to meet their specific requirements.
Multi-cloud and Multi-cluster Support:

Scalability: OpenEBS can manage storage across multiple clouds and clusters, providing a centralized solution for managing storage resources.
Flexibility: Users can deploy OpenEBS in various configurations, such as standalone, replica, or mirror, to meet their specific needs.
Built-in Replication and Backup:

Data Protection: OpenEBS offers built-in replication and backup features to protect data against failures and ensure high availability.
Efficiency: Replication and backup can be configured to meet specific data protection requirements, while minimizing performance overhead.
Enterprise-Grade Features:

Security: OpenEBS includes features such as encryption and access control to protect sensitive data.
Reliability: It provides high availability and fault tolerance, ensuring that applications can continue to operate even in the event of hardware failures.

## PVC with AKS

Create and use a volume with Azure Disks in Azure Kubernetes Service (AKS)

https://learn.microsoft.com/en-us/azure/aks/azure-csi-disk-storage-provision
