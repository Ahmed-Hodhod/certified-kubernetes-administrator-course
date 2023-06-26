# Practice Test - View Certificates
  - Take me to [Practice Test](https://kodekloud.com/topic/practice-test-view-certificate-details/)
  
Solutions to practice test - view certificates
- Identify the certificate file used for the kube-api server
  
  <details>
  
  ```
  $ cat /etc/kubernetes/manifest/kube-apiserver.yaml
  
  Answer: /etc/kubernetes/pki/apiserver.crt
  ```
  </details>
  
- Identify the Certificate file used to authenticate kube-apiserver as a client to ETCD Server
  
  <details>
  ```
  $ cat /etc/kubernetes/manifest/kube-apiserver.yaml
  Answer: /etc/kubernetes/pki/apiserver-etcd-client.crt
  ```
  </details>
  
- Look for kubelet-client-key option in the file /etc/kubernetes/manifests/kube-apiserver.yaml
  
  <details>
  ```
  Answer: /etc/kubernetes/pki/apiserver-kubelet-client.key
  ```
  </details>
  
- Look for cert file option in the file /etc/kubernetes/manifests/etcd.yaml
  
  <details>
  ```
  Answer: /etc/kubernetes/pki/etcd/server.crt
  ```
  </details>
  
- Look for CA Certificate in file /etc/kubernetes/manifests/etcd.yaml
  
  <details>
  ```
  Answer: /etc/kubernetes/pki/etcd/ca.crt
  ```
  </details>
  
- Run the command openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text
  
  <details>
  ```
  $ openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text
  ```
  </details>
  
- Run the command openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text and look for issuer
  
  <details>
  ```
  $ openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text 
  ```
  </details>
  
- Run the command openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text and look at Alternative Names
  
  <details>
  ```
  $ openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text
  ```
  </details>
  
- Run the command openssl x509 -in /etc/kubernetes/pki/etcd/server.crt -text and look for Subject CN.
  
  <details>
  ```
  $ openssl x509 -in /etc/kubernetes/pki/etcd/server.crt -text
  ```
  </details>
  
- Run the command openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text and check on the Expiry date.
  
  <details>
  ```
  $ openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text
  ```
  </details>

   
- Root Certificate:  'openssl x509 -in /etc/kubernetes/pki/ca.crt -text' and look for validity
  
  <details>
  ```
  $ openssl x509 -in /etc/kubernetes/pki/ca.crt -text
  ```
  </details>
  
- Problem: kubectl utility is not able to access kub-apiserver
  You can't use kubectl now, so you should use docker instead
  `docker ps -a | grep -i kube-apiserver`
  `docker logs <container-id> `
  after checking the logs, the kubeapiserver can't connect the etcd server running on port 2379 and this is because the certificate file could not be found
  in the given directory.
  So, you need to change the directory to the right one.
  `cat \etc\kubernetes\manifests\etcd.yaml`

  note : you can use crictl instead of docker based on the environment. 
  Inspect the --cert-file option in the manifests file.
  
  <details>
  ```
  $ vi /etc/kubernetes/manifests/etcd.yaml
  ```
  </details>
  
- ETCD has its own CA. The right CA must be used for the ETCD-CA file in /etc/kubernetes/manifests/kube-apiserver.yaml.
  after checking the logs from the containers, we found that a connection is rejected to the etcd server and we assumed it is from
  the apiserver. so we are gonna check the etcd records in the kube-apiserver.yaml manifest file.
  ETCD CA certificate can be found in /etc/kubernetes/manifests/pki/etcd/ca.crt 
  
  <details>
  ```
  View answer at /var/answers/kube-apiserver.yaml
  ```
  </details>






  
