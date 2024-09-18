# Create and use a volume with Azure Disks in Azure Kubernetes Service (AKS)

## Pre-requisites
1. Azure CLI
2. AKS cluster 

## Dynamically provision a volume
A persistent volume claim (PVC) uses the storage class object to dynamically provision an Azure Disk storage containers. Storage classes define how a unit of storage is dynamically created with a persistent volume.

### Built-In Storage Class

By default, AKS comes with pre-created Storage classes. In that two of them are configured to work with Azure Disks.

1.  The default storage class provisions a standard SSD Azure Disk.
2.  The managed-csi-premium storage class provisions a premium Azure Disk.

You can see the pre-created Storage class with below kubectl command
```command
   kubectl get sc
```
So we can use these pre-created storage class for the PVC.

### 1.  Create Persistent Volume Claim


A PVC automatically provisions storage based on a storage class. Here PVC can use one of the pre-created storage classes to create a standard or premium Azure managed disk.

Create a file name azure-pvc.yaml and copy the following manifests.
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: managed-csi
  resources:
    requests:
      storage: 5Gi
```
 Tip:- To create a disk that uses premium storage, use storageClassName: managed-csi-premium rather than managed-csi.

Create the persistent volume claim using the kubectl apply command as below

```command
kubectl apply -f azure-pvc.yaml
```
### 2.  Use the persistent Volume

After you create the persistent volume claim, you must verify it has a status of Pending. The Pending status indicates it's ready to be used by a pod.

Verify the status of PVC.

```command
kubectl describe pvc azure-managed-disk
```

Create a file "azure-pvc-disk.yaml" and copy in the following manifest.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
    - name: mypod
      image: mcr.microsoft.com/oss/nginx/nginx:1.15.5-alpine
      resources:
        requests:
          cpu: 100m
          memory: 128Mi
        limits:
          cpu: 250m
          memory: 256Mi
      volumeMounts:
        - mountPath: "/mnt/azure"
          name: volume
          readOnly: false
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azure-managed-disk

```

Create a pod 

```command
kubectl apply -f azure-pvc-disk.yaml
```

Check the pod configuration using below command

```command
kubectl describe pod mypod
```
to check the volume configuration check volume and Events.
