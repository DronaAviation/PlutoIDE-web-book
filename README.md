# Pluto IDE Web Book

A web-based interactive project book for **Pluto IDE** — a VS Code extension for firmware development on Pluto nano drones. This single-page application provides comprehensive documentation and project tutorials for both **Pluto X** (Primus X2) and **Pluto 1.2** (Primus V4) drones using C++ and the MagisV2 API.

## Table of Contents

- [Project Description](#project-description)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [How to Start the Project](#how-to-start-the-project)
- [JSON Structure](#json-structure)
- [Rules for Modifying JSON Files](#rules-for-modifying-json-files)
- [Understanding main.html](#understanding-mainhtml)
- [Features](#features)
- [Browser Compatibility](#browser-compatibility)

---

## Project Description

**Pluto IDE Web Book** is an interactive documentation platform that serves as a comprehensive guide for users learning to program Pluto drones using the **Pluto IDE** (VS Code Extension) and **MagisV2 API** in C++. The application provides:

- **Introduction and Platform Overview**: Learn about Pluto drones, their hardware (Primus), and software (Magis)
- **Hardware Documentation**: Detailed Primus flight controller specs, pin maps, Unibus header, motor drivers
- **Software Architecture**: MagisV2 firmware blocks — FC-Data, FC-Config, FC-Control, BMS, Peripherals, Debugging
- **API Quick Reference**: Complete table of all MagisV2 APIs validated against the [official wiki](https://github.com/DronaAviation/MagisV2/wiki)
- **IDE Setup Guide**: Installation, toolchain setup, building, WiFi/USB flashing, Pluto Monitor, Teleplot
- **19 Project Tutorials**: Step-by-step projects from basic LED control to advanced sensor-driven flight, with complete C++ code
- **Interactive Navigation**: Sidebar navigation with grouped projects (Basic / Intermediate / Advanced)

The project is built as a **static single-page application** that requires no build process, no frameworks, and works **fully offline**.

---

## Project Structure

```
PlutoIDE-web-book/
│
├── main.html                # Main HTML file with embedded CSS and JavaScript
├── README.md                # This file
│
└── assets/
    ├── primus-x2.json       # All content, projects, and C++ code
    └── lib/                 # Offline dependencies (PrismJS)
        ├── prism-tomorrow.min.css
        ├── prism.min.js
        ├── prism-c.min.js
        └── prism-cpp.min.js
```

### File Descriptions

- **`main.html`**: Contains the complete application — HTML structure, embedded CSS (dark theme), and JavaScript for dynamic content loading, sidebar navigation, pagination, image zoom, and keyboard shortcuts.

- **`assets/primus-x2.json`**: JSON configuration file containing all content including:
  - 10 documentation sections (intro, platform, hardware, software, tinkering, API ref, IDE setup, developer mode)
  - 19 project tutorials (p0–p18) organized into Basic, Intermediate, and Advanced groups
  - Complete C++ code for every project using MagisV2 APIs

- **`assets/lib/`**: PrismJS syntax highlighting library bundled for offline use (C/C++ support with `prism-tomorrow` dark theme)

---

## Getting Started

### Prerequisites

- A modern web browser (Chrome, Firefox, Safari, Edge)
- A local web server (required — JSON fetch does not work with `file://` protocol)

### Installation

1. **Clone** this repository:
   ```bash
   git clone https://github.com/DronaAviation/PlutoIDE-web-book.git
   cd PlutoIDE-web-book
   ```

2. **No additional dependencies** — this is a pure HTML/CSS/JavaScript application with all libraries bundled offline.

---

## How to Start the Project

### Option 1: Using Python (Recommended)

```bash
python3 -m http.server 8000
# Open http://localhost:8000/main.html
```

### Option 2: Using Node.js

```bash
npx serve .
# Open the URL shown in terminal
```

### Option 3: Using VS Code Live Server

1. Install the "Live Server" extension in VS Code
2. Right-click on `main.html` → "Open with Live Server"

### Option 4: Using PHP

```bash
php -S localhost:8000
# Open http://localhost:8000/main.html
```

> **Note**: Opening `main.html` directly via `file://` will not work because browsers block `fetch()` requests to local JSON files. A local server is required.

---

## JSON Structure

The JSON file (`primus-x2.json`) follows this structure:

### Basic Structure

```json
{
  "_meta": {
    "order": ["intro", "compatibility", "platform", "p0", "p1"],
    "groups": [
      {
        "id": "basic",
        "title": "Basic Projects",
        "items": ["p0", "p1", "p2"]
      }
    ]
  },
  "intro": {
    "title": "1. Introduction",
    "html": "<div class=\"card\"><p>Content here</p></div>"
  },
  "p0": {
    "title": "Project 0: Red LED ON",
    "html": "<div class=\"card\">...</div>"
  }
}
```

### Field Descriptions

#### `_meta` Object

| Field | Required | Description |
|-------|----------|-------------|
| `order` | Yes | Array of section IDs defining navigation sequence and pagination |
| `groups` | Optional | Array of groups for organizing projects in the sidebar |

Each group has:
- `id`: Unique identifier
- `title`: Display name shown in sidebar
- `items`: Array of section IDs belonging to this group

#### Section Objects

| Field | Required | Description |
|-------|----------|-------------|
| `title` | Yes | Section title (shown in `<h1>` and sidebar) |
| `html` | Yes | HTML content wrapped in `<div class="card">` |

### Section ID Naming

- **Documentation sections**: `"intro"`, `"compatibility"`, `"platform"`, `"primus"`, `"magis"`, `"tinkering"`, `"tinkering-loop"`, `"api-ref"`, `"pluto-ide"`, `"dev-mode"`
- **Project sections**: `"p0"`, `"p1"`, `"p2"`, ... `"p18"`
- IDs appear in URL hashes: `#intro`, `#p1`, etc.

---

## Rules for Modifying JSON Files

### 1. JSON Syntax Rules

- Always use **double quotes** for keys and string values
- No trailing commas
- Escape special characters: `\"` for quotes, `\\` for backslashes
- Code blocks use: `<pre><code class=\"language-cpp\">...</code></pre>`

### 2. Adding a New Project

1. Create the section object with `title` and `html`
2. Add the ID to `_meta.order` in the desired position
3. Add to the appropriate group in `_meta.groups`

### 3. HTML Content Guidelines

- Wrap content in `<div class="card">` for consistent styling
- Use `<span class="badge badge-beginner">Beginner</span>` or `badge-intermediate` for level badges
- Use `<div class="callout">` for tips, `callout-warn` for warnings, `callout-success` for positive notes
- Use `<pre><code class="language-cpp">` for C++ code (PrismJS auto-highlights)
- Use `<div class="step">` with `<div class="step-n">` for numbered steps
- Use semantic HTML: `<h2>`, `<h3>`, `<p>`, `<ul>`, `<table>`

### 4. Validation Checklist

Before saving:
- [ ] JSON is valid (use a JSON validator)
- [ ] All IDs in `order` have corresponding section objects
- [ ] All IDs in group `items` exist as sections
- [ ] No duplicate section IDs
- [ ] HTML is properly escaped
- [ ] C++ code uses valid MagisV2 APIs (check [wiki](https://github.com/DronaAviation/MagisV2/wiki))

---

## Understanding main.html

The `main.html` file is a **self-contained single-page application** with three main parts:

### 1. HTML Structure

```
┌─────────────────────────────────────────────┐
│  Sidebar (fixed, 280px)  │  Main Content    │
│  ┌───────────────────┐   │  (scrollable)    │
│  │ Brand / Title     │   │                  │
│  │ Nav Groups        │   │  <h1> Title      │
│  │ Project Links     │   │  <div.card>      │
│  │ Board Selector    │   │  <div.card>      │
│  └───────────────────┘   │  ...             │
├──────────────────────────┴──────────────────┤
│  Pagination Bar (fixed bottom)              │
│  [← Back]     3 / 29     [Next →]          │
└─────────────────────────────────────────────┘
```

### 2. CSS (Embedded)

- **Dark theme** with gradient sidebar
- CSS Variables: `--bg-dark`, `--bg-side`, `--bg-card`, `--accent`, `--border`
- Blue inline code styling (`#58a6ff`)
- Blue active sidebar highlighting
- PrismJS `prism-tomorrow` theme for code blocks
- Responsive: sidebar hides on screens < 768px

### 3. JavaScript

| Function | Purpose |
|----------|---------|
| `loadContent()` | Fetches JSON, initializes navigation and content |
| `renderProjectsNav()` | Generates sidebar links from `_meta.groups` |
| `renderSection(id)` | Renders section HTML, highlights active nav, runs PrismJS |
| `updatePagination(id)` | Updates Back/Next buttons and page indicator |
| `enableImageZoom()` | Adds click-to-zoom on images |

**Navigation**: Hash-based routing (`#intro`, `#p1`). Arrow keys for prev/next. Escape to close image modal.

**Persistence**: Board selection saved to `localStorage`.

---

## Features

- **19 C++ Projects** — Complete code validated against [MagisV2 Wiki](https://github.com/DronaAviation/MagisV2/wiki)
- **Dark Theme UI** — Modern dark theme with gradient sidebar
- **PrismJS Syntax Highlighting** — Bundled offline for C/C++
- **Organized Navigation** — Projects grouped by difficulty (Basic / Intermediate / Advanced)
- **Hash-Based Routing** — Direct links to any section via URL hash
- **Keyboard Navigation** — Arrow keys for prev/next, Escape for modal close
- **Persistent Preferences** — Board selection saved to localStorage
- **Fully Offline** — No CDN, no external dependencies
- **Image Zoom** — Click any image to view full-size in modal
- **Page Counter** — Shows current position (e.g., "5 / 29")

---

## Browser Compatibility

| Browser | Minimum Version |
|---------|----------------|
| Chrome | 90+ |
| Firefox | 88+ |
| Safari | 14+ |
| Edge | 90+ |

**Required Features**: ES6 (async/await), Fetch API, CSS Custom Properties, LocalStorage.

---

## Related Resources

- [MagisV2 Firmware](https://github.com/DronaAviation/MagisV2) — Drone firmware source code
- [MagisV2 Wiki](https://github.com/DronaAviation/MagisV2/wiki) — Complete API documentation
- [Pluto IDE Extension](https://marketplace.visualstudio.com/items?itemName=DronaAviation.pluto-ide) — VS Code extension
- [Pluto IDE Info Page](https://dronaaviation.github.io/Pluto-IDE-Extension-Info-Pages/) — Extension features overview
- [Drona Aviation](https://www.dronaaviation.com) — Official website

---

## Troubleshooting

### JSON Not Loading
- Use a local web server — `file://` protocol blocks `fetch()` requests
- Check browser console for errors
- Validate JSON syntax

### Code Not Highlighted
- Ensure `assets/lib/` contains all 4 PrismJS files
- Check that `main.html` references local paths (not CDN URLs)

### Navigation Not Working
- Verify all IDs in `_meta.order` exist as section objects in the JSON
- Check URL hash format: `#section-id`

---

## Instructions For Contributors

When contributing content:

1. Follow the JSON structure guidelines above
2. Validate JSON before committing
3. Test changes with a local server
4. Ensure C++ code compiles and uses valid MagisV2 APIs
5. Maintain consistent formatting — use `card`, `callout`, `badge`, `step` classes
6. Check the [MagisV2 Wiki](https://github.com/DronaAviation/MagisV2/wiki) for correct API signatures

---

**Happy Tinkering with Pluto IDE!**
