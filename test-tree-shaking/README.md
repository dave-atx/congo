# CSS Tree-Shaking Test Site

This test site verifies that Tailwind v4's tree-shaking works correctly with Congo's `@utility` blocks.

## Test Pages

| Page | Features | Expected CSS |
|------|----------|--------------|
| `/baseline/` | Plain text only | Minimal CSS, no Chroma/ToC/icon styles |
| `/with-code/` | Code blocks | Chroma CSS + copy-button utilities |
| `/with-toc/` | Table of contents | ToC utility + descendant styles |
| `/with-icons/` | Icons | Icon utility |

## Running Tests

```bash
# Build the test site
hugo --source test-tree-shaking --themesDir ../..

# Check output
ls -la test-tree-shaking/public/

# Compare CSS for specific utilities
grep -c "\.icon" test-tree-shaking/public/css/*.css
grep -c "\.toc" test-tree-shaking/public/css/*.css
grep -c "\.chroma" test-tree-shaking/public/css/*.css
```

## Expected Results

1. **Baseline page**: Should have minimal CSS without feature-specific styles
2. **Code page**: Should include Chroma syntax highlighting
3. **ToC page**: Should include `.toc` utility styles
4. **Icons page**: Should include `.icon` utility styles

## Verifying Tree-Shaking

Compare the `hugo_stats.json` file generated during build to see which classes were detected:

```bash
cat test-tree-shaking/hugo_stats.json | jq '.htmlElements.classes' | grep -E "(icon|toc|highlight|copy)"
```
