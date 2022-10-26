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

## Inspecting Deployments
See details of Deployments and ReplicaSets.
```
kubectl get deploy hello-deploy

```

## Accessing the app
- The following YAML defines a Service that works with the Pod replicas previously deployed.

- The Service is deployed, you can access the app by hitting any of the cluster nodes on port 30001

## Perform scaling operations
### Perform a rolling update
- Rolling update on the app already deployed
- it’s vital you understand that update operations are replacement operations. When you “update” a Pod, you’re actually terminating it and replacing it with a brand new one.
- Pods are designed as immutable objects, so you never change or update existing ones.
- There is also a nested .spec.strategy map telling Kubernetes you want this Deployment to:
    As the desired state of the app requests 10 replicas, maxSurge: 1 means you’ll never have more than 11 replicas during the update process, and maxUnavailable: 1 means you’ll never have less than 9. The net result is a rollout that updates two Pods at a time.
- The Deployment spec has a selector block. This is a list of labels the Deployment controller looks for when finding Pods to update during rollout operations
- The Deployment’s label selector is immutable, so you can’t change it once it’s deployed.
- With the updated manifest ready and saved, you can initiate the update by re-posting it to the API server. Be sure to add the --record flag 
## Perform a rollback
- You used the --record flag so Kubernetes would maintain a documented revision history. The following command shows the Deployment with two revisions.
```
kubectl rollout history deployment hello-deploy
```
- You know that rolling updates create new ReplicaSets, and that old ReplicaSets aren’t deleted. The fact the old ones still exist makes them ideal for executing rollbacks, which are the same as updates, just in reverse.

- The following commands show the two ReplicaSet objects. The second command shows the config of the old one and that it still references the old image version
```
kubectl get rs
```
```
kubectl describe rs hello-deploy-65cbc9474c
```
- The following example uses kubectl rollout to revert the application to revision 1. This is an imperative operation and not recommended. However, it’s convenient for quick rollbacks, just make sure you remember to update your source YAML files to reflect the changes you make.
```
kubectl rollout undo deployment hello-deploy --to-revision=1
```

## Rollouts and labels
- You’ve already seen that Deployments and ReplicaSets use labels and selectors to find Pods
## Clean-up
```
kubectl delete -f deploy.yml

kubectl delete -f svc.yml

```