# Jenkins CI Pipeline Demo

## Project Overview

This project demonstrates the implementation of a Continuous Integration (CI) pipeline using Jenkins.

The application is a Java Spring Boot project built with Maven. Jenkins is used to automatically retrieve the source code from GitHub, compile the application, run automated unit tests, package the application, build a Docker image, and push the resulting image to Docker Hub.

The pipeline is defined as code using a declarative `Jenkinsfile` stored in the root of this repository.

---

## Technologies Used

- Java 21
- Spring Boot
- Maven
- Jenkins
- Docker
- Git
- GitHub
- Docker Hub
- Ubuntu Linux

---

## CI Pipeline Workflow

The Jenkins pipeline performs the following workflow:

```text
GitHub Repository
       |
       | Jenkins detects a new commit using Poll SCM
       v
Checkout Source Code
       |
       v
Build with Maven
       |
       v
Run Unit Tests
       |
       v
Package Application
       |
       v
Build Docker Image
       |
       v
Push Docker Image to Docker Hub

Repository Structure
cidemo/
├── src/                    # Spring Boot application source and tests
├── screenshots/            # Evidence of Jenkins pipeline execution
├── Dockerfile              # Instructions for building the Docker image
├── Jenkinsfile             # Jenkins declarative CI pipeline
├── pom.xml                 # Maven project configuration
├── mvnw                    # Maven Wrapper script
├── mvnw.cmd                # Maven Wrapper for Windows
└── README.md               # Project documentation
Jenkins Environment Setup
Jenkins was installed and configured on an Ubuntu Linux virtual machine.

The environment used for the pipeline includes:

Jenkins

Git

Java 21 JDK

Maven

Docker

The Jenkins service account was given permission to communicate with the Docker daemon so that Docker images can be built and pushed from the pipeline.

Jenkins Plugins
The Jenkins environment uses the plugins required to work with Git repositories, declarative pipelines, credentials, and Docker.

The Docker Pipeline plugin is used to authenticate with Docker Hub from the Jenkins pipeline.

Jenkins Credentials
Sensitive credentials are stored using the Jenkins Credentials system rather than being hardcoded in the Jenkinsfile.

The pipeline uses the following Jenkins credential IDs:

Credential ID	Purpose
github-cidemo	Authenticate Jenkins when accessing the GitHub repository
DOCKERHUB	Authenticate Jenkins with Docker Hub
Actual usernames, passwords, and access tokens are not stored in the Jenkinsfile or committed to this repository.

Source Control Integration
The Jenkins pipeline is connected to the main branch of this GitHub repository.

Jenkins uses Poll SCM to automatically check the repository for new commits.

The configured polling schedule is:

H/5 * * * *
This causes Jenkins to check periodically for source code changes, approximately every five minutes.

When Jenkins detects a new commit, it automatically starts a new pipeline execution.

The successful validation run was automatically started by an SCM change rather than manually using the Jenkins Build Now button.

Jenkins Pipeline Stages
The CI pipeline is defined in the Jenkinsfile and contains the following stages.

1. Checkout Code
Jenkins retrieves the latest source code from the main branch of the GitHub repository.

2. Build with Maven
The application is cleaned and compiled using:

mvn clean compile
clean removes artifacts from previous Maven builds, while compile compiles the application's source code and resolves the dependencies required by the project.

3. Unit Tests
Automated tests are executed using:

mvn test
During the validated Jenkins pipeline execution, the test result was:

Tests run: 3, Failures: 0, Errors: 0, Skipped: 0
A test failure causes Maven to return a non-zero exit status. Jenkins therefore marks the stage and pipeline as failed instead of continuing with the artifact stages.

4. Package Application
After the tests pass, Maven packages the Spring Boot application:

mvn package -DskipTests
Tests are skipped during this stage because they have already been executed separately in the Unit Tests stage.

The resulting Spring Boot JAR is created under the target/ directory.

5. Build Docker Image
Jenkins builds a Docker image containing the packaged application.

The image is dynamically tagged using the Jenkins build ID:

tegajunior/cidemo:v${BUILD_ID}
This provides a different image version for each Jenkins build.

6. Push Docker Image
After the image is successfully built, Jenkins authenticates with Docker Hub using credentials stored securely in Jenkins and pushes the image to the Docker registry.

The validated pipeline produced and pushed:

tegajunior/cidemo:v4
Pipeline Validation
The complete CI workflow was tested successfully.

The validation confirmed that:

Jenkins automatically detected a GitHub repository change.

Source code checkout completed successfully.

Maven compilation completed successfully.

All 3 automated tests passed.

The Spring Boot JAR was successfully packaged.

The Docker image was successfully built.

The Docker image was successfully pushed to Docker Hub.

Jenkins reported the final pipeline result as SUCCESS.

Screenshots
Successful Jenkins Pipeline

Automatic SCM Trigger
The following screenshot demonstrates that Jenkins automatically started the pipeline after detecting a source control change.


Automated Unit Tests
The following screenshot shows the automated tests executed during the Jenkins pipeline.


A Docker Hub screenshot showing the published Docker image will also be included as additional artifact evidence.

Security Considerations
Several practices are used to avoid exposing sensitive information:

GitHub credentials are stored in Jenkins Credentials.

Docker Hub credentials are stored in Jenkins Credentials.

Secrets are referenced using Jenkins credential IDs rather than hardcoded values.

Passwords and access tokens are not committed to the Git repository.

Jenkins masks sensitive credential values in pipeline output.


This project demonstrates the following CI concepts:

Jenkins installation and environment configuration.

GitHub source control integration.

Automatic pipeline triggering using Poll SCM.

Pipeline-as-code using a declarative Jenkinsfile.

Automated application compilation with Maven.

Automated unit testing.

Application artifact packaging.

Docker image creation and publishing.

Secure credential management using Jenkins.

End-to-end pipeline validation.

Author
Created as a Jenkins CI/CD learning project.
