# Astro Narrow

[English](README.md) · [简体中文](README.zh-CN.md) · [Hugo Narrow](https://github.com/tom2almighty/hugo-narrow)

Astro Narrow is a content-focused Astro theme inspired by Hugo Narrow. It keeps a compact reading layout while using Astro-native building blocks: content collections, Astro routes, Astro components, and remark/rehype Markdown transforms.

English is the default language. Simplified Chinese is included as an example locale.

## Features

- Astro-native content collections for posts, projects, pages, and typed series reading paths.
- Configurable content types, card styles, list layouts, and home sections.
- Post categories and tags with shareable filtering on the Archives page.
- Ordered article series defined through Astro content references.
- Markdown alerts, tabs, math, Mermaid, galleries, lightbox, and Expressive Code.
- Theme tokens, dark mode, search, dock, table of contents, RSS, and sitemap.
- GitHub Pages workflow for publishing an example site.

## Quick Start

```sh
pnpm install
pnpm dev
pnpm build
```

## Main Config Files

- `src/config/site.ts`: site metadata, author profile, navigation, layout width, comments, analytics, gallery, license.
- `src/config/content.ts`: content type labels, paths, card style, list layout, home sections.
- `src/config/i18n.ts`: locales and localized paths.
- `src/config/theme.ts`: theme switcher options.
- `src/content.config.ts`: Astro content collection schemas.

Navigation can use built-in keys or custom items:

```ts
nav: ['posts', 'series', 'projects', 'archives'],
footerNav: ['archives']
```

## Content Taxonomy

Posts support locale-local `categories` and `tags`. The Archives page discovers terms from published posts and filters them with shareable `category` and `tag` query parameters; no taxonomy registry or standalone tag pages are required.

```yaml
categories: [Guides]
tags: [Astro, Markdown]
```

Projects keep their own `tags` for cards and search, but do not appear in Archives. Regular pages do not have taxonomy fields.

## Ordered Series

Series are curated reading paths rather than taxonomy terms. Define each path as Markdown under `src/content/series/<locale>/` and list typed post references in reading order:

```yaml
---
title: Astro Narrow Practical Guide
description: From content authoring to deployment.
chapters:
  - en/authoring-content-collections
  - en/configure-series
  - en/deploy-github-pages
---
```

The array position is the chapter order. Published series require at least two same-locale, non-draft posts; duplicate references and membership in multiple series fail the build. `/series/` lists reading paths, while each series page renders its introduction and ordered chapters. Series chapters show their position above the post title and use chapter-aware previous/next navigation.

Series is a built-in navigation key. Remove `'series'` from `siteConfig.nav` when the site does not need a primary Series entry. Archives and RSS remain posts-only.

## Markdown Tabs

Tabs use `remark-directive` syntax. The outer container uses four colons because it contains nested directives.

````md
::::tabs
:::tab{title="site.ts"}
```ts
export const siteConfig = {
  contentWidth: '56rem'
}
```
:::

:::tab{title="content.ts"}
```ts
export const contentTypes = {
  posts: { listLayout: 'stack' }
}
```
:::
::::
````

## GitHub Pages

The example workflow is in `.github/workflows/deploy.yml`. Before the first deployment, open repository **Settings > Pages** and set **Build and deployment > Source** to **GitHub Actions**. Without this setting, `actions/deploy-pages` can fail with `HttpError: Not Found`.

The workflow sets `ASTRO_SITE` and `ASTRO_BASE` automatically for both user pages and project pages.

## Development Notes

- Keep changes Astro-native. Do not add Hugo compatibility layers.
- Prefer Markdown-native authoring and remark/rehype transforms over custom HTML snippets.
- Keep user-facing options in config files instead of requiring users to import internal code.
