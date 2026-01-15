# CSS Tree-Shaking Changes for Tailwind v4

This document tracks all CSS changes made to `assets/css/main.css` to improve tree-shakeability with Tailwind CSS v4.

## How Tree-Shaking Works in Congo

1. Hugo generates `hugo_stats.json` containing all CSS classes used in templates
2. Tailwind reads this via `@source "hugo_stats.json"` directive
3. CSS defined with `@utility` blocks is only included if the class name appears in `hugo_stats.json`
4. Raw CSS selectors (e.g., `.class { }`) are ALWAYS included

## Converted to @utility (Tree-Shakeable)

### `@utility icon`
**Original:**
```css
.icon svg {
  @apply h-[1em] w-[1em];
}
```

**Converted:**
```css
@utility icon {
  svg {
    height: 1em;
    width: 1em;
  }
}
```

**Justification:** The `.icon` class is explicitly used in `layouts/_partials/icon.html`. Converting to `@utility` means icon styles are only included when icons are used on a page.

**Git History:** Added in initial commit (Aug 2021) - core icon styling that scales SVGs to text size.

---

### `@utility toc`
**Original:**
```css
.toc {
  max-height: 100vh;
  overflow-y: auto;
  padding-bottom: 50px;
}
```

**Converted:**
```css
@utility toc {
  max-height: 100vh;
  overflow-y: auto;
  padding-bottom: 50px;
}
```

**Justification:** The `.toc` class is applied in `layouts/_partials/toc.html`. Converting to `@utility` means ToC container styles are only included when ToC is enabled.

**Git History:**
- Added Jan 2022 (commit `acc4aee7`) - initial ToC feature
- Scrolling added Dec 2023 (commit `da0ea762`) - for long ToCs

**Note:** Descendant selectors (`.toc ul`, `.toc a`, etc.) must remain raw because they target Hugo-generated HTML inside the ToC.

---

### `@utility highlight-wrapper`
**Original:**
```css
.highlight-wrapper {
  @apply block;
}
```

**Converted:**
```css
@utility highlight-wrapper {
  display: block;
}
```

**Justification:** Class created by `assets/js/code.js` for code block copy functionality.

**Git History:** Added Jan 2022 (commit `add3f764`) - code copy button feature.

---

### `@utility highlight`
**Original:**
```css
.highlight {
  @apply relative z-0;
}
```

**Converted:**
```css
@utility highlight {
  position: relative;
  z-index: 0;
}
```

**Justification:** Hugo generates `.highlight` wrapper for code blocks. Converting allows styles to be excluded on pages without code.

**Note:** The `.highlight:hover > .copy-button` rule must remain raw (parent-child hover relationship).

---

### `@utility copy-button`
**Original:**
```css
.copy-button {
  @apply invisible absolute right-0 top-0 z-10 w-20 cursor-pointer whitespace-nowrap rounded-bl-md rounded-tr-md bg-neutral-200 py-1 font-mono text-sm text-neutral-700 opacity-90 dark:bg-neutral-600 dark:text-neutral-200;
}
.copy-button:hover,
.copy-button:focus,
.copy-button:active,
.copy-button:active:hover {
  @apply bg-primary-100 dark:bg-primary-600;
}
```

**Converted:**
```css
@utility copy-button {
  @apply invisible absolute right-0 top-0 z-10 w-20 cursor-pointer whitespace-nowrap rounded-bl-md rounded-tr-md bg-neutral-200 py-1 font-mono text-sm text-neutral-700 opacity-90 dark:bg-neutral-600 dark:text-neutral-200;

  &:hover,
  &:focus,
  &:active,
  &:active:hover {
    @apply bg-primary-100 dark:bg-primary-600;
  }
}
```

**Justification:** Class created by `assets/js/code.js`. Includes hover/focus states using nested selectors.

---

### `@utility copy-textarea`
**Original:**
```css
.copy-textarea {
  @apply absolute -z-10 opacity-5;
}
```

**Converted:**
```css
@utility copy-textarea {
  @apply absolute -z-10 opacity-5;
}
```

