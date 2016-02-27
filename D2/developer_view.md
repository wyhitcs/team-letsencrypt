
#Introduction
The developer view describes the architecture that supports the software development process. For Let's Encrypt, the developer view communicate the aspects of the architecture of interest to stakeholders from the building, testing, maintaining and enhancing the project.

#Module Structure Model
#Codeline Model
In this section code structure of letsencrypt will be explored. Build, Integration, Test and Release Approach also matters a lot in understanding the project organization. In addition, Technical Debt is also analyzed.

##Source Code Structure
- acme: This package includes implement of ACME protocol objects, e.g. “account” object, “challenge” object, Java Web Key Object
- docs
- examples
- letsencrypt-apache: This package include files for apache plugin. Let’s encrypt allows users to use two kind of plugin: apache and nginx. This package is used for apache. A plugin is a implement of “installer” or “configurator” class, the details can be found in interface.py
- letsencrypt-auto-source: This package serves for the file “letsencrypt-auto”, this file is the entry of the whole project. Which can receive and parse the shell instruction from user, provides guides to users and help users communicate with bulk of our project.

- letsencrypt-compatibility-test: This package is used to test compatibility of let’s encrypt OS enviroment.

- letsencrypt-nginx: Let’s encrypt allows two kind of plugins, Apache and nginx, this package is used to implement nginx plugin. A plugin for let’s encrypty is a implement of “installer” or “configurator” class.

- letsencrypt: This is the bulk of the whole project, it contains cli.py which coordinates the whole project, and annotated ACME object ( that is    to say, it add some additional information to the object of ACME package, those additional information exists for the convenient of   let’s encrypty client)There is a folder “plugin” inside the “letsencrypt” folder, the files inside are used to call the functions in the “letsencrypt-apache” and “letsencrypt-nginx”.

- letshelp-letsencrypt: As its name says, it is responsible for the “help” instruction for let’s encrypt

- tests: Includes some .sh files for test use.
- tools: Include some .sh files which may be used frequently.


##Build, Integration and Test Approach
Let's Encrypt has an offical workfolw for build, integrate and test for developers.

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
#Conclusions
#References
