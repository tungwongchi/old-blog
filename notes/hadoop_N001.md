Hadoop版本兼容情况

1. [hadoop与hive对应关系](http://hive.apache.org/downloads.html)
<pre>
  hive      Hadoop
  1.1.0     1.x.y, 2.x.y  
  1.0.0     1.x.y, 2.x.y
  0.13.1    0.20.x, 0.23.x.y, 1.x.y, 2.x.y
  0.13.0    0.20.x, 0.23.x.y, 1.x.y, 2.x.y
  0.12.0    0.20.x, 0.23.x.y, 1.x.y, 2.x.y
  0.11.0    0.20.x, 0.23.x.y, 1.x.y, 2.x.y
  0.10.0    0.20.x, 0.23.x.y, 1.x.y, 2.x.y
  
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
  Hbase     Hadoop
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
  
hadoop1.2+hbase0.95.0+hive0.11.0会产生hbase+hive的不兼容，创建hive+hbase的关联表就会报pair对异常。
hadoop1.2+hbase0.94.9+hive0.10.0没问题，解决了上个版本的不兼容问题。
hadoop-1.0.3+hive-0.9.0+hive0.92.0兼容
hadoop2.2+hbase0.96+hive0.12兼容(有些小问题，可能需要一些补丁)
</pre>
