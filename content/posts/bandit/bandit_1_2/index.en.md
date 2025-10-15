---
title: "Bandit Level 1 to 2: The Case of the Hyphen File"
subtitle: "Learn how to access files with special names like '-' by specifying their path. A classic Linux command-line trick."
date: 2025-05-24T08:50:46+03:30
lastmod: 2025-10-15T12:11:25+02:00
draft: false
author: "SalehTZ"
authorLink: "#"
description: "A clear walkthrough for solving the OverTheWire Bandit wargame from Level 1 to Level 2. Learn how to handle filenames that start with a special character like a hyphen (-) using './' to specify the path."
license: ""
images: []

tags: ["Bandit", "OverTheWire", "Cybersecurity", "Linux", "Command Line", "CTF", "File Path"]
categories: ["Cybersecurity", "CTF"]

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
share:
  enable: true
comment:
  enable: true
library:
  css:
  js:
seo:
  images: []
---

## Introduction

Welcome back to the OverTheWire Bandit wargame series! If you've just solved Level 0 -> 1, congratulations on taking your first step. In this guide, we'll tackle the next challenge: finding the password to log into `bandit2`.

This level introduces a common and important concept in the Linux command line: how to handle files with names that can be confused with command-line options. Let's dive in.

## The Challenge: Level 1 Goal

The official goal on the OverTheWire website states:

> The password for the next level is stored in a file called **-** located in the home directory.

The key here is the filename: it's a single hyphen (`-`). This is where the trick lies.

## Step-by-Step Walkthrough

Let's work through this puzzle step by step.

### Step 1: Log into `bandit1`

First, use the password you obtained from the previous level to SSH into the `bandit1` user.

```bash
ssh bandit1@bandit.labs.overthewire.org -p 2220
````

Enter the password for `bandit1` when prompted.

### Step 2: Explore the Home Directory

Once you're logged in, the first thing to do is list the files in the current directory to see what we're working with. The `ls -la` command is perfect for this as it shows all files, including hidden ones, with details.

```bash
bandit1@bandit:~$ ls -la
total 20
drwxr-xr-x  2 root    root    4096 Oct 15 10:00 .
drwxr-xr-x 31 root    root    4096 Oct 15 10:00 ..
-rw-r--r--  1 root    root    220 May 15  2017 .bash_logout
-rw-r--r--  1 root    root   3771 May 15  2017 .bashrc
-rw-r--r--  1 root    root    675 May 15  2017 .profile
-r--r-----  1 bandit2 bandit1   33 Oct 15 10:00 -
```

As promised by the level goal, we can see a file named `-`.

### Step 3: The Problem with `cat`

Our first instinct is to use the `cat` command to read the file's content. Let's try it:

```bash
cat -
```

You'll notice that the command doesn't return the password. Instead, the cursor just moves to a new, blank line. This is because many command-line tools, including `cat`, interpret a lone hyphen (`-`) as an argument representing **standard input (stdin)**. Essentially, the `cat` command is now waiting for you to type something.

You can exit this waiting state by pressing `Ctrl+C`.

### Step 4: The Solution - Specifying the Path

So, how do we tell the shell that we mean the *file* named `-` and not the *argument* for standard input? We need to be more specific with the file's path.

In Linux, `./` is a reference to the current directory. By prepending the filename with `./`, we are giving the `cat` command an unambiguous path to the file.

Let's try that:

```bash
cat ./-
```

Success\! The command will now print the contents of the file, which is the password for the next level.

```bash
# yours might be different 
263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```

**Alternative Solution:** You can also use the full path to the file:

```bash
cat /home/bandit1/-
```

This works for the exact same reason—it provides a clear, unambiguous path to the file.

## Key Concepts Learned

This level is simple but teaches a fundamental lesson about the Linux command line:

1. **Special Characters in Filenames:** Files can have names that include characters the shell interprets in special ways (like `-`, `*`, `|`, `>`).
2. **Disambiguation with Paths:** You can force the shell to treat a special filename as a literal file by providing a more explicit path, such as `./filename` (for the current directory) or `/path/to/your/filename` (an absolute path).
3. **Standard Input (`stdin`):** The single hyphen (`-`) is a conventional way to tell a program to read from standard input instead of a file.

## Frequently Asked Questions (FAQ)

**Q: Why does `cat -` wait for input?**
A: The `-` is a POSIX convention that tells a program to read data from standard input (the keyboard, by default) rather than from a file argument.

**Q: What exactly does `./` mean?**
A: In Linux/Unix systems, `.` refers to the current working directory, and `..` refers to the parent directory. So `./-` is the explicit path to a file named `-` located in your current directory.

## Conclusion

And that's it\! You've successfully navigated a classic command-line trick and captured the password for `bandit2`. Make sure to save the password securely.

You're now ready to log out of `bandit1` and tackle the next challenge. Good luck with Level 2 -\> 3\!
