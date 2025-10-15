---
title: "Bandit Level 5 to 6: Mastering the 'find' Command"
subtitle: "Locating a file not by its name, but by its properties: size, type, and permissions. A deep dive into file metadata."
date: 2025-05-27T09:01:10+03:30
lastmod: 2025-10-15T12:59:15+02:00
draft: false
author: "SalehTZ"
authorLink: "#"
description: "A comprehensive guide to solving Bandit Level 5 to 6. Learn how to use the 'find' command to search for files based on specific metadata like size in bytes, file type, and permissions to uncover the hidden password."
license: ""
images: []

tags: ["Bandit", "OverTheWire", "Cybersecurity", "Linux", "Command Line", "CTF", "find command", "File Properties", "Metadata"]
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

So far in the Bandit wargame, we've found files based on their names or by looking in specific places. Level 5 to 6 takes a significant step up in complexity and introduces one of the most powerful file-searching utilities in Linux: the `find` command.

Instead of a name, we are given a set of properties—metadata—that describe the file containing the password. Our task is to translate these properties into a command that can sift through a maze of directories to find our target.

## The Challenge: Level 5 Goal

The official goal on the OverTheWire website gives us a list of clues:

> The password for the next level is stored in a file somewhere under the **inhere** directory and has all of the following properties:
>
> - human-readable
> - 1033 bytes in size
> - not executable

This is a job for a specialist tool. Manually checking every file would be impossible.

## Step-by-Step Walkthrough

Let's break down how to construct the perfect `find` command.

### Step 1: Log into `bandit5`

Use the password from the previous level to SSH into the `bandit5` user.

```bash
ssh bandit5@bandit.labs.overthewire.org -p 2220
````

### Step 2: Explore the `inhere` Directory

First, navigate into the `inhere` directory.

```bash
cd inhere
```

If you list the contents with `ls -l`, you'll see a large number of subdirectories. The password file is hidden somewhere inside one of them.

```bash
drwxr-xr-x 2 root root 4096 Oct 15 10:00 maybehere00
drwxr-xr-x 2 root root 4096 Oct 15 10:00 maybehere01
drwxr-xr-x 2 root root 4096 Oct 15 10:00 maybehere02
...
```

### Step 3: Building the `find` Command

The `find` command works by specifying a starting path followed by a series of tests (or "predicates"). Let's build our command based on the clues.

1. **Starting Path:** We want to search in the current directory (`.`).

      - `find .`

2. **File Size:** The file is `1033` bytes. The `-size` test is used for this. It's crucial to add a `c` at the end of the number to specify *bytes*, otherwise `find` assumes blocks.

      - `-size 1033c`

3. **Permissions:** The file is *not executable*. The `-executable` test checks for execute permissions. To negate a test, we use an exclamation mark `!`.

      - `! -executable`

4. **File Type:** We are looking for a file, not a directory.

      - `-type f`

### Step 4: Executing the Command

Now, let's combine all the pieces into a single, powerful command. We will run this from inside the `inhere` directory.

```bash
find . -type f -size 1033c ! -executable
```

The command will search recursively through all subdirectories and print the path of the one file that matches all our criteria.

```bash
./maybehere07/.file2
```

### Step 5: Verify and Retrieve the Password

We have a file path\! The final clue was that the file is "human-readable." We can quickly verify this with the `file` command.

```bash
file ./maybehere07/.file2
```

The output will confirm it's an ASCII text file.

```bash
./maybehere07/.file2: ASCII text
```

Now we can confidently read the file with `cat` to get the password for `bandit6`.

```bash
cat ./maybehere07/.file2
```

```bash
# yours might be different
HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```

## Key Concepts Learned

1. **The `find` Command:** This is the star of the show. We learned how to use it to search for files based on metadata rather than just names.
2. **File Metadata:** This level emphasizes the importance of metadata, which is data *about* data. We used size, type, and permissions to pinpoint our target.
3. **Command Tests and Negation:** We learned about specific tests like `-size`, `-type`, and `-executable`, and how to reverse their logic using `!`.

## Conclusion

You've successfully wielded one of Linux's most powerful command-line tools to solve a complex search puzzle. The ability to use `find` effectively is a massive step forward in your skills and will be invaluable in countless real-world scenarios.

Save your password and get ready for the next level\!
