---
name: ui-cartography
description: 'Audit a web application viewport, build a semantic DOM structural map, identify layout bugs (letterboxing, collapse, cramped padding, jumping interfaces), and rewrite parent containers to lock spatial dimensions. Used by the design-architect agent for systematic UI refactoring. USE FOR: UI audits, layout debugging, DOM cartography, structural refactoring, viewport analysis.'
user-invocable: true
---

# Automated UI Cartography & Structural Refactoring

> **ID:** `skill_ui_systems_architect` &nbsp;|&nbsp; **Version:** 1.0.0 &nbsp;|&nbsp; **Type:** Execution & Analysis

## Context & Purpose
This skill arms the Design Architect agent with the ability to audit a living web application or desktop window viewport, build a semantic structural map of the active DOM, identify layout bugs (structural collapse, letterboxing, cramped padding, jumping interfaces), and rewrite parent layout containers to lock spatial dimensions. It bridges the gap between visual diagnosis and code-level remediation by producing a complete audit trail before any CSS is touched.

## Phase 1: Operational DOM Cartography

Before writing layout code or proposing CSS alterations, the agent must inspect the active layout landscape. Never refactor from memory — always map first.

### 1.1 Viewport Extraction
Read the outermost layout nodes. Identify if the root container lacks explicit height/width metrics — this is the #1 cause of letterboxing and background bleeding.

**Diagnostic checklist:**
- [ ] Does `<body>` or the root `<div>` carry `width: 100vw; height: 100vh;`?
- [ ] Is `margin: 0` explicitly set on body? (Browser defaults inject ~8px.)
- [ ] Is `overflow: hidden` applied to the root viewport container to suppress unwanted scrollbars?
- [ ] Does the CSS background color on `<body>` match the Electron `BrowserWindow` `backgroundColor` exactly?
- [ ] For frameless windows: does `<body>` carry `border-radius` matching the window's corner radius?

### 1.2 Node Filtering — Structural vs. Presentational
Strip out purely passive presentation elements. Map only the **structural container wrappers** that define the layout skeleton.

| Keep (Structural) | Ignore (Presentational) |
|-------------------|------------------------|
| `.app`, `.layout`, `.card` | `<span>`, text-only nodes |
| `.tabs`, `.tab-panel` | `<img>`, `<svg>`, icons |
| `.header`, `.footer` | Decorative dividers |
| `.balance-row`, `.qr-container` | Color-only wrappers |
| Flex containers, grid containers | Passive `<div>` with no structural rules |
| Fixed-height / min-height elements | Absolutely positioned overlays (unless they affect flow) |

**Output:** A flat list of structural nodes with their CSS layout properties (display mode, width, height, min-height, padding, margin, border, overflow).

### 1.3 Metric Auditing — The Vertical Budget
Log bounding boxes, padding gaps, margin weights, and vertical spacing values for every structural node. Sum them to verify the layout fits within the viewport.

**Audit template (fill for every structural node):**
```
Element: .card-wrapper
  display: flex (column)
  width: 100vw (inherits from body)
  height: 100vh (inherits from body)
  padding: 16px (top) + 16px (bottom) = 32px vertical
  margin: 0
  border: none
  overflow: hidden

Element: .card
  min-height: 580px
  padding: 24px (top) + 24px (bottom) = 48px vertical
  margin: 0 (centered by flexbox parent)
  border: 1px solid var(--border) = 2px vertical
  Total vertical consumption: 630px

Element: .tab-bar
  height: 44px
  margin-bottom: 16px
  Total vertical: 60px

Cumulative inside .card: 48px (card padding) + 60px (tab bar) + 472px (tab content) = 580px
Matches min-height: 580px ✓

Viewport check: 580px + 32px (wrapper padding) = 612px
Window height: 700px → 88px of breathing room ✓
```

**Critical flag:** Note any child components that trigger dynamic, shifting height updates to parent nodes — these are the source of "jumping" interfaces.

## Phase 2: Systematic Refactoring Logic

### Rule 1: Window Letterbox Eradication
If the outer window boundaries show unstyled dead spaces (letterboxing, black bars), target the absolute root node. This is always fixed at the `<body>` or root `<div>` level — never patched on a child.

```css
/* The root lockdown — applies to body or the app mount point */
body,
#root,
.app {
  width: 100vw;
  height: 100vh;
  margin: 0;
  padding: 0;
  overflow: hidden;
  background: var(--color-canvas); /* MUST match Electron backgroundColor */
}

/* Tailwind equivalent */
<body class="w-screen h-screen m-0 p-0 overflow-hidden bg-[#0b0c10]">
```

**Verification:** After applying, inspect the page at every supported window dimension. There must be zero pixels of unstyled dead space at any edge.

