# Pods
Pods are fundamental to running apps on Kubernetes.

Controllers gift Pods with super-powers such as self-healing, scaling, updates and rollbacks. 

If you deploy an app, you deploy it in a Pod. 

If you scale an app up or down, you add or remove Pods.

The process of building and running an app on Kubernetes is roughly as follows:

1. Writeyourapp/code
2. Packageitasacontainerimage
3. WrapthecontainerimageinaPod 
4. RunitonKubernetes

Kubernetes doesn’t allow containers to run directly on a cluster, they always have to be wrapped in a Pod.