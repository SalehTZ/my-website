---
title: "Bandit Level 9 to 10: Sifting Through Binary Data with 'strings'"
subtitle: "Extracting readable text from a compiled binary file to find the hidden password. The magic of the 'strings' command."
date: 2025-10-15T13:40:11+02:00
lastmod: 2025-10-15T13:40:11+02:00
draft: false
author: "SalehTZ"
authorLink: "#"
description: "A clear guide to solving Bandit Level 9 to 10. Learn how to use the 'strings' command to extract human-readable text from a binary file and pipe the output to 'grep' to find the password."
license: ""
images: []

tags: ["Bandit", "OverTheWire", "Cybersecurity", "Linux", "Command Line", "CTF", "strings", "Binary", "grep"]
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

In our journey so far, we've dealt mostly with plain text files. Bandit Level 9 to 10 introduces a new type of challenge: extracting information from a **binary file**. These files are compiled for machines, not humans, and reading them with `cat` results in a mess of unreadable characters.

This level teaches us how to use the `strings` command, a specialized tool for finding and printing the sequences of readable text embedded within binary data. 🧐

***

## The Challenge: Level 9 Goal

The objective for this level is stated as:

> The password for the next level is stored in the file **data.txt** and is one of the only human-readable strings, preceded by several ‘=’ characters.

We have a binary file, `data.txt`, and a clear hint: look for a line starting with equal signs.

***

## Step-by-Step Walkthrough

Let's pull that readable password out of the binary mess.

### Step 1: Log into `bandit9`

Use the unique password you discovered in the last level to SSH into `bandit9`.

```bash
ssh bandit9@bandit.labs.overthewire.org -p 2220
````

### Step 2: Investigate `data.txt`

First, confirm the file exists with `ls`. Then, to understand why we can't use `cat`, try it and see the garbled output.

```bash
ls
# data.txt
cat data.txt
# ^?ELF^B^A^A^@^@^@^@^@^@^@^@^@^C^@>^@^A^@^@^@p... (and so on)
```

This is clearly not a plain text file.

### Step 3: The Solution - The `strings` Command

The `strings` command is designed specifically for this situation. It scans any file (binary or not) and prints out sequences of printable characters it finds.

Let's run it on our file.

```bash
strings data.txt
```

This will produce a lot of output, but it's all readable text\! The password is in this list somewhere. Manually scrolling through it is possible, but not efficient.

```bash
/lib64/ld-linux-x86-64.so.2
libc.so.6
__libc_start_main
GLIBC_2.2.5
...
========== the password is ...
...
and lots of other text
```

### Step 4: Refining the Search with `grep`

We have a clue: the password is on a line preceded by several `=` characters. This is a perfect job for a `grep` pipeline\! We can send the output of `strings` directly to `grep` and filter for our pattern.

Let's search for lines containing `====`.

```bash
strings data.txt | grep "===="
```

This command elegantly filters all the junk and gives us the exact line we need.

```bash
========== the
========== password
E========== is
5========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

The password for `bandit10` is the string of text on that line.

--

## Key Concepts Learned

1. **Binary Files**: We learned that many files on a Linux system are not plain text and require special tools to inspect.
2. **`strings` Command**: This is the key takeaway. `strings` is an essential utility for initial analysis of an unknown binary file in forensics and reverse engineering to find embedded text like configuration details, error messages, or hidden passwords.
3. **Refining Pipelines**: This level reinforces the power of chaining commands. The `strings | grep` combination is a very common and effective pattern for finding specific information inside binary files.

## Conclusion

You've successfully peered into a binary file and extracted the secret hidden within. This skill is a stepping stone into more advanced topics like reverse engineering.

Copy the password you found and get ready to tackle the next level\!
