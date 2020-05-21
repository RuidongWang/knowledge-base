# sql

[MySQL 资源大全中文版](https://github.com/RuidongWang/awesome-mysql-cn)

# mysql 无密码就能登陆的解决方法

**起因:** 配置服务器的时候无法远程连接 mysql，同时服务器上可以直接无密码登陆

**解决方法**

```shell

> use mysql;
 
> update user set authentication_string=password("你的密码") where user='root';  #(无password字段的版本,也就是版本<=5.7的)
> update user set password=password('你的密码') where user='root'; #(有password字段的版本,版本>5.7的)

# 启动密码插件
> update user set plugin="mysql_native_password"; 
 
> flush privileges;
 
> exit;
```
