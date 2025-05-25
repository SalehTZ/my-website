---
title: "Bandit Level 0-1: Your First Steps into the Terminal Abyss (Don't Trip!)"
subtitle: "Connecting, `ls`, `cat`, and the thrilling realization you can now, like, *do stuff* in Linux."
date: 2025-05-24T08:50:46+03:30
lastmod: 2025-05-24T08:50:46+03:30
draft: false
author: "SalehTZ"
authorLink: ""
description: "Welcome to Bandit! In this post, we'll hilariously guide you through connecting to the OverTheWire Bandit server and conquering your first two levels. Prepare to type, to wonder, and to finally feel like a terminal wizard (kinda)."
license: ""
images: []

tags: ["Bandit", "OverTheWire", "Cybersecurity", "Linux", "SSH", "Command Line", "Beginner"]
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
mapbox:
  # ...
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

## Introduction: The Terminal - Your New Happy Place (Maybe?)

Ah, the command line. For some, it's a terrifying black box of cryptic commands. For others, it's a magical portal to total system control (and bragging rights). If you're reading this, you're probably somewhere on the "terrified but curious" spectrum. Good news! The **OverTheWire Bandit** wargame is here to hold your hand (gently, with occasional electric shocks) as you dip your toes into the magnificent ocean of Linux.

Bandit isn't just a game; it's a bootcamp for your brain, teaching you essential Linux commands and cybersecurity concepts without the fear of accidentally wiping your own hard drive (mostly). Each level is a puzzle, and the solution to that puzzle is the password to the next level. It's like a digital scavenger hunt, but instead of finding plastic eggs, you're finding strings of characters that unlock more command-line fun.

In this post, we'll conquer the very first hurdles: connecting to the Bandit server and breezing through Levels 0 and 1. Get ready to feel like a hacker (or at least someone who can proficiently type commands without looking them up every five seconds).

## Level 0: The Grand Entrance (SSH is Your Friend, Mostly)

The goal of Level 0 is deceptively simple: **just log in.** But for the uninitiated, even that can feel like trying to open a jar of pickles that's been sealed since the dawn of time.

Bandit uses `SSH` (Secure Shell) for connections. Think of SSH as a super-secure, encrypted tunnel that lets you control a remote computer as if you were sitting right in front of it. It's basically teleportation for your keyboard strokes.

### The Magic Command:

To connect to Bandit Level 0, you'll need a terminal (or command prompt for Windows users – though a proper terminal like Git Bash or WSL is highly recommended for less pain).

Open your terminal and type this incantation:

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

Let's break down this mystical phrase:

* `ssh`: "Hey, secure shell, let's connect!"
* `bandit0@bandit.labs.overthewire.org`: This is like saying "I want to log in as `bandit0` (the username) on the server located at `bandit.labs.overthewire.org`." It's their digital address.
* `-p 2220`: "And by the way, use port `2220`." Why not the default SSH port 22, you ask? Because life isn't always simple, and this is a wargame. It's like a secret knock.

### The Password Prompt:

When you hit Enter, the server might ask you about a "host key fingerprint" (it's basically verifying that you're connecting to the right server and not some imposter). Type `yes` and hit Enter.

Then, you'll be prompted for a password. The password for `bandit0` is, quite fittingly, `bandit0`.

**Pro-tip:** When you type a password into a Linux terminal, it typically *doesn't show any characters* (not even asterisks). This isn't a bug; it's a feature for security. Just type the password and press Enter. If you've entered it correctly, you'll be greeted by a glorious welcome message from OverTheWire!

```
Welcome to the OverTheWire Bandit Wargame!
...
bandit0@bandit:~$
```

Congratulations! You've successfully completed Level 0. Now, don't just sit there staring at the prompt like a deer in headlights. Let's move on!

## Level 1: The README to Success (And a Tiny Linux Lesson)

You're in! Now what? The goal of Level 1 is to find the password to the *next* level. The level description on the OverTheWire website (which you should **always** read for each level!) tells us:

> *The password for the next level is stored in a file called **readme** located in the home directory.*

Okay, so we're looking for a file named `readme`. But where are we? What files are even here? This is where your first essential Linux commands come into play.

### Command 1: `ls` (List Stuff, Literally)

The `ls` command is your best friend for seeing what's around. It "lists" the contents of the current directory. It's like opening your eyes in a new room.

Type:

```bash
ls
```

And behold! You should see something like:

```
readme
```

Aha! There it is. Just chilling, waiting for you.

### Command 2: `cat` (Concatenate and Display Files, Mostly Display)

Now that you've found the `readme` file, you need to *read* its contents to get the password. This is where the `cat` command comes in. `cat` stands for "concatenate," but its most common use is simply to display the content of a file to your terminal. It's like asking the file to whisper its secrets to you.

Type:

```bash
cat readme
```

And there it is! A string of seemingly random characters. That, my friend, is your password for `bandit1`. Copy it carefully! It will look something like `boJ9jbbUNNfktd78OOpsqOltutMc3MY1` (though it changes periodically, so don't just copy mine!).

### Moving Onward:

Once you have that glorious password, you're ready to exit this level and hop onto `bandit1`.

Type:

```bash
exit
```

This will log you out of the current SSH session. Now, using the password you just found, you'll connect to the next level:

```bash
ssh bandit1@bandit.labs.overthewire.org -p 2220
```

Enter the password you found, and *voilà!* You're logged into `bandit1`. You've officially ascended a level in the hacker game. Take a moment. Breathe it in. You just navigated a remote Linux system!

## Conclusion: You're Not Just Playing, You're Learning!

Congratulations, fledgling terminal warrior! You've successfully navigated the daunting landscape of SSH and tackled your first two Bandit levels. You've learned:

* How to connect to a remote server using `ssh` and a non-standard port.
* The magical power of `ls` to peek into directories.
* The invaluable `cat` command to reveal file contents.

Feeling good? You should be! This is just the beginning of your journey. Each level will introduce a new command or concept, building your cybersecurity toolkit piece by piece. Don't be afraid to experiment, to make mistakes (that's how we learn!), and to Google things you don't understand.

Keep that password handy, because next time, we'll be diving into Bandit Level 2, where things get... interesting. Until then, happy hacking (ethically, of course)!
