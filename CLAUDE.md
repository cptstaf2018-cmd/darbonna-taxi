# CLAUDE.md

This file provides guidance to Claude Code when working with code in this repository.

## Project Overview

**UI UX Pro Max** (`ui-ux-pro-max-skill`) is an AI skill that provides design intelligence for building professional UI/UX across multiple platforms and frameworks. It is published on the Claude Marketplace and installable via the `uipro-cli` npm package.

- **GitHub:** https://github.com/nextlevelbuilder/ui-ux-pro-max-skill
- **Website:** https://uupm.cc
- **npm CLI:** `uipro-cli` (v2.5.0)
- **Current version:** 2.5.0
- **License:** MIT
- **Stars:** 50,000+ | **Forks:** 4,800+
- **Primary language:** Python (search engine); TypeScript (CLI)

The skill activates automatically in supported AI assistants when a user requests UI/UX work. It generates complete design systems using a reasoning engine backed by CSV databases and a BM25 + regex hybrid search engine.

---

## Repository Structure

```
ui-ux-pro-max-skill/
├── src/ui-ux-pro-max/              # Source of Truth for all skill data and logic
│   ├── data/                       # CSV databases
│   │   ├── products.csv            # 161 product type rules (industry-specific)
│   │   ├── styles.csv              # 67 UI styles (Glassmorphism, Brutalism, etc.)
│   │   ├── colors.csv              # 161 color palettes (1:1 with product types)
│   │   ├── typography.csv          # 57 font pairings with Google Fonts imports
│   │   ├── landing.csv             # 24 landing page patterns + CTA strategies
│   │   ├── charts.csv              # 25 chart type recommendations
│   │   ├── ux-guidelines.csv       # 99 UX best practices and anti-patterns
│   │   ├── ui-reasoning.csv        # 161 industry-specific reasoning rules
│   │   ├── design.csv              # Design-level rules
│   │   ├── icons.csv               # Icon set recommendations
│   │   ├── google-fonts.csv        # Google Fonts reference
│   │   ├── app-interface.csv       # App interface guidelines
│   │   ├── react-performance.csv   # React performance guidelines
│   │   ├── draft.csv               # Draft/WIP data
│   │   ├── _sync_all.py            # Data sync helper script
│   │   └── stacks/
│   │       └── react-native.csv    # React Native stack guidelines
│   ├── scripts/
│   │   ├── search.py               # CLI entry point for all searches
│   │   ├── core.py                 # BM25 + regex hybrid search engine
│   │   └── design_system.py        # Design system generation engine
│   └── templates/
│       ├── base/
│       │   ├── skill-content.md    # Common SKILL.md content for all platforms
│       │   └── quick-reference.md  # Quick reference (Claude-specific)
│       └── platforms/              # Platform-specific config templates
│           ├── claude.json
│           ├── cursor.json
│           ├── windsurf.json
│           ├── copilot.json
│           ├── kiro.json
│           ├── roocode.json
│           ├── codex.json
│           ├── trae.json
│           ├── gemini.json
│           ├── opencode.json
│           ├── qoder.json
│           ├── continue.json
│           ├── codebuddy.json
│           ├── droid.json
│           └── agent.json
│
├── cli/                            # npm package: uipro-cli
│   ├── src/
│   │   ├── index.ts                # CLI entry point
│   │   ├── commands/               # Command implementations (init, versions, update)
│   │   ├── utils/                  # Template rendering engine
│   │   └── types/
│   ├── assets/                     # Bundled copies of src/ui-ux-pro-max/ (~564KB)
│   │   ├── data/
│   │   ├── scripts/
│   │   └── templates/
│   ├── package.json                # uipro-cli package config
│   └── tsconfig.json
│
├── .claude/skills/ui-ux-pro-max/   # Claude Code skill (symlinks to src/)
├── .factory/skills/ui-ux-pro-max/  # Droid (Factory) skill (symlinks to src/)
├── .shared/ui-ux-pro-max/          # Symlink to src/ui-ux-pro-max/
├── .claude-plugin/                 # Claude Marketplace publishing
│   ├── plugin.json                 # Plugin manifest (v2.5.0)
│   └── marketplace.json            # Marketplace listing (v2.2.1)
├── .github/                        # GitHub Actions and templates
├── docs/                           # Documentation
├── preview/                        # Preview assets
├── screenshots/                    # Screenshot assets
├── cat-feeding-app/                # Example app built with the skill
├── CLAUDE.md                       # This file
├── README.md                       # Full project documentation
└── LICENSE                         # MIT License
```

---

## Key Commands

### Search (Python — requires Python 3.x, no external deps)

```bash
# Domain-specific search
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain <domain> [-n <max_results>]

# Available domains:
#   product     - Product type recommendations (SaaS, e-commerce, portfolio)
#   style       - UI styles (glassmorphism, minimalism, brutalism) + CSS keywords
#   typography  - Font pairings with Google Fonts imports
#   color       - Color palettes by product type
#   landing     - Page structure and CTA strategies
#   chart       - Chart types and library recommendations
#   ux          - Best practices and anti-patterns

# Stack-specific guidelines
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --stack <stack>
# Stacks: html-tailwind (default), react, nextjs, astro, vue, nuxtjs,
#         nuxt-ui, svelte, swiftui, react-native, flutter, shadcn, jetpack-compose

# Generate a full design system (ASCII output)
python3 src/ui-ux-pro-max/scripts/search.py "beauty spa wellness" --design-system -p "Project Name"

# Generate a full design system (Markdown output)
python3 src/ui-ux-pro-max/scripts/search.py "fintech banking" --design-system -f markdown

# Persist design system to files (Master + Overrides pattern)
python3 src/ui-ux-pro-max/scripts/search.py "SaaS dashboard" --design-system --persist -p "MyApp"
python3 src/ui-ux-pro-max/scripts/search.py "SaaS dashboard" --design-system --persist -p "MyApp" --page "dashboard"
# Creates: design-system/MASTER.md and design-system/pages/<page>.md
```

