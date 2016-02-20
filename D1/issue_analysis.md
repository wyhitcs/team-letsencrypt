Issue Analysis
===========================
###1. Title: Renewer should report failures via exit status [(\#2508)](https://github.com/letsencrypt/letsencrypt/issues/2508)

**Description:** This issue proposes two things: 1) the *renew()* function should exit with a non-zero value if a renew of the certificate fails. It means there should be some feedback to the users/developers if the renew fails. 2) a “quiet” option should be added that, once it is used, there won’t be any status reported on the command line interface.

**Stakeholder:** Developers, users