# The Simple Certificate Authority

In the previous example, Alice and Bob exchanged public keys. If this exchange was compromised in some way, i.e. someone intercepted and changed the message or was imperonating one of the party members, then all future exchanges cannot be trusted. 

The first step in making this a bit more secure is to establish a common entity that both Alice and Bob already trust that can confirm that the party members say who they say they are. 


## Digital Certificates & Certificate Authority

A Digital Certificate is a document that contains a 

- public key 
- identifying information (Bob or Alice's name in this example)
- a digital signature that can be used to verify the contents has not been edited.

A Certificate Authority is a trusted source. It creates digital certificates from certificate requests.

[wikipedia | Public key certificate](https://en.wikipedia.org/wiki/Public_key_certificate)
[fortnet | Digital Certificates](https://www.fortinet.com/resources/cyberglossary/digital-certificates)
[digicert | what is a CA?](https://www.digicert.com/blog/what-is-a-certificate-authority)


## Certificate Signing Request (CSR)

A CSR is a document that provides the nessesary information for a Certificate Authority to issue a Digital Certificate; such as the public key and comon name. It is signed by the corresponding private key. The content and format are dictaated by the PKCS #10 standard.

[SSL | What is a CSR](https://www.ssl.com/faqs/what-is-a-csr/)



## The example:

[SSL | Manually Generate a Certificate Signing Request (CSR) Using OpenSSL](https://www.ssl.com/how-to/manually-generate-a-certificate-signing-request-csr-using-openssl/)

The OpenSSL command below will generate a 2048-bit RSA private key and CSR:

```
openssl req -newkey rsa:2048 -keyout alice.key -out alice.csr
```

Let’s break the command down:

- `openssl` is the command for running OpenSSL.
- `req` is the OpenSSL utility for generating a CSR.
- `-newkey rsa:2048` tells OpenSSL to generate a new 2048-bit RSA private key.
- `-keyout alice.key` specifies where to save the private key file.
- `-out alice.csr` specifies where to save the CSR file.

Alice will then be prompted a bunch of questions - the only required one is the Common Name field. Under normal circumstances is used for the Fully Qualified Domain Name (FQDN) of the website this certificate will protect (more info here [SSL | What is a FQDN](https://www.ssl.com/faqs/what-is-a-fully-qualified-domain-name/)). 

For this example, Alice's common name will be Alice.



