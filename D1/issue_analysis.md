Issue Analysis
===========================
###1. Title: Renewer should report failures via exit status [(\#2508)](https://github.com/letsencrypt/letsencrypt/issues/2508)

**Description:** This issue proposes two things: 1) the *renew()* function should exit with a non-zero value if a renew of the certificate fails. It means there should be some feedback to the users/developers if the renew fails. 2) a “quiet” option should be added that, once it is used, there won’t be any status reported on the command line interface.

**Stakeholder:** Developers, users

###2. Title: Switch le-auto symlink to copy instead [(\#2501)](https://github.com/letsencrypt/letsencrypt/issues/2501)

**Description:** The letsencrypt-auto symlink was turned at the root of the project into a copy of the letsencrypt-auto of the latest release. A line to the release script was added to copy the latest build to the root. Our instructions used to tell people to *git pull* and then run *le-auto* from the root. This will keep them from running a bleeding-edge master version and allow us to tiptoe less around changes to *le-auto* (double reviews, etc.).

**Stakeholder:** Users
