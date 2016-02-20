Issue Analysis
===========================
###1. Title: Renewer should report failures via exit status [(\#2508)](https://github.com/letsencrypt/letsencrypt/issues/2508)

**Description:** This issue proposes two things: 1) the *renew()* function should exit with a non-zero value if a renew of the certificate fails. It means there should be some feedback to the users/developers if the renew fails. 2) a “quiet” option should be added that, once it is used, there won’t be any status reported on the command line interface.

**Stakeholder:** Developers, users

###2. Title: Switch le-auto symlink to copy instead [(\#2501)](https://github.com/letsencrypt/letsencrypt/issues/2501)

**Description:** The letsencrypt-auto symlink was turned at the root of the project into a copy of the letsencrypt-auto of the latest release. A line to the release script was added to copy the latest build to the root. Our instructions used to tell people to *git pull* and then run *le-auto* from the root. This will keep them from running a bleeding-edge master version and allow us to tiptoe less around changes to *le-auto* (double reviews, etc.).

**Stakeholder:** Users

###3. Title: Letsencrypt-auto fails with SSL error to pypi.python.org [(\#2499)](https://github.com/letsencrypt/letsencrypt/issues/2499)
**Description:**
This issue reports a bug of *letsencrypt* that the letsencrypt throws an error while running for SSL process.The issue hasn’t been settled, but the comments proposed that it may be because of some function circumvent hash checking. The relevant functions may be *cryptography()*, *cffi()*, *set_requires()*.

**Stakeholder:**Users
###4. Title: Apache installer has trouble with missing vhost on renewal [(\#2491)](https://github.com/letsencrypt/letsencrypt/issues/2491)

**Description:**
This is a bug may be related to vhost of letsencrypty. The comments says after fixing issue [(\#1991)](https://github.com/letsencrypt/letsencrypt/issues/1991), the problem is somehow reduced, but it still occassionaly happen when users install *letsencrypt* in their first time.

**Stakeholder:** Users

###5. Title: Apache plugin error: 'Error parsing variable' when envvars are used [(\#2481)](https://github.com/letsencrypt/letsencrypt/issues/2481)

**Description:**
This issue reports a bug that when users use the command ./letsencrypt-auto --apache, they receive the following message:
>the apache plugin is not working; there may be problems with your existing configuration.

The error was PluginError (u'Error Parsing variable: ${INSTANCE\_ADDRESS}'). It looks like the plugin is not able to parse the env_var in the address field.
This issue is still open and there is not a solution yet.

**Stakeholder:** Users

###6. Title: Automatic upgrading in letsencrypt-auto downgrades to incompatible version [(\#2490)](https://github.com/letsencrypt/letsencrypt/issues/2490)

**Description:**
This issue reports that the auto script *letsencrypt-auto-source/letsencrypt-auto* self-upgrades the current version to a downgrade incompatible version. This is because that  the auto script which simply checks if the versions are different. The problem is fixed in #2492 and this issue is closed.

**Stakeholder:** Developers, users

###7. Title: Fatal certlint error in recently signed certificate [(\#2463)](https://github.com/letsencrypt/letsencrypt/issues/2463)
**Description:**
This issue is about a bug for the signed certificate. The log is
>FATAL: ASN.1 Error in X520CommonName and for more details, see:
https://crt.sh/?id=12791738&opt=cablint.

A reply to the issue mentions that he has addressed this problem in issue #2488 and it is not a client bug at all. For issue #2488, see https://github.com/letsencrypt/boulder/issues/1488.

**Stakeholder:** Users

###8. Title: webroot only verifies through port 80 [(\#2440)](https://github.com/letsencrypt/letsencrypt/issues/2440)
**Description:**
This issue is about the webroot option. A user wanted to use port other than port 80 to be the webroot option of verification, since his port 80 is firewalled. It turns out that the Letsencrypt has already provided two other validation methods --- the SNI challenge (which can completely use a TLS listener on port 443) and the DNS challenge (which just requires being able to add records to your DNS zone).

**Stakeholder:** Users, developers

###9. Title: Letsencrypt certificate expiration notice [(\#2483)](https://github.com/letsencrypt/letsencrypt/issues/2483)
**Description:**
This issue is about user experience. A user of *Letsencrypt* got an email notifying that the certificate will expire in a few days. However, the problem is that this is about a certificate he regularly replaces by auto-renewing it. It is not solved since there exists situations where a reminder is still worthwhile, even though a replacement certificate has been issued. One obvious heuristic suggested by the developers is to check each of the names on the old certificate. If any publicly-accessible web sites are still using the old certificate, a reminder is definitely in order, whether or not a replacement cert has been issued. This doesn't solve the whole problem, but can be used in conjunction with other heuristics to reduce the number of false negatives without increasing the number of false positives.

**Stakeholder:** Developers, users

###10. Title: How to implement Letsencrypt acme challenge on AWS API Gateway? [(\#2439)](https://github.com/letsencrypt/letsencrypt/issues/2439)
**Description:**
The issue is about a user trying to setup an API Gateway that automatically renews his certificate. He managed to generate a certificate using *letsencrypt* by spinning up an instance. When he tried to setup an endpoint *example.com/.well-known/acme-challenge* he couldn't find any *ode.js* code that will return the appropriate response. The problem was solved by a developer by giving a link about the ACME specification draft, sample code and libraries related. 

**Stakeholder:** Developers, users
