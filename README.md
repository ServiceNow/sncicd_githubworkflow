# Starter Workflows for ServiceNow CI/CD using GitHub Actions

[![Documentation](https://img.shields.io/jenkins/plugin/v/servicenow-cicd.svg?label=Documentation)](https://plugins.jenkins.io/servicenow-cicd)
[![GitHub release](https://img.shields.io/github/release/jenkinsci/servicenow-cicd-plugin.svg?label=Release)](https://github.com/jenkinsci/servicenow-cicd-plugin/releases/latest)
[![Jenkins CI](https://ci.jenkins.io/buildStatus/icon?job=Plugins/servicenow-cicd-plugin/master)](https://ci.jenkins.io/blue/organizations/jenkins/Plugins%2Fservicenow-cicd-plugin/activity/)
[![Jenkins Plugin Installs](https://img.shields.io/jenkins/plugin/i/servicenow-cicd.svg?color=blue)](https://stats.jenkins.io/pluginversions/servicenow-cicd.html)
[![Contributors](https://img.shields.io/github/contributors/jenkinsci/servicenow-cicd-plugin.svg)](https://github.com/jenkinsci/servicenow-cicd-plugin/graphs/contributors)

These are the workflow files for helping people get started with GitHub Actions when building applications on the Now Platform.
Just copy and paste into your pipeline once your workflow is created for your application's linked GitHub repo. 

## Contents

- [Intro](#intro)
- [Usage](#usage)
- [API Docs](#api-docs)
- [List of Build Steps](#build-steps)
- [Integration Tests](#integration-tests)
- [Troubleshooting](#troubleshooting)
- [Support Model](#support-model)
- [Governance Model](#governance-model)

---

## Intro

This extension provides Build Steps for setting up Continuous Integration (CI) or Continuous Delivery (CD) workflows using Jenkins for developing applications on the [Now Platform from ServiceNow](https://www.servicenow.com/now-platform.html). **Click on the below screenshot to see a video for how you can use this extension to get started faster.**

[![Get Started with Jenkins in 10 Minutes](doc/youtube_link_Jenkins.png)](https://www.youtube.com/watch?v=MLa5dOu5zhY "Get Started with Jenkins in 10 Minutes")

The Build Steps are API wrappers for the [CI/CD APIs](https://developer.servicenow.com/dev.do#!/reference/api/paris/rest/cicd-api) first released with Orlando, and do not cover other ServiceNow APIs. They will currently work with the Orlando and Paris releases. 

Please reference our [open-source GitHub repo](https://github.com/jenkinsci/servicenow-cicd-plugin) for the implementation, as well as to submit any Issues or Pull Requests. For bug reports, see [bugs](https://issues.jenkins-ci.org/issues/?filter=22440) or [all open issues](https://issues.jenkins-ci.org/issues/?filter=22441). For documentation, see [official plugin site](https://plugins.jenkins.io/servicenow-cicd). For an example pipeline yml file, please copy from one of the existing [templates](examples/). 

## Usage

### Prerequisites

You will need a GitHub account and a Jenkins box at a minimum. 
1. If you have a github.com account already, you're good to go. You should be able to create a new GitHub repository to get a Git repo URL for using Source Control with an app on your instance. Alternatively, you may need to request an account from your IT or engineering team if you have a GitHub Enterprise instance hosted internally. 
2. To set up your own Jenkins box, you can follow the [Docker image setup instructions at the Jenkins site](https://www.jenkins.io/doc/book/installing/docker/), running on compute such as [AWS EC2](https://aws.amazon.com/ec2/). To create a new Jenkins project, just click on New Item on the main page.

### Quick Setup Guide

1. [Link to Source Control](https://developer.servicenow.com/dev.do#!/learn/learning-plans/paris/new_to_servicenow/app_store_learnv2_devenvironment_paris_linking_an_application_to_source_control) for an application that has been created on your instance. You'll find the link on the main page for your GitHub repository if you're starting fresh. Recommend to link to "master" or "main" branch initially. 
2. At the root of your Git repo directory, create a new file called "Jenkinsfile". Copy paste this [pipeline template](https://github.com/jenkinsci/servicenow-cicd-plugin/blob/master/examples/multibranch_pipeline) into it. Feel free to make modifications to fit your needs and workflows. Remember to change your environment variables to match your application's `sys_id`, ATF Test Suite `sys_id`, etc. On the first time, you can commit and save to the master branch without running the pipeline yet. 
3. To set up Credentials for authenticating into your instances with a service account, go to your Jenkins homepage, "Manage Jenkins" in the left navigation panel, "Manage Credentials" under Security, "(global)" domain, add a new "Username with Password" credential. Plug in the ID you name the credential with into the Jenkinsfile variable. 
4. To set up Credentials for connecting to GitHub, do the same workflow for the above, but use your GitHub username and a [Personal Access Token](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token). At a minimum, you'll need the following list of scopes: admin:org_hook, admin:repo_hook, repo, user:email, workflow.
5. To set up your first Jenkins pipeline, go to your Jenkins homepage, click on "New Item" in the left navigation panel, "Multibranch Pipeline". Select "GitHub" for Branch Sources, select the Credentials you generated for GitHub, fill in the repository URL from GitHub, and validate the connection. For Discover Branches, select "All Branches" for Strategy. You can leave everything else at default. You can validate things are working if the Scan Repository Log actually picks up your branches from GitHub. 
6. You should now be able to create a new feature branch off master branch on your instance, develop features/fixes, commit to Source Control, create a PR, and your CI build will run automatically. Once our CI build passes and your PR is completed and feature branch merged to master, your CD build to deploy the application to production should trigger as well. 

### Other Notes

Tasks are all named starting with the **ServiceNow CI/CD** substring for easier organization and search filtering, and can be added via both the classic editor as well as the YAML editor in Azure DevOps. 

Some Tasks can produce output variables that are consumed as input for other Tasks. For example, the `Publish Application` Task generates a variable `publishVersion` that contains the version number for a recently published app. The `Install Application` Task can then consume this variable and produce a `rollbackVersion` variable that indicates the previous version of that app on the target instance, providing a mechanism for rolling back the application in `Rollback Application`. 

## API docs

The extension's Azure Pipelines Tasks are wrappers for the CI/CD APIs released as a part of Orlando, and will currently work through the Paris release. For more information, please see the ServiceNow [REST API documentation](https://developer.servicenow.com/dev.do#!/reference/api/orlando/rest/cicd-api). Tasks and APIs are not necessarily 1:1 matches; for example, the `ServiceNow CI/CD Start Test Suite` Task will trigger an ATF Test Suite run, get the progress, and when progress reaches 100%, will return the Test Suite result. 




## Marketplace

- https://github.com/marketplace/actions/servicenow-ci-cd-apply-changes
- https://github.com/marketplace/actions/servicenow-ci-cd-install-app
- https://github.com/marketplace/actions/servicenow-ci-cd-publish-app
- https://github.com/marketplace/actions/servicenow-ci-cd-rollback-app
- https://github.com/marketplace/actions/servicenow-ci-cd-run-atf-test-suite
- https://github.com/marketplace/actions/servicenow-ci-cd-activate-plugin
- https://github.com/marketplace/actions/servicenow-ci-cd-rollback-plugin

## Repos
- https://github.com/ServiceNow/sncicd-apply-changes
- https://github.com/ServiceNow/sncicd-install-app
- https://github.com/ServiceNow/sncicd-publish-app
- https://github.com/ServiceNow/sncicd-rollback-app
- https://github.com/ServiceNow/sncicd-tests-run
- https://github.com/ServiceNow/sncicd-plugin-activate
- https://github.com/ServiceNow/sncicd-plugin-rollback

## Support Model

ServiceNow built this integration with the intent to help customers get started faster in adopting CI/CD APIs for DevOps workflows, but __will not be providing formal support__. This integration is therefore considered "use at your own risk", and will rely on the open-source community to help drive fixes and feature enhancements via Issues. Occasionally, ServiceNow may choose to contribute to the open-source project to help address the highest priority Issues, and will do our best to keep the integrations updated with the latest API changes shipped with family releases. This is a good opportunity for our customers and community developers to step up and help drive iteration and improvement on these open-source integrations for everyone's benefit. 

## Governance Model

Initially, ServiceNow product management and engineering representatives will own governance of these integrations to ensure consistency with roadmap direction. In the longer term, we hope that contributors from customers and our community developers will help to guide prioritization and maintenance of these integrations. At that point, this governance model can be updated to reflect a broader pool of contributors and maintainers. 