### Rule 2: Structural Height Locking
When child components have variable content (e.g., Wallet tab vs Relay tab, or a balance view vs an empty state):

```css
.card {
  min-height: 580px; /* locks all child states to identical visual footprint */
  /* When content exceeds 580px, the card grows naturally — but never shrinks below */
}
```

**Tailwind:** `min-h-[580px]`

**Decision tree for centering vs. top-down flow:**

```
Is content taller than viewport?
├── YES → flex-direction: column; justify-content: flex-start;
│         (content flows top-down, scrolls naturally)
└── NO  → display: flex; align-items: center; justify-content: center;
          (card floats in geometric center, canvas absorbs empty space)

Do sibling tabs have unequal content?
├── YES → Lock min-height on the shared card wrapper
│         Use `display: none` / hidden toggle (not opacity or visibility)
└── NO  → min-height: unset (window compacts naturally)
```

### Rule 3: Padding Distribution — The Breathing Room Scale
Apply consistent spacing that creates intentional hierarchy without bloat:

| Context | Value | Rationale |
|---------|-------|-----------|
| Outer viewport wrapper | `padding: 16px` | Balanced breathing room between canvas edge and card |
| Card internal | `padding: 24px` | Generous interior space for content readability |
| Between major sections | `margin-bottom: 16-20px` | Clear visual separation |
| Form input stacks | `margin-bottom: 12px` | Tight but distinct |
| Compact elements (buttons, labels, badges) | `margin: 4-6px` | Minimal grouping |
| Dividers / separators | `margin: 14px 0` | Balanced vertical rhythm |

**Anti-pattern:** Inconsistent padding (e.g., 16px left, 24px right) without intentional asymmetry. If it's not deliberate, make it symmetric.

### Rule 4: Frameless Window Polish (Electron)
For Electron apps with `frame: false` and `transparent: true`:

```css
body {
  border-radius: 16px;
  background: transparent; /* Electron draws the window chrome */
}

.app {
  background: var(--color-canvas); /* #0b0c10 */
  border-radius: 16px;
  overflow: hidden; /* clips content to rounded corners */
}
```

**Critical:** The `backgroundColor` property on `new BrowserWindow({...})` must be an exact hex match to `var(--color-canvas)` in CSS. Mismatch causes visible corner artifacts.

### Rule 5: Drag Region Integrity
For frameless Electron windows, the user must be able to move the window:

```css
.drag-region {
  -webkit-app-region: drag;
}

/* EVERY interactive element inside a drag region must opt out */
button, input, select, a, .no-drag {
  -webkit-app-region: no-drag;
}
```

**Rule of thumb:** Card headers and empty padding areas → `drag`. Buttons, inputs, links, dropdowns → `no-drag`. If the user can't move the window, the drag region is too small or overridden by a child `no-drag` that covers it.

## Phase 3: Delivery Checklist

Before shipping any layout change, verify every item:

- [ ] **No scrollbar** on main views — `overflow: hidden` on the root viewport container
- [ ] **No letterboxing** — zero pixels of dead space at any window edge, at any supported dimension
- [ ] **No layout jump** — switching tabs, toggling states, or changing data never resizes the parent card
- [ ] **Window sizing** — compact on lock screen (`min-height: unset`), expanded when unlocked
- [ ] **All 4 corners rounded** — `border-radius: 16px` on body for frameless windows
- [ ] **Close button functional** — visible, tappable, no drag region overlap
- [ ] **Drag region adequate** — enough `-webkit-app-region: drag` surface to move the window comfortably
- [ ] **No orphaned text** — zero safety badges, instructional labels, or redundant metadata
- [ ] **Palette compliance** — no colors outside the defined visual palette
- [ ] **Complete containment chain** — every parent from root to leaf has explicit layout constraints

## Quick Reference: Common Fixes

| Symptom | Root Cause | Fix |
|---------|-----------|-----|
| Black bars top/bottom | Body height ≠ window height | `height: 100vh; margin: 0` on body |
| Scrollbar on main view | Content overflows viewport | `overflow: hidden` on root, or audit vertical budget |
| Layout jump on tab switch | Unequal card heights across tabs | Lock `min-height` on shared `.card` wrapper |
| Sharp window corners | No border-radius on frameless window | `border-radius: 16px` on body |
| Can't drag window | No drag region or region overridden | `-webkit-app-region: drag` on header, `no-drag` on children |
| Letterboxing on lock screen | Card centered in oversized window | Dynamic window resize: compact for locked, expanded for unlocked |
| Background bleeding at edges | CSS bg ≠ Electron backgroundColor | Match hex values exactly between CSS and BrowserWindow |
| Corner artifacts (frameless) | Content not clipped to border-radius | `overflow: hidden` on `.app` wrapper |
| Elements shift on state change | No min-height on variable-content container | `min-height: [locked-value]` on parent card |
