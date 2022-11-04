# Kubernetes API
## JSON serialization
- Serialization is the process of converting an object into a string, or stream of bytes, so it can be sent over a network and persisted to a data store. The reverse process of converting a string or stream of bytes into and object is deserialization.
- Kubernetes serializes objects, such as Pods and Services, as JSON strings to be sent over HTTP. The process happens in both directions, with clients like kubectl serializing objects when posting to the API server, and the API server serializing responses back to clients. 
## The API Server
- The API server exposes the API over a secure RESTful interface using HTTPS.
- The main job of the API server is to make API available to clients inside and outside the cluster. 
## The kubernetes API
- The API is where all Kubernetes resources are defined.
### The core API group
- They tend to be fundamental objects such as Pods, nodes, Services, Secrets, and ServiceAccounts.

### Named API groups
- The named API groups are the future of the API, and all new resources go into named groups. Sometimes we refer to them as “sub-groups”

### Resources and objects
- Resources can be either namespaced or cluster-scoped. Namespaced objects have to be deployed to a particular Namespace, whereas cluster-scoped objects can either be bound to multiple Namespaces, or exist entirely outside the realm of Namespaces. Node objects are an example of a cluster-scoped resource that exists entirely outside of Namespaces and cannot ever be deployed to one. On the flip side, ClusterRoles are an example of a cluster-scoped object that can be bound to specific Namespaces via ClusterRoleBindings