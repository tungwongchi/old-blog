参考：
  [Effective Java](http://www.oracle.com/technetwork/java/effectivejava-136174.html)

<pre>
1. 静态工厂/简单工厂:
  优点: 
    1. 与构造方法相比, 静态工厂可以自定义更有意义的命名.
    2. 可以保留对象实例缓存, 必要时候才创建对象实例, 从而可以提高性能.
    3. 可以返回子类, 灵活的替代原类型.
    4. 利用泛型特性, 可以自动判断类型不用自行填写泛型. 但是java7之后可以省略泛型.
  缺点: 
    1. 构造方法必须公开
    2. 命名随意, 没有相关文档时无法通用. 
      对此多用valueOf, getInstance, newInstance, getType, newType等习惯的名称来命名.
2. 构建缓存容易遗忘造成内存泄漏, 可以使用WeakHashMap来构建缓存, 引用过期后会自动删除. 
3. finalizer执行时间是不确定的, 方法中的未捕获异常信息不会打印抛出信息. 可以作为资源释放的检测.
4. 重写equals方法: 自反性(自己与自己), 对称性(互换), 传递性, 一致性(多次), 非空性, 必须重写hashCode.
5. 里氏替换原则: 一个类型的任何重要属性也适用于它的子类, 该类型编写的任何方法, 在子类上也要正常运行.
6. java.sql.Timestamp对java.util.Date进行扩展, 其equals方法违反一致性, 不可与父类同时混用.
7. java.net.URL的equals方法需要将域名转换成IP, 域名解析可能违反一致性.
8. equals方法返回true时, hashCode返回值必须相同. 反之, equals方法返回true时, hashCode并非一定不一致.
9. 好的hashCode方法会提高在hash表中存取的性能. 反之, 可能会使得hash表退化成链表.
10. java1.5+提供协变返回类型作为泛型, 就是重写方法的返回值可以是被覆盖方法的子类
</pre>
