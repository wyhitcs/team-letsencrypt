#Deployment View

##Introduction

##Third Party Software Requirements

##Technology Compatibility

##Network Requirements

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
