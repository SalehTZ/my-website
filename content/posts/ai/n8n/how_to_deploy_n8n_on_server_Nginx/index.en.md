---
title: "How to Deploy n8n with Docker and Nginx: The Ultimate Guide. (UPDATE: fix Connection lost issue)"
subtitle: "A Complete Guide to Stable n8n Self-Hosting on Linux (v1.88.0+)"
date: 2025-05-31T15:15:54+03:30
lastmod: 2025-06-01T08:30:00+03:30
draft: false
author: ""
authorLink: ""
description: "Learn how to deploy n8n using Docker and Nginx, and fix the common 'Connection lost' error with specific WebSocket configurations for stable workflow automation."
license: ""
images: []

tags: ["n8n", "Docker", "Nginx", "Deployment", "WebSocket", "Automation"]
categories: [ "n8n", "Docker", "DevOps", "AI"]

featuredImage: "images/posts/n8n/n8n.png"
featuredImagePreview: "images/posts/n8n/n8n.png"

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
  images: ["images/posts/n8n/n8n.png"]

  # ...
---

<!--more-->


## I. Introduction

n8n is a powerful, open-source workflow automation tool that allows you to connect various applications, automate repetitive tasks, and build complex workflows without extensive coding. Its flexibility and growing community make it a fantastic choice for individuals and teams looking to streamline their operations.

However, when self-hosting n8n behind a reverse proxy like Nginx, many users encounter a persistent and frustrating issue: the "Connection lost: You have a connection issue or the server is down. n8n should reconnect automatically once the issue is resolved" error. This often occurs with newer n8n versions that rely more heavily on real-time communication via WebSockets.

This comprehensive guide will walk you through a robust and stable **n8n deployment** using **Docker** and **Nginx**, leveraging n8n's default SQLite database for convenience. More importantly, we'll provide a specific, tested solution to overcome the "Connection lost" error, ensuring your **n8n self-hosting** setup is reliable and performs seamlessly. By the end of this guide, you'll have a fully functional and secure **workflow automation** server.

## II. Prerequisites: What You'll Need

Before we dive into the **n8n setup guide**, ensure you have the following ready on your server:

  * A fresh Linux server (e.g., Ubuntu 22.04 LTS, Debian 12) with root or sudo access.
  * **Docker** installed (refer to the official Docker documentation for installation).
  * **Docker Compose** installed (usually comes with Docker Desktop or can be installed separately).
  * **Nginx** installed (e.g., `sudo apt update && sudo apt install nginx`).
  * A registered domain name (e.g., `n8n.yourdomain.com`) pointing to your server's IP address. This is highly recommended for production environments.
  * Basic command-line familiarity and understanding of Linux file systems.

## III. Step 1: Setting Up Your n8n Docker Environment

To simplify the **Docker n8n** deployment process and manage its services efficiently, we'll use Docker Compose. This allows us to define all our services, networks, and volumes in a single YAML file. By default, n8n uses a SQLite database, which is stored within its data volume.

### Creating the `docker-compose.yml` for n8n

First, create a dedicated directory for your n8n configuration and data. This helps keep your files organized and facilitates upgrades and backups.

```bash
sudo mkdir -p /opt/n8n
cd /opt/n8n
```

Now, create a file named `docker-compose.yml` within this directory and paste the following content. Pay close attention to the environment variables, especially the one crucial for our "Connection Lost" fix.

