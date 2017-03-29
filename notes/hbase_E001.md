environment：
hadoop-0.21.0、hbase-0.20.6

error info: 
<pre>
Exception in thread "main" java.io.IOException: 
Call to localhost/serv6:9000 failed on local exception: 
java.io.EOFException
 10/11/10 15:34:34 ERROR master.HMaster: Can not start master
 java.lang.reflect.InvocationTargetException
         at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
         at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:39)
         at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:27)
         at java.lang.reflect.Constructor.newInstance(Constructor.java:513)
         at org.apache.hadoop.hbase.master.HMaster.doMain(HMaster.java:1233)
         at org.apache.hadoop.hbase.master.HMaster.main(HMaster.java:1274) 
</pre>

root cause and solution: 
版本的问题，hadoop-0.20.2 搭配hbase-0.20.6比较稳当。

reference:
[HBase入门篇](http://www.uml.org.cn/sjjm/201212141.asp)
