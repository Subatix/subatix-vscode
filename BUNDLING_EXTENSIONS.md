# Bundling Extensions in Subatix

## Quick Start: Post-Build Copy

The fastest way to bundle a pre-built extension:

```bash
# 1. Extract VSIX
unzip my-extension.vsix -d /tmp/ext-temp

# 2. Copy to built app (macOS)
cp -r /tmp/ext-temp/extension \
  /Users/vladimirrozhkov/Subatix_dev/vscodium/VSCode-darwin-arm64/Subatix.app/Contents/Resources/app/extensions/my-extension
```

**Paths:**
- Source repo: `/Users/vladimirrozhkov/Subatix_dev/vscodium/vscode/`
- Built app (macOS): `/Users/vladimirrozhkov/Subatix_dev/vscodium/VSCode-darwin-arm64/Subatix.app`
- Extensions folder: `.../Subatix.app/Contents/Resources/app/extensions/`

**⚠️ Caveat:** You must re-copy after every rebuild.

---

## Key Findings

### VSIX = Pre-Built Extension
- VSIX files are just ZIP archives
- Inside: `extension/` folder with `package.json` + `dist/` or `out/` (compiled JS)
- No `npm install` or compilation needed — already bundled

### No node_modules Required
Published VSIX extensions are **fully self-contained**:
- esbuild/webpack bundles all dependencies INTO `extension.js`
- The `package.json` lists deps for development, not runtime
- You won't find `node_modules/` in extracted VSIX — it's not needed
- Exception: Native modules (`.node` files) may be included separately

This is different from **source extensions** in the `extensions/` folder which:
- Need `yarn install` during build
- Have their deps resolved by VSCode's build system
- Are compiled from TypeScript to JavaScript

### Why Build Process Ignores Pre-Built Extensions
VSCode's `build/lib/extensions.ts` scans `extensions/*/package.json` but expects:
- Webpack/esbuild config for compilation
- Source TypeScript files

Pre-built extensions have neither. The build skips them.

### Built-in vs Installed Extensions
- **Built-in:** Bundled with app, shown with `@builtin` filter, cannot uninstall
- **Installed:** User-installed from marketplace, shows in "INSTALLED" count

---

## Making It Permanent

To auto-include extensions in builds, modify:

1. **`build/gulpfile.vscode.js`** — Add post-package copy task
2. **Or** create a `scripts/bundle-extensions.sh` that runs after `yarn gulp vscode-darwin-arm64`

The build already handles `extensions/` folder scanning. For pre-built extensions, a simple file copy into the output is sufficient.

---

## Current Bundled Extensions

| Extension | Size | Type |
|-----------|------|------|
| `theme-subatix` | ~10KB | Theme (source) |
| `vscode-office` | ~7MB | Office viewer (pre-built) |
| `subatix-agent` | ~52MB | AI Agent (pre-built) |

---

## TL;DR

1. **VSIX = ZIP** → Extract and copy `extension/` folder
2. **Pre-built = ready** → No npm install needed
3. **Post-build copy** → Quick but temporary
4. **Permanent** → Modify gulp tasks or add post-build script

