# Namespace
Namespaces are a native way to divide a single Kubernetes cluster into multiple virtual clusters.

## Use cases for Namespaces
Most Kubernetes objects are deployed to a Namespace. These objects are said to be namespaced and include common objects such as Pods, Services and Deployments.


If you don’t explicitly define a target Namespace when deploying a namespaced object, it’ll be deployed to the default Namespace.

## Inspecting Namespaces

The default Namespace is where newly created objects go unless you explicitly specify otherwise. Kube-system is where DNS, the metrics server, and other control plane components run. Kube-public is for objects that need to be readable by anyone. And last but not least, kube-node-lease is used for node heartbeat and managing node leases.

## Creating and managing Namespaces
Create a new Namespace, called “hydra”, with the following imperative command.
```
kubectl create ns hydra
```

The following YAML is from the shield-ns.yml file in the namespaces folder. It defines a simple Namespace called “shield”.

```
kubectl apply -f shield-ns.yml
```

Delete the “hydra” Namespace.
```
kubectl delete ns hydra
```

## Deploying to Namespaces
There are two ways to deploy objects to a specific Namespace:
- Imperatively 
- Declaratively

The imperative method requires you to add the -n or --namespace flag to commands. The declarative method specifies the Namespace in the YAML manifest file.

The application is defined in the shield-app.yml file in the namespaces folder. It defines a Pod.

Deploy it with the following command. You don’t have to specify the Namespace on the command line.
```
kubectl apply -f shield-app.yml
```

## Clean-up
Delete the shield app. Be sure to run the command from the directory where the shield-app.yml file is located.

```
kubectl delete -f shield-app.yml
```

Delete the shield Namespace.
```
kubectl delete ns shield
```