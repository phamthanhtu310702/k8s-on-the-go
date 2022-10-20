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

## Deploying Pods.

Every container in a Pod shares the Pod’s execution environment. So, if a Pod has two containers, both containers share the Pod’s IP address and can access any of the Pod’s volumes to share data.

Pods deployed via controllers have all the benefits of being monitored and managed by a highly-available controller running on the control-plane. 

This is why applications should always store state and data outside the Pod. It’s also why you shouldn’t rely on individual Pods 

## Single-container and multi-container Pods
The process of deploying a Pod to Kubernetes is as follows: 
If the Pod is deployed via a controller, the configuration will be added to the cluster store as part of overall desired state and a controller will monitor it.
### The anatomy of a Pod
The Pod has a set of resources that are shared by every container it runs. These resources include IP address, ports, hostname, sockets, memory, volumes, and more...
- netnamespace:IPaddress,portrange,routingtable... 
- pidnamespace:isolatedprocesstree
- mntnamespace:filesystemsandvolumes...
- UTSnamespace:Hostname
- IPCnamespace:Unixdomainsocketsandsharedmemory
### Pods and shared networking
This means a Pod has its own IP address, a single range of TCP and UDP ports, and a single routing table.

 External access to the containers in the Pod on the left is achieved via the IP address of the Pod coupled with the port of the container you’re trying to reach. 

 Container-to-container communication within the same Pod happens via the Pod’s localhost adapter and a port number. For example, the main container in Figure 4.2 can reach the supporting container on localhost:5000.