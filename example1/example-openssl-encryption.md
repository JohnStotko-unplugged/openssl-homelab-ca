# Asymetric Cryptography to send secure messages using OpenSSL

For this exercise, we will go through how to use asymetric cryptography to send secure messages with the OpenSSL toolkit. In this example, Alice wans to send Bob a message.

## Key Generation

TODO switch to use genpkey instead... see [OpenSSL Documentation | openssl-genpkey Notes](https://docs.openssl.org/master/man1/openssl-genpkey/#notes)

- [OpenSSL Documentation | openssl-genrsa](https://docs.openssl.org/3.4/man1/openssl-genrsa/)
- [OpenSSL Documentation | openssl-rsa](https://docs.openssl.org/3.4/man1/openssl-rsa/)

> There doesn't seem to be any formal standards on naming conventions for key files. However, there are some common ones. ssh-keygen uses `<id-algorithm>` for the private key, and `<id-algorithm>.pub` to denote the public key [die.net | ssh-keygen](https://linux.die.net/man/1/ssh-keygen).

Let's generate some keys. When generating keys, you will be prompted for a passphrase. Since this is just a learning exercise, use something that is easy to remember. When getting data from the key, you will be prompted for this passphrase again.

This command generates a private key for Alice. 
```
openssl genrsa -aes256 -out alice.key 2048
```

This one uses the private key to derive the public key, and writes out the public key.
```
openssl rsa -in alice.key -pubout -out alice.pubkey
```

To inspect the keys, you can use these commands
``` 
openssl rsa -in alice.key -noout -text
openssl rsa -in alice.pubkey -noout -text
```

And let's do the same for Bob
```
openssl genrsa -aes256 -out bob.key 2048
openssl rsa -in bob.key -pubout -out bob.pubkey
```

## Sending the message

- [OpenSSL Documentation | openssl-pkeytutl](https://docs.openssl.org/master/man1/openssl-pkeyutl/)
- [OpenSSL Documentation | openssl-dgst](https://docs.openssl.org/master/man1/openssl-dgst/)

In this directory there is a file called `message.txt`. Alice would like to send the contents of this message to Bob.

First, Alice and Bob exchange public keys. 

Alice has these files
- alice.key
- alice.pubkey
- bob.pubkey
- message.txt

Bob has these files
- bob.key
- bob.pubkey
- alice.pubkey

### Encryption and Signing

Alice will encrypt the message using Bob's public key so that only Bob can read it.

```
openssl pkeyutl -encrypt -in message.txt -inkey bob.pubkey -pubin -out enc_message.txt
```

Alice can then create a signature of the encrypted message so that Bob knows it came from her.

```
openssl dgst -sha256 -sign alice.key -out enc_message.txt.signature enc_message.txt
```

Alice then sends Bob the encrypted message (enc_message.txt) and signature (enc_message.txt.signature)

### Verifying and Decrypting

When Bob recieves this message, he will want to verify it using Alice's public key

```
openssl dgst -sha256 -verify alice.pubkey -signature enc_message.txt.signature enc_message.txt
```

Now that Bob is sure this message came from Alice, he can decrypt the encrypted message using his private key

``` Bob
openssl pkeyutl -decrypt -in enc_message.txt -inkey bob.key 
```