
#Introduction
The developer view describes the architecture that supports the software development process. For Let's Encrypt, the developer view communicate the aspects of the architecture of interest to stakeholders from the building, testing, maintaining and enhancing the project.

#Module Structure Model
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
-issue # 2155
Now, the renewal contain paths relative to config_dir, it is useful to contains such path, but for convenience they ignore it, this may save some time at the first place, but now they have to pay the time back. They decide to do some change and add the path to config_dir now.

-issue #2498 
The client will display all the changes to users. Users can't specify how many lines to show on their screen. The developer choose to ignore this unresonalable fact to save some time, but now they have to pay their time back. The issue has been closed and they add a parameter to the function to enable specifying how many lines to show

-issue #2114
Now, when the letsencrypt-auto fails, the client won't show what exactly fails to the client. Most programs have trace back features while letsecnrypt doesn't. I believe it is the developers who choose to ignore such needs of the client, just for convenience, they choose to do it quick and dirty. It is also a technical debt which they have to pay back.
#Conclusions
#References
- Cairns, C., Allen, S. (2015), [Managing technical debt](https://18f.gsa.gov/2015/10/05/managing-technical-debt/), United States Government, 
