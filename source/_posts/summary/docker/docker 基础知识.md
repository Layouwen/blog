---
title: docker 基础知识
date: 2022-01-23T15:22:00.000Z
tags:
  - 汇总
  - docker
categories:
  - 汇总
  - docker
uuid: 3c1eb364-069d-4376-9e40-d95734e1b175
---

## CMD

CMD 设定默认执行的命令

```dockerfile
CMD ["node", "dist/main.js"]
```

## ENTRYPOINT

ENTRYPOINT 为 docker run 的时候执行的命令, docker run 后续的命令会跟在 ENTRYPOINT 后面

```dockerfile
ENTRYPOINT ["node"]
```

```bash
docker run xxx -v
# node -v
```
