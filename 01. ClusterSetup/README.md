# 01. Cluster Setup
-  NetworkPolicies
- CIS Benchmarks (kube-bench)
- Ingress
- Attack Surfaces (openports, GUI and others)
- Verifying K8s binaries

### Network Policies
NetworkPolicies are an application-centric construct which allow you to specify how a pod is allowed to communicate with various network "entities" over the network
By default, if no policies exist in a namespace, then all ingress and egress traffic is allowed to and from pods in that namespace. The following examples let you change the default behavior in that namespace.

Default deny all ingress traffic 
#### Restricting Default Access with NetworkPolicies
You can create a "default" isolation policy for a namespace by creating a NetworkPolicy that selects all pods but does not allow any ingress traffic to those pods.
    
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
