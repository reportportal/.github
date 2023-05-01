<p align="center">
  <a href="https://reportportal.io" target="blank"><img src="https://raw.githubusercontent.com/reportportal/.github/profile/profile/assets/Github_Header1.jpg" alt="ReportPortal"></a>
</p>

---

<p align="center">
  <a href="https://slack.epmrpp.reportportal.io" target="blank"><img src="https://img.shields.io/badge/slack-join-brightgreen.svg" alt="Join Slack chat!"></a>
  <a href="http://stackoverflow.com/questions/tagged/reportportal" target="blank"><img src="https://img.shields.io/badge/reportportal-stackoverflow-orange.svg?style=flat" alt="stackoverflow"></a>
  <a href="https://reportportal.io/community" target="blank"><img src="https://img.shields.io/badge/contributors-102-blue.svg" alt="GitHub contributors"></a>
  <a href="https://hub.docker.com/u/reportportal/" target="blank"><img src="https://img.shields.io/docker/pulls/reportportal/service-api.svg?maxAge=25920" alt="Docker Pulls"></a>
  <a href="https://www.apache.org/licenses/LICENSE-2.0" target="blank"><img src="https://img.shields.io/badge/license-Apache-brightgreen.svg" alt="License"></a>
  <a href="http://reportportal.io?style=flat" target="blank"><img src="https://img.shields.io/badge/build%20with-❤%EF%B8%8F%E2%80%8D-lightgrey.svg" alt="Build with Love"></a>
</p>

<p align="center">
  <a href="https://www.lambdatest.com/" target="blank"><img src="https://user-images.githubusercontent.com/11332788/230135399-6d839d7f-0dbe-45bf-8f72-dee3a5d69e17.svg" alt="lambdaTest a27c44"></a>
</p>

## Overview

ReportPortal is a service, that provides increased capabilities to speed up results analysis and reporting through the use of built-in analytic features.

ReportPortal is a great addition to Continuous Integration and Continuous Testing process.

What ReportPortal can do?

* Mainstream platforms integration ReportPortal seamlessly integrates with mainstream platforms such as Jenkins, Jira, BDD process, majority of Functional and Unit testing frameworks.
* Real-time integration Real-time integration provides businesses the ability to manage and track execution status directly from the ReportPortal.
* Test case execution results structure Test case execution results are stored following the same structure you have in your reporting suites and test plan. The test cases are shown together with all related data in one place, right where you need it: logs, screenshots, binary data. The execution pipeline of certain test cases are also available for you, so one can see previous execution results in one click.
* Collaborative analysis ReportPortal also gives you the ability to collaboratively analyze the test automation results. Particular test cases can be associated with a product bug, an automation issue, a system issue or can be submitted as an issue ticket directly from the execution result.
* Historical data of test execution ReportPortal provides enhanced capabilities along with auto-results analysis by leveraging historical data of test execution.
* Automatic Analysis With each execution, ReportPortal automatically figures out the root cause of a fail. As a result of this analysis, ReportPortal is marking a test result with a flag. Engineers will be alerted about this issue to provide further analysis: if it has been resolved already or which test results require actual human analysis.

What technologies are used?

* Considering a high load rate and performance requirements, we use cutting edge technologies such as:
* PostgreSQL - The World's Most Advanced Open Source Relational Database.
* REST Web Service - lightweight requests, industry standard.
* Mobile responsive UI - check it at any mobile device with the default browser.

## Repositories structure

Application Core based on micro-services architecture.
Application Core based on micro-services architecture and includes next mandatory services:

![structure](https://raw.githubusercontent.com/reportportal/reportportal/master/public/rp_repo_structure.png)

ReportPortal **server side** consists of the following services:
- [`service-authorization`](https://github.com/reportportal/service-authorization) Authorization Service. In charge of access tokens distribution
- [`service-api`](https://github.com/reportportal/service-api) API Service. Application Backend
- [`service-ui`](https://github.com/reportportal/service-ui) UI Service. Application Frontend
- [`service-index`](https://github.com/reportportal/service-index) Index Service. Info and health checks per service.
- [`service-analyzer`](https://github.com/reportportal/service-auto-analyzer) Analyzer Service. Finds most relevant test fail problem.
- [`gateway`](https://github.com/containous/traefik) Traefik Gateway Service. Main entry point to application. Port used by gateway should be opened and accessible from outside network.
- [`rabbitmq`](https://github.com/rabbitmq) Load balancer for client requests. Bus for messages between servers.
- [`minio`](https://github.com/minio/minio) Attachments storage.

Available plugins developed by ReportPortal team:

- [`plugin-bts-jira`](https://github.com/reportportal/plugin-bts-jira) JIRA Plugin. Interaction with JIRA. [Link to download](https://search.maven.org/search?q=g:%22com.epam.reportportal%22%20AND%20a:%22plugin-bts-jira%22)
- [`plugin-bts-rally`](https://github.com/reportportal/plugin-bts-rally) Rally Plugin. Interaction with Rally. [Link to download](https://search.maven.org/search?q=g:%22com.epam.reportportal%22%20AND%20a:%22plugin-bts-rally%22) 
- [`plugin-saucelabs`](https://github.com/reportportal/plugin-saucelabs) Sauce Labs Plugin. Interaction with Sauce Labs. [Link to download](https://search.maven.org/search?q=g:%22com.epam.reportportal%22%20AND%20a:%22plugin-saucelabs%22)

**Client side** adapters related repositories:

- [`client-*`](https://github.com/reportportal?utf8=%E2%9C%93&q=client-) - API integrations. Http clients, which process HTTP request sending.
- [`agent-*`](https://github.com/reportportal?utf8=%E2%9C%93&q=agent-) - Frameworks integration. Custom reporters/listeners, which monitor test events and trigger event sending via [`client-*`](https://github.com/reportportal?utf8=%E2%9C%93&q=client-)
- [`logger-*`](https://github.com/reportportal?utf8=%E2%9C%93&q=logger-) - Logging integration. Logger appenders, which help to collect logs, bind it with test-case item via `agent-*` and send to server via `client-*`

**Other repositories** stored according to next rules
- [`service-*`](https://github.com/reportportal?utf8=%E2%9C%93&q=service-) - micro-services which are a part of Application
- [`commons-*`](https://github.com/reportportal?utf8=%E2%9C%93&q=commons-) - common libraries, models, etc., used by micro-services