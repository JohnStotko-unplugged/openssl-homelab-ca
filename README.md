# Creating and Using a Certificate Authority for HTTPS support in the Home Lab 



## References

- [GitHub | soarez/ca.md | How to setup your own CA with OpenSSL](https://gist.github.com/soarez/9688998)
- [Gitea | HTTPS Setup](https://docs.gitea.com/administration/https-setup)

## Basic Concepts


For illistrative purposes, let's suppose there are two people of interest.

- **A**lice
- **B**ob
- The rest of the world

Alice and Bob would like to talk. However, they want to make sure 

1. They are not being inpersonated
2. The messages they send to eachother are unaltered
3. The messages they send to eachother cannot be read by someone else

These three needs are what is meant by saying Alice and Bob want to "communicate securely".

### Asymmetric Cryptography

Asymmetric cryptography solves part of this problem by using two related keys, one private, one public.

Ciphered text with the public key can only be deciphered by the corresponding private key, and verifiable signatures with the public key can only be created with the private key.

In our example, Bob and Alice would use openssl to generate their public and private key. They would keep their private keys secret, and tell eachother to use their public keys to cipher messages sent to them.

To see this in action, look at [Asymetric Cryptography to send secure messages using OpenSSL](example1/example-openssl-encryption.md)

**TODO** How does this relate to Digital Signatures?



The weak spot in this arrangment is the key exchange. 

### Public Key Infrastructure

[Okta | What Is Public Key Infrastructure (PKI) & How Does It Work?](https://www.okta.com/identity-101/public-key-infrastructure/)



Q: How does Self-Hosting the CA differ from using a 3rd party?

## Acronyms

|     |                                                                                 |
|-----|---------------------------------------------------------------------------------|
| SSL | Secure Sockets Layer                                                            |
| TLS | Transport Layer Security                                                        |
| CA  | Certificate Authority                                                           |
| RSA | Rivest–Shamir–Adleman public-key cryptosystem used for secure data transmission |
| PKI | Public Key Infrastructure. An arrangment that binds public keys to identiies    |

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

