Our Chapter -- First Version
----------------------------

#Background Knowledge

##SSL/TLS
Transport Layer Security Protocol (TLS) and its prodecessor -- Secure Socket Layer Protocol (SSL) are both referred to as “SSL”, these two protocols aim to provide encryption for communication between two parties, but in SSL/TLS communications, the server side need to provide a Digital Certificate (for convenience may be referred to SSL certificate in the rest of this Chapter) to verify its identity (to prove that the server really own a set of domain names like www.google.com, www.paypal.com etc).

##ACME protocol
ACME refers to Automatic Certificate Management Environment.
The internet entities (client, servers) have needs to verify identity, but today, the verification are done by some ad-hoc mechanisms which is not suitable for further development of online verification. ACME protocol aims to address such issue. It is a protocol to standarlize and automate the procedure of verification. In other words, it is a bag of procedures by doing which a client can get a digital certificate and a Certificate Authority(CA, we discuss it later) can issue a certificate.

##Certificate authority
A Certificate Authority is an online organization which can issue a digital certificate for you.  To cooperate with let’s encrypt (we will discuss it later), a certificate should install a software called boulder.  

##Let’s encrypt
Let’s encrypt is a software implement ACME protocol. It works in this way: when you request a certificate to prove possession of a domain name, let’s encrypt wrap necessary information (your account, the domain name etc) into standard format and send them to boulder of a CA. CA will respond with a challenge, saying that “Since you say you own the domain name xxxxx.com, please add a .txt file containing the string blablabla in the folder aaa/bbb/ccc”. Let’s encrypt will automatically solve this challenge by following the requirement of CA. After solving the challenge, the CA gives you a request.

#Chapter Structure
In previous Section, we introduce some background knowledge to you and offer a brief on what is let’s encrypt. In this section, we will give you a more comprehensive insight of let’s encrypt. The issues covered in this Section includes: what organization and individuals are involve in let’s encrypt, the inner architecture of let’s encrypt, the deployment of letsencrypt, the variability of let’s encrypt. The evolution of let’s encrypt.
We will address those issue by Context View, Development View, Deployment View, Variability Perspective, Evolution Perspective.

