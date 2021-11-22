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

## Secrets

A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key. This sensitive data can be passed to containersat runtime.

Using Secrets as files from a Pod
To consume a Secret in a volume in a Pod:

- Create a secret or use an existing one. Multiple Pods can reference the same secret.
- Modify your Pod definition to add a volume under .spec.volumes[]. Name the volume anything, and have a .spec.volumes[].secret.secretName field equal to the name of the Secret object.
- Add a .spec.containers[].volumeMounts[] to each container that needs the secret. Specify .spec.containers[].volumeMounts[].readOnly = true and .spec.containers[].volumeMounts[].mountPath to an unused directory name where you would like the secrets to appear.
- Modify your image or command line so that the program looks for files in that directory. Each key in the secret data map becomes the filename under mountPath.

## Container Runtime Sandboxes

It is a specialized container runtime providing extra layers of process isolation and greater security. Use container runtime sandboxes in situations where you need extra security around containers
- Untrusted workloads
- small and simple : workloads that dont need direct host access and wont mind performance trade-offs.
- multi-tenant

gVisor/runsc
kata containers

## Understanding Pod to Pod mTLS

mTLS means clients and server mutually authenticate with each other and encrypt their communications.

Certificate Signing
- Kubernetes API
- Certificate Authority
- Programmatic Certificates

### Signing Certificates

Requesting and Signing Certificates.
- Certificate Signing request - Create CSR for obtaining new certificates.
- Approve/Deny - CSR can be approved or Denied
- RBAC - Permissions related to Certificate signing can be managed via RBAC

The following scripts show how to generate PKI private key and CSR.It is important to set CN and O attribute of the CSR. CN is the name of the user and O is the group that this user will belong to.
- openssl genrsa -out myuser.key 2048
- openssl req -new -key myuser.key -out myuser.csr

kubectl get csr
kubectl certificate approve myuser
kubectl get csr/myuser -o yaml
kubectl get csr myuser -o jsonpath='{.status.certificate}'| base64 -d > myuser.crt

