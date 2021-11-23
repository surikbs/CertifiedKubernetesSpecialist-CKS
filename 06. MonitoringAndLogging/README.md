# Monitoring, Logging and Runtime Security

- Behavioral Analytics
- Immutable containers
- Audit logging

### Behavioral Analytics
Process of observing what is going on in a system and identifiying abnormal and potentially malicious events.

Falco is an open-source created by Sysdig. It monitors Linux system calls at runtime and alerts on any suspicious activity based upon configurable rules.

Falco can detect
- Privilege Escalation.
- File Access
- Binaries01

Falco command line: use -r <file> to supply a custom rules, use -M <seconds> to run Falco for a set of number of seconds.

eg: falco -r rules.yml -M 20

### Ensuring Containers are Immutable

Immutable(or stateless) containers do not change during their lifetime. Instead of being changed, they are replaced with new containers.

Immutability has security benefits.
Avoid Elevelated Privileges. 
- securityContext.privileged: true
- hostNetwork: true
- securityContext.allowPrivilegeEscalation: true
- securityContext.runAsUser:root or 0

Immutable containers do not change their code, which means they should not be able to write to the container file system.
Use readOnlyRootFilesystem:true to enforce this. A container without this setting might not be considered immutable.

### Audit logs

Chronological record of actions performed through kubernetes API. They are useful in realtime (threat detection) or for examining what happenend in the cluster after the fact.

The defined audit levels are: 
- None - don't log events that match this rule.
- Metadata - log request metadata (requesting user, timestamp, resource, verb, etc.) but not request or response body.
- Request - log event metadata and request body but not response body. This does not apply for non-resource requests.
- RequestResponse - log event metadata, request and response bodies. This does not apply for non-resource requests.

#### setting up Audit logging

Audit logging is configured by passing flags to kube-apiserver.
- You can pass a file with the policy to kube-apiserver using the --audit-policy-file flag. If the flag is omitted, no events are logged.
- --audit-log-path specifies the log file path that log backend uses to write audit events. Not specifying this flag disables log backend. - means standard out
- --audit-log-maxage defined the maximum number of days to retain old audit log files
- --audit-log-maxbackup defines the maximum number of audit log files to retain
- --audit-log-maxsize defines the maximum size in megabytes of the audit log file before it gets rotated
