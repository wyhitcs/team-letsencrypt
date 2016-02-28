
#Introduction
The development view describes the architecture that supports the software development process. The development view communicates the aspects of the architecture of interest to stakeholders from the building, testing, maintaining and enhancing the project(). Based on this, the following article shows the common design model, module structure model and code line model of Let's Encrypt which give a technical overview of whole project. To learn more details, each model is attached with complete descrption. In addition, technical debt of the project and coresponding solution or plan are described at the end of report.
#Common Design Model
##Common Processing
- **Instruction parser**: 
The parser analyzes the received instructions with the same preset procedure: it uses a sequence of "if" to figure out what kind of instruction it is and then put the parameters into an argument list. Therefore, the parser is an independ module.
- **ACME Objects**: 
ACME components contain all protocal specific code. Whenever a module calls ACME Objects, the procedure is always the same and therefore, it is feasible to put ACME Objects in an isolated module.
- **Plugin**: 
More than three plugins are used to facilitate support for different webservers (i.e. apache, nginx). There are also IDisplay plugins, which implement bindings to alternative UI libraries.
- **Configuration**:
Configuration file can be specified with *letsencrypt-auto --config cli.ini* to change options such as the user's email address.

##Standard Design Approaches
The design of Let’s encrypt is based on plugin architecture, the interfaces available for plugins to implement are defined in [interfaces.py](https://github.com/letsencrypt/letsencrypt/blob/master/letsencrypt/interfaces.py) and [plugins/common.py](https://github.com/letsencrypt/letsencrypt/blob/master/letsencrypt/plugins/common.py#L34). The “plugin” in `letsencrypt` is as follows:
 - **Configurator**:  Implement the`IAuthenticator` and `IInstaller` interfaces.
 - **IDisplay**:  Implement bindings to alternative UI libraries.
 - **Authenticators**: Prove client deserves a certificate for some domain name by solving challenges received from the ACME server.
 - **Installer**: Setup the certificate in a server, possibly tweak the security configuration to make it more correct and secure. 
 
Current client is still in a developer-preview stage, the API may undergo a few changes. Community is still develop it and welcome other developer to contribute.
 
##Common Software
Travis CI provides integration service to build and test software projects hosted at Github. In this way, integrators can transit a great amount of his/her own work to the quality evaluation tools like Travis.

#Module Structure Model
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


##Build, Integration and Test Approach
Let's Encrypt has an offical workflow for build, integrate and test for developers.

###Build
Firstly, run the client in developer mode from local tree. Secondly, find the open issues in the github issue tracker, and start work on something, post a comment to let others know and seek feedback on the plan where appropriate. Once the developer got a working branch, he/she can open a pull request. 
###Integration
Generally it is sufficient to open a pull request and let Github and Travis run integration tests for you. However, if developer prefer to run tests, they can use `Vagrant`, using the Vagrantfile in Let’s Encrypt’s repository.  
###Test
Test the changes, all changes in pull request must have thorough unit test coverage, pass Let's Encrypt's integration tests, and be compliant with the coding style. Let's Encrypt recommend `tox` and `ipdb` for testing and debugging.

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
As shown above, the let's encrypt project have good common design model, good standard design approaches. However, there are many drawbacks. First, let's encrypt has a crappy developer documents. As a new comer it is very hard to see the whole picture of the project, let alone contribute to it. Second, the client is not user friendly, and it is also hard to install (it is designed to be easy to install, but in fact it usually crash without any trace back information, that is some kinds of technical debts, the developers still have no idea to thoroughly solve it. 
- Cairns, C., Allen, S. (2015), [Managing technical debt](https://18f.gsa.gov/2015/10/05/managing-technical-debt/).
- Fowler, M. (2014), [TechnicalDebtQuadrant](http://martinfowler.com/bliki/TechnicalDebt.html).
- Cunningham, W. (2011), [Ward Explains Debt Metaphor](http://c2.com/cgi/wiki?WardExplainsDebtMetaphor).
