---
title: "Bandit Level 8: The Unique Line Challenge (Meeting `sort` and `uniq`)"
subtitle: "Finding the one-of-a-kind password: Because sometimes, individuality is key."
date: 2025-05-27T09:01:22+03:30
lastmod: 2025-05-27T09:01:22+03:30
draft: false
author: "SalehTZ"
authorLink: "#"
description: "Bandit Level 8 challenges you to find a password that appears only once in a file. Learn how to combine the `sort` and `uniq` commands using the powerful pipe (`|`) to filter for unique lines."
license: ""
images: []

tags: ["Bandit", "OverTheWire", "Cybersecurity", "Linux", "Command Line"]
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

*(The short answer for this challenge is available at the end of this post.)*

---

## Introduction: When Repetition is a Problem

You've learned to `grep` for a specific word, which is incredibly useful. But what if the password isn't just *next to* a word, but is *defined* by its uniqueness? Welcome to **Bandit Level 8**, where you'll encounter a file filled with many lines, but only one of them holds the secret to the next level because it appears *only once*.

The level description for Bandit Level 8 tells us:

> *The password for the next level is stored in the file **data.txt** and is the only line that occurs once.*

This is a classic text processing puzzle! We need a way to filter out all the duplicate lines and show only the truly unique ones. For this, we'll introduce two new essential Linux commands: `sort` and `uniq`, and we'll chain them together using the mighty **pipe (`|`)**.

---

## Level 8: The Harmony of `sort` and `uniq` (Thanks to the Pipe)

You've just logged in as `bandit8` with the password from Level 7. Let's `ls` to confirm our file:

{{< highlight bash >}}
ls
{{< /highlight >}}

You'll see:

```
data.txt
```

Excellent. Now, if you `cat data.txt`, you'll probably see a very long list of lines, many of which are identical. Trying to manually find the unique one would be like finding a specific grain of sand on a beach with a magnifying glass.

### Command 1: `sort` (The Organizer)

The `sort` command, as its name implies, sorts lines of text alphabetically or numerically. Why do we need this? Because `uniq` works best on *sorted* input. Imagine you have a stack of identical papers mixed with one unique paper. If they're all shuffled, it's hard to find the unique one. If you sort them into piles of identical papers, the unique one will be in its own pile.

To sort `data.txt`, you'd use:

{{< highlight bash >}}
sort data.txt
{{< /highlight >}}

This will output all the lines from `data.txt`, but now they're in alphabetical order. Still a lot of lines, but now the duplicates are grouped together.

### Command 2: `uniq` (The Duplicates Destroyer)

The `uniq` command filters out (or counts) adjacent duplicate lines. This is why `sort` is crucial first! Without sorting, `uniq` would only catch duplicates that happen to be right next to each other.

The basic `uniq` command, when run on sorted input, will show each unique line, effectively removing consecutive duplicates. But we want lines that occur *only once*. For that, `uniq` has a special option: `-u`.

* `uniq`: Removes consecutive duplicate lines.
* `uniq -u`: Only prints unique lines (those that appear exactly once).

### The Pipe (`|`): Chaining Commands

Here's where the magic happens. The pipe symbol (`|`) takes the *output* of the command on its left and feeds it as the *input* to the command on its right. It's like a conveyor belt for data!

So, to solve this level, we'll:
1.  `sort` the `data.txt` file.
2.  **Pipe (`|`)** the sorted output to `uniq -u`.

The full command looks like this:

{{< highlight bash >}}
sort data.txt | uniq -u
{{< /highlight >}}

Type this into your terminal and press Enter. `sort` will process the file, send its output to `uniq -u`, and `uniq -u` will then print only the line that appeared exactly once.

That single line printed to your screen is the password for `bandit9`! Copy it carefully.

### Moving Onward:

Got that password? You're a text-processing wizard!

{{< highlight bash >}}
exit
{{< /highlight >}}

And finally, connect to the next challenge:

{{< highlight bash >}}
ssh bandit9@bandit.labs.overthewire.org -p 2220
{{< /highlight >}}

Enter your hard-won password, and you're logged into `bandit9`. You've truly mastered the art of extracting unique information!

---

## Conclusion: The Power of Chained Commands

You've successfully conquered Bandit Level 8, adding powerful text processing tools to your Linux command-line arsenal:

* The **`sort`** command for organizing textual data.
* The **`uniq`** command (especially with `-u`) for finding unique lines.
* The incredibly versatile **pipe (`|`)** for chaining commands together, creating powerful data workflows.

Understanding how to combine commands with pipes is a fundamental skill in Linux, enabling you to perform complex operations with simple, concise commands.

---

## SPOILER ALERT: Short Answer for Bandit Level 8

1.  Log in as `bandit8`.
2.  Combine `sort` and `uniq -u` using a pipe:
    {{< highlight bash >}}
    sort data.txt | uniq -u
    {{< /highlight >}}
3.  The single line of output is the password for `bandit9`.
4.  Copy the password.

----

**[Continue to Bandit Level 9!](https://salehtz.ir/bandit_8_9/)**
