---
title: "How to Install Docker and Docker Compose on Ubuntu Server"
subtitle: ""
date: 2025-07-26T11:26:28+03:30
lastmod: 2025-07-26T11:26:28+03:30
draft: false
author: "SalehTZ"
authorLink: "#"
description: ""
license: ""
images: []

tags: ["Docker", "DevOps", "Self-Hosting", "VPS", "Backend"]
categories: ["Docker", "Backend", "DevOps"]

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
Docker has revolutionized how developers build, ship, and run applications. By using containerization, it allows you to package an application with all of its dependencies into a standardized unit for software development. Whether you're setting up a simple project or deploying a complex backend like Appwrite, having Docker installed correctly is the essential first step.

This guide will walk you through the official and most straightforward method to install Docker Engine and Docker Compose on your Ubuntu 22.04 server.

<!--more-->

### Prerequisites

* An Ubuntu 22.04 server.
* A user account with `sudo` privileges.

-----

### Step 1: Update Your System

Before installing new software, it's always a best practice to update your server's package index.

```bash
sudo apt update && sudo apt upgrade -y
```

-----

### Step 2: Install Docker Engine

We will use the official installation script provided by Docker, as it's the most convenient way to get the latest version.

1. **Download the script:**
    This command downloads the script from `get.docker.com` and saves it as `get-docker.sh` in your current directory.

    ```bash
    curl -fsSL https://get.docker.com -o get-docker.sh
    ```

2. **Run the script:**
    Execute the script with `sudo` privileges to install Docker.

    ```bash
    sudo sh get-docker.sh
    ```

3. **Verify the installation:**
    Check that Docker is running correctly by executing the simple `hello-world` image.

    ```bash
    sudo docker run hello-world
    ```

    If the installation was successful, you'll see a message that begins with "Hello from Docker\!"

-----

### Step 3: Configure Docker to Run without `sudo` (Recommended)

By default, you need to use `sudo` to run Docker commands. To run them as your current user, you must add your user to the `docker` group.

1. **Add your user to the `docker` group:**

    ```bash
    sudo usermod -aG docker ${USER}
    ```

2. **Apply the new group membership:**
    For the change to take effect, you need to either log out and log back in, or you can activate the changes for the current terminal session with this command:

    ```bash
    newgrp docker
    ```

Now you should be able to run Docker commands without `sudo`.

-----

### Step 4: Verify Docker Compose Installation

Modern Docker installation scripts, including the one we used, automatically install Docker Compose as a plugin. You don't need a separate installation step.

You can verify it's installed by checking its version:

```bash
docker compose version
```

You should see an output like `Docker Compose version v2.27.0`.

-----

### Conclusion

That's it\! You now have a fully functional Docker and Docker Compose environment on your Ubuntu server. You're ready to start containerizing your applications or deploy powerful tools like Appwrite, Portainer, or anything else that runs in a container.
