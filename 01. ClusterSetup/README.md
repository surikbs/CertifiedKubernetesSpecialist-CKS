# 01. Cluster Setup
-  NetworkPolicies
- CIS Benchmarks (kube-bench)
- Ingress
- Attack Surfaces (openports, GUI and others)
- Verifying K8s binaries

## Network Policies
- NetworkPolicies are an application-centric construct which allow you to specify how a pod is allowed to communicate with various network "entities" over the network
- By default, if no policies exist in a namespace, then all ingress and egress traffic is allowed to and from pods in that namespace. The following examples let you change the default behavior in that namespace.
### Restricting Default Access with NetworkPolicies
- You can create a "default" isolation policy for a namespace by creating a NetworkPolicy that selects all pods but does not allow any ingress traffic to those pods.

Default deny all ingress and all egress traffic

    ---
    apiVersion: networking.k8s.io/v1
    kind: NetworkPolicy
    metadata:
    name: default-deny-all
    namespace: nptest
    spec:
    podSelector: {}
    policyTypes:
    - Ingress
    - Egress

link: https://kubernetes.io/docs/concepts/services-networking/network-policies/#default-deny-all-ingress-and-all-egress-traffic

### Allow limited access with NetworkPolicies
- Network policies do not conflict; they are additive. If any policy or policies select a pod, the pod is restricted to what is allowed by the union of those policies' ingress/egress rules.

    - spec: NetworkPolicy spec has all the information needed to define a particular network policy in the given namespace.
    - podSelector: Each NetworkPolicy includes a podSelector which selects the grouping of pods to which the policy applies. The example policy selects pods with the label "role=db". An empty podSelector selects all pods in the namespace.
    - policyTypes: Each NetworkPolicy includes a policyTypes list which may include either Ingress, Egress, or both. The policyTypes field indicates whether or not the given policy applies to ingress traffic to selected pod, egress traffic from selected pods, or both. If no policyTypes are specified on a NetworkPolicy then by default Ingress will always be set and Egress will be set if the NetworkPolicy has any egress rules.
    - ingress: allow contorl of incoming traffic.
    - egress: allo control of outgoing traffic
    - ports: determine which port(s) will allow traffic for the rule.
    - to/from selectors determine the source and destination of allowed traffic

Example: Accessing a nginx pod from a client pod with default deny policy inplace within the namespace. The client will not be able to access the nginx pod.
- We will have to create two policy types: both ingress and egress.

    Client pod -> Egress Rule -------------------------> Nginx Pod (outgoing traffic from client pod)

    Client pod --------------------------Ingress Rule --> Nginx pod (incoming traffic to Nginx Pod)

Very Important: https://kubernetes.io/docs/concepts/services-networking/network-policies/#behavior-of-to-and-from-selectors


## CIS Benchmark with kube-bench

CIS Center for Internet Security is a non-profit organization  dedicated to promoting digital Security. CIS benchmarks are a set of concensus-driven community standards and best practices for securing systems.

### Fixing Security issues detected by CIS benchmark
- The CIS Benchmark output includes remediation steps which you can use to address issues.
- kubeadm clusters use a kubelet config file /var/lib/kubelet/config.yaml
- the config file for the control plane components (eg. API Server, etcd etc) can be found in /etc/kubernetes/manifests on the controlplane servers.

wget -O kubebench-control-plane.yaml https://raw.githubusercontent.com/aquasecurity/kube-bench/main/job-master.yaml

wget -O kubebench-node.yaml https://raw.githubusercontent.com/aquasecurity/kube-bench/main/job-node.yaml

## Implementing TLS with Ingress

Ingress may provide load balancing, SSL termination and name-based virtual hosting. Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.

External user ------ (https://) ----------> Ingress ----------(http://) --------------> Service

openssl req -x509 -new -keyout ingress-tls.key -subj "/CN=ingress.test" -days 10000 -out ingress-tls.crt

## Securing Node Endpoints
Be aware of what ports your nodes are using. Use Network segmentation and firewalls to keep them safe.

ControlPlane ports
- Ports     | Purpose
- 6443      | Kubernetes API server
- 2379-2380 | etcd
- 10250     | kubelet API
- 10251     | kube-scheduler
- 10252     | kube-controller-manager    

Worker Node Listening Ports (kubeadm defaults)
- 10250 | kubelet API
- 30000-32767 | NodePort 

## Securing GUI Elements

Kubernetes Dashboard
If you are using any kind of GUI interfaces for kubernetes, make sure that you keep them secure.

## Verifying Kubernetes Platform Binaries
kubernetes provides checksum files for their binaries. The checksum  contains a hash calculated from the contents of a valid binary file.

# Revisit
- NetworkPolicies
- CIS Benchmarks
- Ingress tls
- Secure GUI tools
- Verify Binaries