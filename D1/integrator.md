##Integrators:
- [pde](https://github.com/letsencrypt/letsencrypt/issues/created_by/pde)
- [bmw](https://github.com/letsencrypt/letsencrypt/pulls/bmw)

According to the definition posted by by Georgios Gousios, the integrator must act to assure the quality of project by communicating modification requirements to the original contributors. Two developers listed below are responsible to merge pull requests and respond to contributors, no matter the pull requests are accepted or denied.


##Challenge of Integrators
1. The volume of incoming contributions is quite large. Given that the project is very active which receives a new pull request approximately every 3 hours, it is a huge workload for the integrators to test all of them and check the source codes. In addition to the integration of the project, they also have other works to do, which makes time a prior challenge.
2. Maintaining quality is also a major issue. Let's Encrypt is designed to be part of the certificate infrastructure which requires very high quality codes. Considering that the project has over 150 contributors, some of them are not so experienced and their coding styles differ. As a result, maintaining quality (making sure Let's Encrypt can work efficiently and the codes are easy to read) becomes a big challenge.

##Merge Strategies
1. All the pull requests have to pass the Travis test before merging into the master. Travis CI provides continuous integration service to build and test software projects hosted at Github. In this way, integrators can transit a great amount of his/her own work to the quality evaluation tools like Travis.
2. The project fit is a prominent factor for the integrators to decide whether to accept a contribution. Commits which add new but unneeded features to the project, for example, will be rejected by the integrators.
3. The integrators interact actively with the committers by for example, pointing out the potential reasons why the Travis test fails, giving guidance on how to modify the code and complimenting on their work. It helps inspire and encourage the contributors.
4. Higher priority is given to the pull requests relevant to the current opening issues. Since such issues usually address the critical problems such as bugs and user’s requirements for the new features, it helps improve the efficiency to maintain an active project like Let's Encrypt by prioritizing the pull requests in this way.
