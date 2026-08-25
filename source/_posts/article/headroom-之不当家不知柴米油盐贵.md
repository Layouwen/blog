---
title: headroom 之不当家不知柴米油盐贵
uuid: 576f7358-9950-4fc2-851f-b154b5f4ddf1
date: 2026-07-02
tags:
  - headroom
  - 博客
categories:
  - headroom
  - 博客
---
```bash
mkdir headroom-start
uv init --bare
uv python pin 3.14.6
uv add "headroom-ai[proxy]"
vim .env
```

.env

```bash
HEADROOM_HOST=127.0.0.1
HEADROOM_PORT=8787
HEADROOM_MODE=optimize
HEADROOM_LOG_LEVEL=INFO
HEADROOM_TELEMETRY=off

# 自定义模型提供商的 baseUrl
# 火山云
# ANTHROPIC_TARGET_API_URL=https://ark.cn-beijing.volces.com/api/coding
# deepseek
# ANTHROPIC_TARGET_API_URL=https://api.deepseek.com/anthropic
# http://127.0.0.1:4000
# ANTHROPIC_TARGET_API_URL=http://127.0.0.1:4000
```

```bash
mkdir -p scripts logs data
vim scripts/start.sh
```

start.sh

```bash
#!/usr/bin/env bash

set -euo pipefail

PROJECT_DIR="$(cd "$(dirname "$0")/.." && pwd)"
cd "$PROJECT_DIR"

if [[ -f ".env" ]]; then
  set -a
  source .env
  set +a
fi

exec uv run headroom proxy \
  --host "${HEADROOM_HOST:-127.0.0.1}" \
  --port "${HEADROOM_PORT:-8787}"
```

启动服务

```bash
chmod +x scripts/start.sh
./scripts/start.sh
```

验证

```bash
curl http://127.0.0.1:8787/health

curl http://127.0.0.1:8787/stats
```

使用 claude, 需要用 headroom 来 wrap 一下

配置 .zshrc 函数

```bash
hclaude() {

  cd "$HOME/Services/headroom" &&

  uv run headroom wrap claude

}
```

启动 claude

```bash
hclaude
```

