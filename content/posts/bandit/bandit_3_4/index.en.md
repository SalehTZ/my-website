---
title: "Bandit Level 4: The Human-Readable Hunt (No, Not a Book Club)"
subtitle: "Using `file` to find the needle in the binary haystack, or: when `cat` fails you."
date: 2025-05-26T09:08:57+03:30
lastmod: 2025-05-26T09:08:57+03:30
draft: false
author: "SalehTZ"
authorLink: "#"
description: "Bandit Level 4 challenges you to find the *only* human-readable file. Learn how to use the `file` command to identify text files versus weird binary blobs and get that elusive password!"
license: ""
images: []

tags: ["Bandit", "OverTheWire", "Cybersecurity", "Linux", "Command Line", "File Types", "File Command", "Beginner"]
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

## Introduction: Not All Files Are Created Equal

You've navigated directories and uncovered hidden files. You're practically a digital Indiana Jones! But sometimes, even when you find the right file, it just stares back at you like a jumbled mess of symbols. Welcome to **Bandit Level 4**, where you'll learn that not every file is meant for human eyes, and how to tell the difference.

The description for Bandit Level 4 is intriguing:

> *The password for the next level is stored in the only human-readable file in the **inhere** directory.*

"Only human-readable file." This implies two things: first, we're going into an `inhere` directory (again!), and second, there will be multiple files, but only one we can actually understand. This is where your new best friend, the `file` command, comes in!

---

## Level 4: The Typist's Trial (and the `file` Command's Triumph)

You've just logged in as `bandit4` (using the password from Level 3). As per our ritual, `ls` first:

```bash
ls
```

You'll see:

```
inhere
```

Ah, the familiar `inhere` directory. Let's `cd` into it:

```bash
cd inhere
```

Now that you're inside, let's `ls` again to see what treasures (or trash) await:

```bash
ls
```

This time, you're greeted with a sight that might make your eyes glaze over:

```
-file00
-file01
-file02
-file03
-file04
-file05
-file06
-file07
-file08
-file09
```

Ten files! And by their names, they give no clue about their content. You might be tempted to just `cat` each one, hoping for the best.

```bash
cat -file00
```

...and you might get lucky on the first try, or you might see a screen full of garbled nonsense, a "binary file" warning, or even your terminal getting weird. This is because many of these are likely *binary* files – programs, images, or other data not designed for direct human reading. Trying to `cat` a binary file is like trying to read a textbook written in alien hieroglyphs.

### Command 1: `file` (The Digital Forensics Expert)

This is where the super-handy `file` command shines. It inspects a file and tells you its *type*. It's like a digital X-ray machine for your files.

Let's try it on `-file00`:

```bash
file -file00
```

You'll probably get something like:

```
-file00: data
```

"Data" is usually a polite way of saying "not human-readable text." You'll have to repeat this for each file, or get a bit more efficient.

### Becoming a `file` Master: Loop or Specificity

You could go through each file manually: `file -file00`, `file -file01`, etc. Eventually, one of them will stand out.

You'll see something like:

```bash
file -file07 # Or whatever the correct one is for your game instance
```

And the output might be:

```
-file07: ASCII text
```

**"ASCII text!"** That's your golden ticket! "ASCII text" means it's a plain text file, ready for your reading pleasure.

### Command 2: `cat` (The Final Reveal)

Once you've identified the "ASCII text" file (let's say it's `-file07` for this example, but it could be different in your game instance), you know what to do:

```bash
cat -file07
```

And there it is! The password for `bandit5` will be displayed. Copy it carefully!

### Moving Onward:

Got that password? Excellent!

```bash
exit
```

Then, connect to the next level:

```bash
ssh bandit5@bandit.labs.overthewire.org -p 2220
```

Enter your new password, and you're officially on `bandit5`! You're learning some serious detective skills!

---

## Conclusion: The `file` Command, Your New Best Friend

You've successfully navigated Bandit Level 4, adding another critical tool to your command-line arsenal:

* The versatile `file` command for identifying file types, separating the wheat from the binary chaff.
* The persistence to go through multiple files (or use a clever loop, for the overachievers!).

This knowledge isn't just for wargames; it's essential for understanding your Linux system and dealing with various file formats in the real world.

Next time, we'll dive into Bandit Level 5, which promises more thrilling file system antics!

---

## **SPOILER ALERT: Short Answer for Bandit Level 4**

1.  Log in as `bandit4`.
2.  Change directory: `cd inhere`
3.  Use the `file` command on each file to find the one that is `ASCII text`. Example: `file -file00`, `file -file01`, etc.
4.  Once found (e.g., `-file07`), use `cat` to display its content: `cat -file07`.
5.  The output is the password for `bandit5`.

----

**[Continue to Bandit Level 5\!](https://salehtz.ir/bandit_4_5/)**
