
#Introduction
The developer view describes the architecture that supports the software development process. For Let's Encrypt, the developer view communicate the aspects of the architecture of interest to stakeholders from the building, testing, maintaining and enhancing the project.
#Common Design Model

#Module Structure Model
![ModuleStructure](https://github.com/delftswa2016/team-letsencrypt/blob/master/D2/module.structure.png)
The UML component diagram below gives an overview of module structure. Each package means a code module and arrow shows intermodule dependencies[1].
The first layer which is also the closest layer to users offers friendly UI for common clients to set up their own certificates. And the command way is also possible for developers to contribute to the project. 
Receiving commands from upper layer, the coordinator module tries to check the compatibility to ensure the job can be done in certain environment. After that, the commands are parsed by parser and functions offered in next layer are recalled.
As the core layer of whole project, the third layer offers important functions to complete tasks. ACME protocol is a protocol that a certificate authority (CA) and an applicant can use to automate the process of verification and certificate issuance. The package contains a series of codes to complete this task. Challenge module represents automatic module to finish the challenge provided by CA. JSON Web Key (JWK) module offers a cryptographic key in this data structure.  Besides, Let’s Encrypt has a plugin architecture to facilitate support for different webservers, other TLS servers, and operating systems. Currently, general webservers like Apache and Nginx are supported.
There’s no doubt that all previous code is based on python library which is included the bottom layer. In addition, OpenSSL, a software library to be used to secure communications against eavesdropping or to ascertain the identity of the party at the other end, is also the base of authentication process .
#Codeline Model
In this section code structure of letsencrypt will be explored. Build, Integration, Test and Release Approach also matters a lot in understanding the project organization. In addition, Technical Debt is also analyzed.

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


##Build, Integration and Test Approach
Let's Encrypt has an offical workfolw for build, integrate and test for developers.

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

##Documentation
For open source projects, the contribution of open source communities behind the huge development of the project is very evident. So it is very important that a transparent documentation system is followed in such community projects. Without proper documentation it is very difficult for other developers to understand the code developed by any user and to reuse the code or make it more efficient.Unfortunately, for Let's Encrypt, they do not provide a well-structured documentation for developers. 
- No architecture and module information, which makes understanding of how the project works and how the module connect with each other difficult to understand.
- Only few files have comments, which may leads confusion for certain code.

The above aspects make it difficult for developers not in their team to contribute to the project.

##File Organization
A well-structured file organization could help project develop, Let's encrypt is a pretty new project and still have some trouble in this aspect.
- issue [#2155](https://github.com/letsencrypt/letsencrypt/issues/2155):Now, the renewal contain paths relative to config\_dir, it is useful to contains such path, but for convenience they ignore it, this may save some time at the first place, but now they have to pay the time back. They decide to do some change and add the path to config_dir now.

##User Friendly
A good user experience is important for the software, and a crucial first step in achieving this is deploying a user-friendly interface. For Let's Encrypt, user mostly interact with the software when deploy `letsencrypt` for their own website, this progress in done mainly in terminal. This process is not user friendly enough.
- issue [#2498](https://github.com/letsencrypt/letsencrypt/issues/2498): The client will display all the changes to users. Users can't specify how many lines to show on their screen. The developer choose to ignore this unresonalable fact to save some time, but now they have to pay their time back. The issue has been closed and they add a parameter to the function to enable specifying how many lines to show
- issue [#2114](https://github.com/letsencrypt/letsencrypt/issues/2114): Now, when the letsencrypt-auto fails, the client won't show what exactly fails to the client. Most programs have trace back features while letsecnrypt doesn't. I believe it is the developers who choose to ignore such needs of the client, just for convenience, they choose to do it quick and dirty. It is also a technical debt which they have to pay back.


##How developers deal with technical debt
For technical debt, developers always find it via Issues, discuss it and try to find a better solution to fix it. For the technical debt and related issues we mentioned above [#2155](https://github.com/letsencrypt/letsencrypt/issues/2155), [#2498](https://github.com/letsencrypt/letsencrypt/issues/2498) and [#2114](https://github.com/letsencrypt/letsencrypt/issues/2114) they use this kind of method to manage their technical debt. 
#Conclusions
#References
- Cairns, C., Allen, S. (2015), [Managing technical debt](https://18f.gsa.gov/2015/10/05/managing-technical-debt/).
- Fowler, M. (2014), [TechnicalDebtQuadrant](http://martinfowler.com/bliki/TechnicalDebt.html).
- Cunningham, W. (2011), [Ward Explains Debt Metaphor](http://c2.com/cgi/wiki?WardExplainsDebtMetaphor).
