# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a personal blog website built with **Hugo** using the **PaperMod** theme. The site is configured for Chinese content with a dark theme preference and includes features like search, archives, and social media integration.

## Key Configuration

- **Base URL**: https://sunweixiao.github.io/
- **Theme**: PaperMod (git submodule)
- **Language**: Chinese (zh-cn)
- **Content Directory**: `content/`
- **Static Assets**: `static/`
- **Custom Layouts**: `layouts/` (overrides PaperMod theme)

## Development Commands

### Essential Hugo Commands

```bash
# Install Hugo (if not available)
hugo version

# Development server with live reload
hugo server -D

# Build production site
hugo --minify

# Build with draft content
hugo -D

# Clean build cache
hugo mod clean
```

### Content Management

```bash
# Create new post
hugo new posts/my-post-title/index.md

# Create new page
hugo new about.md

# Generate site with future posts
hugo --buildFuture
```

## Architecture & Structure

### Content Organization
- **Posts**: Organized in `content/posts/` with each post in its own directory
- **Pages**: Static pages like `about.md`, `archives.md`, `search.md`
- **Taxonomies**: Categories and tags for post organization

### Theme Customization
- **Custom Layouts**: `layouts/_default/about.html` - custom about page template
- **Partials**: `layouts/partials/comments.html` - custom comments integration
- **Static Assets**: `static/` directory for images, CSS, JS files
- **Assets**: `assets/` directory for processed assets (CSS/JS)

### Key Features Enabled
- **Search**: Fuse.js integration via `content/search.md`
- **Archives**: Archive page via `content/archives.md`
- **Social Links**: GitHub, Bilibili, Douyin configured in `hugo.yaml`
- **Comments**: Enabled via `params.comments: true`
- **Code Copy**: Code block copy buttons enabled
- **Table of Contents**: Auto-generated TOC for posts

### Configuration Files
- **Main Config**: `hugo.yaml` - site-wide configuration
- **Theme**: `themes/PaperMod/` - git submodule (do not edit directly)
- **i18n**: `i18n/` directory for internationalization strings
- **Data**: `data/` directory for structured data files

## Deployment

The site is configured for GitHub Pages deployment. The `public/` directory contains the generated static site, and the baseURL points to the GitHub Pages domain.

## Content Creation Workflow

1. Create new content: `hugo new posts/title/index.md`
2. Add images to the post directory: `posts/title/image.png`
3. Edit frontmatter in the markdown file
4. Test locally: `hugo server -D`
5. Build for production: `hugo --minify`
6. Deploy to GitHub Pages

## Customization Points

- **Theme overrides**: Place custom templates in `layouts/` to override PaperMod
- **Styling**: Add custom CSS in `assets/css/extended/`
- **Social icons**: Configure in `hugo.yaml` under `socialIcons`
- **Menu items**: Configure in `hugo.yaml` under `languages.zh.menus.main`
- **Homepage**: Customize `params.homeInfoParams` in `hugo.yaml`