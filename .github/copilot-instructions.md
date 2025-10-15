# AI Coding Agent Instructions for SalehTZ Blog

## Project Overview
This is a bilingual (English/Farsi) Hugo static site powered by the LoveIt theme. The site focuses on Flutter development, cybersecurity (Bandit CTF), backend development, and tech tutorials.

## Architecture & Key Components

### Multilingual Structure
- **Primary languages**: English (`en`) and Farsi (`fa`)
- **Content files**: Use `.en.md` and `.fa.md` suffixes (e.g., `index.en.md`, `index.fa.md`)
- **RTL support**: Farsi content uses right-to-left layout with custom CSS in `assets/css/_custom.scss`
- **Language configs**: Separate menus and search settings in `hugo.toml` under `[languages.en]` and `[languages.fa]`

### Content Organization
```
content/
├── posts/
│   ├── flutter/          # Flutter development posts
│   ├── bandit/          # CTF cybersecurity challenges
│   ├── backend/         # Backend development
│   ├── ai/              # AI-related content
│   └── linux/           # Linux tutorials
├── about/               # About pages (bilingual)
└── categories/          # Category taxonomy pages
```

### Post Structure Pattern
Each post lives in its own directory with bilingual markdown files:
```yaml
---
title: "Post Title"
subtitle: "Optional subtitle"
date: 2025-05-22T08:21:31+03:30
lastmod: 2025-05-22T08:21:31+03:30
draft: false
author: "SalehTZ"
tags: ["flutter", "dart"]
categories: ["Flutter", "Development"]
featuredImage: ""
hiddenFromHomePage: false
toc:
  enable: true
  auto: true
code:
  copy: true
  maxShownLines: 50
lightgallery: true
---
```

## Development Workflows

### Local Development
```bash
# Start Hugo development server
hugo server -D

# Build for production
hugo --gc --minify

# Test multilingual build
hugo server --buildDrafts --buildFuture
```

### Content Creation
1. Create directory: `content/posts/category/post-name/`
2. Add both language versions: `index.en.md` and `index.fa.md`
3. Use consistent frontmatter structure (see pattern above)
4. Place images in `assets/images/posts/` or `static/images/`

### Deployment
- **Automated**: GitHub Actions workflow (`.github/workflows/hugo.yaml`)
- **Hugo version**: 0.145.0 (extended)
- **Target**: GitHub Pages at `salehtz.github.io/my-website/`
- **Build command**: `hugo --gc --minify --baseURL="${{ steps.pages.outputs.base_url }}/"`

## Project-Specific Conventions

### Theme Customization
- **Base theme**: LoveIt (Git submodule at `themes/LoveIt/`)
- **Custom overrides**: `layouts/baseof.html` includes Clarity analytics
- **Custom styles**: `assets/css/_custom.scss` for RTL support and dark theme colors
- **Color scheme**: Custom purple (`#8B5DFF`) for dark theme headings

### Configuration Patterns
- **Comments**: Giscus integration (repo: `salehTZ/my-website`)
- **Search**: Algolia with separate indexes for English/Farsi
- **Social links**: Configured per language in `hugo.toml`
- **Permalinks**: Simple filename-based: `posts = ":filename"`

### Content Categories
- **Flutter**: Release notes, tips, package tutorials
- **Bandit**: CTF challenge walkthroughs (numbered sequence)
- **Backend**: Docker, N8N, server deployment guides
- **Documentation**: Technical guides and cheat sheets

### Frontmatter Requirements
- Always include `toc.enable: true` for technical posts
- Use `lightgallery: true` for posts with images
- Set `code.copy: true` and `code.maxShownLines: 50` for code-heavy posts
- Add descriptive `tags` and `categories` for discoverability

## Integration Points

### External Services
- **Algolia Search**: Dual-language indexing
- **Giscus Comments**: GitHub Discussions integration
- **Clarity Analytics**: Microsoft Clarity tracking
- **Social Media**: X, Instagram, GitHub, Telegram

### Asset Management
- **Images**: Store in `assets/images/posts/` for processing
- **Static files**: Use `static/` for direct serving (favicons, robots.txt)
- **Custom CSS**: Extend via `assets/css/_custom.scss`

## Critical Files
- `hugo.toml`: Main configuration with bilingual setup
- `layouts/baseof.html`: Custom HTML base with analytics
- `assets/css/_custom.scss`: RTL and theme customizations
- `i18n/*.toml`: Translation strings for UI elements
- `.github/workflows/hugo.yaml`: Automated deployment pipeline

When creating content, always consider the bilingual audience and maintain consistency between English and Farsi versions. Use the established frontmatter patterns and respect the RTL layout requirements for Farsi content.