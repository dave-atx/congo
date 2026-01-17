# CSS Tree-Shaking Refactoring Progress

## Overview
Rewriting `assets/css/main.css` to maximize tree-shakeability with Tailwind v4.

## Tasks

### Phase 1: Utility Conversions
- [x] Convert `.icon svg` to `@utility icon`
- [x] Convert `.toc` base styles to `@utility toc`
- [x] Convert `.highlight-wrapper` to `@utility highlight-wrapper`
- [x] Convert `.highlight` base to `@utility highlight`
- [x] Convert `.copy-button` to `@utility copy-button`
- [x] Convert `.copy-textarea` to `@utility copy-textarea`
- [x] Convert `.katex-display` to `@utility katex-display`
- [x] Convert `.chroma` and all child selectors to `@utility chroma`

### Phase 2: File Organization
- [x] Reorganize main.css with clear sections
- [x] Add comments explaining why raw selectors cannot be converted
- [x] Use idiomatic `@import "tailwindcss" source(none)` syntax
- [x] Add configurable publish directory via Hugo template

### Phase 3: Testing
- [x] Create test-tree-shaking site (moved to separate repo)
- [x] Verify baseline (no features) has minimal CSS
- [x] Verify code blocks include Chroma CSS
- [x] Verify ToC pages include ToC CSS
- [x] Verify icons include icon CSS
- [x] Verify `.chroma .gl` generates `text-decoration-line: underline`

### Phase 4: Documentation
- [x] Document all conversions in CSS-CHANGES.md
- [x] Document limitations and why some CSS can't be tree-shaken
- [x] Add testing instructions
- [x] Add color comparison table (RGB to OKLCH) as PR comment

### Phase 5: PR
- [x] Open PR to dave-atx/congo:tailwind4
- [x] Include before/after analysis
- [x] Address all review comments
- [x] Mark review comments as resolved

## Review Comments Addressed (PR #11)

All 11 review comments have been resolved:

1. **Scheme files**: Use `oklch(100% 0 0)` for `--color-neutral`
2. **Scheme files**: Use constant hue per palette
3. **main.css**: Avoid reordering CSS
4. **main.css**: Change source file configuration
5. **main.css**: Move RTL support to `@utility prose`
6. **main.css**: Convert `.katex-display` to `@utility`
7. **main.css**: Convert `.chroma` to `@utility`
8. **main.css**: Use `@import "tailwindcss" source(none)`
9. **main.css**: Add Hugo template for configurable publish directory
10. **main.css**: Verify `.gl` underline CSS generation

## Notes
- Chroma syntax highlighting now uses `@utility chroma` with nested selectors
- RTL prose overrides moved inside `@utility prose`
- All scheme files use OKLCH colors with consistent hues per palette
- Publish directory is configurable via `site.Params.publishDir`
