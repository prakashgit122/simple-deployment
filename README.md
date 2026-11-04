# Simple Website Deployment on AWS EC2 using Nginx

A beginner-friendly DevOps project demonstrating how to deploy a static
HTML website on an AWS EC2 Ubuntu server using Git, GitHub, SSH, and
Nginx.

## Project Overview

In this project, I created a simple HTML website locally, pushed the
source code to GitHub, launched an Ubuntu EC2 instance on AWS, installed
Nginx, and manually deployed the website to the Nginx web root.

This project helped me understand the basic workflow of hosting an
application on a cloud server.

## Architecture

``` mermaid
flowchart TD
    A[Developer / Local Machine] --> B[Create index.html]
    B --> C[Push source code to GitHub]
    C --> D[Launch AWS EC2 - Ubuntu]
    D --> E[Connect to EC2 using SSH]
    E --> F[Install and start Nginx]
    C --> G[Clone GitHub repository on EC2]
    G --> H[Copy index.html to /var/www/html/]
    F --> I[Nginx serves the website]
    H --> I
    I --> J[User opens EC2 Public IP in a browser]
```

## Technologies and Tools

-   **HTML** -- Created the static website.
-   **Git** -- Version control.
-   **GitHub** -- Hosted the source-code repository.
-   **AWS EC2** -- Provided the Ubuntu virtual server.
-   **SSH** -- Connected securely to the EC2 instance.
-   **Nginx** -- Served the website over HTTP.
-   **Linux commands** -- Installed packages, managed the service,
    cloned the repository, and deployed the file.

## Project Structure

``` text
simple-deployment/
└── index.html
```

## Implementation Steps

### 1. Create the website

Created a project folder named `simple-deployment` and added an
`index.html` file.

Example HTML:

``` html
<!DOCTYPE html>
<html>
<head>
    <title>My First Deployment</title>
</head>
<body>
    <h1>Hello DevOps World!</h1>
    <p>My first website deployment on AWS EC2.</p>
</body>
</html>
```

### 2. Push the code to GitHub

Initialized or used a Git repository, committed the project files, and
pushed the code to a GitHub repository.

Typical Git commands:

``` bash
git add .
git commit -m "Add simple HTML website"
git push
```

### 3. Launch an AWS EC2 instance

-   Launched an Ubuntu EC2 instance.
-   Created or selected an SSH key pair.
-   Allowed SSH (TCP port 22) from my own IP address.
-   Allowed HTTP (TCP port 80) so the website could be accessed in a
    browser.
-   Connected to the instance using SSH.

### 4. Update the server and install Nginx

``` bash
sudo apt update
sudo apt install nginx -y
```

Started Nginx and configured it to start automatically after a server
reboot:

``` bash
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

### 5. Install Git and clone the repository

``` bash
sudo apt install git -y
git --version
git clone https://github.com/<your-username>/simple-deployment.git
cd simple-deployment
ls
```

Replace the example GitHub URL with the URL of your own repository.

### 6. Deploy the website to Nginx

Copied the HTML file into Nginx's default web root on Ubuntu:

``` bash
sudo cp index.html /var/www/html/index.html
```

Verified that the file was present:

``` bash
ls -l /var/www/html/
```

### 7. Verify the deployment

Opened the following address in a browser:

``` text
http://<EC2-PUBLIC-IP>
```

The website displayed the message **"Hello DevOps World!"**.

## Deployment Type

**This is a manual deployment project.** The repository was cloned and
the website file was copied to the Nginx web root manually. CI/CD
automation with Jenkins is not part of this phase.

## Future Improvements

-   Automate deployment using a Jenkins CI/CD pipeline.
-   Trigger deployment when new code is pushed to GitHub.
-   Add a custom domain and HTTPS using a TLS certificate.
-   Add monitoring and logging.

-  -  -  -  -  -------------------------------------------------------  -  -  -  -  -  -  -  
