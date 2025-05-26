---
title: "Benchmarking SVG vs CustomPainter in Flutter"
subtitle: "A deep dive into rendering performance in Flutter apps"
date: 2025-05-19T09:35:12+03:30
lastmod: 2025-05-19T09:35:12+03:30
draft: true
author: "SalehTz"
authorLink: "#"
description: "Explore performance benchmarks comparing SVG and CustomPainter rendering methods in Flutter, using chaos-driven animations, real-time graphs, and auto-benchmarking tools."
license: "CC BY 4.0"
images: []

tags: ["flutter", "performance", "custompainter", "svg", "benchmark"]
categories: ["Flutter", "Performance"]

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

# 🚪 Benchmarking SVG vs CustomPainter in Flutter: Which is More Efficient?

Flutter offers multiple ways to draw shapes, graphics, and complex visuals. Two of the most common approaches are:

* **Using SVG assets** (via `flutter_svg`)
* **Drawing directly using `CustomPainter`**

But which one performs better when rendering **hundreds or thousands of items on screen**? I built a benchmarking app to test this out — and in this article, I’ll walk you through:

* 📱 How the app is structured
* 🗁 Chaos-driven object animations
* 🎛 Controls for item count, complexity, and chaos
* 📊 Auto-benchmarking with real-time performance charts
* 📉 Real results comparing SVG vs CustomPainter
* 🧠 Insights & conclusions

---

## 🛠️ App Architecture: Three Screens, Many Possibilities

The app is structured into 3 main screens:

1. **Home Screen**

   * Adjust settings (item count, chaos intensity, SVG complexity)
   * Launch SVG or CustomPainter test
   * Run Auto-Benchmark

2. **Benchmark Area**

   * Renders moving objects (SVG or CustomPainter)
   * Measures frame rate over time

3. **Benchmark Graphs**

   * Displays results as multi-line charts
   * Compare SVG vs CustomPainter visually

---

## 🔧 Customization Options

From the main screen, users can configure:

* **Item Count**: From 100 to 10,000 items
* **Chaos Intensity**: Controls speed and randomness of bouncing items
* **SVG Complexity**: Switch between different SVG files (e.g. simple, medium, complex, very complex)

Each test runs for a fixed duration (e.g. 2 seconds), and performance data (frame times) is recorded.

---

## 🌀 Chaos-Based Bouncing

To simulate realistic animation stress, we used random bouncing physics inspired by `flutter_physics`. Each item:

* Starts at a random position
* Moves in a random direction
* Bounces off the edges of the screen
* Speed is scaled by “chaos intensity”

This allowed us to simulate real-world dynamic UI elements.

---

## 🖼 SVG vs 🎨 CustomPainter

### SVG Rendering

We used the `flutter_svg` package to load vector files with increasing complexity:

* `simple.svg` – just a few paths
* `medium.svg` – \~100 vector elements
* `complex.svg` – hundreds of elements
* `very_complex.svg` – many layers and masks

All SVG items were loaded as:

```dart
SvgPicture.asset(
  assetPath,
  width: 24,
  height: 24,
)
```

Each one was wrapped in a `RepaintBoundary` to isolate paints and reduce overdraw.

### CustomPainter Rendering

For the CustomPainter alternative, we drew a simple blue circle:

```dart
class _CirclePainter extends CustomPainter {
  @override
  void paint(Canvas canvas, Size size) {
    final paint = Paint()..color = Colors.blue;
    canvas.drawCircle(size.center(Offset.zero), size.width / 2, paint);
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => false;
}
```

This was chosen for fairness — a vector circle is a simple shape like many SVGs.

---

## 📈 Auto-Benchmarking & Graphing

We added a feature to auto-run multiple tests, varying:

* Render mode: SVG vs CustomPainter
* Chaos levels
* Item counts
* SVG complexity

After each test, frame times were recorded and plotted on a chart using `fl_chart`:

* **X-axis**: Time
* **Y-axis**: Frame time (ms)
* **Color-coded lines**: Represent different configurations

We displayed multiple charts side-by-side to visually compare performance.

---

## 📊 Results: What We Discovered

After running the benchmarks, here’s what we observed:

| Scenario                | SVG Avg FPS | CustomPainter Avg FPS |
| ----------------------- | ----------- | --------------------- |
| 1,000 simple items      | 55–60 fps   | 58–60 fps             |
| 1,000 complex items     | 30–40 fps   | 58–60 fps             |
| 5,000 simple items      | 20–30 fps   | 40–50 fps             |
| 10,000 simple items     | 5–15 fps    | 20–30 fps             |
| Complex SVG @ chaos 3.0 | 10–25 fps   | 40–55 fps             |

### Key Takeaways

* **CustomPainter consistently outperformed SVG rendering**, especially with high item counts or complexity.
* **SVG performance degraded rapidly** as complexity increased.
* **Repaint boundaries** helped both approaches but didn’t eliminate the SVG bottlenecks.
* For static or low-count use cases, SVG is still fine and more convenient.
* For highly dynamic or high-count UIs, CustomPainter is the clear winner.

---

## ✅ Conclusions

When to use each approach?

| Use Case                           | Recommendation                   |
| ---------------------------------- | -------------------------------- |
| Static icons or illustrations      | ✅ SVG is fine                    |
| Dynamic animations with many items | ✅ Use CustomPainter              |
| Memory-optimized rendering         | ✅ Use CustomPainter              |
| Designer handoff with SVG files    | ✅ Start with SVG, optimize later |

---

## 🧠 Bonus: Tips for Better Performance

* Use `RepaintBoundary` wisely
* Use `shouldRepaint` = `false` when possible
* Avoid unnecessary state rebuilds
* Profile with Flutter DevTools to catch jank

---

## 🚀 Final Thoughts

This benchmark app helped me *truly understand the rendering cost of SVGs* vs direct canvas painting. By simulating chaos and measuring real-time frame rates, we get a clear picture of how each method performs under stress.

**Want to try it yourself?** You can find the full source code [here](#) *(add GitHub link)*.

---

### 🔗 Connect With Me

If you enjoyed this breakdown, consider:

* ⭐ Starring the repo on GitHub
* 🐦 Following me on Twitter \[@yourhandle]
* 💬 Sharing your experience with vector rendering in Flutter!
