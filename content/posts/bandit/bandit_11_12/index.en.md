---
title: "Bandit Level 11 to 12: Cracking the Caesar Cipher (ROT13)"
subtitle: "Using the 'tr' command to rotate text back to its original form. A lesson in classic, simple ciphers."
date: 2025-10-15T13:45:10+02:00
lastmod: 2025-10-15T13:45:10+02:00
draft: false
author: "SalehTZ"
authorLink: "#"
description: "A detailed guide to solving Bandit Level 11 to 12. Learn what a Caesar cipher (specifically ROT13) is and how to reverse it using the powerful 'tr' (translate) command to find the password."
license: ""
images: []

tags: ["Bandit", "OverTheWire", "Cybersecurity", "Linux", "Command Line", "CTF", "Caesar Cipher", "ROT13", "tr"]
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

After decoding Base64, we now face our first real **cipher** in Bandit Level 11 to 12. The password isn't encoded for data transport; it's been deliberately obfuscated using a simple character-shifting algorithm.

This level introduces the **Caesar cipher**, one of the oldest and most well-known forms of encryption. We'll learn how to crack it with `tr`, a command-line utility for translating characters. 📜

***

## The Challenge: Level 11 Goal

The level's objective is described as follows:

> The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions.

This specific cipher, where letters are rotated by 13 places, is famously known as **ROT13**.

***

## Step-by-Step Walkthrough

Let's reverse time on this ancient cipher.

### Step 1: Log into `bandit11`

Use the decoded Base64 password from the last level to SSH into `bandit11`.

```bash
ssh bandit11@bandit.labs.overthewire.org -p 2220
````

### Step 2: Examine the Enciphered Text

Let's look at the contents of `data.txt`.

```bash
cat data.txt
```

The output will look like nonsense, but you might notice that it retains the structure of words and sentences. This is a hallmark of a simple substitution cipher.

```bash
Gur cnffjbeq vf PVTeKys4T5P-GUYf-fMRf-c0s-g2-m-L-f-
```

### Step 3: The Solution - The `tr` Command

The `tr` command is perfect for this. It "translates" or substitutes characters from one set to another. To reverse ROT13, we need to map every letter to the letter 13 places away from it in the alphabet.

The clever trick with ROT13 is that applying it twice gets you back to the original text. So, we just need to apply the same ROT13 transformation.

* The first half of the alphabet (`A-M` and `a-m`) maps to the second half (`N-Z` and `n-z`).
* The second half (`N-Z` and `n-z`) maps back to the first (`A-M` and `a-m`).

The `tr` command to do this is:

`tr 'INPUT_SET' 'OUTPUT_SET'`

Our input set is all the letters: `A-Za-z`
Our output set is the rotated version: `N-ZA-Mn-za-m`

Notice how `N-ZA-M` correctly maps `A-M` to `N-Z` and `N-Z` back to `A-M`.

### Step 4: Execute the Command

We will pipe the contents of the file into our `tr` command to perform the translation.

```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

The command will process the ROT13 text and print the original, readable English text, revealing the password for `bandit12`.

```bash
The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

--

## Key Concepts Learned

1. **Caesar Cipher/ROT13**: We learned what a simple substitution cipher is. ROT13 is often used in online forums to hide spoilers or punchlines, not for serious security.
2. **`tr` Command**: This is a powerful utility for performing character-based translations on streams of text. It's a fundamental tool for text manipulation on the command line.
3. **Character Sets and Ranges**: We used character ranges like `A-Z` and `a-m` to build our translation sets, a common feature in many command-line tools.

## Conclusion

You've successfully cracked your first cryptographic cipher\! While ROT13 is simple, the principles of analyzing and reversing transformations are fundamental to cybersecurity.

Save the password for `bandit12` and prepare for the next challenge\!
