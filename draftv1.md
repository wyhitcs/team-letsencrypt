#Chapter Outline
##Abstract
Let’s encrypt is software which aims to help you automatically apply Digital certificates and install/renew those certificates. 
In this chapter, we will first introduce some necessary knowledge to you e.g. what is Digital Certificate, what is ACME.
After that, we will give you a brief instruction on the views and perspectives of Let's Encrypt: the Context View, Development View, Deployment View, Variability Perspectives and Evolution Perspectives. 

* Introduction
* Backgroud Knowledge
  * SSL/TLS Certificate
  * ACME Protocol
  * Certificate Authority
  * Let's encrypt
* View and Perspectives
  * Stakeholder Analysis
  * Context View
  * Development View
  * Deployment View
  * Variability Perspectives
  * Evolution Perspectives
* Conclusion


| section              | subsection           |
|----------------------|----------------------|
| 1. Backgroud Knowledge  |  1.1 SSL/TLS Certificate     |
|                         |  1.2 ACME Protocol           |
|                         |  1.3 Certificate Authority   |
|                         |  1.4 Let's encrypt           |
| 2.View and Perspectives |  2.1 Stakeholder Analysis    |
|                         |  2.2 Context View            |
|                         | 2.3 Development View         |
|                         | 2.4 Deployment View          |
|                         | 2.5 Variability Perspectives |
|                         | 2.6 Evolution Perspectives   |



##Plan and current status:
1.	We have finished the first version of Context View, Development View, Deployment View, Variability Perspectives and Evolution Perspectives.
2.	The contents are still in first version, hence they need to be revised.
3.	We have made a little contribution to Let's Encrypt project and further contribution is planned and in progress.

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
2) CA responds the request with a challenge, saying that “since you claim the ownership of the domain xxxxx.com, please add a .txt file containing the string in the folder aaa/bbb/ccc”; 
3) Let’s Encrypt will automatically solve this challenge for the client; 
4) CA grants a certificate after solving the challenge.

#2. Views and Perspectives

In this section, we present a comprehensive study of Let’s Encrypt covering the stakeholders, the architecture, the deployment of the software, the variability and the evolution of Let’s Encrypt.

##2.1 Stakeholders Analysis
We identify 6 types of stakeholders for Let's Encrypt.

###2.1.1 Acquirers

We identified three organizations as the acquirers of Let's encrypt:



