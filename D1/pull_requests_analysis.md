Pull Request Analysis
===========================
### 1. Title: Allow users to choose how many config changes are shown[(\#2498)](https://github.com/letsencrypt/letsencrypt/pull/2498)
**Created by:** [SwartzCr](https://github.com/letsencrypt/letsencrypt/pulls/SwartzCr)

**Description:** Added a new parameter in *client.view_config_changes()* function so that the users could limit the number of lines shown in the command line. i.e. changing  from *client.view_config_changes(config)* to  *client.view_config_changes(config, num=config.num)*, with the parameter *config.num* used to specify the number of lines.

**Stakeholder:** Developers, users

### 2.Title: Print only challenge changes to configs [(\#2496)](https://github.com/letsencrypt/letsencrypt/pull/2262)
**Created by:** [SwartzCr](https://github.com/letsencrypt/letsencrypt/pulls/SwartzCr)

**Description:** Changed the logging of *reverter.view_config_changes()* and rewrote it in another way. This pull request is addressing the issue #2410. 
The issue #2410 is to solve the fact that *view_config_changes()* only displays permanent changes which are saved to a specific file but doesn’t display the temporary changes which will not be saved.

**Stakeholder:** Developers

### 3.Title: Fix apache2.2 redirect problems [(\#2489)](https://github.com/letsencrypt/letsencrypt/pull/2489)

**Created by:** [SwartzCr](https://github.com/letsencrypt/letsencrypt/pulls/SwartzCr)

**Description:** Solved a bug that would cause the letsencrypt-apache to fail. The problem was posted in the issue #2308. According to the issue #2308, letsencrypt_apache uses some of the test libraries from the letsencrypt. However, they strip the tests and the test data from the python-letsencrypt library during its build step to save space in the package and to prevent the distribution of the unneeded files. This is a downstream bug.

**Stakeholder:** Developers, Users

### 4. Title: Fix minor bootstrap problems[(\#2485)](https://github.com/letsencrypt/letsencrypt/pull/2485)

**Created by:** [bmw](https://github.com/bmw)

**Description:** 1) Prevented the script from exiting if it needs to install OS packages. 2) Removed quotes around $SUDO from the *arch_common.sh* and the other bootstrap scripts.

**Stakeholder:** Developers

### 5.Title: Remove acme-challenge after cleaning up all challenges[(\#2480)](https://github.com/letsencrypt/letsencrypt/pull/2480)
**Created by:** [filipochnik](https://github.com/filipochnik)

**Description:** 1) Added tests for cleaning up acme-challenge and refactor path logic in webroot plugin. After cleaning up all the challenges, the acme-challenge will be removed. 2) The response given by the developer gave another error about the travis failure. One more checking for the error code was added to solve the problem.

**StakeHolder:** Developers

### 6.Title: Parse IPv6 and IPv4 virtual host entries correctly [(\#2478)](https://github.com/letsencrypt/letsencrypt/pull/2478)

**Created by:**  [TheBoegl](https://github.com/TheBoegl)

**Description:** 1) Correctly split IPv6 addresses into the host and the port parts. This will work for the normal IPv4 and IPv6 addresses appended by a port number as well as for the IPv6 addresses without a port, which should be the normal IPv6 usage. 2) Added test for virtual host configuration file, which works correctly whether it is a reversed order or an IPv6 address without a given port.

**StakeHolder:** Developers

###7.Title: No conflicting declarations [(\#2262)](https://github.com/letsencrypt/letsencrypt/pull/2262)

**Created by:** [olabini](https://github.com/olabini)

**Description:** 1) Stopped using the options-ssl-nginx.conf but instead read it and added the declarations directly. The reason for doing this is to stop the potential conflicting declarations and make it easier to deal with the other requirements, such as providing the possibility of upgrading cipher suites. It also changes the name of the session cache used, which means that even if an *ssl_session_cache* is defined at the http{ } level, this cache will not conflict with it.

**StakeHolder:** Developers


###8. Title: Support system-default Apache on OS X. Tested on Yosemite (10.10).[(\#2449)](https://github.com/letsencrypt/letsencrypt/pull/2449)

**Created by:** [nneonneo](https://github.com/nneonneo)

**Description:** Added a configuration for the Apache plugin in order to support OSX’s built-in Apache web server. However, it still needs users to enable SSL (including httpd-ssl.conf) and generate a snakeoil CA cert to support the DVSNI check. In addition, the user has to add a vhost configuration to */etc/apache2/other*.

**StakeHolder:** Developers, users

###9.Title: Make argparse dependency unconditional. [(\#2249)](https://github.com/letsencrypt/letsencrypt/pull/2249)

**Created by:** [erikrose](https://github.com/erikrose)


**Description:** Made argparse dependency unconditional which can avoid a branch, giving the bugs one fewer place to hide and can fix the bug of argparse.This is about the Arch linux packaging and only people manually installing the Python packages with pip will use these dependency declarations.

**StakeHolder:** Developers, users

###10.Title: Added some standard apache2 modules that make tests fail.[(\#2105)](https://github.com/letsencrypt/letsencrypt/pull/2105)

**Created by:** [phispi](https://github.com/phispi)


**Description:** Letsencrypt failed on server when parsing the apache2.4 config. It turned out that the files where the error happens are standard apache2 modules. This pull request added the apache2 dav and the dav_svn modules that tests the failure on the server when parsing the apache2.4 config.

**StakeHolder:** Developers
