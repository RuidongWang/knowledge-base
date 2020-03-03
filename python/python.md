# python 
  1. pyhon3在执行sql语句的时候，如果语句中有中文，先把sql语句转换成utf8（看数据库默认编码是什么），再执行语句
  2. python 插入数据库的数据有 `""` 时，可以使用 `pymysql.escape_string()` 将数据强转为字符串。
   >此方法可以防 sql 注入 
  3. python在线程中不要使用全局变量
  4. Windows下，python在线程中：不要使用全局变量的dict和list；线程运行的函数最好不要用dict类型的参数传值; 会内存泄漏
<!--![21871583223678_.pic_hd](https://i.loli.net/2020/03/03/5fUZJtCSFEOXVTr.png)
-->


![21871583223678_.pic_hd](./pic/21871583223678_.pic_hd.jpg)
Django要决定使用哪个模块用作匹配，这个通常由 settings.py 中的 ROOT_URLCONF 的值决定，例如：ROOT_URLCONF = 'test_web.urls' 。就表示将使用 test_web（项目同名app） 这个 app 下的 urls 这个模块文件( 这个模块也称为url的根模块，可修改 )。但如果进来的HttpRequest对象具有一个 urlconf  属性（通过中间件 request processing  设置），则使用这个值来替换 ROOT_URLCONF  设置