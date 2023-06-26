# TLS Basics
  - Take me to [Video Tutorial](https://kodekloud.com/topic/tls-basics/)
  
In this section, we will take a look at TLS Basics

## Certificate
- A certificate is used to guarantee trust between 2 parties during a transaction.
- Example: when a user tries to access web server, tls certificates ensure that the communication between them is encrypted.

  ![cert1](../../images/cert1.PNG)
  
  
## Symmetric Encryption
- It is a secure way of encryption, but it uses the same key to encrypt and decrypt the data and the key has to be exchanged between the sender and the receiver, there is a risk of a hacker gaining access to the key and decrypting the data.

  ![cert2](../../images/cert2.PNG)
  
## Asymmetric Encryption
- Instead of using single key to encrypt and decrypt data, asymmetric encryption uses a pair of keys, a private key and a public key.

  ![cert3](../../images/cert3.PNG)
  
  ![cert4](../../images/cert4.PNG)
  
  ![cert5](../../images/cert5.PNG)
  
  ![cert6](../../images/cert6.PNG)
  

#### How do you look at a certificate and verify if it is legit?
- who signed and issued the certificate.
- If you generate the certificate then you will have it sign it by yourself; that is known as self-signed certificate.

  ![cert7](../../images/cert7.PNG)
  
#### How do you generate legitimate certificate? How do you get your certificates singed by someone with authority?
- That's where **`Certificate Authority (CA)`** comes in for you. Some of the popular ones are Symantec, DigiCert, Comodo, GlobalSign etc.

  ![cert8](../../images/cert8.PNG)
  
  ![cert9](../../images/cert9.PNG)
The CAs use their private keys to validate sign the certificates.
The browser is built-in with the public keys of the CAs.
If you want to issue certificates for sites and domain hosted privately within an organization, you can deploy a CA internally and setup its public key to all browsers within the organization. 
  
  ![cert10](../../images/cert10.PNG)
  
## Public Key Infrastructure
- Trust Building exercise:
  - the site sends generates a pair of keys and send its public key along with a certificate to a CA to sign it with their own private keys 
  - when a user sends a request to the site, the site sends back its certificate signed by the CA
  - the user validates the certifcate using the public keys of the CAs that are stored in the browser
  - the user uses the public key sent with the certificate to encrypt a symmetic key and send it back to the site
  - the site decrypts the symmetric key using its own private key
  - the symmetric key is then used for communication
  - a similar process happens on the client side to verity that he is who he is
  - a client sends a certificate for signing and then send it to the site 
   
   ![pki](../../images/pki.PNG)
   
## Certificates naming convention
- public and private keys are a pair of key. You can encrypt the data with any of them and decrypt the data with the other key. 

  ![cert11](../../images/cert11.PNG)
  
  

  
   

  
  
  

  
  
  
  
  
  

  
  
