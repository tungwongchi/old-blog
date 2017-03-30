HBase版本兼容情况

1. [HBase与JDK版本对应关系](http://hbase.apache.org/book.html#basic.prerequisites)
<pre>
  HBase   JDK
  2.0     8
  1.3     7,8
  1.2     7,8
  1.1     7(8可以工作,测试结果不理想)
  1.0     7(8可以工作,测试结果不理想)
  0.98    6,7(8可以工作,测试结果不理想,编译需要对代码做小改动)
  0.94    6,7
</pre>

2. [hadoop与HBase版本对应关系](http://hbase.apache.org/book.html#hadoop)
<pre>
  Hbase     Hadoop
  0.94.x    1.1.x, 0.23.x
  0.98.x    2.2.0, 2.3.x, 2.4.x, 2.5.x
  1.0.x     2.4.x, 2.5.x, 
  1.1.x     2.4.x, 2.5.x
  1.2.x     2.4.x, 2.5.x, 2.6.1+, 2.7.1+
  1.3.x     2.4.x, 2.5.x, 2.6.1+, 2.7.1+
  2.0.x     2.6.1+, 2.7.1+
</pre>
注: 以下信息参考[hadoop、hbase、hive、zookeeper版本对应关系](http://www.cnblogs.com/jingblogs/p/5500357.html)
<pre>
  0.92.0    1.0.0
  0.92.1    1.0.0
  0.92.2    1.0.0, 1.0.3
  0.94.0    1.0.2
  0.94.1    1.0.3
  0.94.2    1.0.3
  0.94.3    1.0.4
  0.94.4    1.0.4
  0.94.5    1.0.4
  0.94.9    1.2.0
  0.95.0    1.2.0
hadoop1.2+hbase0.95.0+hive0.11.0 会产生hbase+hive的不兼容，创建hive+hbase的关联表就会报pair对异常。
hadoop1.2+hbase0.94.9+hive0.10.0没问题，解决了上个版本的不兼容问题。
hadoop-1.0.3+hive-0.9.0+hive0.92.0兼容
hadoop2.2+hbase0.96+hive0.12兼容(有些小问题，可能需要一些补丁)
</pre>
