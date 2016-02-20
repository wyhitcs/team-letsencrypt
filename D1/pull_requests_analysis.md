Pull Request Analysis
===========================
### 1. Title: Allow users to choose how many config changes are shown[(\#2498)](https://github.com/letsencrypt/letsencrypt/pull/2498)
**Created by:** [SwartzCr](https://github.com/letsencrypt/letsencrypt/pulls/SwartzCr)

**Description:** Added a new parameter in client.view_config_changes() function so that the users could limit the number of lines shown in the command line. i.e. changing  from client.view_config_changes(config) to  client.view_config_changes(config, num=config.num), with the parameter config.num used to specify the number of lines.

**Stakeholder:** Developers, Users

### 2.Title: Print only challenge changes to configs [(\#2496)](https://github.com/letsencrypt/letsencrypt/pull/2262)
**Created by:** [SwartzCr](https://github.com/letsencrypt/letsencrypt/pulls/SwartzCr)

**Description:** Changed the logging of reverter.view_config_changes() and rewrote it in another way. This pull request is addressing the issue #2410. 
The issue #2410 is to solve the fact that view_config_changes() only displays permanent changes which are saved to a specific file but doesn’t display the temporary changes which will not be saved.

**Stakeholder:** Developers
