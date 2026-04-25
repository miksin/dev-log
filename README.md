// Simple Devlog Readme

Overview
- This repository hosts a lightweight devlog focused on Hermes Agent development and related subagents.
- Content is organized under src/content/ with blog posts in src/content/blog/ and project notes in src/content/projects/.

Quick Start
- Install: npm install
- Build: npm run build
- Start (if available): npm run dev or npm run start

Structure highlights
- Site config: src/site.config.ts (title, owner, navigation, SEO)
- Post headers fetch author info from post metadata; owner is no longer shown as a fallback in headers.
- Blog posts live under src/content/blog/.
- Projects live under src/content/projects/.

How to customize
- Change site title and metadata in src/site.config.ts.
- Update blog posts in src/content/blog/ and project notes in src/content/projects/.
- Update header/footer in src/site.config.ts if needed.

Build notes
- This repo uses a standard Astro build setup. Ensure dependencies are installed before building.
