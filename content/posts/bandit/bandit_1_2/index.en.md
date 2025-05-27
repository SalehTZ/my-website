---
title: "Bandit Level 2: The Space Odyssey (and How to Tame It)"
subtitle: "Dealing with spaces in filenames: Because quoting is more than just telling stories."
date: 2025-05-26T09:08:45+03:30
lastmod: 2025-05-26T09:08:45+03:30
draft: false
author: "SalehTZ"
authorLink: "#"
description: "You've conquered the basics, but Bandit Level 2 introduces a classic Linux challenge: filenames with spaces! Learn how to 'quote' your way to victory and grab that elusive password."
license: ""
images: []

tags: ["Bandit", "OverTheWire", "Cybersecurity", "Linux", "Command Line", "Filesystem", "Beginner"]
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

## Introduction: The Unseen Enemy - The Space Character

Welcome back, intrepid terminal traveler! You've successfully navigated the entry point, found your first `readme`, and you're feeling pretty smug, aren't you? Good. Now prepare for **Bandit Level 2**, where the game throws a curveball so subtle, it might just make you question your life choices.

The challenge this time? **Spaces in filenames.** Yes, those innocent little gaps between words that make file names readable to us humans. To a Linux shell, however, a space is a command separator, a signal to treat the next word as a new argument or command. It's like telling your dog to "fetch the ball, stick, and frisbee" – it knows you want three distinct things, not one giant item called "ball stick frisbee."

The level description for Bandit Level 2 states:

> *The password for the next level is stored in a file called **spaces in this filename** located in the home directory.*

"Spaces in this filename." Cute, right? Let's tackle this seemingly simple, yet surprisingly tricky, obstacle.

## Level 2: The Case of the Spaced-Out Filename

You've just logged in as `bandit2` (using the password you found in Level 1). Your first instinct, as always, should be:

```bash
ls
```

And what do you see?

```
spaces in this filename
```

Looks harmless enough. Now, remembering your newfound `cat` wisdom from Level 1, you confidently type:

```bash
cat spaces in this filename
```

...and the terminal stares back at you with a confused expression. You might get an error like:

```
cat: spaces: No such file or directory
cat: in: No such file or directory
cat: this: No such file or directory
cat: filename: No such file or directory
```

"What?! I *just* saw it there!" you exclaim, shaking your fist at the screen. This, my friend, is the shell telling you, "I don't know what 'spaces' is, or 'in', or 'this', or 'filename' as separate entities." Because to the shell, each word separated by a space is a different argument. It's trying to `cat` four different (non-existent) files!

### The Solution: Taming the Spaces with Quotes (or Backslashes)

To make the shell understand that "spaces in this filename" is *one single filename*, you need to tell it explicitly. You have two primary ways to do this:

#### Method 1: The Double Quotes ("") - Your New Best Friend

The easiest and most common way to handle spaces (and other special characters) in filenames is to wrap the entire filename in double quotes. This tells the shell, "Hey, everything inside these quotes is part of the same argument, even if there are spaces."

```bash
cat "spaces in this filename"
```

Hit Enter, and *bam!* The password for `bandit3` is revealed. It's like magic, but it's just proper quoting.

#### Method 2: The Backslash (`\`) - The Escape Artist

Another way to handle spaces is to "escape" each individual space with a backslash (`\`). The backslash tells the shell to treat the character immediately following it literally, rather than interpreting it as a special command.

```bash
cat spaces\ in\ this\ filename
```

This also works perfectly and will spit out the password. It's a bit more typing, which is why double quotes are generally preferred for multi-spaced filenames, but it's good to know both methods.

### Moving Onward:

Copy that password for `bandit3`! Then, as before:

```bash
exit
```

And connect to the next level:

```bash
ssh bandit3@bandit.labs.overthewire.org -p 2220
```

Enter your newly acquired password, and you're in! Congratulations, you've conquered the space-time continuum (or at least, spaces in filenames).

## Conclusion: Quotes - More Than Just for Shakespeare!

You've successfully navigated Bandit Level 2, learning a crucial lesson about how shells interpret commands and arguments. You've mastered:

* Why spaces in filenames can be a pain.
* How to properly quote filenames using double quotes (`""`).
* The alternative method of escaping spaces with backslashes (`\ `).

This skill is incredibly important not just in wargames, but in real-world Linux environments where filenames aren't always as neat and tidy as you'd like.

Next up, Bandit Level 3! Who knows what linguistic gymnastics or file system trickery awaits? Keep those terminal fingers nimble!

----

**[Continue to Bandit Level 3\!](https://salehtz.ir/bandit_2_3/)**