- [Electonic Frontier Foundation](https://www.eff.org/)

- [Mozilla Foundation](https://www.mozilla.org/en-US/)

- [University of Michigan](https://en.wikipedia.org/wiki/University_of_Michigan)


These three foundations aim to provide a free, automated and open certificate authority to users. They establish a public benefit corporation, Internet Security Research Group (ISRG), to serve the purpose. The members in this group are the main contributors of Let’s Encrypt.


###2.1.2 Developers

So far 165 contributors have made contributions to the development of let’s encrypt. The core develoment team consisting of 11 people comes from a public benefit organization [Internet Security Research Group (ISRG)](https://letsencrypt.org/isrg/). The whole project is based on the ACME protocal proposed by ISRG. Unsurprisingly, they made most of the constributions to this project. For a newly established project like let’s encrypt, these developers more or less have responsibilities for testing, maintaining and github management. Here we list several major developers:

- @[Liam Marshall](https://github.com/ArchimedesPi)

- @[bmw](https://github.com/bmw)

- @[Felix Rieseberg](https://github.com/felixrieseberg)

- @[Francois Marier](https://github.com/fmarier)



The other contributors are either the users of let’s encrypt or github users who are interested in this project. Their constributions are mainly about improving commond line, fixing bugs and format.  In the documentation the team mentions that let’s encrypt is a beta software containing plenty of bugs. Thus, there is a lot of work to be done to improve user-friendliness.

###2.1.3 Users

The potential users of Let's encrypt are websites which want to obtain SSL/TLS certificate. Let’s encrypt automates the process of obtaining certificates and moreover, totally free of charge. As a result, the number of its potential users is huge. In fact, Let’s Encrypt has just issued its millionth certificate in March, 2016.


###2.1.4 Sponsors

Let's Encrypt is initialized by a non-profit organization (ISRG) to enbale secure communication over the Internet for the purpose of overcoming financial, technological and education barriers. Corporate Sponsorship helps speed up the process leading to a secure Web. The number of sponsers keeps incresing especially in the last four months. The dramatic rise in the nubmer of sponsors reflects the success and importance of the programme.
![Sponsors Growth](https://github.com/delftswa2016/team-letsencrypt/blob/master/D1/sponsors.png)

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


![Stakeholders Analysis](https://github.com/delftswa2016/team-letsencrypt/blob/master/D1/stakeholdersv2.png)



##2.2 Context View

This section concerns about the relationships, dependencies and interactions between Let’s Encrypt and its environment[[1](#Nick)]. It gives readers an image about the responsibilities and boundaries of  the system.

###2.2.1 System scope and Responsibilities

Let's Encrypt allows users to do following things:

- Create their own account
- Modify their personal profile
- Choose which ACME solution to use (optional)
- Turn on/off the notice of expiring date of certificate (optional)
- Register certificate for their server
- Revoke certificate

###2.2.2 Entities, data and interfaces

An overview of the relationship between Let’s Encrypt and its entities is shown in Figure 3.

As mentioned above, in order to become an Certificate Authority, Let's Encrypt must be authorized by the root certificate provider IdenTrust. The only language dependency of Let's encrypt is Python and the development is based on GitHub platform, where both ISRG members and individual github users can contribute. Let's encrypt also takes advantage of online communicty such as Freenode to allow discussion of issues among developers and users. The releases of different versions will be first strictly tested by Tox (standardize testing) and Travis CI (online test system) and then, posted using the Python Package Index (PYPI). 

Let's Encrypt provides service to a large number of users. Here we only give two examples, [blueboard.cz](https://blueboard.cz/) and [checkdomain](https://www.checkdomain.de/ssl/zertifikat/ssl-free/). Users can operate Let's Encrypt on a series Unix-ish Operating Systems like Arch, Debian, FreeBSD, etc. In addition, Let's Encrypt extends its support for different servers such as Apache, Nginx and Webroot by using a pluging architecture. 

Finally, there exist other softwares based on ACME that compete with Let’s Encrypt like [acme-tiny](https://github.com/diafygi/acme-tiny) and [simp_le](https://github.com/kuba/simp_le).

![contextview](https://github.com/delftswa2016/team-letsencrypt/blob/master/D1/contextview.png)

###2.2.3 Impact of the System on Environment

The impact of system on environment concerns about the dependencies of other system on Let's Encrypt and other systems’ decommissions and data migration. Since Let's Encrypt can be used as a plugin embedded in other certificate systems, the performance of those systems depend on Let's Encrypt. As a newly developed project, there is not any system decommissions because of Let's Encrypt so far. Finally, Let's Encrypt is independently developed by ISRG.



##2.3 Development View

The development view describes the architecture that supports the software development process.
The development view communicates the aspects of the architecture of interest to stakeholders from the building, testing, maintaining and enhancing the project[[1](#Nick)].
Based on this, the following article shows the common design model, module structure model and code line model of Let's Encrypt which give a technical overview of whole project.
To learn more details, each model is attached with complete descrption. In addition, technical debt of the project and coresponding solution or plan are described at the end of report.

###2.3.1 Module Structure Model
![Module Structure Model](https://github.com/delftswa2016/team-letsencrypt/blob/master/D2/module.structure.png)


The UML component diagram below gives an overview of module structure. Each package means a code module and arrow shows intermodule dependencies[[1](#Nick)].

- **User layer**: The first layer which is also the closest layer to users offers friendly UI for common clients to set up their own certificates. And the command way is also possible for developers to contribute to the project. 

- **Parser layer**: Receiving commands from upper layer, the coordinator module tries to check the compatibility to ensure the job can be done in certain environment. After that, the commands are parsed by parser and functions offered in next layer are recalled.

- **Function layer**: As the core layer of whole project, the third layer offers important functions to complete tasks. ACME protocol is a protocol that a certificate authority (CA) and an applicant can use to automate the process of verification and certificate issuance. The package contains a series of codes to complete this task. Challenge module represents automatic module to finish the challenge provided by CA. JSON Web Key (JWK) module offers a cryptographic key in this data structure.  Besides, Let’s Encrypt has a plugin architecture to facilitate support for different webservers, other TLS servers, and operating systems. Currently, general webservers like Apache and Nginx are supported.

- **Platform layer**: There’s no doubt that all previous code is based on python library which is included the bottom layer. In addition, OpenSSL, a software library to be used to secure communications against eavesdropping or to ascertain the identity of the party at the other end, is also the base of authentication process.

###2.3.2 Codeline Model

In this section, code structure of letsencrypt will be explored.
Build, Integration, Test and Release Approach also matters a lot in understanding the project organization.
In addition, Technical Debt is also analyzed.

####2.3.2.1 Source Code Structure

![Source Code Structure](https://github.com/delftswa2016/team-letsencrypt/blob/master/D2/code.structure.png)


- **acme** - ACME protocol implementation in Python.
  - **jose** - Implementation of the standards developed by “JavaScript Object Signing and Encyption”.
  - **\*.py** - Implementation of the ACME protocal objects.

- **docs** - Documentations.

- **letsencrypt-apache** - Two plugins are allowed: apache and nginx. This package is for apache plugin. 

- **letsencrypt-nginx** - This package is for nginx plugin. 

- **letsencrypt-auto-source** - This file is to receive and parse the shell instruction from user.

- **letsencrypt** - Source code of the whole project.
  - **display** - UI methods for user operations.
  - **plugins** - Plugins used to obtain certificates such as nginx.
  - **cli.py** - System entry code.
  - **client.py** - Implementation of the certificate obtain, install and revoke procedures.
  - **\*.py** - Implementation of the annotated ACME objects.

- **letshelp-letsencrypt** - This is responsible for the “help” instruction for let’s encrypt.

- **tests** - Include .sh files for test use.

- **tools** - Include .sh files which are frequently used.


####2.3.2.3 Build, Integration and Test Approach

To provide developers a convenient environment and also protect project source version, Let's Encrypt has an offical workflow for building, integrating and testing. 

##### **Build**

Let's Encrypt supports multiple operating systems. Users can install client according to their requirements. However, developers should have their own local copies based on their platform and run the client in developer mode from local tree for technical test. 

##### **Integration**

To contribute to Let's Encrypt project, developers have to follow the strict integration process made by project team.

- Developers can firstly find open issues in the github issue tracker or post new issues for their own idea.

- Then developers start to work on the problem, post a comment to let others know and seek feedback on the plan where appropriate.

- Once the developer gets a working branch, he/she can open a pull request. All changes in pull request must pass complete test. Generally it is sufficient to open a pull request and let Github and Travis run integration tests for you. However, if developers prefer to run tests, they can use Vagrant, using the Vagrantfile in Let’s Encrypt’s repository. 

- Let's Encrypt integrator will review the pull request and make a decision to merge or not.

##### **Test**

Before merging the changes in pull requests, all of them must have thorough unit test coverage, pass Let's Encrypt's integration tests, and be compliant with the coding style. This is done by popular online testing platform Travis CI. 
And developers can also do their own test before posting a request. Tox is recommended as official tools for running a full set of test like config file parsing test. For debugging, ipdb is introduced to execlude a series of faults.

###2.3.3 Common Design Model

####2.3.3.1 Common Processing

- **Instruction parsing**: 
Parsing instrcutions is considered to be a common process because 1) many modules rely on the user's instructions to determine the subsequent actions. 2) the parsing functions in different modules are similar to each other. Hence, the parsers should be put into a separate module. In fact, all codes relevant to the parser functions reside in the file *letsencrypt-auto* and *cli.py*.

- **ACME Objects processiong**: 
To implement ACME Protocol, letsencyrpt contains ACME Objects such as ACME account and ACME Exceptions. Those Objects are used almost everywhere in letsencrypt. When the user wants to request an account, for example, an account Object is created and returned. Such account will later be used when a certificate needs to be requested or renewed. Many modules of let's encrypt need to process ACME object(initiate,update,trasmit,destroy),so, for convenience concern the developers of let's encrypt wrap the functions used to process ACME Objects into a module.

- **Configuration processing**:
Configuration Object is "a bag of variables" used by almost all the other modules. For instance, the user might want to register his/her information like "I have a domain name abc.com; my account is xxxxx; my email is xxxx@gmail.com; my private key stores in xxxxx; please give me a certificate for that domain name". By passing a configuration Object containing these attributes, it enables letsencrypt to automatically obtain, renew and revoke a certificate.The need for processing a configuration Object (update/trasmit/destroy) is common in almost all modules of let's encrypt. For convenience concern, the configuration Objects (as well as the relevant functions) are wrapped into a module.

####2.3.3.2 Standard Design Approaches

The most important component of letsencrypt is its plugin.
There are 4 major types of plugins: User Interface (UI), Configurator, Authenticator and Installer.
The project uses interfaces to standardlize the contributions from developers.
The interfaces available for implementation of these plugins are defined in [interfaces.py](https://github.com/letsencrypt/letsencrypt/blob/master/letsencrypt/interfaces.py) and [plugins/common.py](https://github.com/letsencrypt/letsencrypt/blob/master/letsencrypt/plugins/common.py#L34). 

- **Configurator**:  This component is to automatically change the configurations. For example, if the user wants to                       change the folder to store certificates, it can be achieved by changing the configuration with the help of a corresponding                              configurator.  Considering that different configurators are for different purposes, letencrypt allows                           the developers to write their own configurator, which has to implement the corresponding interface.

 - **IDisplay**:  This plugin implements bindings to alternative UI libraries. Letsencrypt allows contributors to write their own UI, which has to implement IDisplay.

 - **Authenticators**: This component is to prove that the client deserves a certificate for some domain name by solving challenges received from ACME server.                         For different types of challenges (e.g. simple HTTP challenge, DVSNI challenge), different                                   Authenticators are needed. Letsencrypt allows developers to construct new authenticator,  which has to                 implement Authenticators Interface.

 - **Installer**: It setups the certificate in a server and possibly tweaks the security configuration to make it reliable and secure. For different web servers in user's machine, different Installers are needed. Developers are allowed to construct their own                          Installer, which has to implement Installer Interface.
 
####2.3.3.3 Standardlization of Testing
 
There are two types of tests: Unit Test and Intergration Test.

 - **Unit Test**: 
 Unit Test in letsencrypt aims to check whether the code style is compatiable with the required style. There are two files related to Unit Test: *pep8* and *tox.ini*. The Test can run off-line by developers.

- **Integration Test**: 
Integration Test is to test whether letsencrypt works well with boulder (a software runs in Certificate Authority which generate SSL/TLS). It is an online test running automatically to test a pull request. The module corresponding to Intergration Test is Travis CI.





###2.3.4 Technical Debt

The concept of technical debt refers to the accumulated consequences of the quick but dirty design into an evolving software program [[2](#Fowler)]. In other words, the danger occurs when people rush software by simply adding features into the program but never reflect their understanding of those features. As the “debt” accumulates, the complexity of maintaining the programs to reduce its deterioration to the whole software increases [[3](#Cunningham)].

####2.3.4.1 Code Duplication

In issue [#383](https://github.com/letsencrypt/letsencrypt/issues/383), code duplication exsits between apache and nginx plugins; and in issue [#698](https://github.com/letsencrypt/letsencrypt/issues/383), [Dockerfile-dev](https://github.com/letsencrypt/letsencrypt/blob/26c1f003d0d05397154fe63e1f452ed2148cfe75/Dockerfile-dev) and [Dockerfile](https://github.com/letsencrypt/letsencrypt/blob/26c1f003d0d05397154fe63e1f452ed2148cfe75/Dockerfile) also duplicated.

Duplicated code is code that has been produced by copying and then adapting existing code. Also known as “copy-and-paste development”, this strategy of producing code is frequently employed as a way of reusing software. 
On the first impression, code duplication seems to be a desirable approach to development as it is associated with reuse, implementation speed-up, and developer care. However, in the long term, code duplication implications can be very negative.
First of all, code duplication causes an increase in software size. Secondly, duplication has a direct influence on the difficulty of maintaining already developed code. Additionally, code-duplication can also be a sign of poor design, indicating that generic functionality has been not properly abstracted. Consequently, on long term, especially during the maintenance phase, code duplication is to be avoided.

####2.3.4.2 Documentation

For open source projects, the contribution of open source communities behind the huge development of the project is very evident. So it is very important that a transparent documentation system is followed in such community projects. Without proper documentation it is very difficult for other developers to understand the code developed by any user and to reuse the code or make it more efficient.Unfortunately, for Let's Encrypt, they do not provide a well-structured documentation for developers. There are 59 issuses labeled `documentation`. No architecture and module information, which makes understanding of how the project works and how the module connect with each other difficult to understand, which makes it difficult for developers not in their team to contribute to the project. In addition, insufficient documentation may also cause troubles for users.

####2.3.4.3 How developers deal with technical debt

For technical debt, developers always find it via Issues, discuss it and try to find a better solution to fix it. For the technical debt and related issues we mentioned above [#2155](https://github.com/letsencrypt/letsencrypt/issues/2155), [#2498](https://github.com/letsencrypt/letsencrypt/issues/2498) and [#2114](https://github.com/letsencrypt/letsencrypt/issues/2114) they use this kind of method to manage their technical debt. 
- Code Duplication: Recent advances in static code analysis and hardware performance made possible tool-based localization of code duplication in industrial contexts. 
For developers, they could use this kind of tools to detect duplicates and then improved by sharing common util functions (as they did in [#382](https://github.com/letsencrypt/letsencrypt/pull/382)).

- Documentation: The developers are professionals who may not be aware of the not sufficient documentation, but they are willing to fix it when users make an issue on the github as what they did in issue [#2216] (https://github.com/letsencrypt/letsencrypt/issues/2216) and [#2271](https://github.com/letsencrypt/letsencrypt/issues/2271).

##2.4 Deployment View

Considering the wide use of Let’s Encrypt, it is important to describe the deployment of software to guarantee the proper operating in different environments. 
As defined[[1](#Nick)], deployment describes the environment into which the system will be deployed and dependencies that the system has on elements of it. 
In this part, we will introduce a series of constraints about Let’s Encrypt including third party software requirements, technology compatibility and network requirements.

###2.4.1 Third Party Software Requirements

Identifying third party software requirements is essential for both developers and users. 
For developers, they can clearly know what tools or libraries are available for further use. 
Users can know what is exactly needed to apply such software on their environments. 

For Let’s  Encrypt, following softwares are required to be installed on user machine:

- **Python**: Python programming language;

- **OpenSSL**: a software library implementing the Secure Sockets Layer (SSL) and Transport Layer Security (TLS) protocols;

- **ACME**: Automatic Certificate Management Environment (ACME) protocol implementation in Python;

- **ConfigArgParse**: Python command-line parsing library;

- **Python Package Index (PyPI)**: a tool to help package and share Python modules;

- **cryptography**: a package which provides cryptographic recipes and primitives to Python developers;

- **psutil**: a cross-platform library for retrieving information on running processes and system utilization;

###2.4.2 Technology Compatibility

1. **Python**: It is required to use Python 2.6 or 2.7, while Python 3.x support is currently not available. When installing Let’s Encrypt, it will automatically check the version of Python in user environment and install it if there is no proper library.  

2. **ConfigArgParse**: It is required that the version of ConfigArgParse must higher than 0.9.3.

3. **cryptography**: At least version 0.7 should be used.

4. **psutil**: The version of psutil must higher than 2.1.0

###2.4.3 Network Requirements

Let’s Encrypt runs a certificate management agent on the web server. The node in the system can be divided into two categories: Server and Client. On its client side, it requires port 80 or 443 to be available. On its server side, Let’s Encrypt has rate limits for certificate issuance. These limits are in place primarily to protect services from both accidental and intentional abuse. Let’s Encrypt has the following rate limits in place:

- **Names/Certificate**:  is the limit on how many domain names user can include in a single certificate. This is currently limited to 100 names, or websites, per certificate issued.

- **Certificates/Domain**:  user could run into through repeated re-issuance. This limit measures certificates issued for a given combination of Public Suffix + Domain (a "registered domain"). This is limited to 5 certificates per domain per week.

- **Registrations/IP address**: limits the number of registrations users can make in a given time period; currently 500 per 3 hours. This limit should only affect the largest users of Let's Encrypt.

- **Pending Authorizations/Account**: limits how many times an ACME client can request a domain name be authorized without actually fulfilling on the request itself. This is most commonly encountered when developing ACME clients, and this limit is set to 300 per account per week.


There is no limit to the number of certificates that can be issued to different domains.


##2.5 Variability Perspective

In [[6](#Sven)], variability describes the ability to derive different products from a common set of artifacts. It is important for a good software to equip with variability to adapt to different environments, which largely satisfies the requirements of different stakeholders. Without doubt, Let’s Encrypt is such a software in developing. In the first section of report, a list of features and dependencies are given. A related model is built upon this. Additionally, the implementation and binding time of features are presented. Finally, the strategy to realize variabilities is posted.

In [[6](#Sven)], evolution is defined as the process of dealing with change in the system development lifecycle. After a series changes, Let’s Encrypt is desired to be more flexible for users. In the second section, the evolution history of  Let’s Encrypt is given and related issues are analyzed.

###2.5.1 Variable Features

According to [[6](#Sven)], a feature is a characteristic or end-user-visible behavior of a software system. Features are used in product-line engineering to specify and communicate commonalities and differences of the products between stakeholders, and to guide structure, reuse, and variation across all phases of the software life cycle. In this section,  a series of features for Let’s Encrypt are identified and commented. In addition, they are classified into four different parts.

####2.5.1.1 Feature List

##### **Environment**

The first part concerns about the environment that the software runs in. 

- OS support: Let’s encrypt supports multiple Unix-ish Operating Systems including Arch, Debian, FreeBSD, etc. Based on user’s system, Let’s Encrypt will install automatically, or user can manually choose his OS.

- Python-version support: The Let’s Encrypt Client presently only runs on Unix-ish OSes that include Python 2.6 or 2.7; Python 3.x support will be added after the Public Beta launch.

##### **Client(GUI)**

- UI/Command switch: Let’s Encrypt supports ncurses and text (-t) UI, or can be driven entirely from the command line. Users can choose according to their requirements.

- Notification of configuration change: As a user, you can switch on/off notification of change, for example, if you change the storage path of some files, there should be some notification for the change by default, but you can choose to close this function. And a result of your certificate generation will also be shown on window.

- User authority: There are two levels for authority, ’user’ and ’super-user’ (developer), in ’user’ mode you are not allowed to use some instructions and some information are hidden. In ’super-user’ level, all functions are open.

- Virtual Environment: To implement some functions, Let’s Encrypt allow users to use virtualenv (virtual environment package), while you can choose not to use it.

- Usage (As plugin or client): With no doubt, Let’s encrypt can serve as an independent application. Additionally, it can also serve as a third party module in other applications which means that users use it as a plugin.

- Input method: User can choose to input arguments for functions from keyboard or from a file.

##### **Plugin**

- Web server support: Let’s Encrypt client supports a number of different “plugins” that can be used to obtain and/or install certificates. Plugins that can obtain a specified certificate are called “authenticators” and can be used with the “certonly” command. Plugins that can install a cert are called “installers”. Apache, Nginx and webroot are plugins currently supported.

##### **Configuration**

- Logging-level: The application is setup using serilog for logging. There are many logger levels like debug and customer. In customer level, some detail information are hidden to prevent ’noise’, but in debug level you can see more information.

- Allow specifying Encryption algorithm: ACME protocol enables multiple kinds of encryption algorithm like RSA and MD5. As an implementation of ACME, Let’s encrypt enable those algorithms as well.

- Editable Certificate and key storage path: After installing, the path to store your certificate has been specified by default setting, but users are allow to edit it and assign another path.

- Editable Log file storage path: After installing, the path of the log documents has been specified by default setting, but users are allow to edit it and assign it another path.

- Personal profile: The most important information of your profile is your email address and users have to offer an effective profile when apply for a certificate. And also they are allow to edit it.

- Multiple Challenge solution: There are many ways to solve challenges (so that you can prove you own a certain domain name):Simple HTTP/DVSNI/ private key/DNS verification. By default Let’s encrypt will choose one of them by preset algorithms but you can also specify it manually.

- Optional update way (Manual or automatic): Once there is a newer version of let’s encrypt released, the Let’s Encrypt in user’s machine will update itself automatically, but you can also choose to disable automatically update and do it manually

- Optional ACME compliant services: As an implementation of ACME, Let’s Encrypt supports not only Let’s Encrypt CA but also other ACME compliant services. User can set up their choice in command.

- Optional redirect: Optionally install a http -> https redirect. Once user finish SSL certificate, browsers  can use https instead of http. User can  choose use only or a hybrid strategy (http and https are both valid)  by changing the configuration.

- Verification: Let’s Encrypt currently supports two kinds of verification, Domain-Validated (DV) and Organization-Validated (OV) Certificate. Users can choose according to their requirements.

- Revocation: It is common that when user lose the ownership of the server he needs to revoke the certificate issued before. And this is possible to complete this task in Let’s Encrypt.

- Adjustable key bit-length: Because users have different requirement for key bit-length, it is possible to change it in Let’s Encrypt while 2048 is default value.

- Valid period: Considering about the safety of server and users who log in their web, the certificate is not permanently valid and owners must update it when expiring. In Let’s Encrypt, a certificate is valid for 90 days.

- Email notification: For good user experience, Let’s Encrypt records the registration date of certificate and kindly remind user to update their certificates by email when 30 days rest for validity.


####2.5.1.2 Stakeholders Related
There are two stakeholders affected by listed features:

Users: Users are the stakeholders affected most, because each creation and change of features are done for better user experience. 

Developers: Developers are also related to these features. By accept suggestions and issues posted by others (users or developers), developers try to make Let’s Encrypt more flexible and robust.

####2.5.1.3 Feature Dependencies and Model

Mainly all features can be classified into four categories, environment, client, plugin and configuration. Among them, there are dependencies, which means that some features depend on others. For example, users have to choose “SuperUser” authority in order to enter debug mode, choose challenge solution and manual update method. 

In addition, there are several constraints between plugin and configuration features. Configuration feature relies on the specified server choice that are Nginx and Apache. Furthermore, currently users can install a “http -> https” redirect, so their site effectively runs https only, however, this relies on the plugin of Apache server.

Finally, it’s convenient for users to get a kind email notification about the expiring date of certificate, which is surely related to the valid period of cert.

![Feature Dependencies and Model](https://github.com/delftswa2016/team-letsencrypt/blob/master/D3/Feature.Model.png)

 
####2.5.1.4 Feature Binding Time

Features in Let's Encrypt are with different binding times, most features at run time, others at compile time[[7](#Lee)].

##### **Environment**

- OS support:
**run time binding**, determined after deployed to a machine.

- Python-version support: 
**run time binding**, determined after deployed to a machine and Let’s encrypt will automatically detect what versions of python are running in the machine.

##### **Client (GUI)**

- UI/Command switch: 
**compile time binding**, unable to be disabled after deployed to a machine.

- Notification of configuration change: 
**run time binding**, can be disabled or enabled by user.

- User authority:
**compile time binding**, with two authority level: user and super user.

- Virtual Environment: 
**run time binding**, can be enabled or disabled by user after deployed to a machine.

- Multiple Usage: 
**compile time binding**, unable to be specified after deployed to user's computer.

- Input method:
**run time binding**, can be specified by users using command line.

##### **Plugin**:
- Web server support: 
**compile time binding**, always support web servers like apache, nginx.

##### **Configuration**:
- Multiple Logging-level:
**run time binding**, log level can be switched by command line.

- Allow specifying Encryption algorithm: 
**run time binding**, can be disabled after deployed to a machine.

- Editable Certificate and key storage path:
**run time binding**, storage path can be changed after deployment and this feature can even be disabled by user.

- Editable Log file storage path:
**run time binding**, storage path can be changed after deployment and this feature can even be disabled by user.

- Personal profile:
**run time binding**, users can edit their profile after installation.

- Multiple Challenge solution: 
**run time binding**, users can manually select a way to solve the challenge.

- Optional update way: 
**run time binding**, can be determined by users after installation.

- Optional ACME compliant services:
**run time binding**, users can select other CAs (not only Let’s encrypt CA) to trust.

- Optional redirect: 
**run time binding**, can be switched between http/https at anytime after getting SSL certificates.

- Verification method: 
**run time binding**, users can choose to do Domain verification (DV) or Organization verification (OV).

- Revocation: 
**run time binding**, users can do revocation at any time.

- Adjustable key bit-length: 
**run time binding**, the length can be specified after deployed to user's machine

- Valid period: 
**compile time binding**, the valid period can not be determined by users.

- Email notification: 
**compile time binding**, users are not allow to disable this feature.

###2.5.2 Implementation Strategy

####2.5.2.1 Interface Design

The key to implement variability and configurability is using interface. All the interfaces need to be implemented are stored in the file `interface.py`. In the [developer guide](https://letsencrypt.readthedocs.org/en/latest/contributing.html) announce that what interface should be implemented by a specific kind of class. The interface.py and the developer guide together draw a outline for the let’s encrypt: what configuration can be done, what parameter can be specified, what Operation System it should support, what plugin it should support. Hence the variability and configurability has been implemented.

####2.5.2.2 Configuration File

It is possible to specify configuration file with letsencrypt-auto --config cli.ini (or shorter -c cli.ini). In the configuration file, users could change the length of RSA key, registered e-mail address, domains need certification, text interface or ncurses, authenticator, etc. An example configuration file is shown below:

```
# This is an example of the kind of things you can do in a configuration file.
# All flags used by the client can be configured here. Run Let's Encrypt with
# "--help" to learn more about the available options.

# Use a 4096 bit RSA key instead of 2048
rsa-key-size = 4096

# Uncomment and update to register with the specified e-mail address
# email = foo@example.com

# Uncomment and update to generate certificates for the specified
# domains.
# domains = example.com, www.example.com

# Uncomment to use a text interface instead of ncurses
# text = True

# Uncomment to use the standalone authenticator on port 443
# authenticator = standalone
# standalone-supported-challenges = tls-sni-01

# Uncomment to use the webroot authenticator. Replace webroot-path with the
# path to the public_html / webroot folder being served by your web server.
# authenticator = webroot
# webroot-path = /usr/share/nginx/html
```

By default, the following locations are searched:

- `/etc/letsencrypt/cli.ini`

- `$XDG_CONFIG_HOME/letsencrypt/cli.ini` (or `~/.config/letsencrypt/cli.ini` if `$XDG_CONFIG_HOME is not set`).

###2.5.3 Evolution History of Variability and Configurability

The Let’s Encrypt had already included many features in the first released version. More features are added in the following releases. The contributions focus mainly on the support for Operation Systems, different versions of python, improvements of configuration file and command line flags, support for new plugins, and implementations of more ACME challenge solutions. 

Release note is a good method to figure out the evolution history. To see the changes of the given releases in Let’s Encrypt , we inspect the github milestone for the following releases:

- [0.1.1](https://github.com/letsencrypt/letsencrypt/issues?q=milestone%3A0.1.1)- This version includes important bugfixes over the Public Beta / 0.1.0 version:
1) fixed a confusing UI path that caused some users to repeatedly renew their certs; 2) completed numerous Apache configuration parser fixes; 3) fixed `--webroot` permission handling for non-root users.

- [0.2.0](https://github.com/letsencrypt/letsencrypt/issues?q=is%3Aissue+milestone%3A0.2.0)- This version added Apache plugin support for non-Debian based systems and PyOpenSSL support for versions 0.13 or 0.14. In addition, it supports HTTP to HTTPS
redirect on some systems.

- [0.3.0](https://github.com/letsencrypt/letsencrypt/issues?q=is%3Aissue+milestone%3A0.3.0)- This version enabled a non-interactive mode by including `-n` or `--non-interactive` on the command line to guarantee the client will not prompt when running automatically.

- [0.4.0](https://github.com/letsencrypt/letsencrypt/issues?q=milestone%3A0.4.0)- In this version, two major changes were made to support the variability and configurability: 1) a new subcommand renew can be used to renew the existing certificates as they approach expiration; 2)  full support for Python 2.6 is enabled.

- [0.4.2](https://github.com/letsencrypt/letsencrypt/issues?q=is%3Aissue+milestone%3A0.4.2)- A patch fixing problems of using letsencrypt `renew` with configuration files from private beta has been added.

Issues and pull requests relevant to the variability and configurability can be classified as follows:

- **Improvement of Operation Systems support**: the first release version only supported Ubuntu with version before 14.04. The supports for Debian, Mac, CentOS and later version Ubuntu are enabled in the following versions. The relevant issues include [issue 292](https://github.com/letsencrypt/letsencrypt/pull/292), [508](https://github.com/letsencrypt/letsencrypt/pull/508), [840](https://github.com/letsencrypt/letsencrypt/pull/840), [1206](https://github.com/letsencrypt/letsencrypt/pull/1206), [1232](https://github.com/letsencrypt/letsencrypt/pull/1232). 

- **Support for different version of Python**: at the beginning, Let’s encrypt only supported python 2.6. The support for later versions of python, especially python 3.x, is enabled now. The relevant issues include [issue 605](https://github.com/letsencrypt/letsencrypt/pull/605), [957](https://github.com/letsencrypt/letsencrypt/pull/957), [1508](https://github.com/letsencrypt/letsencrypt/pull/1508).

- **Improvement of configuration**: numerous configuration options are added and the command lines becoms increasingly user-friendly. For example, users can now specify the parameters such as the length of the RSA key. The relevant issues include [issue 82](https://github.com/letsencrypt/letsencrypt/pull/82), [368](https://github.com/letsencrypt/letsencrypt/pull/368), [404](https://github.com/letsencrypt/letsencrypt/pull/404).

- **Support for different webserver plugins**: while the early version of Let’s encrypt only supported apache and nginx, now the supports for nginx, webroots, standalone are enabled. The relevant issues are [issue 232](https://github.com/letsencrypt/letsencrypt/pull/232), [387](https://github.com/letsencrypt/letsencrypt/pull/387), [895](https://github.com/letsencrypt/letsencrypt/pull/895), [1395](https://github.com/letsencrypt/letsencrypt/pull/1395).

- **Implementations of more ACME challenge solutions**: ACME protocols allows different kinds of challenge solution, but Let’s encrypt only implemented a small number of them. However, in the recent releases more challenge solutions are added by contributors. The relevant issues are [issue 232](https://github.com/letsencrypt/letsencrypt/pull/232), [291](https://github.com/letsencrypt/letsencrypt/pull/291), [387](https://github.com/letsencrypt/letsencrypt/pull/387).

##2.6 Evolution Perspective

As the business maxim tells us “the only constant  is change”[[1](#Nick)], a major concern for architects is how to build a flexible system to adapt to inevitable changes. 
As a result, there is constant pressure to change the system’s behavior, which in many cases requires architectural tactics to ease such process. The term evolution is used as the process of dealing with changes encountered during the development lifecycle.

###2.6.1 Requirements Capture

To identify the evolution needs, the system requirements need to be re-analyzed to find out which of them will likely need to change over time.

####2.6.1.1 Identify Requirements

Based on the roadmap inferred from the existing document and discussion on forum and github, the key evolution requirements in the future can be summerized as follows:


|  Dimensions of Change | Magnitude of change required| Likelihood of change  |Timescale of change required|
|:-------------:|:-------------:|:-------------:|:-------------:|
| Less rate limits on the number of certificates issued per domain(**Growth**). | large-scale, high-risk|80%|approximately 7~10 months |
| Full support for web server like Nginx(**Functional**).     |  medium-scale, low-risk      |100% |approximately 3~4 months|
|Full IPv6 Support(**Functional**).| medium-scale, low-risk      |100% |approximately 2 months|
|Certificate Compatibility with Windows XP(**Platform**).|small-scale(defect correction), low-risk|100%|approximately 2~3 months|
|Elliptic Curve Cryptography (ECC) Support(**Functional**).|medium-scale, high-risk|60%|approximately 8~10 months |

####2.6.1.2 Evolution Tradeoff

The tradeoff depends on the type of the system, the likehood of the change and the confidence to defer the change until it is required. It means the prioritized evolution requirements will take flexibility into account at the beginning while the others defer the efforts. 

The architects of Letsencypt considers support for IPv6 and Nginx at initial development since they are the necessary features the project wants to include. By creating extensible interfaces for IPv6 and Nginx, the system becomes flexible for later change.

The issues of lessening rate limits on the number of certificates issued per domain and certificate compatibility with Windows XP are also considered even not with the highest priority. The reason is that the compatibility problem is a small-scale issue in our case while the rate limit demands more on the hardware support than software architecture. 

ECC support is deferred until it is required for a reason that this feature is not needed recently and possibly will not happen in the future. Thus, it is not worth investing efforts in it at the very beginning.  

###2.6.2 Architectural Tactics

With the development of Let’s Encypt, various architecture tactics are used to make it more flexible to accomodate changes.

####2.6.2.1 Create extensible interfaces

Let’s Encrypt supports different servers by adding extensible plugins, which means developers can add their own plugin according to their requirements.

####2.6.2.2 Build variation points into the software

To  make Let’s Encrypt more flexible, developers design variation points into software. 
For example, Let’s Encrypt supports a series of configurable parameters like adjustable key bit-length and optional ACME compliant services. 
This allow some aspects of the system’s operation to be changed over time without modifying its implementation[[1](#Nick)].

####2.6.2.3 Achieve reliable change	

To achieve reliable change, Let’s encrypt takes the following strategies:

- Create mechanisms to roll back unsuccessful deployments: 
The source code is on Github, which is a perfect tool for version control. 
Developers are able to records changes to a file or set of files over time so that they can recall specific versions later.

- Automated testing: 
Let’s Encrypt uses `tox` for testing. 
`tox` starts a full set of tests, it includes apacheconftest, unit tests for specific Python versions, code coverage, style of the whole project, etc. 

- Continuous integration: 
When making changes, it is always best to receive bad news as early as possible. 
Travis CI is used to do automated testing for every pull request to Let’s Encrypt project before merging. 
They set up project in multiple Python versions(2.6,2.7,3.3,3.4,3.5) and multiple plugins. [(links to `.travis.yml` for letsencrypt)](https://github.com/letsencrypt/letsencrypt/blob/master/.travis.yml)

##References

1. <div id="Nick"/>Nick Rozanski and Eoin Woods. Software Systems Architecture: Working with Stakeholders using Viewpoints and Perspectives. Addison-Wesley, 2012.

2. <div id="Fowler">Fowler Martin. TechnicalDebtQuadrant. http://martinfowler.com/bliki/TechnicalDebt.html. 2014.

3. <div id="Cunningham">Cunningham Ward. Ward Explains Debt Metaphor. http://c2.com/cgi/wiki?WardExplainsDebtMetaphor. 2011.

4. <div id="lfam">lfam. Packaging. https://github.com/letsencrypt/letsencrypt/wiki/Packaging. 2016.

5. <div id="Let">Let's Encrypt Project. Let’s Encrypt client documentation! https://letsencrypt.readthedocs.org/en/latest/index.html.

6. <div id="Sven">Sven Apel, Don Batory, Christian Kästner, Gunter Saake.Feature-Oriented Software Product Lines. 2013.

7. <div id="Lee">Lee, Jihyun, & Hwang Sunmyung. A review on variability mechanisms for product lines. International Journal of Advanced Media and Communication, 5(2-3), 172-181. 2014


