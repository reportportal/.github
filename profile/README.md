<p align="center">
  <a href="https://reportportal.io" target="blank"><img src="https://raw.githubusercontent.com/reportportal/.github/profile/profile/assets/github_header.jpg" alt="ReportPortal"></a>
</p>

---

<p align="center">
  <a href="https://slack.epmrpp.reportportal.io" target="blank"><img src="https://img.shields.io/badge/slack-join-brightgreen.svg" alt="Join Slack chat!"></a>
  <a href="http://stackoverflow.com/questions/tagged/reportportal" target="blank"><img src="https://img.shields.io/badge/reportportal-stackoverflow-orange.svg?style=flat" alt="stackoverflow"></a>
  <a href="https://reportportal.io/community" target="blank"><img src="https://img.shields.io/badge/contributors-102-blue.svg" alt="GitHub contributors"></a>
  <a href="https://hub.docker.com/u/reportportal/" target="blank"><img src="https://img.shields.io/docker/pulls/reportportal/service-api.svg?maxAge=25920" alt="Docker Pulls"></a>
  <a href="https://www.apache.org/licenses/LICENSE-2.0" target="blank"><img src="https://img.shields.io/badge/license-Apache-brightgreen.svg" alt="License"></a>
  <a href="https://www.lambdatest.com/" target="blank"><img src="https://img.shields.io/badge/Tested%20on-LambdaTest-blue" alt="lambdaTest a27c44"></a>
  <a href="http://reportportal.io?style=flat" target="blank"><img src="https://img.shields.io/badge/build%20with-❤%EF%B8%8F%E2%80%8D-lightgrey.svg" alt="Build with Love"></a>
</p>

## Overview

[ReportPortal]([url](http://reportportal.io/)) is an open-source, service-oriented, web-based platform that serves as the single entry point for your entire automated testing process. It provides enhanced **machine learning analytics**, advanced visualization, and collaborative capabilities to your testing landscape. By processing large volumes of real-time test results from various platforms and types, ReportPortal unifies your test results, helping teams identify and focus on critical issues to improve efficiency and elevate the overall quality of the product, accompanied by Quality Gates for the automated decision returned into the CI/CD pipelines.

## What Can ReportPortal Do?

Leveraging the power of AI and machine learning, ReportPortal offers a suite of unique capabilities:
- **Single Entry Point**: Streamline your testing process by having all your testing results, regardless of the platform or type, fed into a centralized system.
- **Unification of Test Results**: Collect, analyze, and present test results from various platforms and types for a comprehensive view of your testing process.
- **Failure Categorization (Triaging)**: Facilitate efficient triaging of failed test results, **using AI** to categorize failures and highlight the most critical issues.
- **Advanced Analysis**: Use **Machine Learning algorithms** to analyze test reports and failures, identify patterns, and predict potential issues. This reduces the team's effort in handling test results, allowing them to concentrate solely on new failures or enhancing test coverage.
- **Quality Gates**: Define and implement specific criteria that need to be satisfied before moving to the next development phase. Automate GO/NO-GO decisions based on testing results and integrate a feedback loop into the CI/CD pipelines.
- **Dashboarding**: Harness data visualization to track and understand trends, facilitating more informed decision-making.
- **Real-Time Reporting**: Monitor test execution in real-time, with live metrics, logs, and launch statistics available at a glance, so you can save a time on early reaction.
- **Historical Data Analysis**: Compare test results over time to identify trends and patterns.
- **Collaboration Tools**: Easily share insights, test results, and observations with your team, fostering a collaborative approach to troubleshooting and issue resolution.
- **Integration**: Seamless compatibility with popular testing frameworks ([JUnit, PyTest, TestNG, etc.]([url](https://github.com/reportportal?q=agent-&type=all&language=&sort=))) and CI/CD tools (Jenkins, Bamboo, etc.).


<p align="center">
  <a href="https://reportportal.io/installation" target="blank"><img src="https://raw.githubusercontent.com/reportportal/.github/profile/profile/assets/install_banner.png" alt="Install ReportPortal"></a>
</p>

## Repositories structure

Application Core based on micro-services architecture.
Application Core based on micro-services architecture and includes next mandatory services:

![structure](https://raw.githubusercontent.com/reportportal/reportportal/master/public/rp_repo_structure.png)

ReportPortal **server side** consists of the following services:

* [`service-authorization`](https://github.com/reportportal/service-authorization) Authorization Service. In charge of access tokens distribution
* [`service-api`](https://github.com/reportportal/service-api) API Service. Application Backend
* [`service-ui`](https://github.com/reportportal/service-ui) UI Service. Application Frontend
* [`service-index`](https://github.com/reportportal/service-index) Index Service. Info and health checks per service.
* [`service-analyzer`](https://github.com/reportportal/service-auto-analyzer) Analyzer Service. Finds most relevant test fail problem.
* [`gateway`](https://github.com/containous/traefik) Traefik Gateway Service. Main entry point to application. Port used by gateway should be opened and accessible from outside network.
* [`rabbitmq`](https://github.com/rabbitmq) Load balancer for client requests. Bus for messages between servers.
* [`minio`](https://github.com/minio/minio) Attachments storage.

Available plugins developed by ReportPortal team:

* [`plugin-bts-jira`](https://github.com/reportportal/plugin-bts-jira) JIRA Plugin. Interaction with JIRA. [Link to download](https://search.maven.org/search?q=g:%22com.epam.reportportal%22%20AND%20a:%22plugin-bts-jira%22)
* [`plugin-bts-rally`](https://github.com/reportportal/plugin-bts-rally) Rally Plugin. Interaction with Rally. [Link to download](https://search.maven.org/search?q=g:%22com.epam.reportportal%22%20AND%20a:%22plugin-bts-rally%22)
* [`plugin-saucelabs`](https://github.com/reportportal/plugin-saucelabs) Sauce Labs Plugin. Interaction with Sauce Labs. [Link to download](https://search.maven.org/search?q=g:%22com.epam.reportportal%22%20AND%20a:%22plugin-saucelabs%22)

**Client side** adapters related repositories:

* [`client-*`](https://github.com/reportportal?utf8=%E2%9C%93&q=client-) - API integrations. Http clients, which process HTTP request sending.
* [`agent-*`](https://github.com/reportportal?utf8=%E2%9C%93&q=agent-) - Frameworks integration. Custom reporters/listeners, which monitor test events and trigger event sending via [`client-*`](https://github.com/reportportal?utf8=%E2%9C%93&q=client-)
* [`logger-*`](https://github.com/reportportal?utf8=%E2%9C%93&q=logger-) - Logging integration. Logger appenders, which help to collect logs, bind it with test-case item via `agent-*` and send to server via `client-*`

**Other repositories** stored according to next rules
* [`service-*`](https://github.com/reportportal?utf8=%E2%9C%93&q=service-) - micro-services which are a part of Application
* [`commons-*`](https://github.com/reportportal?utf8=%E2%9C%93&q=commons-) - common libraries, models, etc., used by micro-services
