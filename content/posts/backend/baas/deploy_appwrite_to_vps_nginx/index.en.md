---
title: "Deploying Appwrite on a VPS with Nginx: A Step-by-Step Guide"
subtitle: "A comprehensive, step-by-step tutorial to install and deploy Appwrite"
date: 2025-07-26T11:10:34+03:30
lastmod: 2025-07-26T11:10:34+03:30
draft: false
author: "SalehTZ"
authorLink: "#"
description: "A comprehensive, step-by-step tutorial on how to install and deploy Appwrite on a VPS that is already running an Nginx web server. This guide covers Docker installation, reverse proxy configuration, and securing your setup with free SSL from Let's Encrypt for a production-ready environment."
license: ""
images: []

tags: ["Appwrite", "Nginx", "DevOps", "Self-Hosting", "VPS", "Backend"]
categories: ["Backend", "DevOps"]

featuredImage: ""
featuredImagePreview: ""

hiddenFromHomePage: false
hiddenFromSearch: false
twemoji: false
lightgallery: true
ruby: true
fraction: true
fontawesome: true
linkToMarkdown: true
rssFullText: false

toc:
  enable: true
  auto: true
  keepStatic: false
code:
  copy: true
  maxShownLines: 50
math:
  enable: false
  # ...
mapbox:
  # ...
share:
  enable: true
  # ...
comment:
  enable: true
  # ...
library:
  css:
    # someCSS = "some.css"
    # located in "assets/"
    # Or
    # someCSS = "https://cdn.example.com/some.css"
  js:
    # someJS = "some.js"
    # located in "assets/"
    # Or
    # someJS = "https://cdn.example.com/some.js"
seo:
  images: []
  # ...
---


Self-hosting Appwrite gives you complete control over your backend infrastructure, data, and costs. It's a powerful solution for developers who want a flexible Backend-as-a-Service (BaaS) without vendor lock-in. However, a common challenge arises when you want to deploy Appwrite on a Virtual Private Server (VPS) that's already running other websites using an Nginx web server.

<!--more-->

How do you run both without them fighting over the same network ports? The answer is a **reverse proxy**.

In this guide, we'll walk you through the entire process of installing Appwrite on your VPS and configuring Nginx to seamlessly route traffic to it. By the end, you'll have a secure, production-ready Appwrite instance running on your own domain.

### Prerequisites

Before we begin, make sure you have the following:

* A VPS running a modern Linux distribution (like Ubuntu 22.04).
* Nginx already installed and running on your VPS.
* A domain name (e.g., `yourdomain.com`) pointed to your VPS's IP address. We will use a subdomain like `appwrite.yourdomain.com`.
* Root or `sudo` access to your server.

### The Core Challenge: Port Conflicts

By default, both Nginx and Appwrite want to use ports `80` (for HTTP) and `443` (for HTTPS) to handle web traffic. If Nginx is already using them, the Appwrite installation will fail.

Our strategy is to:

1. Install Appwrite and tell it to listen on an internal, unused port (like `8080`).
2. Configure Nginx to act as a "traffic cop." It will listen on the public ports `80` and `443` and forward any requests for `appwrite.yourdomain.com` to Appwrite's internal port.

-----

### Step 1: Install Docker and Docker Compose

Appwrite is packaged as a set of Docker containers, so Docker is a mandatory prerequisite. We have a comprehensive, step-by-step guide on setting this up correctly.

➡️ **[How to Install Docker and Docker Compose on Ubuntu 22.04.](https://www.salehtz.ir/ultimate_guide_to_install_docker)**

Once you have Docker installed and running, you can proceed to the next step.

-----

### Step 2: Install Appwrite on a Custom Port

Now we'll run the Appwrite installation script. The key is to specify custom ports when prompted.

1. In your home directory, run the installer:

    ```bash
    docker run -it --rm \
        --volume /var/run/docker.sock:/var/run/docker.sock \
        --volume "$(pwd)"/appwrite:/usr/src/code/appwrite:rw \
        --entrypoint="install" \
        appwrite/appwrite:latest
    ```

2. The installer will ask a series of questions. Pay close attention to the ports:

      * **Choose your HTTP port:** `8080` (or another unused port)
      * **Choose your HTTPS port:** `80443` (or another unused port)
      * **Enter your secret API key:** Press Enter to let it generate a secure one.
      * **Enter your Appwrite hostname:** `appwrite` (or any name you want)
      * **Enter a DNS A record hostname...:** `appwrite` (or any name you want)

The installer will now download all the Appwrite service images and start them. Your Appwrite instance is running, but it's only accessible internally on port `8080`.

-----

### Step 3: Configure Nginx as a Reverse Proxy

Let's tell Nginx how to forward traffic to our new Appwrite instance.

1. Create a new Nginx configuration file for your Appwrite subdomain:

    ```bash
    sudo nano /etc/nginx/sites-available/appwrite.yourdomain.com
    ```

2. Paste the following server block into the file. This configuration is essential as it includes headers needed for Appwrite to function correctly and adds support for the WebSockets used by Appwrite's Realtime service.

    ```nginx
    server {
        listen 80;
        server_name appwrite.yourdomain.com;

        location / {
            proxy_pass http://localhost:8080;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            
            # WebSocket support for Appwrite Realtime
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
        }
    }
    ```

3. Enable this site configuration by creating a symbolic link to the `sites-enabled` directory:

    ```bash
    sudo ln -s /etc/nginx/sites-available/appwrite.yourdomain.com /etc/nginx/sites-enabled/
    ```

4. Test your Nginx configuration to make sure there are no syntax errors:

    ```bash
    sudo nginx -t
    ```

5. If the test is successful, reload Nginx to apply the changes:

    ```bash
    sudo systemctl reload nginx
    ```

-----

### Step 4: Secure Your Domain with SSL (Let's Encrypt)

Your Appwrite instance is now accessible over HTTP, but never run a production application without SSL/TLS encryption. We'll use Certbot to get a free certificate from Let's Encrypt.

1. Install Certbot and its Nginx plugin:

    ```bash
    sudo apt install certbot python3-certbot-nginx -y
    ```

2. Run Certbot. It will automatically detect your Nginx configuration for `appwrite.yourdomain.com` and set up SSL.

    ```bash
    sudo certbot --nginx -d appwrite.yourdomain.com
    ```

    Follow the on-screen prompts. When asked about redirecting traffic, choose the option to redirect all HTTP traffic to HTTPS for the best security.

-----

### You're Live\

Congratulations\! Open your web browser and navigate to `https://appwrite.yourdomain.com`. You should see the Appwrite sign-up page, served securely by Nginx.

You have successfully deployed a powerful, self-hosted backend on your own infrastructure, ready to power your next great project.
