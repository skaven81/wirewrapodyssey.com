# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static documentation website for the **Wire Wrap Odyssey** — a custom 8-bit microcoded homebrew CPU built from 7400-series logic. The site documents the CPU architecture, peripherals, video card, software tools, and construction process.

## Development

There is no build system, package manager, or bundler. The site is pure static HTML/CSS/JavaScript. To preview it locally, serve the directory with any static HTTP server:

```bash
python3 -m http.server 8080
# or
npx serve .
```

Note: The site **must** be served over HTTP (not opened as `file://`) because content pages are loaded via `fetch()` calls.

## Architecture

The site is a **single-page application** built without any frameworks:

- `index.html` — the shell: contains the sidebar navigation, header/footer chrome, and all JavaScript logic. Content is injected into `<div id="content">` and the title into `<span id="section-header">`.
- All other `*.html` files — content pages, fetched on demand by the `show_content(id)` JS function. Each must contain exactly two elements:
  - `<div class='section-title'>` — page title text
  - `<div class='section-body'>` — page body HTML

Navigation works via URL hash (`#section-name`). Calling `show_content('foo')` fetches `/foo.html`, extracts the two divs, and injects them into the shell. Back/forward browser history is supported via `pushState`.

Accordion sections in the sidebar (Architecture, Peripherals, Video Card, Software) use `accordion()` / `accordion_open()` / `accordion_close()`. To deep-link to a page inside an accordion from within content, use `javascript:accordion_and_show_content('accordion-id', 'page-id')`.

## Content Page Template

Every new content page must follow this structure:

```html
<!--
vim: ts=2 sts=2 sw=2 expandtab
-->
<html>
<body>
  <div class='section-title'>
    Page Title Here
  </div>
  <div class='section-body'>
    <!-- content here -->
  </div>
</body>
</html>
```

## Styling

- W3.CSS framework: `/css/w3.css` (local copy)
- Blue theme: `/css/w3-theme-blue.css` (local copy)
- Font Awesome 4.7 (CDN)
- Roboto font (Google Fonts CDN)
- 2-space indentation throughout (vim modeline: `ts=2 sts=2 sw=2 expandtab`)

Use W3.CSS utility classes for layout and styling. Figures use `class='figure-container'` with `class='figure-caption'`. Code/register names use `<tt>` tags; register names specifically use `<tt class=register>`.

## Adding a New Page

1. Create the `<pageid>.html` content file.
2. Add a nav link in `index.html` using `href="javascript:show_content('<pageid>')"`.
3. If under an accordion section, add it inside the appropriate `<div id="...-accordion">` block and add a corresponding entry in the accordion nav link.
