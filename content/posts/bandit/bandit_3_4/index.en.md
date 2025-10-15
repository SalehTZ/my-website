---
title: "Bandit Level 3 to 4: Uncovering Dot-Prefixed Files"
subtitle: "The password is in a file that starts with '...'. A guide to finding hidden files in Linux with 'ls -a'."
date: 2025-05-26T09:08:57+03:30
lastmod: 2025-10-15T12:48:30+02:00
draft: false
author: "SalehTZ"
authorLink: "#"
description: "Follow this step-by-step guide for OverTheWire's Bandit Level 3 to 4. Learn how files starting with dots, like '...Hiding-From-You', are hidden by default and how to reveal them using the 'ls -a' command."
license: ""
images: []

tags: ["Bandit", "OverTheWire", "Cybersecurity", "Linux", "Command Line", "CTF", "Hidden Files", "ls"]
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

After navigating tricky filenames, Bandit Level 3 to 4 introduces us to a core concept of Linux/Unix-like operating systems: hidden files. These files, often used for configuration, don't show up in normal directory listings simply because their names begin with a dot.

This level will teach you how to look past the obvious and uncover the secrets hidden within a directory.

## The Challenge: Level 3 Goal

The official goal on the OverTheWire website states:

> The password for the next level is stored in a hidden file in the **inhere** directory.

The clues are clear: we need to find a directory named `inhere` and then find a hidden file inside it. In this case, the file is named `...Hiding-From-You`.

## Step-by-Step Walkthrough

Let's find that hidden password.

### Step 1: Log into `bandit3`

Use the password you acquired from the previous level to SSH into the `bandit3` user.

```bash
ssh bandit3@bandit.labs.overthewire.org -p 2220
````

### Step 2: Find the `inhere` Directory

Once logged in, list the contents of the home directory to locate the `inhere` directory.

```bash
ls
```

You should see the `inhere` directory listed.

```
inhere
```

### Step 3: Change Into the Directory

Now, navigate into the `inhere` directory using the `cd` (change directory) command.

```bash
cd inhere
```

### Step 4: The Mystery of the Empty Directory

If you try listing the files with the standard `ls` command, it will look like the directory is empty.

```bash
ls
```

The command returns nothing. This is the core of the challenge. The file is hidden because its name starts with a `.` character.

### Step 5: Reveal the Hidden File

In Linux, any file or directory that starts with a dot is considered hidden. To see these files, you must use the `-a` (all) flag with the `ls` command.

```bash
ls -a
```

The output will now show the hidden file, which normal `ls` did not.

```
.  ..  ...Hiding-From-You
```

We can see the file we're looking for: `...Hiding-From-You`. The `.` and `..` entries represent the current and parent directories, respectively.

### Step 6: Read the Password

Now that we know the filename, we can use the `cat` command to read its contents and get the password for the next level.

```bash
cat ...Hiding-From-You
```

The command will print the password for `bandit4` to your terminal.

```
# yours might be different
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```

## Key Concepts Learned

1. **Hidden Files (Dotfiles):** Files and directories starting with a `.` are hidden from standard `ls` listings. This is a convention, not a security feature.
2. **The `ls -a` Command:** The `-a` or `--all` flag for the `ls` command is essential for viewing all entries in a directory, including hidden ones.
3. **Directory Navigation:** The `cd` command is a fundamental tool for moving through the filesystem.

## Conclusion

Congratulations\! You've successfully uncovered a hidden file and captured the password for `bandit4`. This skill is crucial, as you'll often need to inspect or modify hidden configuration files when working with Linux systems.

Proceed to the next level with your newfound knowledge\!
