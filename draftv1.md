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

##2.3 Development View

The development view describes the architecture that supports the software development process.
The development view communicates the aspects of the architecture of interest to stakeholders from the building, testing, maintaining and enhancing the project(Nick, 2012).
Based on this, the following article shows the common design model, module structure model and code line model of Let's Encrypt which give a technical overview of whole project.
To learn more details, each model is attached with complete descrption. In addition, technical debt of the project and coresponding solution or plan are described at the end of report.

###2.3.1 Common Design Model

####2.3.1.1 Common Processing

- **Instruction parser**: 
The parser analyzes the received instructions with a preset workflow. Parsing instrcutions is considered to be a common process because 1) many modules rely on the user's instructions to determine the subsequent actions. 2) the parsing functions in different modules are similar to each other. Hence, the parsers should be put into a separate module. In fact, all codes relevant to the parser functions reside in the file *letsencrypt-auto* and *cli.py*.

- **ACME Objects**: 
To implement ACME Protocol, letsencyrpt contains ACME Objects such as ACME account and ACME Exceptions. Those Objects are used almost everywhere in letsencrypt. When the user wants to request an account, for example, an account Object is created and returned. Scuh account will later be used when a certificate needs to be requested or renewed. Therefore, all ACME Objects are wrapped into a module so that they can easily be called and maintained.

- **Configuration**:
Configuration Object is "a bag of variables" used by almost all the other modules. For instance, the user might want to register his/her information like "I have a domain name abc.com; my account is xxxxx; my email is xxxx@gmail.com; my private key stores in xxxxx; please give me a certificate for that domain name". By passing a configuration Object containing these attributes, it enables letsencrypt to automatically obtain, renew and revoke a certificate. To facilitate the modules to pass parameters using the configuration objects, these objects (as well as the relevant functions) are wrapped into a module.

####2.3.1.2 Standard Design Approaches

