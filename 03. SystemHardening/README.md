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

## Minimize IAM Roles
kubernetes containers running on AWS may be able to access IAM credentials and gain access to aws resources.
- principle of least privileges
- Block access

## Minimize external access to the network
kubernetes uses a virtual cluster network to allow communication between pods to pods and services freely and transparently, regardless on which node they are on.

## Appropriately use kernel hardening tools such as AppArmor, seccomp
AppArmor provides granular access control for programs running on Linux systems. An AppArmor profile is a set of rules that define what program can and cannot do.

An AppArmor profile can be loaded into AppArmor at the server level and activated in one of two modes.
- Complain mode: it reports and does not prevent the programs to run.
- Enforce mode: it reports and prevents the program to run.

### Using AppArmor in K8s container

 $ sudo apparmor_parser /path/to/file

- by default, the above command will load the profile in enforce mode. Use -C flag for complain mode. The command should be executed on all the nodes.
- use annotations to apply profiles to specific containers.