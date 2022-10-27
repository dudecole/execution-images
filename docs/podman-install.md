## Container Engine Requirements

### Container Engines

Ansible Builder (`ansible-builder`) is capable of using
`docker` or `podman` container engines to create the Execution
Environment images.  This document was 
created using `podman`, however docker engine will have the 
equivalent capabilities in terms of `ansible-builder` 
using docker engine for creating the Execution/Container images. 

>    NOTE: `Docker` engine lacks the functionality to disable TLS/SSL certificate inspection.

### Podman 

**STEP 1:**

Install Podman

    sudo yum install podman


