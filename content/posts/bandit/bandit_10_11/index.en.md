---
title: "Bandit Level 10 to 11: The Secret Language of Base64"
subtitle: "Decoding data that's not meant to be secret, just unreadable. A practical guide to the 'base64' command."
date: 2025-10-15T13:42:18+02:00
lastmod: 2025-10-15T13:42:18+02:00
draft: false
author: "SalehTZ"
authorLink: "#"
description: "A complete walkthrough for solving Bandit Level 10 to 11. Learn what Base64 encoding is and how to use the 'base64 -d' command to decode a file and reveal the password for the next level."
license: ""
images: []

tags: ["Bandit", "OverTheWire", "Cybersecurity", "Linux", "Command Line", "CTF", "Base64", "Encoding", "Decoding"]
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

So far, we've located files, filtered their contents, and even extracted text from binary data. Bandit Level 10 to 11 introduces a new core concept: **data encoding**. The password is in plain sight, but it's been transformed into a different format.

This level teaches us how to recognize and reverse Base64 encoding, a common method for representing binary data as text. 🧑‍💻

***

## The Challenge: Level 10 Goal

The objective for this level is straightforward:

> The password for the next level is stored in the file **data.txt**, which contains base64 encoded data.

Our mission is to decode this data to reveal the original password.

***

## Step-by-Step Walkthrough

Let's translate this encoded message.

### Step 1: Log into `bandit10`

Use the password you extracted from the binary file in the last level to SSH into `bandit10`.

```bash
ssh bandit10@bandit.labs.overthewire.org -p 2220
````

### Step 2: Inspect the Data

First, take a look at the contents of `data.txt`.

```bash
cat data.txt
```

You will see a long, continuous string of letters, numbers, and perhaps a `+` or `/` sign, ending with an `=` sign. This is a classic signature of Base64 encoding.

```bash
VGhlIHBhc3N3b3JkIGlzIElGcjlCbzJDV0luV3p6N0dJU1JzT0VobEpERXNqcE5NCg==
```

### Step 3: The Solution - The `base64` Command

Linux provides a dedicated utility for this task: the `base64` command. By default, it encodes data. To decode, we use the `-d` (or `--decode`) flag.

We can tell the command to decode the file directly.

### Step 4: Execute the Decode Command

Run the `base64` command with the `-d` flag on our `data.txt` file.

```bash
base64 -d data.txt
```

The command will process the encoded string and print the original, human-readable text to the terminal, which is our password.

```bash
# Example Output
The password is IF...
```

**Alternative Method (Piping):**

You can also achieve the same result by piping the output of `cat` into the `base64` command.

```bash
cat data.txt | base64 -d
```

This is a common pattern when you're working with streams of data rather than just files.

-----

## Key Concepts Learned

1. **Base64 Encoding**: We learned that Base64 is an *encoding* scheme, not an *encryption* one. It is used to safely transmit binary data through text-only channels (like email or HTML). It is easily reversible by anyone.
2. **The `base64` Command**: This is the key tool. We learned how to use it and its most important flag, `-d`, for decoding.
3. **Recognizing Encoded Data**: You're now more familiar with what Base64-encoded text looks like, which is a valuable skill for identifying data types during a CTF or penetration test.

## Conclusion

You've successfully decoded your first piece of Base64 data\! This is a fundamental skill, as encoding is used everywhere in computing.

Save the password for `bandit11` and let's keep the momentum going\!
