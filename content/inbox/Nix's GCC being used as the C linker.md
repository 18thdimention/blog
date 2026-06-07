---
goal:
tags:
  - bug
created: 2026-05-20T23:50
---
Add this to `.cargo/config.toml`
```toml
[target.aarch64-apple-darwin]
linker = "/usr/bin/clang"
```