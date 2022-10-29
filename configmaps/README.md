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

