---
layout: page
title: CI/CD
permalink: /ci-cd/
---

#### CI: Continuous Integration

- Includes automation of builds, unit/functional tests, static code analysis (these are defined by team/developers)
- Team notified of results (if fail: not sent off for automated deployment; if success: send off for deployments)
- Before a team member's code is merged into a project, the code is verified/tested/reviewed as part of this process
- Continuous Integration are constantly running the tests to make sure they pass, run builds to make sure they are successful, etc.

Software Possibilities: Jenkins, TeamCity, Gitlab CI/CD Runner, Bamboo, Zephyr


#### CD: Continuous Deployment/Delivery

- Continuous delivery: process of sending an application to any environment in automated fashion, providing feedback about success/failure of delivery
- Continuous deployment: deploying the most updated code (the most recently successful build/tested code from Git) to the servers (dev, staging, production)
- Identical environments (for dev, staging, production) and deployment of production builds made in same manner (as for dev, staging)
- Orchestration and Configuration Management

Configuration Management & Provisioning: automate setup of environment in Infrastructure (i.e. in Cloud computing environment from above). Helps automate making mass changes to lots of environments as well (i.e. changing ports on all servers, installing Tomcat on multiple servers)

Ansible and Docker/Kubernetes can be used to configure the server(s) and deploy the application (code) to those servers for use. The CI/CD software helps to automate this whole process.

Software Possibilitites: Ansible, Docker, Kubernetes, Salt, Chef, Puppet



#### More Content To Come...