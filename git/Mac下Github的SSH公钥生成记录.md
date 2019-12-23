# Mac下Github的SSH公钥生成

生成公私钥对

```shell
➜  ~ cd .ssh
➜  .ssh ssh-keygen -t rsa -C "youremail@example.com"
```

需要设置密码就输入密码
不需要设置密码就直接按回车

在 github 上添加

```shell
➜  .ssh cat id_rsa.pub
```

将公钥拷贝到 github 中



