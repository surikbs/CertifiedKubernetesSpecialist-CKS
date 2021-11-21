# Domain 5 - SupplyChain Security


## Static Analysis of user workloads 
   ### Docker file.
    - USER root : The container process runs as Root by default. Its usually a good practice to avoid this. 
        Make sure that the final USER directive in the dockerfile is not set to root or 0.
    - :latest tag : Try to avoid using the :latest tag in the FROM directive of your docker file. 
        Instead, reference to a specific, versioned tag.
    - Unnecessary software: do not install unnecessary softwares.
    - Sensitive data: use kubernetes secrets to pass sensitive data to the container at the runtime.

   ### Resource YAML files
    - HostNamespaces : dont let containers user hostnamespaces.
    - Privileged Mode : use it unless absolutely necessary.
    - :latest Tag : avoid automatically downloading new, potentially unvetted images.
    - Run as Root: avoid running as root.

   ### Scanning images for known Vulnerabilities.
    - Trvy is a commandline tool that allows you to scan container images for vulnerabilities that have already
        been discovered and documented by the security experts.
    - Trvy generates a reort that lists known vulnerabilities found in the images.

## Scanning images with an Admission Controllers

   - The ImagePolicyWebhook admission controller allows a backend webhook to make admission decisions.
   - Intercept requests to the kube API before objects are created. It allows, prevent or make changes to the objects before creating.
   - Allows to use custom logic to approve or deny the creation of workloads.
   - you can use external applications as admission controller to scan images for vulnerabilities automatically as workloads get created.
