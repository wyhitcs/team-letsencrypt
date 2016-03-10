Context View
Context View describes the relationships, dependencies and interactions between the system and its environment(Nick,2012). 
===========================
###System scope and Responsibilities

The responsibility of Let's Encrypt includes a series functions provided by the system for users.

- users can register certificate for their server
- users can revoke certificate
- users can create their own account  
- users can modify their profile
- users can turn on/off the notice of expiring date of certificate 
- users can choose which ACME solution to use

Let's Encrypt is a software concerning network security. Its functions are surely about that. However, what is different is that, it also cares about user experience, the full automation and easy use both highlight this.

###Entities, data and interfaces

####Entities and Interfaces

There're ten entities for Let's Encrypt. 
First, the development of Let's Encrypt is based on GitHub platform. Second, Python is identified as the only language dependency.
Third, the community, Freenode, is refered as the platform that developers communicate on. Fourth, as an authority,  Let's Encrypt must be qualified with root certificate, which is provided by IdenTrust. Fifth, the release is based on the Python Package Index (PYPI).  Sixth, Let's Encrypt extends its support of different servers by adding plugins including Apache, Nginx and Webroot. Seventh, the test part is extended by test tools, Tox and Travis CI online test system. Eighth, the operating system is another entity including a series Unix-ish Operating Systems like Arch, Debian, FreeBSD, etc. Ninth, the users are a large number of websites including blueboard.cz, checkdcmain. Finally, competitors are other softwares based on ACME.

###Impact of the System on Environment

The impact of system on environment concerns about the dependencies of other system on Let's Encrypt and other systems decommissions and data migration. Let's Encrypt can be used as a plugin embedded in other certificate system, thus the performance of such system will depend on Let's Encrypt. Since Let's Encrypt is still deveoping and incomplete, there is no system decommissions because of it. However, because it is free and automatic, it does form a big threat for its competitors. Finally, Let's Encrypt is software independently developed by ISRG. Their code and data including its protocols, plugins and client are all original.

![context view](https://github.com/delftswa2016/team-letsencrypt/blob/master/D1/contextview.png)

###Reference
N. Rozanski, E. Woods. 2012. Software Systems Architecture: Working with Stakeholders Using Viewpoints and Perspectives. Addison-Wesley Professional.
