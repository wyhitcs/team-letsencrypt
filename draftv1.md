#1. Background Knowledge

##1.1 SSL/TLS

Transport Layer Security Protocol (TLS) and its predecessor -- Secure Socket Layer Protocol (SSL) are both referred to as “SSL”. 
These two protocols aim to provide encrypted communication between two parties. For SSL/TLS communications, the server side needs to provide a Digital Certificate (for convenience it is referred to SSL certificate for the rest of this Chapter) to verify its identity (to prove that the server owns a set of domain names such as www.google.com, www.paypal.com).

##1.2 ACME protocol

The identity of internet entities (client and server) need to be verified. However, today’s verification is done by some ad-hoc mechanisms which are not suitable for development of online verification in the future. 
ACME protocol aims to address such issue by standardizing and automating the procedure of verification. In other words, it is a “bag of procedures” by doing which a Certificate Authority(CA) issues a certificate to a client.

##1.3 Certificate Authority
CA is an online organization which issues a Digital Certificate.
A certificate needs to install a software called boulder to enable Let’s Encrypt, which will be discussed later.

##1.4 Let’s Encrypt

Let’s Encrypt is a software implementing ACME protocol.
The workflow is summarized as follows: 
1) when a certificate is requested, Let’s Encrypt wraps necessary information (the account, the domain name etc) into standard format and sends them to boulder of a CA; 
2) CA responds the request with a challenge, saying that “since you claim the ownership of the domain xxxxx.com, please add a .txt file containing the string blablabla in the folder aaa/bbb/ccc”; 
3) Let’s Encrypt will automatically solve this challenge for the client; 
4) CA grants a certificate after solving the challenge.

#2. Views and Perspectives

In this section, we present a comprehensive study of Let’s Encrypt covering the stakeholders, the architecture, the deployment of the software, the variability and the evolution of Let’s Encrypt.

##2.1 Stakeholders Analysis
We identify 6 types of stakeholders for Let's Encrypt.

###2.1.1 Acquirers

Acquirers are the people who launch and organize this project.
Their job also includes estimating the needs of users and setting goals for developers.


For Let’s Encrypt, there are 3 acquirers:

- [Electonic Frontier Foundation](https://www.eff.org/)

- [Mozilla Foundation](https://www.mozilla.org/en-US/)

- [University of Michigan](https://en.wikipedia.org/wiki/University_of_Michigan)


The Electronic Frontier Foundation is the most important acquirers who provides most of labor to Let’s Encrypt.
Mozilla Foundation and University Michigan also have personnel devoting into this project.

###2.1.2 Developers

Developers are the people who devote themselves directly to the development of the project.
For Let’s Encrypt, the developers are mainly programmers.

There are 165 contributors for Let’s Encrypt and here we only list the most important ones:

- @[Liam Marshall](https://github.com/ArchimedesPi)

- @[bmw](https://github.com/bmw)

- @[Felix Rieseberg](https://github.com/felixrieseberg)

- @[Francois Marier](https://github.com/fmarier)

- @[Harlan Lieberman-Berg](https://github.com/hlieberman)

- @[Joona Hoikkala](https://github.com/joohoi)

- @[Jacob Hoffman-Andrews](https://github.com/jsha)

- @[Jakub Warmuz](https://github.com/kuba)

- @[Martin Thomson](https://github.com/martinthomson)

- @[Peter Eckersley](https://github.com/pde)

- @[Roland Bracewell Shoemaker](https://github.com/rolandshoemaker)

- @[Sagi Kedmi](https://github.com/sagi)

Since the project lasts for only a short time (less than 4 months) and still in progress, some developers are also responsible for testing and maintaining. For example, the developer @[bmw](https://github.com/bmw) also responds to issues and pull requests in github.

###2.1.3 Users

Users are those who use the products. For Let’s Encrypt, the users are all web servers who use Let’s Encrypt to request Digital Certificate for SSL/TLS communication. The number of users is growing fastly since the beginning of the project.

###2.1.4 Sponsors

Sponsors are those who provides financial support for the project.
For Let’s Encrypt, sponsors provide financial supports for programmers.

The major sponsors are listed below:

- [Mozilla](https://www.mozilla.org/)

- [Akamai](http://www.akamai.com/)

- [Cisco](http://www.cisco.com/)

- [Electronic Frontier Foundation](https://www.eff.org/)

- [OVH](https://www.ovh.com/)

- [Chrome](https://www.google.com/chrome/)

###2.1.5 Additional Stakeholder

- [IdenTrust](https://www.identrust.com/) (Root certificate provider)

Let us first explain the concept of root certificate providers.
A certificate provider is a root certificate provider if and only if:
1) it can authorize other organizations to be the new certificate providers;
2) it doesn’t need any other organizations to authorize it to be a certificate provider.
For example, a certificate providers A can authorize organization B to be a new certificate provider.
Later, B can authorize C to be a new certificate provider.
A in our case is the root certificate provider.
IdenTrust is a root certificate provider who authorizes Let’s Encrypt.

The graph of stakeholders is shown below:


![stakeholder](https://github.com/delftswa2016/team-letsencrypt/blob/master/D1/stakeholder.png)

                                          Figure 1.1. Stakeholders Analysis


##2.2 Context View

This section is about Context View, which includes three subsections: system scope, entities and interfaces and impact to environment.

###2.2.1 System scope and Responsibilities

Users can do the following things using Let's Encrypt:

- users can register certificate for their server

- users can revoke certificate

- users can create their own account

- users can modify their profile

- users can turn on/off the notice of expiring date of certificate

- users can choose which ACME solution to use

Let's Encrypt allows users to obtain, renew and revoke a certificate.
Besides, it also improves user experience by for example, allowing users to turn on/off the notice of expiring date of a certificate.

###2.2.2 Entities, data and interfaces

####2.2.2.1 Entities and Interfaces

There are ten entities related to Let's Encrypt.
First, the development of Let's Encrypt is based on GitHub platform.
Second, Python is identified as the only language dependency.
Third, Freenode (online community) provides a platform for the developers to communicate.
Fourth, Let's Encrypt is authorized by a root certificate provider IdenTrust.
Fifth, the release is based on the Python Package Index (PYPI).
Sixth, Let's Encrypt extends its support to a larger number of servers by adding plugins such as Apache, Nginx and Webroot.
Seventh, the testing process is facilitated by test tools such as Tox and Travis CI. 
Eighth, Let’s Encrypt supports a number of operating systems including Unix-ish OS (Arch, Debian, FreeBSD etc).
Ninth, the users are a large number of web servers including [blueboard.cz](https://blueboard.cz/) and [checkdomain](https://www.checkdomain.de/ssl/zertifikat/ssl-free/).
Finally, competitors are other softwares based on ACME.


####2.2.2.2 Impact of the System on Environment

The impact of system on environment concerns about the dependencies of other system on Let's Encrypt and other systems’ decommissions and data migration.
Since Let's Encrypt can be used as a plugin embedded in other certificate systems, the performance of those systems depend on Let's Encrypt.
As a newly developed project, there is not any system decommissions so far.
Finally, Let's Encrypt is independently developed by ISRG.

