# Linux 相关知识点

## Ubuntu 图形节点终端翻墙

**问题描述：** Ubuntu 有翻墙的软件，但是问题是正常情况下终端是无法翻墙的。

### 1. 准备工具

翻墙工具：electron-ssr

一份可用的 SSR 订阅连接

### 2. 步骤
启动 electron-ssr 后，复制终端代理指令即可

```sh
export http_proxy="http://127.0.0.1:12333"
export https_proxy="http://127.0.0.1:12333"
```

### 3. 关键工具
polipo
