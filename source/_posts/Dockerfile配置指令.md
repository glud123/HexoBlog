---
title: Dockerfile配置指令
date: 2019-09-27 11:32:28
tags: [Docker]
categories: Docker
---

## Dockerfile配置指令

指令书写格式为

`INSTRUCTION arguments`

接下来介绍一下，Dockerfile都有哪些指令以及相关说明

### FROM



指令格式

```
FROM <image> [AS <name>] #
或者 
FROM <image>[:<tag>] [AS <name>]
或者
FROM <image>[@<digest>] [AS <name>]
```

