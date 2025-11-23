# Creating and Using a Certificate Authority for HTTPS support in the Home Lab 



## References

- [GitHub | soarez/ca.md | How to setup your own CA with OpenSSL](https://gist.github.com/soarez/9688998)
- [Gitea | HTTPS Setup](https://docs.gitea.com/administration/https-setup)

## Acronyms

|     |   |
|-----|---|
| SSL |   |
| TLS |   |
| CA  | Certificate Authority  |
| RSA |   |

## Explanation of Files

- **ca.key**    RSA Private Key for your Certifiacate Authority  
- **ca.crt**                                                     
- **cert.pem**                                                   
- **key.pem**                                                    


This command generates an RSA private key for your CA. The -aes256 flag encrypts the key with a passphrase, and 2048 specifies the key length in bits. You will be prompted to enter and verify a strong passphrase.

```
openssl genrsa -aes256 -out ca.key 2048
```

Create the Self-Signed CA Certificate.
This command uses the generated private key to create a self-signed X.509 certificate for your CA.

```
openssl req -new -x509 -key ca.key -days 3650 -sha256 -out ca.crt
```

-new -x509: Creates a new self-signed certificate.
-key ca.key: Specifies the private key to use.
-days 3650: Sets the certificate validity period to 10 years (365 * 10 days). Adjust as needed.
-sha256: Uses SHA256 for the signature hash.
-out ca.crt: Specifies the output file for the CA certificate.
You will be prompted to enter the passphrase for ca.key and then to provide distinguished name information (Country, State, Locality, Organization, Organizational Unit, Common Name, Email Address). The Common Name (CN) should clearly identify your CA (e.g., "My Root CA").

