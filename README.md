# kerkerj's blog

Source code for [blog.kerkerj.in](https://blog.kerkerj.in/), built with [Hugo](https://gohugo.io/) and [Stack](https://stack.jimmycai.com/) theme.

## Write a new post

Create a markdown file in `content/post/`:

```markdown
---
title: "My New Post"
date: 2026-02-22
description: "A short summary of the post"
categories: ['Category']
tags: ['tag1', 'tag2']
---

Your content here.
```

## Preview locally

```bash
hugo server
```

## Deploy

Push to `main`. GitHub Actions will automatically build and deploy.

```bash
git add -A
git commit -m "post: my new post title"
git push
```

## Configuration

- **Hugo version**: `0.156.0` (pinned in GitHub Actions)
- **Theme**: Stack (copied into repo, not a submodule)
- **Base URL**: `https://blog.kerkerj.in/`
- **Permalink**: `/:year/:month/:slug/`
