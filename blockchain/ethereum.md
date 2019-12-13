# Ethereum 遇到的问题



## Ethereum 启动遇到的问题



## Ethereum JavaScript Console 遇到的问题

【问题描述】：解锁账户时 `account unlock with HTTP access is forbidden`

【原因分析】：新版本 `geth`，出于安全考虑，默认禁止了 `HTTP` 通道解锁账户，[相关issue](https://github.com/ethereum/go-ethereum/pull/17037) 

【解决方法】：在启动的时候加上 `--allow-insecure-unlock`
