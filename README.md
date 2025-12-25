# Movie Application Deployment

This project demonstrates automated deployment of a **Movie Application** using **Jenkins**, **GitLab**, and **Apache2** on test and production servers.

Website deployed - https://spectacular-cupcake-7fc7e7.netlify.app/

## Table of Contents

1. [Project Overview](#project-overview)  
2. [Prerequisites](#prerequisites)  
3. [Infrastructure](#infrastructure)  
4. [Jenkins Pipeline](#jenkins-pipeline)  

## Project Overview

This repository contains the source code for a **Movie Application**. The deployment pipeline ensures that every push to the **`main` branch** in GitLab automatically triggers Jenkins to deploy the latest version to:

- **Test Server** for QA and verification  
- **Production Server** for live use  

## Prerequisites

- Jenkins installed with **SSH Agent Plugin**  
- GitLab repository with the Movie Application code  
- Test and Production servers with **Apache2** installed  
- Jenkins user can SSH into servers with proper credentials  
- GitLab credentials stored in Jenkins  

## Infrastructure

| Component             | IP Address      | Purpose                       |
|----------------------|----------------|-------------------------------|
| GitLab Server         | 192.168.1.10   | Source code repository        |
| Test Server           | 192.168.1.20   | Deployment for testing        |
| Production Server     | 192.168.1.30   | Deployment for live app       |
| Jenkins Server        | 192.168.1.5    | CI/CD automation              |

## Jenkins Pipeline

The pipeline is defined in a `Jenkinsfile` located in the root of the GitLab repository. It contains the following stages:

1. **Checkout Code from GitLab**
2. **Deploy to Test Server**
3. **Deploy to Production Server**
4. **Post Build Notifications** (success/failure)

**Sample `Jenkinsfile` snippet:**

```groovy
checkout([$class: 'GitSCM',
  branches: [[name: 'main']],
  userRemoteConfigs: [[
      url: 'http://192.168.1.10/root/estate.git',
      credentialsId: 'gitlab-credentials-id'
  ]]
])
