---
title: docker 基础知识
tags:
  - 汇总
  - docker
categories:
  - 汇总
  - docker
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