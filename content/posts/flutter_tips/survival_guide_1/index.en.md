---
title: "Mobile Developers Survival Guide: Using Myket Mirror for Gradle During Internet Outages"
subtitle: "How to download Android and Flutter dependencies during severe network disruptions."
date: 2026-06-30T14:14:59+02:00
lastmod: 2026-06-30T14:14:59+02:00
draft: false
author: "SalehTZ"
authorLink: "#"
description: "In this survival guide, we will learn how to bypass internet disruptions in Iran by configuring the Myket mirror in Gradle, allowing you to build Flutter, Kotlin, and Android projects without issues."
license: ""
images: []

tags: ["Flutter", "Android", "Kotlin", "Gradle", "Myket", "Iran", "Survival Guide", "Internet Outage"]
categories: ["Programming", "Tutorial"]

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
# A Survival Guide for Flutter, Kotlin, and Android Developers

During times when internet access in Iran is cut off or severely disrupted due to specific conditions like war or internal restrictions, one of the biggest problems for mobile developers is losing access to Gradle packages and libraries. This issue can completely bring the development process to a halt.

This post is the result of my own experiences during the periods when the internet in Iran was disconnected. I want to share this solution so others can use it during these tough situations. Think of this as a "Survival Guide" for Flutter, Android, and Kotlin developers.

Below, we will learn how to use the [Myket Mirror](https://maven.myket.ir/) to bypass this problem.

---

## Solution 1: Setting up the mirror for a specific project

If you want to manually use the Myket repository for a specific project of your choice, the official solution provided by Myket can be found [here](https://maven.myket.ir/services/maven.html).

To do this, simply open your project's `build.gradle` or `settings.gradle` file and add the Myket URL to the `repositories` block:

```gradle
repositories {
    maven { url "[https://maven.myket.ir](https://maven.myket.ir)" }
    google()
    mavenCentral()
}

```

This method works fine, but if you frequently create new projects, manually configuring this for every single project is quite exhausting. So, let's move on to the second solution.

---

## Solution 2: Global configuration for all projects

This is the golden method! With this approach, for every Flutter/Kotlin/Android project you create, you can use these repositories without needing to make any changes inside the project itself.

The solution involves creating or editing the `init.gradle.kts` file and placing the corresponding code inside it.

### File path in different operating systems

* **Linux & macOS:** `~/.gradle/init.gradle.kts` (In your HOME directory)
* **Windows:** `C:\Users\YourUsername\.gradle\init.gradle.kts` (Replace `YourUsername` with your actual system username)

### The `init.gradle.kts` code

Copy the code below and paste it into this file:

```kotlin
import org.gradle.api.artifacts.repositories.MavenArtifactRepository
import org.gradle.api.artifacts.dsl.RepositoryHandler

fun RepositoryHandler.enableMirror() {
    all {
        if (this is MavenArtifactRepository) {
            val originalUrl = this.url.toString().removeSuffix("/")
            urlMappings[originalUrl]?.let { mirrorUrl ->
                println("Repository[$originalUrl] is mirrored to $mirrorUrl")
                setUrl(mirrorUrl)
            }
        }
    }
}

val urlMappings = mapOf(
    "[https://repo.maven.apache.org/maven2](https://repo.maven.apache.org/maven2)" to "[https://maven.myket.ir](https://maven.myket.ir)",
    "[https://dl.google.com/dl/android/maven2](https://dl.google.com/dl/android/maven2)" to "[https://maven.myket.ir](https://maven.myket.ir)",
    "[https://plugins.gradle.org/m2](https://plugins.gradle.org/m2)" to "[https://maven.myket.ir/](https://maven.myket.ir/)"
)

gradle.allprojects {
    buildscript {
        repositories.enableMirror()
    }
    repositories.enableMirror()
}

gradle.settingsEvaluated {
    pluginManagement.repositories.enableMirror()
    dependencyResolutionManagement.repositories.enableMirror()
}

```

With this configuration, any request Gradle makes to download libraries from blocked servers will be automatically redirected to the Myket mirror.

I hope this post helps you keep your projects running smoothly during the tough days of internet outages.
