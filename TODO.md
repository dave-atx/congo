# CSS Tree-Shaking Refactoring Progress

## Overview
Rewriting `assets/css/main.css` to maximize tree-shakeability with Tailwind v4.

## Tasks

### Phase 1: Utility Conversions
- [ ] Convert `.icon svg` to `@utility icon`
- [ ] Convert `.toc` base styles to `@utility toc`
- [ ] Convert `.highlight-wrapper` to `@utility highlight-wrapper`
- [ ] Convert `.highlight` base to `@utility highlight`
- [ ] Convert `.copy-button` to `@utility copy-button`
- [ ] Convert `.copy-textarea` to `@utility copy-textarea`

### Phase 2: File Organization
- [ ] Reorganize main.css with clear sections
- [ ] Add comments explaining why raw selectors cannot be converted

### Phase 3: Testing
- [ ] Create test-tree-shaking site
- [ ] Verify baseline (no features) has minimal CSS
- [ ] Verify code blocks include Chroma CSS
- [ ] Verify ToC pages include ToC CSS
- [ ] Verify icons include icon CSS

### Phase 4: Documentation
- [ ] Document all conversions in CSS-CHANGES.md
- [ ] Document limitations and why some CSS can't be tree-shaken
- [ ] Add testing instructions

### Phase 5: PR
- [ ] Open PR to dave-atx/congo:tailwind4
- [ ] Include before/after analysis

## Notes
- Chroma syntax highlighting (~200 lines) CANNOT be converted - targets Hugo-generated classes
- RTL prose overrides MUST remain raw - need specificity to override typography plugin
- Hamburger menu uses CSS state selectors (:has, :checked) - cannot be utilities
