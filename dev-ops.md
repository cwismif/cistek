## DevOps
---
Having been the de facto DevOps engineer in several app development teams,  learning that DevOps should prioritise keeping the velocity of work high (fast feedback, quick build workflows), automation (repetitive, error prone tasks need automation), observability (logs and metrics)  and consistency (code style, runtime environments). 

#### Standardised development environments
Tools such as DevBox can quickly create a reproducible and fully equipped development environment, that can be shared and used to onboard quickly  
    
#### Fast feedback
Developer productivity depends on the fast feedback of their changes. Linters and compilers can highlight errors almost immediately (syntax, NPE potential etc..) plus they can enforce custom code style. Many other bugs are caught quickly if unit test coverage is good, the further along the build pipeline you go, the slower the feedback but generally speaking, if your local machine is powerful enough.  Building locally gives faster feedback  
    
#### Containers for local builds
Using a shared, identical container image in which to perform the local build is quick and gives consistent build tooling, linting, compilation, unit testing, integration testing, BDD testing, static code analysis (style, bugs, security) and performance testing.  Eventually remote CI can continue the workflow in containers based on the same build image which results in a stable workflow environment. Problems caused by the runtime environment would have been caught locally, and if a problem does occur in remote CI, it's easier to reproduce that locally for easier analysis and debugging.  
    
#### Local runtime dependencies
By using containers an application under test can have external services it integrates with and run them on the same virtual network as the build container, all locally. Although hardware can constrain this.  
    
#### Agnostic application & runtime images
The image doesn’t have to change again after all workflow checks are complete. Externalised configuration or creating a configuration layer on the application base image can ensure some consistency in local and remote staging or production environments. Incorrect configuration could introduce issues. However, keeping the application and the runtime environment as similar as possible whether in local, remote stage and production will exclude bugs or vulnerabilities being introduced that are independent of configuration.  
    
#### Monitoring
Although an NFR, this is something that can be easily unit tested and published as a team-wide library.  Some metrics are automatically provided by using libraries such as Spring Actuator, or an agent can be included in the classpath which exposes endpoints for scraping or pushes events to a distributed service such as New Relic and  Prometheus.  Logs can be gathered in an ElasticSearch cluster by using an agent called LogStash.  There are many mechanisms  
    
#### Alerting
Based on metrics, provided by the application, kubernetes cluster and/or cloud provider, rules can be applied which result in various actions such as an email to someone, a broadcast on a Slack channel, or a page to an engineer to investigate

