# Cluster Hardening
- Service Accounts
- RBAC
- The kubernetes API
- Kubernetes Updates

## Service Accounts
### RBAC review
#### Role and ClusterRole
- A Role always sets permissions within a particular namespace; when you create a Role, you have to specify the namespace it belongs in.

- ClusterRole, by contrast, is a non-namespaced resource. The resources have different names (Role and ClusterRole) because a Kubernetes object always has to be either namespaced or not namespaced; it can't be both.

#### RoleBinding and ClusterRoleBinding 
- A role binding grants the permissions defined in a role to a user or set of users. It holds a list of subjects (users, groups, or service accounts), and a reference to the role being granted. A RoleBinding grants permissions within a specific namespace whereas a ClusterRoleBinding grants that access cluster-wide.

#### Service Accounts
- SA give pods access to kube API. Their permissions are governed by the regular RBAC objects.  
##### Restricting Service Accounts Permissions
##### Restricting Access to kubernetes API
- The kubernetes API is a http interface. Limit network access to the API using network segementation/firewalls.
