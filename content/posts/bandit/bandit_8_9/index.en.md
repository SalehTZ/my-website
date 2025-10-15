---
title: "Bandit Level 8 to 9: Finding the Unique Line"
subtitle: "Harnessing the power of 'sort', 'uniq', and 'grep' to isolate the one-of-a-kind password."
date: 2025-05-27T09:01:30+03:30
lastmod: 2025-10-15T13:30:55+02:00
draft: false
author: "SalehTZ"
authorLink: "#"
description: "Solve Bandit Level 8 to 9 by creating a powerful command-line pipeline. Learn to use 'sort', 'uniq -c', and 'grep' to find the only line that appears just once in a large dataset."
license: ""
images: []

tags: ["Bandit", "OverTheWire", "Cybersecurity", "Linux", "Command Line", "CTF", "sort", "uniq", "grep", "pipeline"]
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

In the last level, we used `grep` to find a known pattern in a file. For Bandit Level 8 to 9, the challenge is more abstract: we need to find a line that is unique within a file filled with duplicates. The password isn't next to a keyword; the password *is* the unique line itself.

This requires us to move beyond single commands and learn how to create a **pipeline**, chaining multiple tools together to perform a complex data analysis task in a single line.

## The Challenge: Level 8 Goal

The level's objective is as follows:

> The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once.

Our task is to count the occurrences of every line and find the one with a count of 1.

## Step-by-Step Walkthrough

Let's build a powerful command-line pipeline to solve this.

### Step 1: Log into `bandit8`

Use the password found with `grep` in the previous level to SSH into the `bandit8` user.

```bash
ssh bandit8@bandit.labs.overthewire.org -p 2220
````

### Step 2: Examine the File

A quick `ls` shows our target, `data.txt`. Viewing the first few lines with `head data.txt` will reveal that the file contains many repeated lines of text. A manual search is out of the question.

### Step 3: Building the Pipeline

We will solve this by chaining three commands together using the pipe (`|`) operator. The pipe sends the output of one command to be the input of the next.

#### Part 1: Sorting the Data

The key to finding unique lines is the `uniq` command, but it has one important requirement: it can only detect duplicates that are **adjacent** to each other. Therefore, our first step must be to sort the file.

```bash
sort data.txt
```

This command reads `data.txt` and outputs a new version where all identical lines are grouped together.

#### Part 2: Counting the Unique Lines

Now that the data is sorted, we can pipe it to `uniq`. We'll use the `-c` flag, which tells `uniq` to prefix each line with its frequency count.

```bash
sort data.txt | uniq -c
```

The output of this command will look something like this:

```bash
      8 EN632PlfYiZBNKiN9K9tQ11RkR2nN13J
     10 Fv430J02120232120002131230230213
      5 G3020213Fv430J021202321200021312
...
      1 UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
...
```

#### Part 3: Filtering for the One

We are almost there\! The list is now perfectly formatted for our final step. We just need to find the line that has a count of `1`. This is a job for our old friend, `grep`. We can pipe the output of our `sort | uniq` chain directly into `grep`.

We will search for lines that start with a `1` followed by a space.

```bash
sort data.txt | uniq -c | grep " 1 "
```

### Step 4: Final Command and Output

Executing the full pipeline gives us exactly what we need: the one unique line and its count.

```bash
      1 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

The password for `bandit9` is the text part of that line.

## Key Concepts Learned

1. **Command-Line Pipelines (`|`):** This is the most crucial concept of this level. The pipe operator is the glue that allows you to combine simple, specialized tools (`sort`, `uniq`, `grep`) to perform complex tasks.
2. **`sort`:** An essential utility for ordering data, often used as a preparatory step for other commands like `uniq`.
3. **`uniq -c`:** A powerful command for counting occurrences of adjacent lines in a sorted stream of data.
4. **Data Munging:** This entire process is a form of "data munging" or "data wrangling"—the process of cleaning, transforming, and filtering data to make it useful.

## Conclusion

Congratulations\! You've just created and executed a sophisticated data processing pipeline, a skill that is fundamental to working effectively on the command line. This pattern of `sort | uniq -c` is extremely common for analyzing log files and other text-based data.

Save your password and get ready for the next challenge\!

[Bandit 10](https://salehtz.ir/bandit_9_10)
