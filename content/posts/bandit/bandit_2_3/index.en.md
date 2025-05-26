---
title: "Bandit Level 3: The Case of the Invisible File (and How to Find It)"
subtitle: "Mastering `cd` and `ls -a`: Because sometimes, files just want to play hide-and-seek."
date: 2025-05-26T09:08:51+03:30
lastmod: 2025-05-26T09:08:51+03:30
draft: false
author: "SalehTZ"
authorLink: "#"
description: "Bandit Level 3 asks you to find a hidden file. Learn how to navigate directories with `cd` and reveal sneaky hidden files using `ls -a`. It's like finding a secret passage in a video game!"
license: ""
images: []

tags: ["Bandit", "OverTheWire", "Cybersecurity", "Linux", "Command Line", "Hidden Files", "Directories", "Beginner"]
categories: ["Cybersecurity", "CTF", "Bandit", "OverTheWire"]
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

<!--more-->

## Introduction: The Secret Life of Files

You've mastered logging in, and you've tamed the wild beast of spaces in filenames. Bravo! Now, prepare yourself for **Bandit Level 3**, where the game gets a bit more... covert. This level introduces you to **hidden files** and how to actually *move around* the file system.

The level description from OverTheWire hints:

> *The password for the next level is stored in a hidden file in the **inhere** directory.*

"Hidden file"? "Inhere directory"? Sounds like someone's playing games with us. Let's find out how to peek behind the digital curtains.

---

## Level 3: Navigating the Maze and Uncovering Secrets

You've just logged in as `bandit3` using the password from Level 2. As always, your first move should be:

```bash
ls
```

And what do you see?

```
inhere
```

Aha! There's our mysterious `inhere` directory. But wait, it's a *directory*, not a file. How do we get inside?

### Command 1: `cd` (Change Directory - Your Digital Teleportation)

The `cd` command stands for "change directory." It's how you move from one folder (or directory) to another in the Linux file system. Think of it like walking into a different room in a house.

To enter the `inhere` directory, simply type:

```bash
cd inhere
```

Hit Enter. Your command prompt might change, or you might just not get an error, indicating success! You've successfully "teleported" into `inhere`.

### Command 2: `ls -a` (List All - Revealing the Invisible)

Now that you're *inside* the `inhere` directory, your next logical step is to see what's in there. So you type:

```bash
ls
```

...and you see... nothing. Or maybe just an empty line. "What?! There's supposed to be a hidden file!" you scream internally.

This is the trick of hidden files. In Linux (and Unix-like systems), any file or directory whose name starts with a **dot (`.`)** is considered "hidden" by default. This means `ls` won't show it unless you explicitly ask it to. It's not truly secret or encrypted; it's just politely tucked away.

To reveal *all* files, including the hidden ones, you need to use the `-a` (for "all") option with `ls`:

```bash
ls -a
```

Now, what do you see?

```
.  ..  .hidden
```

Success! You see `.` (which represents the current directory), `..` (which represents the parent directory), and our elusive friend: **`.hidden`**. That's our target!

### Command 3: `cat` (The Grand Reveal)

Now that you've found the hidden file, you know what to do! Use `cat` to display its contents:

```bash
cat .hidden
```

And just like that, the password for `bandit4` will appear on your screen. Copy it down carefully!

### Moving Onward:

Once you have that glorious password:

```bash
exit
```

Then, armed with your new password, connect to the next level:

```bash
ssh bandit4@bandit.labs.overthewire.org -p 2220
```

Enter the password, and *boom!* You're logged into `bandit4`. You're becoming a seasoned explorer of the Linux file system!

---


## Conclusion: Nothing is Truly Hidden (If You Know the Right Command)

You've successfully conquered Bandit Level 3, adding crucial tools to your command-line arsenal:

* The mighty `cd` for navigating directories like a pro.
* The essential `ls -a` for unmasking hidden files.

This knowledge is invaluable for understanding how files are structured and managed in Linux, and for finding things that aren't immediately obvious.

Next time, we'll dive into Bandit Level 4, where we'll encounter even more interesting challenges. Until then, keep exploring!


## TL;DR -> Short Answer
**TL;DR:** To solve Bandit Level 3, use `cd inhere` to enter the directory, then `ls -a` to list all files (including hidden ones). `cat` the hidden file (which starts with a `.`) to get the password.