**Justification:** Class created by `assets/js/code.js` for the hidden textarea used in copy functionality.

---

## Cannot Convert (Must Remain Raw)

### Global Link/Button Transitions
```css
body a,
body button {
  @apply transition-colors;
}
```
**Reason:** Targets ALL `<a>` and `<button>` elements globally. Cannot be an opt-in utility class.

---

### Search Input Pseudo-elements
```css
#search-query::-webkit-search-cancel-button,
#search-query::-webkit-search-decoration,
#search-query::-webkit-search-results-button,
#search-query::-webkit-search-results-decoration {
  @apply hidden;
}
```
**Reason:** Uses ID selector with browser-specific pseudo-elements. `@utility` doesn't support ID selectors or vendor prefixes.

---

### Hamburger Menu State
```css
body:has(#menu-controller:checked) {
  @apply h-screen overflow-hidden;
}
#menu-button:has(#menu-controller:checked) {
  @apply invisible;
}
#menu-controller:checked ~ #menu-wrapper {
  @apply visible opacity-100;
}
```
**Reason:** CSS-only state management using `:has()`, `:checked`, and sibling combinators. These are structural selectors that cannot be utilities.

---

### RTL Prose Overrides
```css
.prose blockquote {
  @apply rtl:border-l-0 rtl:border-r-4 rtl:pr-4;
}
/* ... more RTL rules ... */
```
**Reason:** Must override `@tailwindcss/typography` plugin defaults. Requires `.prose` descendant selectors for proper specificity.

---

### Prose First-Child
```css
.prose div.min-w-0.max-w-prose > *:first-child {
  @apply mt-3;
}
```
**Reason:** Uses compound class selector with `:first-child` pseudo-class targeting dynamic content.

---

### ToC Descendant Styles
```css
.toc ul, .toc li { @apply list-none px-0 leading-snug; }
.toc ul ul { @apply ps-4; }
.toc a { @apply font-normal text-neutral-700 dark:text-neutral-400; }
.toc ul > li { @apply rtl:mr-0; }
```
**Reason:** Hugo's `.TableOfContents` generates nested `<ul>` and `<li>` without controllable classes.

---

### Highlight Hover Interaction
```css
.highlight:hover > .copy-button {
  @apply visible;
}
```
**Reason:** Parent-child hover relationship cannot be expressed as a single utility.

---

### KaTeX Overflow
```css
.katex-display {
  overflow: auto hidden;
}
```
**Reason:** `.katex-display` is generated by the KaTeX library, not theme templates.

---

### Table/Code Responsive Fixes
```css
table {
  @apply block overflow-auto md:table;
}
code {
  word-wrap: break-word;
  @apply break-words;
}
```
**Reason:** Targets raw HTML elements generated by Hugo's Markdown renderer.

---

### Chroma Syntax Highlighting (~200 lines)
```css
.chroma { ... }
.chroma .k { ... }
.chroma .s { ... }
/* ... many more token classes ... */
```
**Reason:** Hugo's Chroma syntax highlighter generates these class names. They cannot be modified by the theme.

---

## Testing Tree-Shaking

Use the `test-tree-shaking/` directory to verify:

```bash
# Build test site
hugo --source test-tree-shaking --themesDir ../..

# Check which utilities are included
grep -c "@utility icon" public/css/*.css
grep -c ".toc" public/css/*.css
```

### Test Cases
1. **Baseline page** (no code, no ToC, no icons) - Should have minimal CSS
2. **Code page** - Should include Chroma and copy-button styles
3. **ToC page** - Should include toc styles
4. **Icon page** - Should include icon styles

---

## Guidelines for Custom CSS

When adding custom CSS to Congo:

1. **Prefer `@utility` blocks** for styles tied to specific classes you control
2. **Use raw selectors** only when targeting:
   - Library-generated classes (KaTeX, Mermaid, etc.)
   - Markdown-generated elements
   - Pseudo-elements or state selectors
3. **Document why** if you must use raw selectors

Example custom utility:
```css
/* In assets/css/custom.css */
@utility my-custom-card {
  @apply rounded-lg shadow-md p-4;
}
```
