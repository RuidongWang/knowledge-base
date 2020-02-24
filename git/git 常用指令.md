# git 常用指令

## Git submodule 子模块的管理和使用

### 1. 添加子模块
```shell
git submodule add [url] filename
```

更新子模块到最新版本

```
git submodule update
```

更新子模块为远程项目的最新版本

```
git submodule update --remote
```

## gitignore 无法生效

**问题:** 有时候，我们修改.gitignore文件无效，如添加dist/，保存之后，修改dist文件夹内容发现还是会跟踪dist文件夹。

**原因:** git比较的是当前工作区与上一次commit的版本，之前版本是跟踪了dist文件夹的。

解决办法：
```
git rm -r --cached dist
```
此命令 是把当前dist的跟踪记录删除，但是工作区中dist文件夹还是存在的。
然后
```
git add .
git commit -m"git忽略dist文件夹"
```