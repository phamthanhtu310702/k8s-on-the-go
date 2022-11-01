# StateFul sets
## The theory of StatefulSets
- It’s often useful to compare StatefulSets with Deployments. Both are first-class API objects and follow the typical Kubernetes controller architecture. They’re both implemented as controller that operate reconciliation loops watching the state of the cluster, via the API server, and moving the observed state into sync with desired state. Deployments and StatefulSets also support self-healing, scaling, updates, and more.
- There are some vital differences between StatefulSets and Deployments. StatefulSets guarantee.
    - Predictable and persistent Pod names, host names, volume bindings

- These three properties form the state of a Pod, sometimes referred to as its sticky ID. StatefulSets ensure this state/sticky ID is persisted across failures, scaling, and other scheduling operations, making them ideal for applications that require unique Pods that are not interchangeable.
- The name of the StatefulSet is tkb-sts and it defines three Pod replicas running the mongo:latest image. You post this to the API server, it’s persisted to the cluster store, the replicas are assigned to cluster nodes, and the StatefulSet controller monitors the state of the cluster making sure observed state matches desired state.
### StatefulSet Pod naming
- All Pods managed by a StatefulSet get predictable and persistent names. These names are vital, and are at the core of how Pods are started, self-healed, scaled, deleted, attached to volumes, and more.
- The format of StatefulSet Pod names is "StatefulSetName-Integer". The integer is a zero-based index ordinal, which is just a fancy way of saying “number starting from 0”.
### Ordered creation and deletion
- Another fundamental characteristic of StatefulSets is the controlled and ordered way they start and stop Pods.
- StatefulSets create one Pod at a time, and always wait for previous Pods to be running and ready before creating the next. This is different from Deployments that use a ReplicaSet controller to start all Pods at the same time, causing potential race conditions.
- Scaling operations are also governed by the same ordered startup rules. For example, scaling from 3 to 5 replicas will start a new Pod called tkb-sts-3 and wait for it the be running and ready before creating tkb-sts-4. Scaling down follows the same rules in reverse – the controller terminates the Pod with the highest index ordinal (number) first, waits for it to fully terminate before terminating the Pod with the next highest ordinal.
- Knowing the order in which Pods will be scaled down, as well as knowing that Pods will not be terminated in parallel, is a game-changer for many stateful apps. 
    - For example, clustered apps that store data can potentially lose data if multiple replicas go down at the same time. StatefulSets guarantee this will never happen. You can also inject other delays via things like terminationGracePeriodSeconds to further control the scaling down process. All in all, StatefulSets bring a lot to the table for clustered apps that create and store data.
### Deleting StatefulSets
- Firstly, deleting a StatefulSet does not terminate Pods in order. With this in mind, you may want to scale a StatefulSet to 0 replicas before deleting it.
- You can also use terminationGracePeriodSeconds to further control the way Pods are terminated. It’s common to set this to at least 10 seconds to give applications running in Pods a chance to flush local buffers and safely commit any writes that are still “in-flight”.
### StatefulSets and Volumes
- When a StatefulSet Pod is created, any volumes it needs are created at the same time and named in a special way that connects them to the right Pod .
- Volumes are appropriately decoupled from Pods via the normal Persistent Volume Claim system. This means volumes have separate lifecycles to Pods, allowing them to survive Pod failures and termination operations. For example, any time a StatefulSet Pod fails or is terminated, associated volumes are unaffected. This allows replacement Pods to attach to the same storage as the Pods they’re replacing. This is true, even if replacement Pods are scheduled to different cluster nodes.

### Handling failures

### Network ID and headless Services
- StatefulSets are for applications that need Pods to be predictable and long-lived. As a result, other parts of the application as well as other applications may need to connect directly to individual Pods. To make this possible, StatefulSets use a headless Service to create predictable DNS hostnames for every Pod
- A headless Service is just a regular Kubernetes Service object without an IP address (spec.clusterIP set to None). It becomes a StatefulSet’s governing Service when you list it in the StatefulSet manifest under spec.serviceName.
- The Service will create DNS SRV records for each Pod replica that matches the label selector of the headless Service. Other Pods and apps can then find members of the StatefulSet by performing DNS lookups against the name of the headless Service. 