# Service discovery
There are two major components to service discovery:
- ***Registration***
- ***Discovery***
## Service registration
- Service registration is the process of an application posting its connection details to a service registry so other apps can find it and consume it.
-
- K8s uses its internal DNS as a serivce registry
- All k8s services automatically register their detals with DNS
-  Kubernetes provides a well-known internal DNS service that we usually call the “cluster DNS”.
- The Service’s front-end configuration (name, IP, port) is registered with DNS and the Service can be discovered by apps and clients.
## Service discovery
## Service discovery and Namespaces
- Namespaces let you partition the address space below the cluster domain.
For example, creating a couple of Namespaces called dev and prod will give you two new address spaces.
- Objects can connect to Services in the local Namespace using short names such as ent and cer. But connecting to objects in a remote Namespace requires FQDNs such as ent.dev.svc.cluster.local and cer.dev.svc.cluster.local.

- All new Service objects are automatically registered with the cluster DNS, and all containers are pre-configured to use the cluster DNS for service discovery.
The cluster DNS resolves Service names to ClusterIPs. These are stable virtual IPs on a special network called the service network. Although there are no routes to this network, the kube-proxy configures all cluster nodes to redirect traffic destined for the service network to Pod IPs on the Pod network.