```yaml
version: '3.8'

services:
  n8n:
    image: n8n.io/n8n # Using the official n8n Docker image
    restart: always # Ensure n8n restarts automatically if it crashes or the server reboots
    ports:
      # Bind n8n to localhost:5678. Nginx will proxy external requests to this port.
      # If Nginx is on a different machine, replace 127.0.0.1 with 0.0.0.0
      - "127.0.0.1:5678:5678"
    environment:
      # IMPORTANT: Replace n8n.yourdomain.com with your actual domain name
      - N8N_HOST=n8n.yourdomain.com
      - N8N_PORT=5678
      - N8N_PROTOCOL=http # Nginx will handle HTTPS
      # The WEBHOOK_URL is critical for webhooks to function correctly.
      # Ensure it matches your Nginx proxy domain and protocol (http here, Nginx handles https)
      - WEBHOOK_URL=http://n8n.yourdomain.com/

      # <<< THE CRUCIAL FIX for "Connection Lost" >>>
      # This environment variable explicitly tells n8n to use WebSockets
      # for its push backend, which is essential for newer n8n versions
      # when proxied by Nginx.
      - N8N_PUSH_BACKEND=websocket

      # Security: Generate a strong, unique encryption key
      # Use `openssl rand -base64 32` to generate a secure key
      - N8N_ENCRYPTION_KEY=YOUR_SUPER_SECURE_N8N_ENCRYPTION_KEY_HERE_CHANGE_ME_NOW

      # Basic Authentication for n8n UI (Highly Recommended)
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=n8nadmin # Choose a strong username
      - N8N_BASIC_AUTH_PASSWORD=YOUR_STRONG_N8N_PASSWORD_HERE_CHANGE_ME_NOW

      # Note: By default, n8n uses SQLite. No database specific environment
      # variables are needed for this setup. The SQLite database file
      # will be stored persistently in the 'n8n_data' volume.

    # Persistent storage for n8n data (workflows, credentials, settings, SQLite database)
    volumes:
      - n8n_data:/home/node/.n8n

volumes:
  n8n_data: # Defines a Docker volume for persistent storage
```

**Before saving:**

  * **Crucially, replace `n8n.yourdomain.com` with your actual domain.**
  * **Generate a strong `N8N_ENCRYPTION_KEY`**. The n8n documentation recommends `openssl rand -base64 32`.
  * **Set a strong `N8N_BASIC_AUTH_USER` and `N8N_BASIC_AUTH_PASSWORD`.**

### Running n8n with Docker Compose

Once your `docker-compose.yml` file is configured, navigate to `/opt/n8n` in your terminal and start n8n:

```bash
docker-compose up -d
```

This command will download the `n8n.io/n8n` Docker image (if not already present), create the `n8n_data` volume, and start the n8n container in detached mode (`-d`).

### Initial Verification

You can check if the n8n container is running correctly by listing your Docker containers:

```bash
docker ps
```

And view its logs to ensure it's starting without immediate errors:

```bash
docker-compose logs -f n8n
```

You should see output indicating n8n is starting up and listening on port `5678`.

## IV. Step 2: Configuring Nginx as a Reverse Proxy

**Nginx** will act as our **reverse proxy**, sitting in front of the n8n Docker container. This setup allows you to:

  * Use a custom domain (e.g., `n8n.yourdomain.com`).
  * Handle SSL/TLS encryption (HTTPS), which is essential for security.
  * Proxy requests to the correct internal Docker port.
  * Most importantly, facilitate WebSocket communication for the n8n UI and real-time updates.

### Nginx Configuration for n8n: Enabling WebSockets

Create a new Nginx server block configuration file. A common location for this is `/etc/nginx/sites-available/`.

```bash
sudo nano /etc/nginx/sites-available/n8n.yourdomain.com.conf
```

Paste the following configuration, replacing `n8n.yourdomain.com` with your actual domain:

```nginx
server {
    listen 80; # Listen for standard HTTP requests
    server_name n8n.yourdomain.com; # Replace with your actual domain

    location / {
        # Proxy requests to the n8n Docker container running on localhost:5678
        proxy_pass http://127.0.0.1:5678/;

        # Standard Nginx proxy headers
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # <<< THE NGINX FIX for "Connection Lost" - WebSocket Proxying >>>
        # These headers are critical for Nginx to correctly proxy WebSocket connections.
        # Newer n8n versions rely on WebSockets for real-time UI updates.
        proxy_http_version 1.1; # Enable HTTP/1.1 for WebSocket support
        proxy_set_header Upgrade $http_upgrade; # Pass the Upgrade header from client to server
        proxy_set_header Connection 'upgrade'; # Instruct Nginx to "upgrade" the connection to WebSocket

        # Prevents caching issues that can interfere with WebSocket persistence
        proxy_cache_bypass $http_upgrade;

        # Optional: Increase proxy timeouts for long-running n8n workflows
        # proxy_read_timeout 3600s; # Adjust as needed for long polling or large data transfers
        # proxy_send_timeout 3600s; # Adjust as needed
    }

    # You will add HTTPS configuration here after setting up Certbot
}
```

