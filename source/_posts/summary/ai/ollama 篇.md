---
title: ollama 篇
date: 2026-05-26T13:36:35.000Z
tags:
  - 汇总
  - ollama
categories:
  - 汇总
  - ai
uuid: e094521d-2380-4172-9495-1bf9076a8cec
---
ollama 官网 [https://ollama.com/](https://ollama.com/)
ollama 阿里云镜像 [https://registry.ollama.ai/](https://registry.ollama.ai/)

配置阿里镜像

```bash
mkdir -p ~/.ollama
cat << EOF > ~/.ollama/config.json
{
    "registry": {
        "mirrors": {
            "registry.ollama.ai": "https://registry.ollama.ai"
        }
    }
}
EOF
```
