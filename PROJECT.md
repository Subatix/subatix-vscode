# Subatix

**Privacy-first VSCode fork with built-in AI agent.**

## What is Subatix?

A custom fork of Visual Studio Code with:
- **VSCodium patches** — No telemetry, no Microsoft tracking, extensions from Open VSX
- **Custom branding** — Subatix icons, themes, and product identity
- **Pre-bundled AI** — Subatix Agent included out-of-the-box
- **Open source** — MIT licensed, no proprietary components

## Key Features

- ✅ **Privacy:** All telemetry removed, VSCodium marketplace patches applied
- ✅ **Extensions:** Open VSX marketplace instead of Microsoft's
- ✅ **Built-in Agent:** AI coding assistant pre-installed
- ✅ **Office Support:** View Excel/Word files directly in editor
- ✅ **Custom Theme:** Subatix Dark/Light themes

## Repository Structure

- **Upstream:** `Microsoft/vscode` (pull VSCode updates)
- **Origin:** `Subatix/subatix-vscode` (our fork)
- **Extensions:** VSCodium infrastructure (GitHub releases, not marketplace)

## Quick Start

```bash
# Install dependencies
yarn install

# Development mode
yarn watch  # Terminal 1
# Press F5 with "VS Code (Debug Observables)" config

# Production build (macOS ARM64)
yarn gulp vscode-darwin-arm64

# Package for distribution
cd VSCode-darwin-arm64
zip -r Subatix-darwin-arm64.zip Subatix.app
```

## Documentation

- `BUNDLING_EXTENSIONS.md` — How to add pre-built extensions
- `product.json` — All branding and configuration
- `build/` — Build scripts and gulp tasks

---

**Built with ❤️ for privacy-focused developers**


