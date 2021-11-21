# SystemHardening

- Host OS security
- IAM Roles
- Network Security
- AppArmor

## Host OS Security Concerns
### HostNamespaces
Containers use OS namespaces (different from a k8s namespace) to isolate themselves from other containers and the host. Pods can use a hostnamespace but it comes with security risks.
- hostIPC
- hostNetwork
- hostPID

All these settings are set to false by default. Enable them when its absolutely necessary.

### Priviledged Mode
Allows containers to access host-level resources and capabilities, much like a non-container process running directly on the hosts.

## IAM Roles
