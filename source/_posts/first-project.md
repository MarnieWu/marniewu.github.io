---
title: First Project
date: 2024-11-01 16:06:44
tags: 
   - setup
---

## Getting Started

安装 anchor —— Solana 智能合约框架

1.用 cargo 安装 avm ，Anchor 版本管理器

```bash
cargo install --git https://github.com/coral-xyz/anchor --tag v0.30.1 anchor-cli
```

2.使用 avm 安装最新版本 anchor

```bash
avm install latest
```

3.检查安装好的版本

```bash
anchor --version
```

## Trouble Shooting

1. `error: no such command: build-sbf`

   ```bash
   cargo build
   cargo build-sbf --force-tools-install
   ```

2.
