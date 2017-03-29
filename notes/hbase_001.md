参考: [HBase入门篇](http://www.uml.org.cn/sjjm/201212141.asp)

1. Hadoop的项目中包含了那些产品

  1.1. Pig 是在MapReduce上构建的查询语言(SQL-like),适用于大量并行计算。
  
  1.2. Chukwa 是基于Hadoop集群中监控系统，简单来说就是一个“看门狗” (WatchDog)
  
  1.3. Hive 是DataWareHouse 和 Map Reduce交集，适用于ETL方面的工作。
  
  1.4. HBase 是一个面向列的分布式数据库。
  
  1.5. Map Reduce 是Google提出的一种算法，用于超大型数据集的并行运算。
  
  1.6. HDFS 可以支持千万级的大型分布式文件系统。
  
  1.7. Zookeeper 提供的功能包括：配置维护、名字服务、分布式同步、组服务等，用于分布式系统的可靠协调系统。
  
  1.8. Avro 是一个数据序列化系统，设计用于支持大批量数据交换的应用。

注1.在此机器上安装[splunk](http://www.splunk.com/base/Documentation/latest/Installation/InstallonLinux)可以方便对所有日志查看

--------------------------------------------------------

2. HBase的一些内存实现原理
  
  2.1. HMaster— HBase中仅有一个Master server。
  
  2.2. HRegionServer—负责多个HRegion使之能向client端提供服务，在HBase cluster中会存在多个HRegionServer。
  
  2.3. ServerManager—负责管理Region server信息，如每个Region server的HServerInfo(这个对象包含HServerAddress和startCode),已load Region个数，死亡的Region server列表
  
  2.4. RegionManager—负责将region分配到region server的具体工作，还监视root和meta 这2个系统级的region状态。
  
  2.5. RootScanner—定期扫描root region，以发现没有分配的meta region。
  
  2.6. MetaScanner—定期扫描meta region,以发现没有分配的user region。

--------------------------------------------------------

3. HBase Shell的一些基本操作命令
  
  3.1. 创建表: create '表名称', '列名称1','列名称2',...'列名称N'
  
  3.2. 添加记录: put '表名称', '行名称', '列名称', '值'
  
  3.3. 查看记录: get '表名称', '行名称'
  
  3.4. 查看表中的记录总数: count '表名称'
  
  3.5. 删除记录: delete '表名' ,'行名称' , '列名称'
  
  3.6. 删除一张表(先要禁用表才能对进行删除): drop '表名称'
  
  3.7. 禁用表: disable '表名称'
  
  3.8. 启用表: enable '表名称'
  
  3.9. 查看所有记录: scan "表名称"
  
  3.10. 查看某个表某个列中所有数据: scan "表名称" , ['列名称']
  
  3.11. 查看某个表某个列中部分数据: scan "表名称" , {COLUMNS => ['列名称'], LIMIT => 10, STARTROW => '行名称'}

--------------------------------------------------------

4. HBase Java API

  4.1. jar包: hbase-0.20.6.jar, hadoop-core-0.20.1.jar

--------------------------------------------------------

5. HBase优化参数
  
  5.1. hbase.client.write.buffer
  
  描述：这个参数可以设置写入数据缓冲区的大小，当客户端和服务器端传输数据，服务器为了提高系统运行性能开辟一个写的缓冲区来处理它， 
  这个参数设置如果设置的大了，将会对系统的内存有一定的要求，直接影响系统的性能
  
  5.2. hbase.master.meta.thread.rescanfrequency
  
  描述：多长时间 HMaster对系统表 root 和 meta 扫描一次，这个参数可以设置的长一些，降低系统的能耗。
  
  5.3. hbase.regionserver.handler.count
  
  描述：由于HBase/Hadoop的Server是采用Multiplexed, non-blocking I/O方式而设计的，所以它可以透过一个Thread来完成处理，
  但是由于处理Client端所呼叫的方法是Blocking I/O，所以它的设计会将Client所传递过来的物件先放置在Queue，
  并在启动Server时就先产生一堆Handler(Thread)，该Handler会透过Polling的方式来取得该物件并执行对应的方法，默认为25，
  根据实际场景可以设置大一些。
  
  5.4. hbase.regionserver.thread.splitcompactcheckfrequency
  
  描述：这个参数是表示多久去RegionServer服务器运行一次split/compaction的时间间隔，当然split之前会先进行一个compact操作.
  这个compact操作可能是minor compact也可能是major compact.compact后,会从所有的Store下的所有StoreFile文件最大的那个取midkey.
  这个midkey可能并不处于全部数据的mid中.一个row-key的下面的数据可能会跨不同的HRegion。
  
  5.5. hbase.hregion.max.filesize
  
  描述：HRegion中的HStoreFile最大值，任何表中的列族一旦超过这个大小将会被切分，而HStroeFile的默认大小是256M。
  
  5.6. hfile.block.cache.size
  
  描述：指定 HFile/StoreFile 缓存在JVM堆中分配的百分比，默认值是0.2，意思就是20%，而如果你设置成0，就表示对该选项屏蔽。
  
  5.7. hbase.zookeeper.property.maxClientCnxns
  
  描述： 这项配置的选项就是从zookeeper中来的，表示ZooKeeper客户端同时访问的并发连接数，ZooKeeper对于HBase来说就是一个入口
  这个参数的值可以适当放大些。
  
  5.8. hbase.regionserver.global.memstore.upperLimit
  
  描述：在Region Server中所有memstores占用堆的大小参数配置，默认值是0.4，表示40%，如果设置为0，就是对选项进行屏蔽。
  
  5.9. hbase.hregion.memstore.flush.size
  
  描述：Memstore中缓存的内容超过配置的范围后将会写到磁盘上，例如：删除操作是先写入MemStore里做个标记，
  指示那个value, column 或 family等下是要删除的，HBase会定期对存储文件做一个major compaction，
  在那时HBase会把MemStore刷入一个新的HFile存储文件中。如果在一定时间范围内没有做major compaction，
  而Memstore中超出的范围就写入磁盘上了。
