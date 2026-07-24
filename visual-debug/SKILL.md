---
name: visual-debug
description: "Visual debug of web interfaces via screenshots with Playwright MCP. Trigger words: debug visual, screenshot, print visual, capturar tela, debug visually, visual test, take screenshot."
---

# 📸 Visual Debug Skill

## When to Use

Activate when you need to validate visually:
- Page layouts (desktop/mobile)
- Responsiveness
- Colors and typography
- Component states (hover, active, disabled)
- Deploy verification (before/after)

## Core Tools

**Playwright MCP** (23 tools):
- `playwright_browser_navigate` — navigate to URL
- `playwright_browser_take_screenshot` — capture viewport
- `playwright_browser_snapshot` — a11y snapshot (alternative)
- `playwright_browser_resize` — resize viewport (mobile test)

## Procedure

### 1. Navigate to Page

```bash
playwright_browser_navigate(url: "http://localhost:3000")
```

### 2. Take Screenshot

```bash
playwright_browser_take_screenshot(
  type: "png",
  filename: "tmp/<description>.png"
)
```

### 3. (Optional) Resize for Mobile

```bash
playwright_browser_resize(width: 375, height: 812)  # iPhone
playwright_browser_take_screenshot(filename: "tmp/mobile.png")
```

## File Structure

Screenshots saved directly in:

```
/<project>/
├── tmp/                   # Screenshots (do not commit)
│   ├── homepage.png
│   └── mobile.png
└── .gitignore             # tmp/ listed
```

**.tmp/** is listed in `.gitignore`.

## Completion Criteria

Visual debug **complete** when:
- [ ] Screenshot captured successfully
- [ ] File saved in `tmp/`
- [ ] User can view and validate

## Important Notes

- **ALWAYS** save to `tmp/` (relative to project)
- **ALWAYS** include timestamp in filename for versioning
- **NEVER** commit screenshots (protected by `.gitignore`)
- **ALWAYS** verify server is running before navigating
- For mobile: use `playwright_browser_resize` before screenshot
- For hover states: use `playwright_browser_hover` before screenshot

## Examples

### Debug homepage layout

```
1. playwright_browser_navigate(url: "http://localhost:3000")
2. playwright_browser_take_screenshot(filename: "tmp/homepage.png")
```

### Test responsiveness

```
1. playwright_browser_navigate(url: "http://localhost:3000")
2. playwright_browser_resize(width: 375, height: 812)
3. playwright_browser_take_screenshot(filename: "tmp/iphone.png")
4. playwright_browser_resize(width: 1440, height: 900)  # back to desktop
```

### Validate deploy

```
1. playwright_browser_navigate(url: "https://gabrielandmariana.vercel.app")
2. playwright_browser_take_screenshot(filename: "tmp/prod.png")
```
