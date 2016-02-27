
#Introduction
The developer view describes the architecture that supports the software development process. For Let's Encrypt, the developer view communicate the aspects of the architecture of interest to stakeholders from the building, testing, maintaining and enhancing the project.

#Module Structure Model
#Codeline Model
##Source Code Structure
##Build, Integration and Test Approach
Let's Encrypt has an offical workfolw for build, integrate and test for developers.

###Build
Firstly, run the client in developer mode from local tree. Secondly, find the open issues in the github issue tracker, and start work on something, post a comment to let others know and seek feedback on the plan where appropriate. Once the developer got a working branch, he/she can open a pull request. 
###Integration
Generally it is sufficient to open a pull request and let Github and Travis run integration tests for you. However, if developer prefer to run tests, they can use `Vagrant`, using the Vagrantfile in Let’s Encrypt’s repository.  
###Test
Test the changes, all changes in pull request must have thorough unit test coverage, pass Let's Encrypt's integration tests, and be compliant with the coding style. Let's Encrypt recommend `tox` and `ipdb` for testing and debugging.

##Release Process
##Configuration Management
#Technical Debt
#Conclusions
#References