## Stakeholders and Context View
Before talking about Context View, we plan to talk something about stakeholder, that will help you better understand context view.
###Stakeholders
We identify 5 major kinds of  and an additional kind of stakeholders for letsencrypt:
- **Acquirers**:
Acquirers are the ones who launch and organize the developing of let’s encrypt.  Estimate the needs of users and set goals of developers.
For let’s encrypt, there are 3 acquirers:
- [Electonic Frontier Foundation](https://www.eff.org/)
- [Mozilla Foundation](https://www.mozilla.org/en-US/)
- [University of Michigan](https://en.wikipedia.org/wiki/University_of_Michigan)
The Electonic Frontier Foundation is the most important acquirers who provides most of labor to let’s encrypt.
Mozilla Foundation and University Michigan also have personnel devoting into this project.
**Developers:**
Developers are the one who devote themselves directly to the project. In let’s encrypt, the developers are mainly programmers who do coding jobs.
There are 165 contributors for let’s encrypt, we only list the most important contributors below, they are the major developers of let’s encrypt.
- [Liam Marshall](https://github.com/ArchimedesPi)
- [bmw](https://github.com/bmw)
- [Felix Rieseberg](https://github.com/felixrieseberg)
- [Francois Marier](https://github.com/fmarier)
- [Harlan Lieberman-Berg](https://github.com/hlieberman)
- [Joona Hoikkala](https://github.com/joohoi)
- [Jacob Hoffman-Andrews](https://github.com/jsha)
- [Jakub Warmuz](https://github.com/kuba)
- [Martin Thomson](https://github.com/martinthomson)
- [Peter Eckersley](https://github.com/pde)
- [Roland Bracewell Shoemaker](https://github.com/rolandshoemaker)
- [Sagi Kedmi](https://github.com/sagi)

This developer team includes the people who technically contribute to the Let's Encrypt project. Because the project is built a short time before (Dec 3, 2015) and still in progress, developers may also do test and maintaining jobs. For example, ´bmw´ who develops the client also responds to issues or pull requests from others. Without doubt, the team is the core of whole project.

**Users:**
Users are those who use the products. For let’s encrypt, the users are all web servers who use let’s encrypt to request digital certificate for SSL/TLS communication. Now, the users are not too many, but since the let’s encrypt is free and reliable, the number of the users is growing very fast.

**Sponsors:**
Sponsors are those who provides financial support for a project. For let’s encrypt, the sponsors are those who provides financial supports for programmers. The major sponsors are listed below:
- [Mozilla](https://www.mozilla.org/)
- [Akamai](http://www.akamai.com/)
- [Cisco](http://www.cisco.com/)
- [Electronic Frontier Foundation](https://www.eff.org/)
- [OVH](https://www.ovh.com/)
- [Chrome](https://www.google.com/chrome/)
Mozilla and Chrome are famous web browsers. Cisco is most important router producers, Electronic Frontier Foundation is the organization which launch let’s encrypt developing. OVH is an Internet Service Provider providing dedicated servers, shared and cloud hosting, domain registration, and VOIP telephony services. Akamai is famous Content Delivery Network providers.

**Additional Stakeholder**
- [IdenTrust](https://www.identrust.com/) (Root certificate provider)
We need to explain the concept of Root Certificate Providers. A certificate  provider is a root certificate provider if and only if: 1.it can authorize other organizations to be new certificate providers; 2. It doesn’t need any other organization to authorize it to be certificate providers, in the other words, it is born to be certificate providers.
For example, a Certificate providers A can authorize organization B to be a new certificate provider. Later, B can authorize C to be a new certificate provider in this case, A is the so called certificate Providers.
IdenTrust is a Root Certificate Provider who authorize let’s encrypt.

The graph for major stakeholders are below:

![stakeholder](https://github.com/delftswa2016/team-letsencrypt/blob/master/D1/stakeholder.png)


##Context View
Now we are fully prepared to discuss Context View. This section includes： System Scope;  entities and interfaces; impact to enviroment

###System scope and Responsibilities:
The responsibility of Let's Encrypt includes a series functions provided by the system for users.
•	users can register certificate for their server
•	users can revoke certificate
•	users can create their own account
•	
•	users can modify their profile
•	users can turn on/off the notice of expiring date of certificate
•	users can choose which ACME solution to use
Let's Encrypt is a software concerning network security. Its functions are surely about that. However, what is different is that, it also cares about user experience, the full automation and easy use both highlight this.





###Entities, data and interfaces
**Entities and Interfaces**
There're ten entities for Let's Encrypt. First, the development of Let's Encrypt is based on GitHub platform. Second, Python is identified as the only language dependency. Third, the community, Freenode, is refered as the platform that developers communicate on. Fourth, as an authority, Let's Encrypt must be qualified with root certificate, which is provided by IdenTrust. Fifth, the release is based on the Python Package Index (PYPI). Sixth, Let's Encrypt extends its support of different servers by adding plugins including Apache, Nginx and Webroot. Seventh, the test part is extended by test tools, Tox and Travis CI online test system. Eighth, the operating system is another entity including a series Unix-ish Operating Systems like Arch, Debian, FreeBSD, etc. Ninth, the users are a large number of websites including blueboard.cz, checkdcmain. Finally, competitors are other softwares based on ACME.
**Impact of the System on Environment**
The impact of system on environment concerns about the dependencies of other system on Let's Encrypt and other systems decommissions and data migration. Let's Encrypt can be used as a plugin embedded in other certificate system, thus the performance of such system will depend on Let's Encrypt. Since Let's Encrypt is still deveoping and incomplete, there is no system decommissions because of it. However, because it is free and automatic, it does form a big threat for its competitors. Finally, Let's Encrypt is software independently developed by ISRG. Their code and data including its protocols, plugins and client are all original.

![context view](https://github.com/delftswa2016/team-letsencrypt/blob/master/D1/contextview.png)



#Development View

The development view describes the architecture that supports the software development process. The development view communicates the aspects of the architecture of interest to stakeholders from the building, testing, maintaining and enhancing the project(Nick, 2012). Based on this, the following article shows the common design model, module structure model and code line model of Let's Encrypt which give a technical overview of whole project. To learn more details, each model is attached with complete descrption. In addition, technical debt of the project and coresponding solution or plan are described at the end of report.

#Common Design Model

##Common Processing

- **Instruction parser**: 
The parser analyzes the received instructions with a preset workflow. Parsing instrcutions is considered to be a common process because 1) many modules rely on the user's instructions to determine the subsequent actions. 2) the parsing functions in different modules are similar to each other. Hence, the parsers should be put into a separate module. In fact, all codes relevant to the parser functions reside in the file *letsencrypt-auto* and *cli.py*.

- **ACME Objects**: 
To implement ACME Protocol, letsencyrpt contains ACME Objects such as ACME account and ACME Exceptions. Those Objects are used almost everywhere in letsencrypt. When the user wants to request an account, for example, an account Object is created and returned. Scuh account will later be used when a certificate needs to be requested or renewed. Therefore, all ACME Objects are wrapped into a module so that they can easily be called and maintained.

- **Configuration**:
Configuration Object is "a bag of variables" used by almost all the other modules. For instance, the user might want to register his/her information like "I have a domain name abc.com; my account is xxxxx; my email is xxxx@gmail.com; my private key stores in xxxxx; please give me a certificate for that domain name". By passing a configuration Object containing these attributes, it enables letsencrypt to automatically obtain, renew and revoke a certificate. To facilitate the modules to pass parameters using the configuration objects, these objects (as well as the relevant functions) are wrapped into a module.

##Standard Design Approaches
The most important component of letsencrypt is its plugin. There are 4 major types of plugins: User Interface (UI), Configurator, Authenticator and Installer. The project uses interfaces to standardlize the contributions from developers.
The interfaces available for implementation of these plugins are defined in [interfaces.py](https://github.com/letsencrypt/letsencrypt/blob/master/letsencrypt/interfaces.py) and [plugins/common.py](https://github.com/letsencrypt/letsencrypt/blob/master/letsencrypt/plugins/common.py#L34). 

 - **Configurator**:  This component is to automatically change the configurations. For example, if the user wants to                       change the folder to store certificates, it can be achieved by changing the configuration with the help of a corresponding                              configurator.  Considering that different configurators are for different purposes, letencrypt allows                           the developers to write their own configurator, which has to implement the corresponding interface.

 - **IDisplay**:  This plugin implements bindings to alternative UI libraries. Letsencrypt allows contributors to write their own UI, which has to implement IDisplay.

 - **Authenticators**: This component is to prove that the client deserves a certificate for some domain name by solving challenges received from ACME server.                         For different types of challenges (e.g. simple HTTP challenge, DVSNI challenge), different                                   Authenticators are needed. Letsencrypt allows developers to construct new authenticator,  which has to                 implement Authenticators Interface.

 - **Installer**: It setups the certificate in a server and possibly tweaks the security configuration to make it reliable and secure. For different web servers in user's machine, different Installers are needed. Developers are allowed to construct their own                          Installer, which has to implement Installer Interface.
 
 
##Standardlization of Testing
There are two types of tests: Unit Test and Intergration Test.
 - **Unit Test**: 
 Unit Test in letsencrypt aims to check whether the code style is compatiable with the required style. There are two files related to Unit Test: *pep8* and *tox.ini*. The Test can run off-line by developers.

- **Integration Test**: 
Integration Test is to test whether letsencrypt works well with boulder (a software runs in Certificate Authority which generate SSL/TLS). It is an online test running automatically to test a pull request. The module corresponding to Intergration Test is Travis CI.


#Module Structure Model

![ModuleStructure](https://github.com/delftswa2016/team-letsencrypt/blob/master/D2/module.structure.png)

The UML component diagram below gives an overview of module structure. Each package means a code module and arrow shows intermodule dependencies(Nick, 2012).

The first layer which is also the closest layer to users offers friendly UI for common clients to set up their own certificates. And the command way is also possible for developers to contribute to the project. 

Receiving commands from upper layer, the coordinator module tries to check the compatibility to ensure the job can be done in certain environment. After that, the commands are parsed by parser and functions offered in next layer are recalled.

As the core layer of whole project, the third layer offers important functions to complete tasks. ACME protocol is a protocol that a certificate authority (CA) and an applicant can use to automate the process of verification and certificate issuance. The package contains a series of codes to complete this task. Challenge module represents automatic module to finish the challenge provided by CA. JSON Web Key (JWK) module offers a cryptographic key in this data structure.  Besides, Let’s Encrypt has a plugin architecture to facilitate support for different webservers, other TLS servers, and operating systems. Currently, general webservers like Apache and Nginx are supported.

There’s no doubt that all previous code is based on python library which is included the bottom layer. In addition, OpenSSL, a software library to be used to secure communications against eavesdropping or to ascertain the identity of the party at the other end, is also the base of authentication process .

#Codeline Model

In this section, code structure of letsencrypt will be explored. Build, Integration, Test and Release Approach also matters a lot in understanding the project organization. In addition, Technical Debt is also analyzed.

##Source Code Structure

![SourceCodeStructure](https://github.com/delftswa2016/team-letsencrypt/blob/master/D2/code.structure.png)

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

##Coding Style

The developers of Let's Encrypt follow the **Google Python Style Guide**, And use **Sphinx-style** for documentation. 'pylint' is used to check coding style.
```
def foo(arg):
    """Short description.

    :param int arg: Some number.

    :returns: Argument
    :rtype: int

    """
    return arg
```
##Build, Integration and Test Approach

To provide developers a convenient environment and also protect project source version, Let's Encrypt has an offical workflow for building, integrating and testing. 

###Build

Let's Encrypt supports multiple operating systems. Users can install client according to their requirements. However, developers should have their own local copies based on their platform and run the client in developer mode from local tree for technical test.  

###Integration

To contribute to Let's Encrypt project, developers have to follow the strict integration process made by project team.

- Developers can firstly find open issues in the github issue tracker or post new issues for their own idea.

- Then developers start to work on the problem, post a comment to let others know and seek feedback on the plan where appropriate.

- Once the developer gets a working branch, he/she can open a pull request. All changes in pull request must pass complete test. Generally it is sufficient to open a pull request and let Github and Travis run integration tests for you. However, if developers prefer to run tests, they can use Vagrant, using the Vagrantfile in Let’s Encrypt’s repository. 

- Let's Encrypt integrator will review the pull request and make a decision to merge or not.

###Test

Before merging the changes in pull requests, all of them must have thorough unit test coverage, pass Let's Encrypt's integration tests, and be compliant with the coding style. This is done by popular online testing platform Travis CI. 
And developers can also do their own test before posting a request. Tox is recommended as official tools for running a full set of test like config file parsing test. For debugging, ipdb is introduced to execlude a series of faults.

##Release Process

Let's Encrypt release packages and upload them to PyPI (wheels and source tarballs).

- https://pypi.python.org/pypi/acme

- https://pypi.python.org/pypi/letsencrypt

- https://pypi.python.org/pypi/letsencrypt-apache

- https://pypi.python.org/pypi/letsencrypt-nginx

- https://pypi.python.org/pypi/letshelp-letsencrypt

The following scripts are used in the process:

- https://github.com/letsencrypt/letsencrypt/blob/master/tools/dev-release.sh

- https://gist.github.com/kuba/b9a3a2ca3bd35b8368ef

Let's Encrypt currently version as 0.0.0.devYYYYMMDD, and will change at GA time to the following scheme:

- 0.1.0

- 0.2.0dev for developement in master

- 0.2.0 (only temporarily in master)

- ...

Tracking issue for non-dev release scripts: https://github.com/letsencrypt/letsencrypt/issues/1185

##Configuration Management

To ensure repeatability and technical integrity, Let's encrypt define a configuration file. It is possible to specify configuration file with `letsencrypt-auto --config cli.ini` (or shorter `-c cli.ini`).
By default, the following locations are searched:

- `/etc/letsencrypt/cli.ini`

- `$XDG_CONFIG_HOME/letsencrypt/cli.ini` (or `~/.config/letsencrypt/cli.ini` if `$XDG_CONFIG_HOME` is not set).

All flags used by the client can be configured, including RSA key size, registed e-mail address, specified domains which generate certificates for, text interface or ncurses, standalone authenticator, webroot authenticator.

#Technical Debt

The concept of technical debt refers to the accumulated consequences of the quick but dirty design into an evolving software program (Fowler, 2014). In other words, the danger occurs when people rush software by simply adding features into the program but never reflect their understanding of those features. As the “debt” accumulates, the complexity of maintaining the programs to reduce its deterioration to the whole software increases (Cunningham, 2011).

##Code Duplication

In issue [#383](https://github.com/letsencrypt/letsencrypt/issues/383), code duplication exsits between apache and nginx plugins; and in issue [#698](https://github.com/letsencrypt/letsencrypt/issues/383), [Dockerfile-dev](https://github.com/letsencrypt/letsencrypt/blob/26c1f003d0d05397154fe63e1f452ed2148cfe75/Dockerfile-dev) and [Dockerfile](https://github.com/letsencrypt/letsencrypt/blob/26c1f003d0d05397154fe63e1f452ed2148cfe75/Dockerfile) also duplicated.

Duplicated code is code that has been produced by copying and then adapting existing code. Also known as “copy-and-paste development”, this strategy of producing code is frequently employed as a way of reusing software. 
On the first impression, code duplication seems to be a desirable approach to development as it is associated with reuse, implementation speed-up, and developer care. However, in the long term, code duplication implications can be very negative.
First of all, code duplication causes an increase in software size. Secondly, duplication has a direct influence on the difficulty of maintaining already developed code. Additionally, code-duplication can also be a sign of poor design, indicating that generic functionality has been not properly abstracted. Consequently, on long term, especially during the maintenance phase, code duplication is to be avoided.


##Documentation

For open source projects, the contribution of open source communities behind the huge development of the project is very evident. So it is very important that a transparent documentation system is followed in such community projects. Without proper documentation it is very difficult for other developers to understand the code developed by any user and to reuse the code or make it more efficient.Unfortunately, for Let's Encrypt, they do not provide a well-structured documentation for developers. There are 59 issuses labeled `documentation`. No architecture and module information, which makes understanding of how the project works and how the module connect with each other difficult to understand, which makes it difficult for developers not in their team to contribute to the project. In addition, insufficient documentation may also cause troubles for users.





##How developers deal with technical debt

For technical debt, developers always find it via Issues, discuss it and try to find a better solution to fix it. For the technical debt and related issues we mentioned above [#2155](https://github.com/letsencrypt/letsencrypt/issues/2155), [#2498](https://github.com/letsencrypt/letsencrypt/issues/2498) and [#2114](https://github.com/letsencrypt/letsencrypt/issues/2114) they use this kind of method to manage their technical debt. 
- Code Duplication: Recent advances in static code analysis and hardware performance made possible tool-based localization of code duplication in industrial contexts. 
For developers, they could use this kind of tools to detect duplicates and then improved by sharing common util functions (as they did in [#382](https://github.com/letsencrypt/letsencrypt/pull/382)).

- Documentation: The developers are professionals who may not be aware of the not sufficient documentation, but they are willing to fix it when users make an issue on the github as what they did in issue [#2216] (https://github.com/letsencrypt/letsencrypt/issues/2216) and [#2271](https://github.com/letsencrypt/letsencrypt/issues/2271).

#Conclusions

As shown above, the let's encrypt project have good common design model, good standard design approaches. However, there are many drawbacks. First, let's encrypt has a crappy developer documents. As a new comer it is very hard to see the whole picture of the project, let alone contribute to it. Second, the client is not user friendly, and it is also hard to install (it is designed to be easy to install, but in fact it usually crash without any trace back information, that is some kinds of technical debts, the developers still have no idea to thoroughly solve it. 

#References

- Nick, R., Eoin, W. (2012), Software Systems Architecture

- Cairns, C., Allen, S. (2015), [Managing technical debt](https://18f.gsa.gov/2015/10/05/managing-technical-debt/).

- Fowler, M. (2014), [TechnicalDebtQuadrant](http://martinfowler.com/bliki/TechnicalDebt.html).

- Cunningham, W. (2011), [Ward Explains Debt Metaphor](http://c2.com/cgi/wiki?WardExplainsDebtMetaphor).

- lfam (2016), [Packaging](https://github.com/letsencrypt/letsencrypt/wiki/Packaging)

- Let's Encrypt Project (2015), [Let’s Encrypt client documentation!](https://letsencrypt.readthedocs.org/en/latest/index.html)





Variability Perspective
===========================
##Introduction

In (Sven, 2013), variability describes the ability to derive different products from a common set of artifacts. It is important for a good software to equip with variability to adapt to different environments, which largely satisfies the requirements of different stakeholders. Without doubt, Let’s Encrypt is such a software in developing. In the first section of report, a list of features and dependencies are given. A related model is built upon this. Additionally, the implementation and binding time of features are presented. Finally, the strategy to realize variabilities is posted.

In (Sven, 2013), evolution is defined as the process of dealing with change in the system development lifecycle. After a series changes, Let’s Encrypt is desired to be more flexible for users. In the second section, the evolution history of  Let’s Encrypt is given and related issues are analyzed.

##Variable Features
According to (Sven, 2013), a feature is a characteristic or end-user-visible behavior of a software system. Features are used in product-line engineering to specify and communicate commonalities and differences of the products between stakeholders, and to guide structure, reuse, and variation across all phases of the software life cycle. In this section,  a series of features for Let’s Encrypt are identified and commented. In addition, they are classified into four different parts.
###Feature List

####Environment

The first part concerns about the environment that the software runs in. 

- OS support: Let’s encrypt supports multiple Unix-ish Operating Systems including Arch, Debian, FreeBSD, etc. Based on user’s system, Let’s Encrypt will install automatically, or user can manually choose his OS.

- Python-version support: The Let’s Encrypt Client presently only runs on Unix-ish OSes that include Python 2.6 or 2.7; Python 3.x support will be added after the Public Beta launch.

####Client(GUI)

- UI/Command switch: Let’s Encrypt supports ncurses and text (-t) UI, or can be driven entirely from the command line. Users can choose according to their requirements.

- Notification of configuration change: As a user, you can switch on/off notification of change, for example, if you change the storage path of some files, there should be some notification for the change by default, but you can choose to close this function. And a result of your certificate generation will also be shown on window.

- User authority: There are two levels for authority, ’user’ and ’super-user’ (developer), in ’user’ mode you are not allowed to use some instructions and some information are hidden. In ’super-user’ level, all functions are open.

- Virtual Environment: To implement some functions, Let’s Encrypt allow users to use virtualenv (virtual environment package), while you can choose not to use it.

- Usage (As plugin or client): With no doubt, Let’s encrypt can serve as an independent application. Additionally, it can also serve as a third party module in other applications which means that users use it as a plugin.

- Input method: User can choose to input arguments for functions from keyboard or from a file.

####Plugin

- Web server support: Let’s Encrypt client supports a number of different “plugins” that can be used to obtain and/or install certificates. Plugins that can obtain a specified certificate are called “authenticators” and can be used with the “certonly” command. Plugins that can install a cert are called “installers”. Apache, Nginx and webroot are plugins currently supported.

####Configuration

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

###Stakeholders Related
There are two stakeholders affected by listed features:

Users: Users are the stakeholders affected most, because each creation and change of features are done for better user experience. 

Developers: Developers are also related to these features. By accept suggestions and issues posted by others (users or developers), developers try to make Let’s Encrypt more flexible and robust.

###Feature Dependencies and Model

Mainly all features can be classified into four categories, environment, client, plugin and configuration. Among them, there are dependencies, which means that some features depend on others. For example, users have to choose “SuperUser” authority in order to enter debug mode, choose challenge solution and manual update method. 

In addition, there are several constraints between plugin and configuration features. Configuration feature relies on the specified server choice that are Nginx and Apache. Furthermore, currently users can install a “http -> https” redirect, so their site effectively runs https only, however, this relies on the plugin of Apache server.

Finally, it’s convenient for users to get a kind email notification about the expiring date of certificate, which is surely related to the valid period of cert.

![Feature Dependencies and Model](https://github.com/delftswa2016/team-letsencrypt/blob/master/D3/Feature.Model.png)

###Feature Binding Time

Features in Let's Encrypt are with different binding times, most features at run time, others at compile time.

####Environment

- OS support:
**run time binding**, determined after deployed to a machine.

- Python-version support: 
**run time binding**, determined after deployed to a machine and Let’s encrypt will automatically detect what versions of python are running in the machine.

####Client (GUI)

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

####Plugin:
- Web server support: 
**compile time binding**, always support web servers like apache, nginx.

####Configuration:
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


##Implementation Strategy

###Interface Design

The key to implement variability and configurability is using interface. All the interfaces need to be implemented are stored in the file `interface.py`. In the [developer guide](https://letsencrypt.readthedocs.org/en/latest/contributing.html) announce that what interface should be implemented by a specific kind of class. The interface.py and the developer guide together draw a outline for the let’s encrypt: what configuration can be done, what parameter can be specified, what Operation System it should support, what plugin it should support. Hence the variability and configurability has been implemented.

###Configuration File

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


##Evolution History of Variability and Configurability

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

##Reference
- Slides lecture February 26th

- Sven Apel, Don Batory, Christian Kästner, Gunter Saake (2013). Feature-Oriented Software Product Lines. 

- Rozanski and Woods' _Evolution Perspective_ (chapter 28, variation points, p. 556 (digital book p. 523)). 

- Lee, J., & Hwang, S. (2014). A review on variability mechanisms for product lines. International Journal of Advanced Media and Communication, 5(2-3), 172-181.



