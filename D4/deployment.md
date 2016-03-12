#Deployment View

##Introduction

Considering the wide use of Let’s Encrypt, it is important to describe the deployment of software to guarantee the proper operating in different environments. 
As defined(Nick, 2012), deployment describes the environment into which the system will be deployed and dependencies that the system has on elements of it. 
In this part, we will introduce a series of constraints about Let’s Encrypt including third party software requirements, technology compatibility and network requirements.

##Third Party Software Requirements

##Technology Compatibility

1. **Python**: It is required to use Python 2.6 or 2.7, while Python 3.x support is currently not available. When installing Let’s Encrypt, it will automatically check the version of Python in user environment and install it if there is no proper library.  

2. **ConfigArgParse**: It is required that the version of ConfigArgParse must higher than 0.9.3.

3. **cryptography**: At least version 0.7 should be used.

4. **psutil**: The version of psutil must higher than 2.1.0


##Network Requirements

Let’s Encrypt runs a certificate management agent on the web server. The node in the system can be divided into two categories: Server and Client. On its client side, it requires port 80 or 443 to be available. On its server side, Let’s Encrypt has rate limits for certificate issuance. These limits are in place primarily to protect services from both accidental and intentional abuse. Let’s Encrypt has the following rate limits in place:

- **Names/Certificate**:  is the limit on how many domain names user can include in a single certificate. This is currently limited to 100 names, or websites, per certificate issued.

- **Certificates/Domain**:  user could run into through repeated re-issuance. This limit measures certificates issued for a given combination of Public Suffix + Domain (a "registered domain"). This is limited to 5 certificates per domain per week.

- **Registrations/IP address**: limits the number of registrations users can make in a given time period; currently 500 per 3 hours. This limit should only affect the largest users of Let's Encrypt.

- **Pending Authorizations/Account**: limits how many times an ACME client can request a domain name be authorized without actually fulfilling on the request itself. This is most commonly encountered when developing ACME clients, and this limit is set to 300 per account per week.


There is no limit to the number of certificates that can be issued to different domains.

##Results

If the system is successfully deployed, then all of the registration files and keys could be seen using `tree /etc/letsencrypt/` Then the output should be like this:

```
/etc/letsencrypt
├── accounts (ROOT PROTECTED)
│   └── acme-v01.api.letsencrypt.org
│       └── directory
│           └── 405ef3114adb6eabf5311420fa3f162d
│               ├── meta.json
│               ├── private_key.json
│               └── regr.json
├── archive (ROOT PROTECTED)
│   └── example.com
│       ├── cert1.pem       (server public certificate)
│       ├── chain1.pem      (intermediate certificate(s))
│       ├── fullchain1.pem  (server cert followed by intermediate(s))
│       └── privkey1.pem    (server private keypair)
|                           (NOTE: your browser already has the root cert)
├── csr
│   └── 0000_csr-letsencrypt.pem
├── keys (ROOT PROTECTED)
│   └── 0000_key-letsencrypt.pem
├── live (ROOT PROTECTED)
│   └── example.com
│       ├── cert.pem -> ../../archive/example.com/cert1.pem
│       ├── chain.pem -> ../../archive/example.com/chain1.pem
│       ├── fullchain.pem -> ../../archive/example.com/fullchain1.pem
│       └── privkey.pem -> ../../archive/example.com/privkey1.pem
└── renewal
    └── example.com.conf
```
