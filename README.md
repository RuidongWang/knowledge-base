# knowledge-base
开发中遇到的坑

1. pyhon3在执行sql语句的时候，如果语句中有中文，先把sql语句转换成utf8（看数据库默认编码是什么），再执行语句
