---
title: "Cryptography"
date: 2024-07-29T03:09:01+05:30
tags: ["cryptography", "unix"]
description: "Notes on encryption private and public keys"
---

Every time I am working on something that uses or involves encryption I scramble through wikipedia about some terminologies and algorithms. Hope this will help me save some time in future when I work on something involving cryptography.

# 1. Asymmetric Key Cryptography / Public Key Cryptography
This involves private key and public key. Most common used algorithms that we see - RSA, ECDSA, EdDSA (ed25519).
In a public-key encryption system, anyone with a public key can encrypt a message, yielding a ciphertext, but only those who know the corresponding private key can decrypt the ciphertext to obtain the original message.

- #### Can the private key encrypt a message?
    Yes the private key can encrypt a message but it is only used for digital signatures only not for bidirectional communication.

- #### If private key isn't used to encrypt then how does bidirectional communication work? (e.g SSH, TLS)
    In bidirectional communication two sets of public-private key pairs are used. According to wikipedia - 
    "In the Diffie–Hellman key exchange scheme, each party generates a public/private key pair and distributes the public key of the pair. After obtaining an authentic (n.b., this is critical) copy of each other's public keys, Alice and Bob can compute a shared secret offline. The shared secret can be used, for instance, as the key for a symmetric cipher, which will be, in essentially all cases, much faster."


# 2. Symmetric Key Cryptography
This is not called private key cryptography as one might try to guess, of course a secret key is used which is not shared.
Symmetric-key algorithms use the same cryptographic keys for both the encryption of plaintext and the decryption of ciphertext.
AES is the most common symmetric key algorithm that we come across.
After exchanging public keys TLS, SSH and other cryptosystems use them to generate symmetric key for further communication which is faster.



# 3. Is it possible to extract public key from private key?
Yes and No.

Let's consider RSA only here.
the secret exponent e is private
the public exponent d is public, often 2^16-1 = 65537
and n = p * q is included with public and private key.

Now if you throw away p and q you can't extract d given e because you'd have to factor out p and q from n.

When you say "private key" it also refers to the file that stores the secret such as .pem. This file is essentially a text file containing base64 encoded data in ASN.1 format  between opening and closing messages like this.
```
-----BEGIN PRIVATE KEY-----
MIIBVgIBADANBgkqhkiG9w0BAQEFAASCAUAwggE8AgEAAkEAq7BFUpkGp3+LQmlQ
Yx2eqzDV+xeG8kx/sQFV18S5JhzGeIJNA72wSeukEPojtqUyX2J0CciPBh7eqclQ
2zpAswIDAQABAkAgisq4+zRdrzkwH1ITV1vpytnkO/NiHcnePQiOW0VUybPyHoGM
/jf75C5xET7ZQpBe5kx5VHsPZj0CBb3b+wSRAiEA2mPWCBytosIU/ODRfq6EiV04
lt6waE7I2uSPqIC20LcCIQDJQYIHQII+3YaPqyhGgqMexuuuGx+lDKD6/Fu/JwPb
5QIhAKthiYcYKlL9h8bjDsQhZDUACPasjzdsDEdq8inDyLOFAiEAmCr/tZwA3qeA
ZoBzI10DGPIuoKXBd3nk/eBxPkaxlEECIQCNymjsoI7GldtujVnr1qT+3yedLfHK
srDVjIT3LsvTqw==
-----END PRIVATE KEY-----
```

The format stores all the parameters which can be used to generate a public key. You can get the public key from private pem files by using ssh-keygen or openssl
```
   $ ssh-keygen -f id_ed25519 -y
   $ ssh-keygen -f rsa.pem -y
   $ openssl ec -in private.pem -pubout
   $ openssl rsa -in rsa.pem -pubout
```

So you can extract public key from the private key file but obviously you cannot extract one parameter from another.

# References

* https://en.wikipedia.org/wiki/Symmetric-key_algorithm
* https://en.wikipedia.org/wiki/Public-key_cryptography
* https://superuser.com/questions/1515261/how-to-quickly-identify-ssh-private-key-file-formats
* https://lapo.it/asn1js/
