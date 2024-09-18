# Create and use a volume with Azure Files in Azure Kubernetes Service (AKS)

If multiple pods need concurrent access to the same storage volume, you can use Azure Files to connect using the Server Message Block(SMB) protocol. This article shows you how to dynamically create an Azure file share for use by multiple pods in a AKS cluster.


## Pre-requisites

1. Storage account
2. Azure cli


