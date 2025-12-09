---
title: "Waydroid is FIRE: Native Android on Linux (X11 & Wayland Guide)"
subtitle: "How to run Android apps natively on Linux using Waydroid. Includes a guide for both X11 (using Weston) and Wayland users, plus Flutter development setup."
date: 2025-12-08T14:30:44+03:30
lastmod: 2025-12-08T14:30:44+03:30
draft: false
author: ""
authorLink: ""
description: ""
license: ""
images: []

tags: ["linux", "android", "waydroid", "kde", "weston", "flutter", "adb", "ubuntu"]
categories: ["Development", "Linux Tutorials"]

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


Imagine running Android apps on your Linux desktop—not in a slow, heavy emulator, but **natively**, sharing your kernel with the host. The performance is absolutely **FIRE**.

<!--more-->

That’s what [Waydroid](https://waydro.id/) does. It uses Linux namespaces (containers) to run a full Android system directly on your OS. I recently set it up to test Flutter applications, and the speed difference compared to the standard Android Studio emulator is night and day.

Here is how to get it running, whether you are on Wayland or X11.

## 1. Installation

The installation process is straightforward for most distributions. You can find the specific commands for your machine in the [official documentation here](https://docs.waydro.id/usage/install-on-desktops#ubuntu-debian-and-derivatives).

For **Ubuntu/Debian** users, it generally looks like this:

```bash
sudo apt install curl ca-certificates -y
curl https://repo.waydro.id | sudo bash
sudo apt install waydroid -y
````

Once installed, initialize it with:

```bash
sudo waydroid init
```

Then make sure you have Wayland Session enabled (Ubuntu 22.04+). See Below:

{{< bookmark
    url="https://linuxconfig.org/how-to-enable-disable-wayland-on-ubuntu-22-04-desktop"
    title="How to enable/disable Wayland on Ubuntu 22.04"
    desc="A step-by-step guide to switching between X11 and Wayland display servers on Ubuntu Jammy Jellyfish."
    image="https://upload.wikimedia.org/wikipedia/commons/e/e0/Ubuntu_logo_orange.png"
>}}

## 2\. Running Waydroid: X11 vs. Wayland

This is where things get interesting. Waydroid, as the name implies, **requires a Wayland display server**. Your experience depends on your current session type.

### Option A: You are already using Wayland

If you are running a modern desktop (GNOME on Wayland, KDE Plasma Wayland), you are in luck. Waydroid works out of the box.

Just run:

```bash
waydroid show-full-ui
```

The Android interface will appear directly on your desktop as a native window.

### Option B: You are using X11 (The Weston Workaround)

If you are on X11 (like many KDE/Nvidia users), running the command above will give you this error:
`[13:05:12] Wayland socket '/run/user/1000/wayland-0' doesn't exist`

To fix this, we use **Weston**, a nested Wayland compositor that runs inside an X11 window.

1. **Install Weston:** `sudo apt install weston` (or your distro's equivalent).
2. **Launch Weston:** Run `weston` in your terminal. A new window will pop up.
3. **Launch Waydroid:** Click the terminal icon *inside* the Weston window and run:

    ```bash
    waydroid show-full-ui
    ```

## 3\. Troubleshooting: The "Black Screen" on KDE

A common issue, especially for KDE or Nvidia users, is the window opening but remaining completely black. This is usually a GPU driver conflict.

The fix is to force software rendering (guest mode). It’s slightly slower but rock-solid:

```bash
waydroid session stop
waydroid prop set persist.waydroid.gpu_mode guest
waydroid show-full-ui
```

*(To revert to GPU mode later, replace `guest` with `host`)*.

## 4\. Developer Setup: Flutter & ADB

If you are a developer, you'll notice Waydroid doesn't automatically appear in `adb devices`. Since it runs in a container, it has its own IP address.

### Connect via TCP/IP

1. **Get the IP:** Run `sudo waydroid shell ip addr show eth0` and copy the IP (e.g., `192.168.240.112`).
2. **Connect:** Run `adb connect 192.168.240.112:5555`.
3. **Verify:** Run `flutter devices`. You should now see the Waydroid device listed\!

### Fix Flutter Hanging on Launch

If `flutter run` starts the app but hangs at "Waiting for VM Service port...", your host firewall is blocking the connection.

Allow traffic from the Waydroid subnet (adjust the IP to match yours):

```bash
# For UFW users (Ubuntu/Mint)
sudo ufw allow from 192.168.240.0/24
```

## Conclusion

Waydroid is a game-changer for Linux-based Android development. Whether you are natively on Wayland or bridging it via Weston on X11, the setup is worth the effort for the incredible performance boost.

Check out the [official Waydroid docs](https://docs.waydro.id/) for more advanced configuration\!
