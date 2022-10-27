# Kubernetes Service
You’ve deployed applications to Kubernetes and seen how controllers add self-healing, scaling and rollouts. Despite all of this, Pods are still unreliable and you should not connect directly to them. This is where Services come to the rescue providing stable and reliable networking for a set of unreliable Pods.
## Setting the scene
- When Pods fail, they get replaced by new ones with new IPs. Scaling-up introduces new Pods with new IP addresses. Scaling down removes Pods. Rolling updates also replace existing Pods with new ones with new IPs. This creates massive IP churn and demonstrates why you should never connect directly to any particular Pod.
- First, when talking about Services with a capital “S”, we’re talking about the Service object in Kubernetes that provides stable networking for Pods. Just like a Pod, ReplicaSet, or Deployment, a Kubernetes Service is a REST object in the API that you define in a manifest and post to the API server.
- Second, every Service gets its own stable IP address, its own stable DNS name, and its own stable port.
- Third, Services use labels and selectors to dynamically select the Pods to send traffic to.
## Service Theory
- Service thrown into the mix. The Service fronts the Pods with a stable IP, DNS name, and port. It also load-balances traffic to Pods with the right labels.
- With a Service in place, the Pods can scale up and down, they can fail, and they can be updated and rolled back. And clients will continue to access them without interruption. This is because the Service is observing the changes and updating its list of healthy Pods. But it never changes its stable IP, DNS, and port.
### Labels and loose coupling
- Services are loosely coupled with Pods via labels and selectors. 
- The Deployment has a Pod template with the same two labels. This means any Pods it deploys will match the Service’s selector and receive traffic from it. This loose coupling is how Services know which Pods to send traffic to.
### Services and Endpoint objects
- As Pods come-and-go (scaling, failures, rollouts etc.), the Service dynamically updates its list of healthy matching Pods. It does this through a combination of label selection and a construct called an Endpoints object.
- Every time you create a Service, Kubernetes automatically creates an associated Endpoints object. The Endpoints object is used to store a dynamic list of healthy Pods matching the Service’s label selector.
### Accessing Services from inside the cluster
- Kubernetes supports several types of Service. The default type is ClusterIP.
- A ClusterIP Service has a stable virtual IP address that is only accessible from inside the cluster.
### Accessing Services from outside the cluster
- NodePort Services build on top of the ClusterIP type and enable external access via a dedicated port on every cluster node. 
- You already know the default Service type is ClusterIP, and it registers a DNS name, virtual IP, and port with the cluster’s DNS. NodePort Services build on this by adding a NodePort that can be used to reach the Service from outside the cluster.
### Summary of Service theory
The front-end of a Service provides an immutable IP, DNS name and port that is guaranteed not to change for the entire life of the Service. The back-end of a Service uses labels and selectors to load-balance traffic across a potentially dynamic set of application Pods.
