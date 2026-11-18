# Automated deployment using Jenkins, GitHub integration and webhooks.

A hands-on DevOps project that automates deployment of a static website
using **GitHub, Jenkins, AWS EC2, and Nginx**.

![Jenkins CI/CD architecture](project-2-architecture.png)

## Project Overview

When code is pushed to the GitHub `main` branch, a GitHub webhook
notifies Jenkins. Jenkins checks out the repository, validates the
application file, and deploys the updated HTML page to the Nginx web
root on an AWS EC2 Ubuntu instance.

## Architecture

``` text
Developer
   |
   | git push
   v
GitHub Repository (main)
   |
   | Push webhook
   v
Jenkins on AWS EC2
   |
   | Pipeline: Checkout -> Build/Validate -> Deploy
   v
/var/www/html
   |
   v
Nginx
   |
   v
Updated website in browser
```

## Technology Stack

-   **Cloud:** AWS EC2 (Ubuntu 24.04)
-   **CI/CD:** Jenkins
-   **Source Control:** Git and GitHub
-   **Web Server:** Nginx
-   **Pipeline:** Jenkins Pipeline (Jenkinsfile)
-   **Automation Trigger:** GitHub webhook (push event)

## Repository Structure

The repository contains an existing root-level `index.html` for
Project 1. Project 2 is kept in its own folder:

``` text
simple-deployment/
├── index.html
└── project-2-jenkins-cicd/
    ├── index.html
    ├── Jenkinsfile
    ├── README.md
```

## Pipeline Stages

1.  **Checkout** -- Jenkins clones the `main` branch of the GitHub
    repository.
2.  **Build / Validate** -- Jenkins checks that
    `project-2-jenkins-cicd/index.html` exists. Since this is a static
    HTML application, no compilation step is required.
3.  **Deploy** -- Jenkins copies the Project 2 `index.html` into
    `/var/www/html/`, where Nginx serves it.

```

## EC2 Deployment Permissions

Jenkins runs as the `jenkins` Linux user. A dedicated group grants
controlled write access to the Nginx document root rather than using
broad permissions:

``` bash
sudo groupadd webdeploy
sudo usermod -aG webdeploy jenkins
sudo chgrp webdeploy /var/www/html
sudo chmod 775 /var/www/html
sudo chmod g+s /var/www/html
sudo systemctl restart jenkins
```

The Jenkins user must have `webdeploy` group membership for the
deployment copy command to work.

## Jenkins Job Configuration

Create a Pipeline job in Jenkins and configure:

-   **Definition:** Pipeline script from SCM
-   **SCM:** Git
-   **Repository URL:**
    `https://github.com/prakashgit122/simple-deployment.git`
-   **Branch:** `*/main`
-   **Script Path:** `project-2-jenkins-cicd/Jenkinsfile`
-   **Build Trigger:** GitHub hook trigger for GITScm polling

## GitHub Webhook Configuration

In the GitHub repository, open **Settings → Webhooks → Add webhook** and
configure:

-   **Payload URL:** `http://<EC2-PUBLIC-IP>:8080/github-webhook/`
-   **Content type:** `application/json`
-   **Event:** Just the push event

A push to `main` was tested and successfully triggered Jenkins Build #2
automatically. Checkout, Build, and Deploy all completed successfully.

> **Security note:** The webhook test was performed while Jenkins port
> 8080 was reachable by GitHub. After testing, the EC2 security group
> was changed to restrict port 8080 to the developer's IP. With that
> restriction in place, GitHub cannot reach the webhook directly, so
> webhook automation needs a secure, publicly reachable ingress before
> it can continue. Do not leave Jenkins port 8080 open to `0.0.0.0/0`
> permanently. For production, configure a domain and HTTPS reverse
> proxy (or another secure webhook ingress), protect the Jenkins UI, and
> use a webhook secret.

## Results

-   Connected GitHub with Jenkins.
-   Configured a Jenkins Pipeline using a Jenkinsfile stored in source
    control.
-   Validated the static HTML application.
-   Deployed the application to Nginx on AWS EC2.
-   Verified that a GitHub push triggered a Jenkins build automatically.
-   Confirmed successful Checkout, Build, and Deploy stages.

------------------------------------------------------------------------

