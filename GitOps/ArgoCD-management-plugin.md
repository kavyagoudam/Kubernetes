# How we used ArgoCD management plugin to deploy dynamic objects

Sometimes relying only on ArgoCD “native” config management is not suitable for all of your deployment needs.
The Default management plugins are: Helm Jsonnet and Kustomize.

In this post, I will demonstrate how we utilized the ArgoCD management plugin (CMP) to create an application using our custom yaml generate command.

https://medium.com/taranis-ag/how-we-used-argocd-management-plugin-to-deploy-dynamic-objects-76a59f0309b8