### Enabling and Testing Nginx

After saving the file, you need to enable the new Nginx configuration by creating a symlink to the `sites-enabled` directory:

```bash
sudo ln -s /etc/nginx/sites-available/n8n.yourdomain.com.conf /etc/nginx/sites-enabled/
```

Test your Nginx configuration for syntax errors:

```bash
sudo nginx -t
```

If you see `syntax is ok` and `test is successful`, you can reload Nginx to apply the changes:

```bash
sudo systemctl reload nginx
# Or, if reload doesn't work:
# sudo systemctl restart nginx
```

Your **Nginx reverse proxy** is now configured to point to your n8n Docker container.

## V. Step 3: Resolving the "Connection Lost" Issue (Your Specific Solution)

The "Connection lost" error is a common pain point for users deploying n8n behind a reverse proxy, especially with `n8n Version: 1.88.0` and newer. It primarily occurs because the reverse proxy (Nginx, in our case) isn't correctly handling or "upgrading" the connection to a **WebSocket** protocol, which n8n now extensively uses for its real-time user interface and backend communication.

Our solution specifically addresses this **WebSocket fix** by making two crucial adjustments:

### Part 1: Nginx WebSocket Headers Configuration

As shown in the Nginx configuration above, the following lines are paramount within the `location /` block:

```nginx
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_cache_bypass $http_upgrade;
```

  * `proxy_http_version 1.1;`: Explicitly sets the proxy HTTP version to 1.1. This is necessary because WebSockets were introduced with HTTP/1.1's upgrade mechanism.
  * `proxy_set_header Upgrade $http_upgrade;`: This directive tells Nginx to pass the `Upgrade` request header from the client to the n8n server. The `Upgrade` header is used to initiate a protocol upgrade, in this case, from HTTP to WebSocket.
  * `proxy_set_header Connection 'upgrade';`: This directive instructs Nginx to add the `Connection: upgrade` header to the proxied request. This header works in conjunction with `Upgrade` to signal a protocol upgrade request.
  * `proxy_cache_bypass $http_upgrade;`: This is an important detail. It tells Nginx to bypass any caching mechanisms if an `Upgrade` header is present. Caching WebSocket connections would break their persistent nature.

These four lines ensure that Nginx correctly interprets and forwards the WebSocket handshake and subsequent data flow, preventing the "Connection lost" error due to improper proxying.

### Part 2: `N8N_PUSH_BACKEND=websocket` Environment Variable

In your `docker-compose.yml` file, we added this specific environment variable:

```yaml
          - N8N_PUSH_BACKEND=websocket
```

While the Nginx configuration handles the proxying, this environment variable is equally vital. It explicitly instructs the n8n application itself to use **WebSockets** for its internal "push backend" (for things like real-time UI updates, notifications, and possibly workflow execution feedback). Without this, even with correct Nginx proxying, n8n might not initiate WebSocket connections internally, leading to connection issues.

By combining both the Nginx WebSocket proxy headers and the `N8N_PUSH_BACKEND=websocket` environment variable, you create a robust and stable **n8n deployment** that eliminates the dreaded "Connection lost" message. This **troubleshooting n8n** solution has been verified on `n8n Version: 1.88.0` and is expected to work for subsequent versions.

## VI. Step 4: Securing Your n8n Deployment with SSL

It is critical to secure your **n8n deployment** with SSL/TLS (HTTPS) to encrypt all communication between your users and your n8n server. This is especially important as n8n handles sensitive workflow data and potentially external API keys. We'll use **Certbot** with **Let's Encrypt** for free and easy SSL certificates, following the recommended Snap installation method.

### Adding SSL/TLS with Certbot (Let's Encrypt)

Certbot, provided by the Electronic Frontier Foundation (EFF), automates the process of obtaining and renewing SSL certificates from Let's Encrypt. The recommended installation method is via Snap, which ensures you have the latest version and simplifies maintenance.