The most important component of letsencrypt is its plugin.
There are 4 major types of plugins: User Interface (UI), Configurator, Authenticator and Installer.
The project uses interfaces to standardlize the contributions from developers.
The interfaces available for implementation of these plugins are defined in [interfaces.py](https://github.com/letsencrypt/letsencrypt/blob/master/letsencrypt/interfaces.py) and [plugins/common.py](https://github.com/letsencrypt/letsencrypt/blob/master/letsencrypt/plugins/common.py#L34). 

- **Configurator**:  This component is to automatically change the configurations. For example, if the user wants to                       change the folder to store certificates, it can be achieved by changing the configuration with the help of a corresponding                              configurator.  Considering that different configurators are for different purposes, letencrypt allows                           the developers to write their own configurator, which has to implement the corresponding interface.

 - **IDisplay**:  This plugin implements bindings to alternative UI libraries. Letsencrypt allows contributors to write their own UI, which has to implement IDisplay.

 - **Authenticators**: This component is to prove that the client deserves a certificate for some domain name by solving challenges received from ACME server.                         For different types of challenges (e.g. simple HTTP challenge, DVSNI challenge), different                                   Authenticators are needed. Letsencrypt allows developers to construct new authenticator,  which has to                 implement Authenticators Interface.

 - **Installer**: It setups the certificate in a server and possibly tweaks the security configuration to make it reliable and secure. For different web servers in user's machine, different Installers are needed. Developers are allowed to construct their own                          Installer, which has to implement Installer Interface.
 
####2.3.1.3 Standardlization of Testing
 
There are two types of tests: Unit Test and Intergration Test.

 - **Unit Test**: 
 Unit Test in letsencrypt aims to check whether the code style is compatiable with the required style. There are two files related to Unit Test: *pep8* and *tox.ini*. The Test can run off-line by developers.

- **Integration Test**: 
Integration Test is to test whether letsencrypt works well with boulder (a software runs in Certificate Authority which generate SSL/TLS). It is an online test running automatically to test a pull request. The module corresponding to Intergration Test is Travis CI.

###2.3.2 Module Structure Model
![ModuleStructure](https://github.com/delftswa2016/team-letsencrypt/blob/master/D2/module.structure.png)

                                          Figure 2.1 Module Structure Model

The UML component diagram below gives an overview of module structure. Each package means a code module and arrow shows intermodule dependencies(Nick, 2012).

The first layer which is also the closest layer to users offers friendly UI for common clients to set up their own certificates. And the command way is also possible for developers to contribute to the project. 

Receiving commands from upper layer, the coordinator module tries to check the compatibility to ensure the job can be done in certain environment. After that, the commands are parsed by parser and functions offered in next layer are recalled.

As the core layer of whole project, the third layer offers important functions to complete tasks. ACME protocol is a protocol that a certificate authority (CA) and an applicant can use to automate the process of verification and certificate issuance. The package contains a series of codes to complete this task. Challenge module represents automatic module to finish the challenge provided by CA. JSON Web Key (JWK) module offers a cryptographic key in this data structure.  Besides, Let’s Encrypt has a plugin architecture to facilitate support for different webservers, other TLS servers, and operating systems. Currently, general webservers like Apache and Nginx are supported.

There’s no doubt that all previous code is based on python library which is included the bottom layer. In addition, OpenSSL, a software library to be used to secure communications against eavesdropping or to ascertain the identity of the party at the other end, is also the base of authentication process.

###2.3.3 Codeline Model

In this section, code structure of letsencrypt will be explored.
Build, Integration, Test and Release Approach also matters a lot in understanding the project organization.
In addition, Technical Debt is also analyzed.

####2.3.3.1 Source Code Structure

![SourceCodeStructure](https://github.com/delftswa2016/team-letsencrypt/blob/master/D2/code.structure.png)

                                          Figure 2.2 Source Code Structure

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

####2.3.3.2 Coding Style

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

####2.3.3.3 Build, Integration and Test Approach

To provide developers a convenient environment and also protect project source version, Let's Encrypt has an offical workflow for building, integrating and testing. 

#####2.3.3.3.1 Build

Let's Encrypt supports multiple operating systems. Users can install client according to their requirements. However, developers should have their own local copies based on their platform and run the client in developer mode from local tree for technical test. 

#####2.3.3.3.2 Integration

To contribute to Let's Encrypt project, developers have to follow the strict integration process made by project team.

- Developers can firstly find open issues in the github issue tracker or post new issues for their own idea.

- Then developers start to work on the problem, post a comment to let others know and seek feedback on the plan where appropriate.

- Once the developer gets a working branch, he/she can open a pull request. All changes in pull request must pass complete test. Generally it is sufficient to open a pull request and let Github and Travis run integration tests for you. However, if developers prefer to run tests, they can use Vagrant, using the Vagrantfile in Let’s Encrypt’s repository. 

- Let's Encrypt integrator will review the pull request and make a decision to merge or not.

#####2.3.3.3.3 Test

Before merging the changes in pull requests, all of them must have thorough unit test coverage, pass Let's Encrypt's integration tests, and be compliant with the coding style. This is done by popular online testing platform Travis CI. 
And developers can also do their own test before posting a request. Tox is recommended as official tools for running a full set of test like config file parsing test. For debugging, ipdb is introduced to execlude a series of faults.

####2.3.3.4 Release Process

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

####2.3.3.5 Configuration Management

To ensure repeatability and technical integrity, Let's encrypt define a configuration file. It is possible to specify configuration file with `letsencrypt-auto --config cli.ini` (or shorter `-c cli.ini`).
By default, the following locations are searched:

- `/etc/letsencrypt/cli.ini`

- `$XDG_CONFIG_HOME/letsencrypt/cli.ini` (or `~/.config/letsencrypt/cli.ini` if `$XDG_CONFIG_HOME` is not set).

All flags used by the client can be configured, including RSA key size, registed e-mail address, specified domains which generate certificates for, text interface or ncurses, standalone authenticator, webroot authenticator.

###2.3.4 Technical Debt

The concept of technical debt refers to the accumulated consequences of the quick but dirty design into an evolving software program (Fowler, 2014). In other words, the danger occurs when people rush software by simply adding features into the program but never reflect their understanding of those features. As the “debt” accumulates, the complexity of maintaining the programs to reduce its deterioration to the whole software increases (Cunningham, 2011).

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

