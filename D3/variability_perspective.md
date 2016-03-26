
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

Mainly all features can be classified into four categories, environment GUI, client, plugin and configuration. Among them, there are dependencies, which means that some features depend on others. For example, users have to choose “SuperUser” authority in order to enter debug mode, choose challenge solution and manual update method. 

In addition, there are several constraints between plugin and configuration features. Configuration feature relies on the specified server choice that are Nginx and Apache. Furthermore, currently users can install a “http -> https” redirect, so their site effectively runs https only, however, this relies on the plugin of Apache server.

Finally, it’s convenient for users to get a kind email notification about the expiring date of certificate, which is surely related to the valid period of cert.

Your can find more details about this model in the picture and the textual content below:


![Feature Dependencies and Model](https://github.com/delftswa2016/team-letsencrypt/blob/master/D3/Feature.Model.png)

There are four classes of feature in Let's Encrypt: Environment, Client GUI, Plugin and Configuration.

####Enviroment

The enviroment feature of Let's Encrypt is consist of 2 parts: Operation System Support and Python Support.
Let's Encrypt supports the following classed of Operation System: FreeBSD (a kind of UNIX), OpenBSD (also a kind of UNIX), ArchLinux (kind of Linux), Debian, Fedora, Gentoo. Let's Encrypt can automatically adapt to your Operation System.

Let's Encrypt provides support for Python 2.6 and Python 2.7, for other version of python, it doesn't perfectly support. So we only identify support of Python 2.6/2.7 as a feature of Let's Encrypt.

####Client GUI

This class includes all features about Client Graphic User Interface.

- display: users can switch between graphic interface or command line interface
- Notification: users can choose the way to receive notifications: by email/pop out window/no notification
- User Authority: there are 2 kinds of users: user and super user. The former doesn't have authority to use some instructions while the latter has.
- Virtual Enviroment: For convenience of developing, users can choose to user Virtual Enviroment. (If you have no idea about virtual enviroment, Virtual Machine is a good analogy)
- Usage: Let's Encrypt can be used as an independent software. It can also be used as a plugin for other software.
- Input Method: There are two kinds of input method to choose: users can input a instruction from key board; users can also input the instruction from a pre-stored file. The second method is very convenient if there are a lot of instructions to input.



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
