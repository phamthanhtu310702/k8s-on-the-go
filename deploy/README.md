# Kubernetes Deployments
Kubernetes offers several controllers that augment Pods with important capabilities. The Deployment controller is specifically designed for stateless apps

## Deployment theory
- The Deployment spec is a declarative YAML object where you describe the desired state of a stateless app
- The controller aspect is highly-available and operates as a background loop reconciling observed state with desired state.
- You start with a stateless application, package it as a container, then define it in a Pod template. At this point you could run it on Kubernetes. However, static Pods like this don’t self-heal, they don’t scale, and they don’t allow for easy updates and rollbacks. For these reasons, you’ll almost always wrap them in a Deployment object.
-  In fact, there are three levels of nesting:
    - The container holds the application 
    - The Pod augments the container with labels, annotations, and other metadata useful for Kubernetes
    - The Deployment further augments things with scaling and updates.
### Deployments and Pods
- A Deployment object only manages a single Pod template.
- A Deployments can manage multiple replicas of the same Pod. For example, the front-end Deployment might be managing 5 identical front-end Pod replicas.
### Deployments and ReplicaSets
- ReplicaSets manage Pods and bring self-healing and scaling. Deployments manage ReplicaSets and add rollouts and rollbacks.
### Self-healing and scalability
- If a Pod managed by a Deployment object fails, it will be replaced - self-healing
- If a Pod managed by a Deployment object see increased and decreased load, they can be scaled
### State
***Desired state***

***Observed state (current state)***

***Reconciliation***


- If they don’t match, a process of reconciliation brings them back together.

### Declarative vs imperative
The declarative model is all about describing an end-goal – telling Kubernetes what you want. The imperative
model is all about long lists of commands to reach an end-goal – telling Kubernetes how to do something.
### Controllers and reconciliation
ReplicaSets are implemented as a controller running as a background reconciliation loop checking the right number of Pod replicas are present on the cluster
## Rolling updates with Deployments
- Now, assume you’re exposed to a known vulnerability and need to rollout a newer image with the fix. To do this, you update the same Deployment YAML file with the new image version and re-post it to the API server. This updates the existing Deployment object with a new desired state requesting the same number of Pods but all running the newer image.
- You now have two ReplicaSets – the original one for the Pods with the old image, and a new one for the Pods with the new image. As Kubernetes increases the number of Pods in the new ReplicaSet (with the new version of the image) it decreases the number of Pods in the old ReplicaSet (with the old version of the image). Net result, you get a smooth incremental rollout with zero downtime.
### Rollbacks
- A Deployment that’s been updated once. The initial release created the ReplicaSet on the left, and the update created the one on the right. You can see the ReplicaSet for the initial release has been wound down and no longer manages any Pods. The one associated with the update is active and owns all the Pods.
- Older ReplicaSets are wound down and no longer manage any Pods. However, their configurations still exist on the cluster, making them a great option for reverting to previous versions.
- The process of a rollback is the opposite of a rollout – you wind one of the old ReplicaSets up while you wind the current one down
## Create a Deployment