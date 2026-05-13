# Jenkins Configuration as Code (JCasC)

## Overview

This project demonstrates how to deploy and configure Jenkins using:

- Docker Desktop
- Docker Compose
- Jenkins Configuration as Code (JCasC)
- YAML-based Jenkins configuration

Instead of configuring Jenkins manually through the UI, this setup automatically provisions Jenkins using configuration files written in YAML.

This follows the **Infrastructure as Code (IaC)** approach where Jenkins configuration is stored, versioned, and managed as code.

---

# What is Jenkins Configuration as Code (JCasC)?

Jenkins Configuration as Code (JCasC) is a Jenkins plugin that allows Jenkins to be configured automatically using YAML files.

With JCasC, Jenkins can automatically:
- Install plugins
- Configure security
- Create users
- Configure credentials
- Set system settings
- Configure pipelines

This eliminates the need for repetitive manual setup through the Jenkins UI.

---

# Project Architecture

```text
+-----------------------+
| Docker Desktop        |
|                       |
|  +-----------------+  |
|  | Jenkins Docker  |  |
|  | Container       |  |
|  +-----------------+  |
|          |            |
|          v            |
|  Jenkins YAML Config  |
|  (JCasC)              |
+-----------------------+
```

---

# Technologies Used

| Technology | Purpose |
|---|---|
| Docker Desktop | Container runtime |
| Docker Compose | Multi-container orchestration |
| Jenkins | CI/CD automation server |
| JCasC Plugin | Jenkins configuration automation |
| YAML | Declarative configuration |

---

# Project Structure

```text
jenkins-configuration-as-code/
│
├── docker-compose.yml
├── Dockerfile
├── plugins.txt
├── casc_configs/
│   └── jenkins.yaml
└── README.md
```

---

# File Explanation

## 1. docker-compose.yml

Used to start the Jenkins container.

Responsibilities:
- Define container ports
- Configure environment variables
- Mount volumes
- Persist Jenkins data

Common ports:

| Port | Purpose |
|---|---|
| 8080 | Jenkins UI |
| 50000 | Jenkins agents |

---

## 2. Dockerfile

Used to create a custom Jenkins image.

Responsibilities:
- Install required plugins
- Copy configuration files
- Prepare Jenkins environment

Example:

```dockerfile
FROM jenkins/jenkins:lts

COPY plugins.txt /usr/share/jenkins/ref/plugins.txt

RUN jenkins-plugin-cli --plugin-file /usr/share/jenkins/ref/plugins.txt
```

---

## 3. plugins.txt

Contains the list of Jenkins plugins to install automatically.

Example:

```text
configuration-as-code
git
workflow-aggregator
docker-workflow
```

---

## 4. jenkins.yaml

Main Jenkins Configuration as Code file.

Defines:
- Users
- Security
- System settings
- Jenkins configuration

Example:

```yaml
jenkins:
  systemMessage: "Jenkins configured automatically"

  securityRealm:
    local:
      allowsSignup: false
      users:
        - id: admin
          password: admin123
```

---

# How This Project Works

## Step 1 — Start Jenkins Using Docker Compose

Run:

```bash
docker-compose up -d
```

This command:
- Pulls Jenkins image
- Builds custom Docker image
- Starts Jenkins container
- Mounts required volumes

---

## Step 2 — Jenkins Loads JCasC Configuration

During startup, Jenkins reads configuration files from:

```yaml
CASC_JENKINS_CONFIG=/var/jenkins_home/casc_configs
```

This environment variable tells Jenkins where the YAML configuration files are located.

---

## Step 3 — Jenkins Applies Configuration Automatically

Jenkins automatically configures:
- Security
- Users
- Plugins
- Credentials
- System settings

without requiring manual UI configuration.

---

# Prerequisites

Before running this project, install:

- Docker Desktop
- Docker Compose

Verify installation:

```bash
docker --version
docker-compose --version
```

---

# Setup Instructions

## Clone Repository

```bash
git clone https://github.com/swastik7777/jenkins-configuration-as-code.git
```

---

## Navigate to Project

```bash
cd jenkins-configuration-as-code
```

---

## Start Jenkins

```bash
docker-compose up -d
```

---

## Verify Running Containers

```bash
docker ps
```

---

## Access Jenkins

Open browser:

```text
http://localhost:8080
```

---

# Common Docker Compose Commands

## Start Containers

```bash
docker-compose up -d
```

## Stop Containers

```bash
docker-compose down
```

## Restart Jenkins

```bash
docker-compose restart
```

## View Logs

```bash
docker-compose logs -f
```

---

# Advantages of JCasC

## Reproducibility

Jenkins can be recreated anytime using the same YAML configuration.

---

## Version Control

All Jenkins configuration can be stored and tracked in GitHub.

---

## Faster Setup

New Jenkins environments can be provisioned quickly.

---

## Disaster Recovery

If Jenkins crashes:
- Restart container
- Reload YAML configuration
- Restore Jenkins automatically

---

## Team Collaboration

Infrastructure changes can be reviewed using pull requests.

---

# Best Practices

## Store Configurations in Git

Keep all Jenkins configuration files under version control.

---

## Avoid Manual UI Changes

Manual changes may be overwritten after container restart.

---

## Use Persistent Volumes

Persist Jenkins data:

```yaml
volumes:
  - jenkins_home:/var/jenkins_home
```

---

## Secure Credentials

Avoid hardcoding passwords directly inside YAML files.

Use:
- Environment variables
- Docker secrets
- Jenkins credentials store

---

# Troubleshooting

## Jenkins Not Starting

Check logs:

```bash
docker-compose logs -f
```

---

## Plugin Installation Errors

Verify plugin compatibility and versions.

---

## YAML Syntax Errors

Ensure YAML indentation and formatting are correct.

---

## Configuration Not Loading

Verify:

```bash
CASC_JENKINS_CONFIG
```

path is configured correctly.

---

# Real-World Use Cases

This setup is useful for:
- CI/CD environments
- DevOps automation
- Jenkins infrastructure provisioning
- Reproducible Jenkins deployments
- Enterprise Jenkins management

---

# Useful References

- Jenkins Official Documentation  
  https://www.jenkins.io/doc/

- Jenkins Configuration as Code  
  https://www.jenkins.io/doc/book/managing/casc/

- JCasC Plugin  
  https://plugins.jenkins.io/configuration-as-code/

---

# Summary

This project demonstrates how to:

- Deploy Jenkins using Docker Compose
- Configure Jenkins automatically using JCasC
- Manage Jenkins as code
- Eliminate manual Jenkins setup
- Create reproducible Jenkins environments

Using Docker + JCasC makes Jenkins:
- Portable
- Automated
- Reproducible
- Easy to maintain