### CLI (npm)

```bash
# Install CLI globally
npm install -g uipro-cli

# Install skill for a specific AI assistant
uipro init --ai claude      # Claude Code
uipro init --ai cursor      # Cursor
uipro init --ai windsurf    # Windsurf
uipro init --ai copilot     # GitHub Copilot
uipro init --ai kiro        # Kiro
uipro init --ai roocode     # Roo Code
uipro init --ai codex       # Codex CLI
uipro init --ai gemini      # Gemini CLI
uipro init --ai trae        # Trae
uipro init --ai opencode    # OpenCode
uipro init --ai continue    # Continue
uipro init --ai qoder       # Qoder
uipro init --ai codebuddy   # CodeBuddy
uipro init --ai droid       # Droid (Factory)
uipro init --ai all         # All assistants
uipro init --offline        # Use bundled assets, skip GitHub download

uipro versions              # List available versions
uipro update                # Update to latest version
```

### CLI Build (Bun)

```bash
cd cli
bun run build                                        # Compile TypeScript to dist/
node dist/index.js init --ai claude --offline        # Test locally
```

### Claude Marketplace Installation

```
/plugin marketplace add nextlevelbuilder/ui-ux-pro-max-skill
/plugin install ui-ux-pro-max@ui-ux-pro-max-skill
```

---

## Architecture

### Search Engine

The search engine (`src/ui-ux-pro-max/scripts/core.py`) uses **BM25 ranking combined with regex matching**. Domain auto-detection is available when `--domain` is omitted.

### Design System Generation

The design system engine (`src/ui-ux-pro-max/scripts/design_system.py`) performs 5 parallel searches:
1. Product type matching (161 categories)
2. Style recommendations (67 styles)
3. Color palette selection (161 palettes)
4. Landing page patterns (24 patterns)
5. Typography pairing (57 font combinations)

Output includes: Pattern, Style, Colors, Typography, Key Effects, Anti-patterns, and a Pre-delivery checklist.

### Persist / Hierarchical Retrieval Pattern

When `--persist` is used, the generated design system is saved to:
- `design-system/MASTER.md` — Global source of truth
- `design-system/pages/<page>.md` — Page-specific overrides

When building a specific page, check for the page override first; if it exists, its rules take priority over MASTER.md.

---

## Sync Rules (Source of Truth: `src/ui-ux-pro-max/`)

1. **Data & Scripts** — Edit only in `src/ui-ux-pro-max/`. Changes are automatically available via symlinks in `.claude/`, `.factory/`, `.shared/`.

2. **Templates** — Edit in `src/ui-ux-pro-max/templates/`.

3. **CLI Assets** — Must be manually synced before publishing to npm:
   ```bash
   cp -r src/ui-ux-pro-max/data/* cli/assets/data/
   cp -r src/ui-ux-pro-max/scripts/* cli/assets/scripts/
   cp -r src/ui-ux-pro-max/templates/* cli/assets/templates/
   ```

4. **Platform folders** (`.cursor/`, `.windsurf/`, etc.) — Never edit manually. The CLI generates these from templates during `uipro init`.

---

## Prerequisites

- **Python 3.x** — Required for the search engine. No external Python packages needed.
- **Bun** — Required for building the CLI (`cd cli && bun run build`).
- **Node.js / npm** — Required to publish or install `uipro-cli`.

---

## Git Workflow

Never push directly to `main`. Always:

1. Create a new branch: `git checkout -b feat/<name>` or `fix/<name>`
2. Commit changes with a descriptive message
3. Push branch: `git push -u origin <branch>`
4. Open a PR: `gh pr create`

---

## Supported Platforms & Stacks

### AI Assistants (15)

Claude Code, Cursor, Windsurf, Antigravity, GitHub Copilot, Kiro, Codex CLI, Qoder, Roo Code, Gemini CLI, Trae, OpenCode, Continue, CodeBuddy, Droid (Factory)

### Tech Stacks (13)

| Category | Stacks |
|---|---|
| Web (HTML) | HTML + Tailwind (default) |
| React Ecosystem | React, Next.js, shadcn/ui |
| Vue Ecosystem | Vue, Nuxt.js, Nuxt UI |
| Other Web | Svelte, Astro |
| iOS | SwiftUI |
| Android | Jetpack Compose |
| Cross-Platform | React Native, Flutter |

---

## Design Database Summary

| Asset | Count |
|---|---|
| UI Styles | 67 |
| Color Palettes | 161 |
| Font Pairings | 57 |
| Industry Reasoning Rules | 161 |
| Landing Page Patterns | 24 |
| Chart Types | 25 |
| UX Guidelines | 99 |
| Supported Tech Stacks | 13 |
