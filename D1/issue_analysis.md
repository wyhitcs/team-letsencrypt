Issue Analysis
===========================
###1. Title: Renewer should report failures via exit status [(\#2508)](https://github.com/letsencrypt/letsencrypt/issues/2508)

**Description:** This issue proposes two things: 1) the *renew()* function should exit with a non-zero value if a renew of the certificate fails. It means there should be some feedback to the users/developers if the renew fails. 2) a “quiet” option should be added that, once it is used, there won’t be any status reported on the command line interface.

**Stakeholder:** Developers, users

###2. Title: Switch le-auto symlink to copy instead [(\#2501)](https://github.com/letsencrypt/letsencrypt/issues/2501)

**Description:** The letsencrypt-auto symlink was turned at the root of the project into a copy of the letsencrypt-auto of the latest release. A line to the release script was added to copy the latest build to the root. Our instructions used to tell people to *git pull* and then run *le-auto* from the root. This will keep them from running a bleeding-edge master version and allow us to tiptoe less around changes to *le-auto* (double reviews, etc.).

**Stakeholder:** Users

###3. Title: Letsencrypt-auto fails with SSL error to pypi.python.org [(\#2499)](https://github.com/letsencrypt/letsencrypt/issues/2499)
**Description:**
This issue reports a bug of *letsencrypt* that the letsencrypt throws an error while running for SSL process.The issue hasn’t been settled, but the comments proposed that it may be because of some function circumvent hash checking. The relevant functions may be *cryptography()*, *cffi()*, *set_requires()*.

**Stakeholder:**Users
###4. Title: Apache installer has trouble with missing vhost on renewal [\#2491](https://github.com/letsencrypt/letsencrypt/issues/2491)

**Description:**
This is a bug may be related to vhost of letsencrypty. The comments says after fixing issue [(\#1991)](https://github.com/letsencrypt/letsencrypt/issues/1991), the problem is somehow reduced, but it still occassionaly happen when users install *letsencrypt* in their first time.

**Stakeholder:** Users

###5. Title: Apache plugin error: 'Error parsing variable' when envvars are used [\#2481](https://github.com/letsencrypt/letsencrypt/issues/2481)

**Description:**
This issue reports a bug that when users use the command ./letsencrypt-auto --apache, they receive the following message:
>the apache plugin is not working; there may be problems with your existing configuration.

The error was PluginError (u'Error Parsing variable: ${INSTANCE\_ADDRESS}'). It looks like the plugin is not able to parse the env_var in the address field.
This issue is still open and there is not a solution yet.

**Stakeholder:** Users
