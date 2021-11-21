# 01. Cluster Setup
-  NetworkPolicies
- CIS Benchmarks (kube-bench)
- Ingress
- Attack Surfaces (openports, GUI and others)
- Verifying K8s binaries

### Network Policies
- NetworkPolicies are an application-centric construct which allow you to specify how a pod is allowed to communicate with various network "entities" over the network
- By default, if no policies exist in a namespace, then all ingress and egress traffic is allowed to and from pods in that namespace. The following examples let you change the default behavior in that namespace.
#### Restricting Default Access with NetworkPolicies
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

#### Allow limited access with NetworkPolicies
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
    
    Client pod --------------------------Ingress Rule -> Nginx pod (incoming traffic to Nginx Pod)
