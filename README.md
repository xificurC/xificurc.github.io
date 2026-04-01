# Incomplete Thought

Personal blog at [xificurc.github.io](https://xificurc.github.io). Built with [Zola](https://www.getzola.org/).

## Local dev

```
zola serve
```

Open http://127.0.0.1:1111.

## New post

Create `content/blog/<slug>.md`:

```markdown
+++
title = "Post Title"
date = 2026-03-19
+++

Your content here.
```

## Deploy

Push to `master`. GitHub Actions builds and deploys automatically.

**One-time setup**: Go to repo Settings → Pages → Source → select "GitHub Actions".

## Theme

[zola-bearblog](https://codeberg.org/alinnow/zola-bearblog) added via git subtree. To pull upstream updates:

```
git subtree pull --prefix=themes/zola-bearblog \
  https://codeberg.org/alinnow/zola-bearblog.git main --squash
```
