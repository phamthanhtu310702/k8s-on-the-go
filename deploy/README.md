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