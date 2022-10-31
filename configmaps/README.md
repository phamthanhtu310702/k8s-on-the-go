# Configmaps
## ConfigMap theory
- Kubernetes provides an object called a ConfigMap (CM) that lets you store configuration data outside of a Pod. It also lets you dynamically inject the config into a Pod at run-time.
- ConfigMaps are typically used to store non-sensitive configuration data such as:
    -  Environment variables
    - Server configs and database configs
    - Hostname
    - Service ports
    - Account names
- You should not use ConfigMaps to store sensitive data such as certificates and passwords. Kubernetes provides a different object, called a Secret, for storing sensitive data.

## How ConfigMaps work
- At a high-level, a ConfigMap is a place to store configuration data that can be seamlessly injected into containers at runtime. 
- Once data is stored in a ConfigMap, it can be injected into containers at run-time via any of the following methods:
    - Environment variables
    - Arguments to the container's startup command
    - Files in the volume
### ConfigMaps and Kubernetes-native apps
- A Kubernetes-native application is one that knows it’s running on Kubernetes and can talk to the Kubernetes API. As a result, they can access ConfigMap data directly via the API without needing things like environment variables and volumes. 
- This can simplify application configuration, but the application will only run on Kubernetes (Kubernetes lock-in). At the time of writing, Kubernetes-native applications are rare.
## Hands-on with ConfigMaps


### Inspecting ConfigMaps
- List all ConfigMaps in the current Namespace.
```
kubectl get cm
```
- You can also see the entire object by using the -o yaml flag with kubectl get.

```
kubectl get cm testmap1 -o yaml
```
- An interesting thing to note is that ConfigMap objects don’t have the concept of state (desired state and actual state). This is why they have a data block instead of spec and status blocks.

## Creating ConfigMaps declaratively
- they don’t have a spec section. Instead, they have a data section that defines the map of key/values.
```
kubectl apply -f singlemap.yml
```
- ConfigMaps are extremely flexible and can be used to insert complex configurations, including JSON files and even scripts, into containers at run-time.
### Injecting ConfigMap data into Pods and containers
- There are three main ways to inject ConfigMap data into a container:
### ConfigMaps and environment variables
- When the Pod is scheduled and the container started(envpod.yml), FIRSTNAME and LASTNAME will be created as standard Linux environment variables inside the containe
- A drawback to using ConfigMaps with environment variables is that environment variables are static. This means updates made to the map are not reflected in running containers. For example, if you update the values of the given and family entries in the ConfigMap, environment variables in existing containers won’t see the updates. This is a major reason environment variables aren’t very good.
### ConfigMaps and container startup commands
- Using ConfigMaps with container startup commands is an extension of environment variables. As such, it suffers from the same limitations – updates to entries in the map will not be reflected in running containers.
### ConfigMaps and volumes
- Using ConfigMaps with volumes is the most flexible option. You can reference entire configuration files, as well as make updates to the ConfigMap and have them reflected in running containers. This means you can make changes to entries in a ConfigMap, after you’ve deployed a container, and those changes be seen in the container and available for running applications. The updates may take a minute or so to appear in the container.
    - Create ConfigMaps
    - Create ConfigMaps Volume in Pod template
    - Mount Config vol into the container
    - Entries in the Configmap will appear in the contianer as files
spec.volumes creates in-Pod ConfigMap volume based on ConfigMap
spec.containers.volume mounts the in-Pod ConfigMap volume to the container

- Run the command to see the env variables
```
kubectl exec cmvol -- ls /etc/name
```

## Hands-on with Secrets