1.  **Ensure Snapd is installed and up to date:**
    Most modern Linux distributions, including Ubuntu 16.04 LTS and later, come with `snapd` pre-installed. You can ensure it's up to date with:

    ```bash
    sudo snap install core
    sudo snap refresh core
    ```

    If `snapd` is not installed on your system, you can typically install it via your distribution's package manager (e.g., `sudo apt install snapd` on Debian/Ubuntu).

2.  **Remove any conflicting Certbot OS packages:**
    To avoid conflicts, it's best to remove any Certbot packages that might have been installed via your system's package manager (`apt`, `dnf`, `yum`, etc.):

    ```bash
    sudo apt remove certbot # For Debian/Ubuntu. Adjust for your OS if needed.
    ```

3.  **Install Certbot via Snap:**
    Install the classic Certbot snap package:

    ```bash
    sudo snap install --classic certbot
    ```

4.  **Create a symbolic link for the `certbot` command:**
    To ensure the `certbot` command is easily accessible from your PATH:

    ```bash
    sudo ln -s /snap/bin/certbot /usr/bin/certbot
    ```

5.  **Obtain and install the SSL certificate for your domain:**
    Certbot's Nginx plugin will automatically configure Nginx to use your new certificate and set up redirects from HTTP to HTTPS. Run the following command, replacing `n8n.yourdomain.com` with your actual domain:

    ```bash
    sudo certbot --nginx -d n8n.yourdomain.com
    ```

    Follow the interactive prompts:

      * You'll be asked to enter an email address for urgent renewal and security notices.
      * You'll need to agree to the Let's Encrypt Terms of Service.
      * Certbot will detect your Nginx configuration and ask if you want to redirect HTTP traffic to HTTPS (recommended).

    Once completed, Certbot will automatically modify your Nginx configuration file (`/etc/nginx/sites-available/n8n.yourdomain.com.conf`) to include the `listen 443 ssl;` directives and paths to your newly acquired certificate files. It also sets up a systemd timer or cron job for automatic certificate renewal, so you don't have to worry about manual renewals every 90 days.

After Certbot completes, your Nginx configuration file for `n8n.yourdomain.com` will be updated to include HTTPS listeners and certificate paths. You should now be able to access n8n via HTTPS.

## VII. Verification and Access

Once all steps are complete, your **n8n deployment** should be fully operational and accessible via your domain name.

1.  Open your web browser and navigate to `https://n8n.yourdomain.com`.
2.  You should be prompted for the basic authentication credentials (`n8nadmin` and your strong password) that you set in the `docker-compose.yml`.
3.  After logging in, you should see the n8n dashboard without any "Connection lost" messages. The UI should be responsive, indicating that the WebSocket connection is stable.

If you encounter any issues, always start by checking:

  * Nginx error logs: `sudo tail -f /var/log/nginx/error.log`
  * n8n Docker container logs: `docker-compose logs -f n8n`
  * Ensure your domain's DNS records are correctly pointing to your server's IP.

## VIII. Conclusion

You have successfully deployed **n8n with Docker and Nginx**, utilizing its default SQLite database for a straightforward setup. Furthermore, you've implemented a crucial **WebSocket fix** for the "Connection lost" error, and secured your instance with SSL/TLS. This robust **n8n setup guide** provides a stable foundation for all your **workflow automation** needs, allowing you to leverage n8n's full potential without disruptive connection issues.

By following these steps, you've created a reliable, secure, and performant environment for your n8n workflows. We encourage you to explore n8n's capabilities further and start automating your tasks\! Share your experiences or any questions in the comments below. Happy automating\!

-----

#### Keywords

`n8n deployment`, `Docker n8n`, `Nginx reverse proxy`, `n8n connection lost fix`, `WebSocket proxy`, `n8n self-hosting`, `workflow automation`, `Docker Compose`, `Certbot`, `Let's Encrypt`, `SSL/TLS`, `n8n setup guide`, `open-source automation`, `troubleshooting n8n`

#### References

  * **n8n Docker Installation Guide:** [https://docs.n8n.io/hosting/installation/docker/](https://docs.n8n.io/hosting/installation/docker/)
  * **Certbot Nginx Snap Installation Instructions:** [https://certbot.eff.org/instructions?ws=nginx\&os=snap](https://certbot.eff.org/instructions?ws=nginx&os=snap)
