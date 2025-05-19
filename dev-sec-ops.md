## DevSecOps
---
Integrating vulnerability management into the build workflow, staging, and production. 

#### Pre-commit hooks

Run on every developer machine and allows scripts to run that identify problems before a commit (eg. regex that matches a string that looks like a secret!)

#### Build workflows perform

SAST the analysis of source and compiled code. DAST is analysis of vulnerabilities in an application while it is being executed. SCA where open-source dependencies are queried against vulnerability databases. Container image security scans analyse the container image using CVE and NST databases to identify vulnerabilities.   
Tooling includes: SonarQube, Snyk, Checkmarx, Black Duck, JFrog

#### SIEM - Security Information and Event Management

In production, SIEM, the real time analysis of application logs to identify attacks and trigger alerts. Tooling includes: DataDog, Splunk, Elastic Security - all of which ingest log events generally.  There are plenty of other tools.  

#### RASP - Runtime Application Self-Protection

Looks very promising as it operates by monitoring the runtime application and prevents malicious code from executing.  Some features suggest even zero-day attacks can be protected against. 

#### Patch management

Part of vulnerability management and continuously addresses recently discovered vulnerabilities and applies patches to fix these vulnerabilities, which can be manually applied or an automated apply that goes through the build workflow.

#### Infrastructure

Since the web application is only a part of the attack surface, DevSecOps need to be involved in securing the infrastructure in their domain. 

* Networks can be secured by using subnets and controlling the traffic flowing between them.  Private subnets can host the most sensitive resources, with network ACLs giving fine-grained control of permitted traffic.    
* Firewalls can be applied to resources attached to the subnets, with their own explicit rules.  Hosts on the network also have their own firewalls.   
* In addition to this, a web application firewall (WAF) can block traffic at OSI L7 and can block attacks based on HTTP request  information such as SQL injection, cross-site forgery and cross-site scripting.  It can also use its rate-limiter to mitigate DDoS attacks.   
* Encryption of data at rest and encryption of data in transit should be standard.  Keys can be rotated automatically at a period selected (AWS defaults to yearly).  TLS 1.3 should be used within the network.  
* Use network traffic logs (eg. AWS VPC flow logs) to detect unusual traffic  
* Access Control:  principle of least privilege should be applied to access control, Permissions should be granted that allow the completion of a task but no more than that, and for only the period of time it takes to accomplish the task.  Taken literally this isn’t very practical, but the essence is sensible.  There should be a limited number of engineers with the permissions to access and configure production; each engineer should have a limited domain.  2FA would grant more security for those with production access. In fact no one engineer, as senior as they are, ought to be given the power to bring down vital components of important systems.  
* Strong mechanisms for authenticating and authorising application clients.  Whitelists and deny-by-default rules are a secure practice but not always practical
