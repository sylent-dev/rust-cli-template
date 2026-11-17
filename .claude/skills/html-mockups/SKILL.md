---
name: html-mockups
description: Build the interactive HTML mock-up of a terminal interface or of a rich output (tables, colors, progress) from its approved wireframe, in docs/mockups/, every screen drawn as a terminal (a grid of cells, the named colors, with and without colors), with every scenario and state, and 3 or 4 styles compared with tabs. Use it after the wireframes are approved, when the owner asks for a mock-up, a style, a visual design, or a new section of an existing mock-up. Serving and checking the page is the mockup-screenshots skill; iterating on the chosen style, the mockup-decisions skill.
---

# HTML mock-ups

Goal: choose the style with real content, every scenario and every state, before writing the interface. A plain command, whose output is a few lines of text, needs no mock-up: its usage, its help and an example of each output go in `specs.md` (`process.md`, Specifications). The mock-up answers one question at a time: the layout is already settled by the wireframe (`excalidraw-wireframes` skill), so the discussion is only about the look.

## 1. Build the page

Before drawing the options of a design question:

- **Ask the owner for their own idea first**, or draw it alongside: on a desktop application, theirs beat the three entries drawn for a bug report.
- **Look at the real data** the terminal will show: the longest names, the largest numbers, the number of rows, which decide the widths of the columns and what is cut first.

Start from `assets/mockup-template.html` (read its comments), copied to `docs/mockups/<name>.html`:

- **Small files**, so that a change reads only what it needs: the page `docs/mockups/<name>.html` only loads, in order, the files of `docs/mockups/<name>/`: `css/` (the terminal themes, the tokens, the page, then one sheet per block of the interface), `js/` (the shared helpers and icons, one script per block, the sample data, the rendering last) and `sections/` (one script per section, each adding its windows to `SECTIONS`). Plain `<link>` and `<script>` tags, no modules, so that the files share their constants. The template is one file: split it as soon as the page has more than a few blocks.
- **Every frame of the wireframe becomes a terminal**, at its size in columns and rows (the smallest the application supports, and a wide one), with sample data close to real ones (long names, large numbers, the other language). Add every scenario and state in `SECTIONS`: empty, loading, waiting, each error, each kind of content. **A new section goes at the top** of `SECTIONS`, so that the owner finds it at once; past ten sections, the page shows a table of contents under its header, one link per section.
- **3 or 4 styles on the same page**, switched with the tabs at the top; the address keeps the choice (`?theme=…`) to share it. Each style only sets **tokens** (CSS variables under `:root[data-theme="…"]`): the terminal color of each role (accent, weak text, results, errors), the text styles, the borders and symbols. The components use nothing else, so that a style is cheap to add or tweak.
- The styles explore **new directions** for this application, its users and its subject, from sober to spectacular: the owner decides by seeing, and a variant set aside is not wasted. If the project has a `design-journal.md`, it records past experiences: it may inspire one style, never all of them, and nothing in it is a default.
- **Only what a terminal shows**: a grid of cells in a common monospace font (the application doesn't choose the font of the terminal), the 16 named colors first, which follow the user's theme (a 256-color or true-color value names the named color it falls back to), and only the styles terminals support: bold, dim, underline, reverse (italic isn't everywhere). An effect of the browser (a shadow, a rounded corner, a font, a gradient) would be approved and then never built.
- **With and without colors**: tabs show each terminal in a dark and a light theme, and without colors (`NO_COLOR`); a meaning never rests on color alone, a symbol or a word goes with it (✓ ✗, `error:`).
- **Symbols and borders** in Unicode and box-drawing characters, each with its ASCII fallback, one cell each: a wide emoji takes two cells in some terminals and breaks the grid.
- **Every notice is drawn with its way out**: how it closes, for how long, what brings it back. A notice the user can't dismiss stays for as long as they ignore it: a dot on an icon and a line in the settings, left until the user updates, gave way to a bubble that closes for good and a discreet link where the version is written.
- **A motion is chosen by seeing it**, not by reading about it: draw each spinner or progress bar as a terminal that plays it, at the speed of the application (its frames and their delay), then write them in `specs.md` and the tokens.

Pitfalls met before:

- A class shared by two components (`top`, `online`) silently broke the layout twice: prefix the classes of the application (`app-`) and of the states (`is-`).
- A JavaScript function named like a browser global (`top`, `name`, `status`) breaks the page: give the components specific names.
- A line drawn in HTML counts its cells on its plain text, not on its markup: the template keeps each line as a plain text and its HTML, so that the borders stay aligned.

## 2. Hand it over

Serve the page and check each change with a headless screenshot before giving the owner its link (`mockup-screenshots` skill). Once the owner picks a style, iterate on it one decision at a time, then write the design tokens (`mockup-decisions` skill).
