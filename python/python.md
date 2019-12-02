# python 
  1. pyhon3在执行sql语句的时候，如果语句中有中文，先把sql语句转换成utf8（看数据库默认编码是什么），再执行语句
  2. python 插入数据库的数据有 `""` 时，可以使用 `pymysql.escape_string()` 将数据强转为字符串。
   >此方法可以防 sql 注入 
  3. python在线程中不要使用全局变量
  4. Windows下，python在线程中：不要使用全局变量的dict和list；线程运行的函数最好不要用dict类型的参数传值; 会内存泄漏
