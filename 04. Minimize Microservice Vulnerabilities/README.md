# Minimize Microservice Vulnerabilities

- Security Context
- PodSecurity Policy
- OPA Gatekeeper
- Secrets
- Runtime Sandboxes
- mTLS and Certificates

## Security Context
A security context defines privilege and access control settings for a Pod or Container.
Always keep in mind that the Security context is set at pod level or at the container level. They are different from each other. Security Context settings at the pod level applies to all containers in the pod and the settings at the container level apply to the individual containers within the pod.

## Pod Security Policy 
A Pod Security Policy is a cluster-level resource that controls security sensitive aspects of the pod specification. They allow cluster administrators to control what security-related configurations Pods are allowed to run with.

They allow an administrator to control the few of following:
- Privileged Mode : Allow or disallow running containers in privileged mode.
- Host Namespaces : Block host namespace settings.
- runAsUser, runAsGroup, supplementalGroups : The user and group IDs of the container.
- allowPrivilegeEscalation, defaultAllowPrivilegeEscalation : Restricting escalation to root privileges
- Volumes : control which volume types are allowed for Pod storage volumes
- allowedHostPaths : limit hostPath volumes to only specific paths.

Pod security policy control is implemented as an optional admission controller. PodSecurityPolicies are enforced by enabling the admission controller, but doing so without authorizing any policies will prevent any pods from being created in the cluster.
- Activate the PodSecurityPolicy admission controller by adding it to "kube-apiserver --enable-admission-plugins"
- Pod security policy control is implemented as an optional admission controller. PodSecurityPolicies are enforced by enabling the admission controller, but doing so without authorizing any policies will prevent any pods from being created in the cluster.
- RBAC is a standard Kubernetes authorization mode, and can easily be used to authorize use of policies.
    - a Role or ClusterRole needs to grant access to use the desired policies.
    - Then the (Cluster)Role is bound to the authorized user(s)

## OPA Gatekeeper
Open Policy Agent (OPA) Gatekeeper allows you to enforce highly customizable policies on any kind of k8s object at creation time.

OPA can enforce policies like:
- All images must be from approved repositories
- All ingress hostnames must be globally unique
- All pods must have resource limits
- All resources must have a label that lists a point-of-contact

With the integration of the OPA Constraint Framework, a Constraint is a declaration that its author wants a system to meet a given set of requirements. The Constraint Template that allows us to declare new Constraints. Each template describes both the Rego logic that enforces the Constraint and the schema for the Constraint, which includes the schema of the CRD and the parameters that can be passed into a Constraint, much like arguments to a function.

