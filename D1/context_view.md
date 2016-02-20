Context View
===========================
###System scope and Responsibilities
The system scope of let’s encrypt is:
helping ACME clients (web servers which need certificates) and ACME servers(i.e. certificate authorities (CAs)) to deal with request/response of digital certificates. i.e. The computers using TLS protocols need Digital Certificate from a CA to identify themselves.
And let’s encryption serves as a communicator between web servers and CAs.

###Environment
All computers communicating with TLS protocols constitute the environment of let’s encrypt.
Interaction between system and environment can be depicted as following scenarios:

1. If computer A is communicating with computer B, and A need a digital certificate to certificates that A is really A (not some bad guys spoofing B) and the public key A sends to B is the real public key from A. A can request a certificate from let’s encrypt. A send its public key to let’s encrypt, and let’s encrypt will encrypt the as well as and some basic information of computer A (may be the name, url of A, the name and url of root CA) with the private key of let’s encrypt.

2. B receive the certificate of A (the certificate is issued by let’s encrypt). If B trust let’s encrypt or the root CA of let’s encrypt(that means that B has a public key of let’s encrypt), B can decrypt the certificate and find that: first, A is really A. And then, B can take out the public key of A inside the certificate and use it to decrypt message from A and encrypt message to A.
