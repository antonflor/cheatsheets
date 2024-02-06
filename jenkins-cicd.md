# Jenkins CI/CD Cheat Sheet

## Introduction to Jenkins

Jenkins is an open-source automation server that enables developers to build, test, and deploy their software. It facilitates continuous integration and continuous delivery in software projects by automating various stages of the delivery process. Jenkins supports numerous plugins which extend its functionality for building, deploying, and automating across various platforms.

## Key Concepts

- **Job/Project**: The basic entity in Jenkins, representing a task or a series of tasks to be executed.

- **Build**: The process of executing a job/project.

- **Node/Agent**: A machine which is part of the Jenkins environment and is capable of executing jobs.

- **Executor**: A slot for execution in a node, determining how many jobs can run simultaneously on that node.

- **Workspace**: A directory on the node where Jenkins executes the build.

- **Pipeline**: A suite of jobs or a single job that defines the sequence of steps for continuous delivery.

- **Plugin**: An add-on that extends Jenkins' capabilities.

- **Jenkinsfile**: A text file containing the definition of a Jenkins Pipeline, typically checked into source control.

## Jenkins Operations

- **Starting Jenkins**
  - Typically, Jenkins runs as a standalone application in its own process with the built-in Java servlet container/application server (Jetty).

- **Accessing Jenkins**
  - Jenkins can be accessed through a web interface at `http://[JENKINS_SERVER]:8080`.

- **Creating a Job**
  - In Jenkins, jobs can be created through the web interface, typically by selecting "New Item", then choosing the job type.

- **Configuring a Job**
  - Each job can be configured with specific parameters, source control settings, build triggers, build steps, and post-build actions.

- **Running a Build**
  - A build can be triggered manually, by source control changes, or on a schedule.

- **Monitoring Build Results**
  - Jenkins provides detailed results on the web interface for each build, including console output, test results, and artifacts.

## Jenkins Pipeline

- **Pipeline Script**
  - Jenkins Pipeline uses a specific domain-specific language (DSL) for defining builds. This can be scripted in a `Jenkinsfile`.

- **Declarative vs. Scripted Pipeline Syntax**
  - Declarative Pipeline syntax is newer and provides a simpler and more readable syntax. Scripted Pipeline offers more flexibility and control.

- **Pipeline Stages**
  - Pipelines are typically divided into stages, such as Build, Test, and Deploy, each of which contains steps to execute.

- **Pipeline Steps**
  - Steps are individual tasks that execute commands, scripts, or actions.

- **Jenkinsfile Example**
  - A `Jenkinsfile` might look like this:
    ```groovy
    pipeline {
        agent any
        stages {
            stage('Build') {
                steps {
                    // Build steps go here
                }
            }
            stage('Test') {
                steps {
                    // Test steps go here
                }
            }
            stage('Deploy') {
                steps {
                    // Deploy steps go here
                }
            }
        }
    }
    ```

## Advanced Features

- **Distributed Builds**
  - Jenkins can distribute build tasks across multiple nodes, enabling faster builds and tests.

- **Notifications**
  - Jenkins can notify stakeholders of build results through various means like email, Slack, etc.

- **Integration with Version Control Systems**
  - Jenkins integrates with systems like Git, Subversion, etc., to trigger builds on code changes.

- **Plugins**
  - Jenkins' functionality can be extended through plugins, which are available for a wide range of tools and platforms.
