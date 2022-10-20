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

Where are three main reasons for Pods:

## Pods augment containers: 

• Labelsandannotations

• Restartpolicies

• Probes(startupprobes,readinessprobes,livenessprobes,andpotentiallymore) 

• Affinityandanti-affinityrules

• Terminationcontrol

• Securitypolicies

• Resourcerequestsandlimits

## Pods assist in scheduling
Every container in a Pod is guaranteed to be scheduled to the same cluster node. 

## Pods enable resource sharing
Pods provide a shared execution environment for one or more containers.

• Sharedfilesystem

• Sharednetworkstack(IPaddressandports...) 

• Sharedmemory

• Sharedvolumes

Every container in a Pod shares the Pod’s execution environment. So, if a Pod has two containers, both containers share the Pod’s IP address and can access any of the Pod’s volumes to share data.