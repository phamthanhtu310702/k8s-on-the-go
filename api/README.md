# API security and RBAC
- Role-based access control (RBAC)
## Authentication
- At the heart of authentication are credentials. All requests to the API server have to include credentials, and the authentication layer is responsible for verifying them. If verification fails, the API server returns an HTTP 401 and the request is denied. If it passes, it moves on to authorization.
## Authorization (RBAC)
- The most common authorization module is RBAC (Role-Based Access Control). At the highest level, it’s about three things.
    - Users
    - Actions
    - Resources
- Which users can perform which actions against which resources.
### Users and Permissions
- Two concepts are vital to understanding Kubernetes RBAC.
    - Roles
    - RoleBindings
- Roles define a set of permissions, and RoleBindings grant those permissions to users.
#### Looking closer at rules in Role object
- The previous Role object has three properties:
    - apiGroup
    - resources
    - verbs
- apiGroups and resources define the object, and verbs define the actions. 
### Cluster-level users and permissions
-  Kubernetes actually has 4 RBAC objects:
    - Roles
    - Cluster Roles
    - RoleBindings
    - ClusterRoleBindings
- Roles and RoleBindings are namespaced objects. This means they can only be applied to a single Namespace. ClusterRoles and ClusterRoleBindings are cluster-wide objects and apply to all Namespaces. All 4 are defined in the same API sub-group and their YAML structures are almost identical.
- A powerful pattern is to define Roles at the cluster level (ClusterRoles) and bind them to specific Namespaces via RoleBindings.
- This lets you define common roles once, and re-use them across multiple Namespaces. For example, the following YAML defines the same “read-deployments” role, but this time at the cluster level. This can be re-used in selected Namespaces via RoleBindings.
- The only differences with the earlier one are that this one has its kind set to ClusterRole instead of Role, and it doesn’t have a metadata.namespace property.
### Pre-created users and permissions
### Summarising authorization
- Authorization ensures authenticated users are allowed to carry out the actions they’re attempting. RBAC is a popular Kubernetes authorization module and implements least privilege access based on a deny-by-default model where all actions are assumed to be denied unless a rule exists that allows it. The model is similar to a whitelist firewall where everything is blocked and you open up access by creating allow rules.

- Kubernetes RBAC uses Roles and ClusterRoles to create permissions, and it uses RoleBindings and Cluster- RoleBindings to grant those permissions to users.
## Admission control
- Admission control runs immediately after successful authentication and authorization, and it’s all about policies. There are two types of admission controllers.
    - Mutating 
    - Validating
- The names are self-explanatory. Mutating controllers check for compliance and can modify requests, whereas validating controllers check for policy compliance but cannot modify requests.
## Summary 
- All requests to the API server include credentials and pass through authentication, authorization, and admission control. The connection between the client and the API server is also secured with TLS.
- The authentication layer is responsible for validating the identity of requests. Client certificates are commonly used, and integration with AD and other IAM services is recommended for production clusters. Kubernetes does not have its own identity database, meaning it doesn’t store or manage user accounts.
- The authorization layer checks whether the authenticated user is authorized to carry out the action in the request. This layer is also pluggable and the most common module is RBAC. RBAC comprises 4 objects that define permissions and assign them to users.
- Admission control kicks in after authorization and is responsible for enforcing policies. Validating admission controllers reject requests if they don’t conform to policy, whereas mutating admission controllers can modify requests to enforce policies.