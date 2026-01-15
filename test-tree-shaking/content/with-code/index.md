---
title: "Code Block Test"
description: "Page with code blocks to test Chroma CSS inclusion"
showCodeCopyButtons: true
---

This page contains code blocks which should trigger inclusion of:
- Chroma syntax highlighting CSS
- Code copy button CSS

## JavaScript Example

```javascript
function greet(name) {
  console.log(`Hello, ${name}!`);
  return true;
}

const result = greet("World");
```

## Python Example

```python
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)

print(factorial(5))
```

## Go Example

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```
