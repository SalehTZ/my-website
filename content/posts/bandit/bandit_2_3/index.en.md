---
title: "Bandit Level 2 to 3: The Ultimate Filename Challenge"
subtitle: "Combining techniques to handle filenames that start with '--' and contain spaces. The ultimate test of your command-line skills."
date: 2025-05-26T09:08:51+03:30
lastmod: 2025-10-15T12:35:32+02:00
draft: false
author: "SalehTZ"
authorLink: "#"
description: "A crucial walkthrough for OverTheWire's Bandit Level 2 to 3 with a new, tricky filename: '--spaces in this filename--'. Learn how to combine path specification and quoting to read files with complex names."
license: ""
images: []

tags: ["Bandit", "OverTheWire", "Cybersecurity", "Linux", "Command Line", "CTF", "File Names", "Spaces", "Special Characters"]
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

Welcome to the next stage of the Bandit wargame. This level, from 2 to 3, introduces a filename that brilliantly combines the challenges of the previous two levels. We'll need to handle a name that not only contains spaces but also starts with double hyphens (`--`), which can be confused for command options.

Mastering this challenge will solidify your understanding of how the Linux shell parses filenames and arguments.

## The Challenge: Level 2 Goal (Updated)

The password for the next level is stored in a file with a particularly tricky name located in the home directory:

> `--spaces in this filename--`

This filename is designed to break simple commands. Let's see how to tackle it.

## Step-by-Step Walkthrough

Here is the breakdown of how to solve this puzzle.

### Step 1: Log into `bandit2`

As always, start by using the password from the previous level to SSH into `bandit2`.

```bash
ssh bandit2@bandit.labs.overthewire.org -p 2220
````

### Step 2: Discover the File

List the files in the home directory to see our target.

```bash
bandit2@bandit:~$ ls
--spaces in this filename--
```

The file is there, just as described.

### Step 3: The Problem - Why Simple Methods Fail

Let's see what happens when we try the methods from the previous levels in isolation.

1. **Just using `cat`:** The shell will interpret `--spaces` as an option for `cat`, and the rest as separate arguments. This will fail.

    ```bash
    bandit2@bandit:~$ cat --spaces in this filename--
    cat: unrecognized option '--spaces'
    Try 'cat --help' for more information.
    ```

2. **Just using quotes:** Quoting correctly groups the name into a single argument, but that argument *still* starts with `--`. The `cat` command will still think it's an option, not a filename.

    ```bash
    bandit2@bandit:~$ cat "--spaces in this filename--"
    cat: unrecognized option '--spaces in this filename--'
    Try 'cat --help' for more information.
    ```

We need a more robust solution that handles both issues at once.

### Step 4: The Combined Solution

The key is to tell the shell explicitly that this is a file path, not an option.

#### Method 1: Specifying the Path (Recommended)

This is the most reliable method. By prepending `./` to the filename, you tell the shell to look for a file in the current directory (`.`). This prevents the `cat` command from ever interpreting the name as an option. We still need quotes to handle the spaces.

```bash
bandit2@bandit:~$ cat "./--spaces in this filename--"
```

This command works perfectly and will display the password for `bandit3`.

```bash
# yours might be different
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```

#### Method 2: Using the `--` Separator

Another powerful technique is to use a double-dash (`--`) as a separate argument *for the `cat` command*. This special argument tells `cat` (and most other standard Linux commands) to stop processing options. Every argument that comes after the `--` will be treated as a literal filename.

```bash
bandit2@bandit:~$ cat -- "--spaces in this filename--"
```

This also works flawlessly. The first `--` is for `cat`, and the quoted string is the filename we want to open.

## Key Concepts Learned

1. **Combining Techniques:** Real-world scenarios often require you to combine multiple command-line tricks. In this case, we used both path specification (`./`) and quoting (`"..."`) to solve the problem.
2. **The `--` Separator:** The utility of the `--` argument is reinforced here. It's a lifesaver for programmatic scripts or any situation where filenames might start with a hyphen.
3. **Order of Operations:** The shell first processes quotes and paths, then passes the resulting arguments to the command. The command then parses those arguments as options or filenames. Understanding this order is key.

## Conclusion

Excellent work\! By solving this level, you've demonstrated a strong grasp of how to handle even the most inconveniently named files. You've now earned the password for `bandit3`.

Keep this momentum going as you log out and move on to the next challenge\!
