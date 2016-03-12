#Evolution Perspective

##Introduction
As the business maxim tells us “the only constant  is change” （Nick, 2012）, a major concern for architects is how to build a flexible system to adapt to inevitable changes. 
As a result, there is constant pressure to change the system’s behavior, which in many cases requires architectural tactics to ease such process. The term evolution is used as the process of dealing with changes encountered during the development lifecycle. 

##Requirements Capture

To identify the evolution needs, the system requirements need to be re-analyzed to find out which of them will likely need to change over time. 

###Identify Requirements

Based on the roadmap inferred from the existing document and discussion on forum and github, the key evolution requirements in the future can be summerized as follows:



|  Dimensions of Change | Magnitude of change required| Likelihood of change  |Timescale of change required|
|:-------------:|:-------------:|:-------------:|:-------------:|
| Less rate limits on the number of certificates issued per domain(**Growth**). | large-scale, high-risk|80%|approximately 7~10 months |
| Full support for web server like Nginx(**Functional**).     |  medium-scale, low-risk      |100% |approximately 3~4 months|
|Full IPv6 Support(**Functional**).| medium-scale, low-risk      |100% |approximately 2 months|
|Certificate Compatibility with Windows XP(**Platform**).|small-scale(defect correction), low-risk|100%|approximately 2~3 months|
|Elliptic Curve Cryptography (ECC) Support(**Functional**).|medium-scale, high-risk|60%|approximately 8~10 months |

###Evolution Tradeoff

The tradeoff depends on the type of the system, the likehood of the change and the confidence to defer the change until it is required. It means the prioritized evolution requirements will take flexibility into account at the beginning while the others defer the efforts. 

The architects of Letsencypt considers support for IPv6 and Nginx at initial development since they are the necessary features the project wants to include. By creating extensible interfaces for IPv6 and Nginx, the system becomes flexible for later change.

The issues of lessening rate limits on the number of certificates issued per domain and certificate compatibility with Windows XP are also considered even not with the highest priority. The reason is that the compatibility problem is a small-scale issue in our case while the rate limit demands more on the hardware support than software architecture. 

ECC support is deferred until it is required for a reason that this feature is not needed recently and possibly will not happen in the future. Thus, it is not worth investing efforts in it at the very beginning.  


##Architectural Tactics

With the development of Let’s Encypt, various architecture tactics are used to make it more flexible to accomodate changes. 

###Create extensible interfaces

Let’s Encrypt supports different servers by adding extensible plugins, which means developers can add their own plugin according to their requirements.

###Build variation points into the software

To  make Let’s Encrypt more flexible, developers design variation points into software. 
For example, Let’s Encrypt supports a series of configurable parameters like adjustable key bit-length and optional ACME compliant services. 
This allow some aspects of the system’s operation to be changed over time without modifying its implementation(Nick, 2012).

###Achieve reliable change	

To achieve reliable change, Let’s encrypt takes the following strategies:

- Create mechanisms to roll back unsuccessful deployments: 
The source code is on Github, which is a perfect tool for version control. 
Developers are able to records changes to a file or set of files over time so that they can recall specific versions later.

- Automated testing: 
Let’s Encrypt uses `tox` for testing. 
`tox` starts a full set of tests, it includes apacheconftest, unit tests for specific Python versions, code coverage, style of the whole project, etc. 

- Continuous integration: 
When making changes, it is always best to receive bad news as early as possible. 
Travis CI is used to do automated testing for every pull request to Let’s Encrypt project before merging. 
They set up project in multiple Python versions(2.6,2.7,3.3,3.4,3.5) and multiple plugins. [(links to `.travis.yml` for letsencrypt)](https://github.com/letsencrypt/letsencrypt/blob/master/.travis.yml)

