---
title: "Bandit Level 5: The Goldilocks File Hunt (Just Right!)"
subtitle: "Using `find` to pinpoint the perfect file by size, readability, and permission."
date: 2025-05-26T09:09:12+03:30
lastmod: 2025-05-26T09:09:12+03:30
draft: false
author: "SalehTZ"
authorLink: "#"
description: "Bandit Level 5 challenges your file-finding skills! Learn to use the powerful `find` command to locate a file with specific properties: human-readable, a precise size, and not executable."
license: ""
images: []

tags: ["Bandit", "OverTheWire", "Cybersecurity", "Linux", "Command Line", "Find Command", "File Permissions", "File Size", "Beginner"]
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

## Introduction: When "Just Find It" Isn't Enough

You've successfully navigated directories, unmasked hidden files, and even differentiated between plain text and binary goo. You're becoming quite the file system connoisseur! But **Bandit Level 5** ups the ante. It's no longer about *just* finding a file; it's about finding the *right* file, based on a very specific shopping list of properties.

The level description for Bandit Level 5 reads:

> *The password for the next level is stored in a file somewhere in the **inhere** directory and has the following properties:*
> * human-readable
> * 1033 bytes in size
> * not executable

"Human-readable" we know. "1033 bytes"? That's a very precise size. "Not executable"? Ah, file permissions are joining the party! This is a job for the Swiss Army knife of Linux commands: `find`.

---

## Level 5: The `find` Command - Your Digital Bloodhound

You've logged in as `bandit5` with the password from Level 4. A quick `ls` will show you our familiar entry point:

```bash
ls
```

Output:

```
inhere
```

Naturally, we'll `cd` into that `inhere` directory:

```bash
cd inhere
```

Now, if you try an `ls` or even `ls -a`, you'll likely see a massive number of files and directories. Manually checking each one for human-readability, size, and executability would be a tedious, soul-crushing task. This is exactly what the `find` command was made for.

### The `find` Command: Filtering Like a Pro

The `find` command is incredibly powerful for locating files and directories based on various criteria. You tell it *where* to look and *what* properties the files should have.

Here's the breakdown of the options we'll use for Bandit Level 5:

* **`.`**: This tells `find` to start searching in the *current directory* (and its subdirectories).
* **`-type f`**: We're looking for a *file*, not a directory. (`f` for file).
* **`-size 1033c`**: We want a file exactly `1033` bytes in size. The `c` stands for bytes.
* **`-readable`**: This filters for files that are human-readable (like plain text). This is often equivalent to `grep`ping for `ASCII text` from `file` output, but `find` has a built-in option!
* **`!-executable`**: This is a bit tricky. The `!` negates the next condition. So, `! -executable` means "NOT executable." This is important because the password file should *not* be a program.

Putting it all together, the command looks like this:

```bash
find . -type f -size 1033c -readable ! -executable
```

Type this into your terminal and press Enter. If you've got it right, `find` will spit out the name of the file that matches *all* these criteria!

It might look something like:

```
./maybehereisyourpasswordfile
```

Or just `f77777777777777777777777` or similar. The actual name doesn't matter as long as it's the *only* one.

### The Grand Reveal: `cat`ting the Perfect File

Once `find` has done its detective work and presented you with the path to the perfect file, you know what to do! Use `cat` to display its contents:

```bash
cat ./maybehereisyourpasswordfile # (Use the actual filename find gave you!)
```

And there it is! The password for `bandit6`. Copy that precious string!

### Moving Onward:

Got that password? Awesome!

```bash
exit
```

Then, you know the drill – connect to the next level:

```bash
ssh bandit6@bandit.labs.overthewire.org -p 2220
```

Enter your freshly found password, and just like that, you're on `bandit6`. You're mastering the art of targeted file discovery!

---

## Conclusion: `find` - The Power to Pinpoint

You've successfully conquered Bandit Level 5, adding a seriously powerful tool to your Linux command-line arsenal:

* The versatile and incredibly useful `find` command, allowing you to locate files based on multiple criteria (type, size, readability, executability, and much more!).
* A deeper understanding of file properties and permissions.

The `find` command is invaluable for system administration, scripting, and of course, wargames. Mastering it will save you countless hours of manual searching.

Next time, we'll dive into Bandit Level 6, where the challenges continue to evolve. Keep that `find` command handy!

---

## SPOILER ALERT: Short Answer for Bandit Level 5

1.  Log in as `bandit5`.
2.  Change directory: `cd inhere`
3.  Use the `find` command with the specified criteria:
    ```bash
    find . -type f -size 1033c -readable ! -executable
    ```
4.  `cat` the filename that `find` outputs (e.g., `cat ./the_found_filename`).
5.  The output is the password for `bandit6`.

----

**[Continue to Bandit Level 6\!](https://salehtz.ir/bandit_5_6/)**
