参考：
  [Effective Java](http://www.oracle.com/technetwork/java/effectivejava-136174.html)

<pre>
静态工厂/简单工厂:
  优点: 
    1. 与构造方法相比, 静态工厂可以自定义更有意义的命名.
    2. 可以保留对象实例缓存, 必要时候才创建对象实例, 从而可以提高性能.
    3. 可以返回子类, 灵活的替代原类型.
    4. 利用泛型特性, 可以自动判断类型不用自行填写泛型. 但是java7之后可以省略泛型.
  缺点: 
    1. 构造方法必须公开
    2. 命名随意, 没有相关文档时无法通用. 
      对此多用valueOf, getInstance, newInstance, getType, newType等习惯的名称来命名.

</pre>
