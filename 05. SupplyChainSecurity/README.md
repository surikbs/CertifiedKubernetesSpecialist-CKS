# Domain 5 - SupplyChain Security

Analyzing the docker file
-  USER root : The container process runs as Root by default. Its usually a good practice to avoid this.
- :latest tag : Try to avoid using the :latest tag in the FROM directive of your docker file. Instead, reference to a specific, versioned tag.
- Unnecessary software: do not install unnecessary softwares.
- Sensitive data: use kubernetes secrets to pass sensitive data to the container at the runtime.
