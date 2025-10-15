---
title: "Bandit Level 4 to 5: Finding Human-Readable Text in a Data Maze"
subtitle: "Using the 'file' command to distinguish ASCII text from binary data and locate the password file."
date: 2025-05-26T09:09:12+03:30
lastmod: 2025-10-15T12:52:43+02:00
draft: false
author: "SalehTZ"
authorLink: "#"
description: "A complete guide for solving OverTheWire Bandit Level 4 to 5. Learn how to use the powerful 'file' command to analyze a directory of files and identify the single human-readable ASCII text file containing the password."
license: ""
images: []

tags: ["Bandit", "OverTheWire", "Cybersecurity", "Linux", "Command Line", "CTF", "file command", "ASCII", "Binary"]
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

In the last level, we learned to find files that were intentionally hidden from view. Now, in Bandit Level 4 to 5, the challenge is different. The password file isn't hidden, but it's lost in a crowd of other files that are unreadable to humans.

This level teaches us a critical skill: how to programmatically identify the *type* of data a file contains, allowing us to quickly find the needle in the haystack.

## The Challenge: Level 4 Goal

The official goal on the OverTheWire website states:

> The password for the next level is stored in the only human-readable file in the **inhere** directory.

The key phrase here is "human-readable." We need a tool that can tell us which file is plain text and which ones are binary data.

## Step-by-Step Walkthrough

Let's dive in and analyze these files.

### Step 1: Log into `bandit4`

Use the password you found in the `.hidden` file from the previous level to SSH into `bandit4`.

```bash
ssh bandit4@bandit.labs.overthewire.org -p 2220
````

### Step 2: Investigate the `inhere` Directory

First, `cd` into the `inhere` directory as instructed.

```bash
cd inhere
```

Now, list its contents.

```bash
ls
```

You'll see a list of ten files, all with similar names.

```bash
-file00  -file02  -file04  -file06  -file08
-file01  -file03  -file05  -file07  -file09
```

### Step 3: The Problem of Trial and Error

You could try to `cat` each file one by one, but you'll quickly run into a problem. Most of the files contain binary data, and trying to print them to your terminal will result in a mess of garbled characters and might even cause your terminal to behave strangely.

```bash
cat ./-file00 
# ^<83>^C^C^@^B^@^ ... (gibberish output)
```

We need a smarter approach.

### Step 4: The Solution - The `file` Command

The perfect tool for this job is the `file` command. It examines a file's contents and tells you what type of file it is. We can use a wildcard (`*`) to run the command on all the files in the directory at once.

```bash
file ./*
```

This command will produce a clean, informative list.

```bash
-file00: data
-file01: data
-file02: data
-file03: data
-file04: data
-file05: data
-file06: data
-file07: ASCII text
-file08: data
-file09: data
```

The output clearly shows that `-file07` is "ASCII text," which is the human-readable file we're looking for\!

### Step 5: Retrieve the Password

Now that we have identified the correct file, we can safely use `cat` to read its contents. Since the filename starts with a hyphen, it's best practice to use the `./` prefix.

```bash
cat ./-file07
```

This will print the password for `bandit5`.

```bash
# yours might be different
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```

## Key Concepts Learned

1. **File Content Analysis:** We learned that you cannot rely on filenames or extensions to know what a file contains. The `file` command inspects the actual data to make a determination.
2. **ASCII vs. Binary Data:** This level provides a practical example of the difference between human-readable text (ASCII) and machine-readable data (binary).
3. **Shell Wildcards (`*`):** The asterisk is a powerful tool for applying a command to multiple files at once, saving significant time and effort.

## Conclusion

You've successfully sifted through a directory of miscellaneous data to find the one piece of useful information. The `file` command is an indispensable utility in cybersecurity and system administration for quickly understanding the contents of a filesystem.

Save the password and get ready for Level 5\!
