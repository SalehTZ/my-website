---
title: "راهنمای بقای برنامه‌نویسان موبایل: استفاده از میرور مایکت برای Gradle در زمان قطعی اینترنت"
subtitle: "چگونه در شرایط اختلال اینترنت، وابستگی‌های اندروید و فلاتر را دانلود کنیم؟"
date: 2026-06-30T15:23:45+03:30
lastmod: 2026-06-30T15:23:45+03:30
draft: false
author: "SalehTZ"
authorLink: "#"
description: "در این راهنمای بقا یاد می‌گیریم چطور با تنظیم میرور مایکت در Gradle، اختلالات و قطعی اینترنت ایران را دور بزنیم و پروژه‌های فلاتر، کاتلین و اندروید را بدون مشکل بیلد کنیم."
license: ""
images: []

tags: ["Flutter", "Android", "Kotlin", "Gradle", "Myket", "Iran", "Survival Guide", "میرور مایکت"]
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
# راهنمای بقا برای برنامه‌نویسان فلاتر، کاتلین و اندروید

توی زمان‌هایی که اینترنت ایران بخاطر شرایط مختلف مثل جنگ یا اختلالات داخلی قطع میشه، یکی از بزرگترین مشکلات برنامه‌نویس‌های موبایل، عدم دسترسی به پکیج‌ها و لایبرری‌های Gradle هست. این مشکل میتونه کل روند توسعه رو متوقف کنه.

این پست حاصل تجربیات من توی مدت زمانیه که اینترنت ایران قطع شده بود و حالا میخوام این راه حل رو به بقیه هم منتقل کنم تا اون‌ها هم بتونن توی این شرایط استفاده کنن. پس این یک Survival Guide برای برنامه‌نویس‌های فلاتر/اندروید/کاتلین هست.

در ادامه یاد می‌گیریم که چطور از [میرور مایکت](https://maven.myket.ir/) برای دور زدن این مشکل استفاده کنیم.

---

## راه حل اول: تنظیم میرور برای یک پروژه خاص

اگر میخواید برای هر پروژه دلبخواه خودتون به صورت جداگانه ریپازیتوری مایکت رو استفاده کنید، راه حلی که مایکت برای Maven داده در [این لینک](https://maven.myket.ir/services/maven.html) قابل مشاهده است.

برای این کار کافیه فایل `build.gradle` یا `settings.gradle` پروژه‌تون رو باز کنید و آدرس مایکت رو به بخش `repositories` اضافه کنید:

```gradle
repositories {
    maven { url "[https://maven.myket.ir](https://maven.myket.ir)" }
    google()
    mavenCentral()
}

```

این روش خوبه، اما اگر مدام پروژه‌های جدید می‌سازید، تنظیم کردن دستی برای هر پروژه واقعا خسته کننده‌ست. پس بریم سراغ راه حل دوم.

---

## راه حل دوم: تنظیم گلوبال برای تمام پروژه‌ها

این روش طلاییه! با این کار برای هر پروژه فلاتر/کاتلین/اندروید که می‌سازید، بدون نیاز به تغییر داخل پروژه می‌تونید از این ریپازیتوری‌ها استفاده کنید.

راه حل ساختن یا ویرایش فایل `init.gradle.kts` هست و قرار دادن کدهای مربوطه داخل این فایل.

### مسیر فایل در سیستم عامل‌های مختلف

* **لینوکس و مک:** `~/.gradle/init.gradle.kts` (در مسیر HOME)
* **ویندوز:** `C:\Users\YourUsername\.gradle\init.gradle.kts` (به جای `YourUsername` اسم یوزر خودتون رو بذارید)

### کدهای فایل `init.gradle.kts`

کدهای زیر رو کپی کنید و داخل این فایل قرار بدید:

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

با این تنظیمات، هر درخواستی که گریدل برای دانلود لایبرری‌ها سمت سرورهای مسدود شده میفرسته، به صورت خودکار به میرور مایکت ریدایرکت میشه.

امیدوارم این پست بتونه توی روزهای سخت قطعی اینترنت کمکتون کنه تا پروژه‌هاتون متوقف نشن.
