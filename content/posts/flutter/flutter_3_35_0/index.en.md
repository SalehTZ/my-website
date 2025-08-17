---
title: "What's New in Flutter 3.35.0"
subtitle: "A Look at the Latest Features and I’m Proud to Be Part of It"
date: 2025-08-16T10:00:00+03:30
lastmod: 2025-08-16T10:00:00+03:30
draft: false
author: "SalehTZ"
authorLink: "https://github.com/salehtz"
description: "Exploring the exciting updates in Flutter's 3.35.0 release, from web improvements to a personal contribution."
license: ""
images: []

tags: ["flutter", "flutter update", "flutter 3.35"]
categories: ["Flutter", "Release Notes"]

featuredImage: "images/posts/flutter_3_35_0.png"
featuredImagePreview: "images/posts/flutter_3_35_0.png"

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
seo:
  images: ["images/posts/flutter_3_35_0.png"]
---


<!-- # Flutter 3.35.0 is Here — and I’m Proud to Be Part of It -->

Flutter 3.35.0 has officially landed, bringing a host of improvements across **web**, **desktop**, **accessibility**, **UI components**, and **tooling**.

<!--more-->

This release is special for me — **my own contribution** is included:

> **Fix text selection toolbar alignment for RTL languages**  
> by [@SalehTZ](https://github.com/SalehTZ) in [#169854](https://github.com/flutter/flutter/pull/169854)  

This fix improves the user experience for **right-to-left languages** like Persian, Arabic and others by ensuring the text selection toolbar aligns correctly, making editing smoother and more intuitive for millions of users.

---

## What’s New in Flutter 3.35.0

### **Web & Performance**

- **Stateful Hot Reload on Web** is now enabled by default — no experimental flag needed.
- **Wasm (WebAssembly) Dry Runs** added to JS builds, warning about compatibility issues before deployment.

### **Accessibility**

- **SemanticsLabelBuilder** for dynamic accessibility labels.
- **SliverEnsureSemantics** keeps off-screen slivers in the accessibility tree.
- Improved screen reader support for Android TalkBack, iOS VoiceOver, and custom painters.
- RTL improvements — including my contribution!

### **UI Enhancements**

- **DropdownMenuFormField** (Material 3) for form-ready dropdowns.
- **Scrollable NavigationRail** and customizable NavigationDrawer headers/footers.
- Cupertino updates: new ExpansionTile, better iOS-style shapes, haptic feedback improvements.

### **Rendering & Framework**

- Multiple **Impeller rendering engine** optimizations.
- More precise control over sliver paint order.

### **Platform Modernization**

- Minimums: iOS 13, macOS 10.15, Android SDK 24.
- Native Assets Preview for easier C/C++/Rust integration.
- Windows/Linux: merged UI/platform threads, multi-window support, Linux software rendering.
- Web: minified Wasm builds, hot reload enabled by default.

### **Tooling**

- Experimental **Widget Previews**.
- Wider IDE support (Android Studio Meerkat/Narwhal, CLion, GoLand, PyCharm, etc.).
- MCP Server in stable for AI-assisted workflows.

### **Breaking Changes**

- `DropdownButtonFormField.value` → `initialValue`.
- Redesigned `Radio` widget.
- `Form` can no longer be a sliver.

---

## 🔗 Further Reading

Here are the official announcements on X (formerly Twitter):
<div style="display: flex; justify-content: space-around; flex-wrap: wrap; align-items: flex-start;">
    <div style="flex: 1; min-width: 300px; margin: 10px;">
        {{< x user=FlutterDev id=1956060796657545268 >}}
    </div>
    <div style="flex: 1; min-width: 300px; margin: 10px;">
        {{< x user=FlutterDev id=1956075863058927700 >}}
    </div>
</div>

## Resources

- [Flutter 3.35.0 Release Notes](https://docs.flutter.dev/release/release-notes/release-notes-3.35.0)  
- [What’s New in Flutter 3.35 — Medium](https://medium.com/flutter/whats-new-in-flutter-3-35-c58ef72e3766)  
- [My Contribution: Fix text selection toolbar alignment for RTL languages](https://github.com/flutter/flutter/pull/169854)  

---

I’m honored to contribute to a framework used by millions of developers worldwide. While this release contains one of my pull requests, I'm excited that the next one will include even more.
Here’s to more open-source contributions and better experiences for **all** users, in **all** languages.
