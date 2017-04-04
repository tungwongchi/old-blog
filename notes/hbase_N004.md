参考：
  [HBase in Action](https://www.manning.com/books/hbase-in-action), 
  [source code](https://github.com/hbaseinaction)

<pre>
1. HBase是为半结构化数据和水平可扩展性设计的, 是无模式, 无类型的数据库
2. 配置hbase.hregion.memstore.flush.size 可以设置memstore缓冲区大小
3. 系统可以从WAL中自动恢复上次因意外而没有同步到磁盘的数据
4. 系统优化查询使用LRU(近期最少使用算法)针对每个列族建立Blockcache缓存区, 缓存访问频繁的数据
5. hbase将数据先写入WAL(预写日志)和memstore中, memstore写满后才会同步到磁盘生成HFile, 
6. 同一个HFile不能有多个列族, 但同一个列族可以有多个HFile
7. minor compaction会将多个小HFile合并生成一个大的HFile, 可以微调性能
8. major compaction是将一个列族的所有HFile合并成一个HFile, 相当耗费资源, 但是唯一释放磁盘的机会
9. 由于列族有各自的memstore和HFile, 相互之间没有性能影响
10. deleteColumns是删除单元, deleteColumn是删除单元内的数据
11. 删除操作只是做标记, 直到major compaction时才会真正删除释放磁盘空间
12. 单元中更新数据的历史数据过多, major compaction时会删除更早的数据
13. Scaner.setCaching(int)可以设置查询缓存行数, 默认为1行. 设置过高可能导致交互时间过长.
14. Hadoop环境中, 长把DataNode和MapReduce TaskTracker并列配置在一起, 尽量避免网络传输
15. RegionServer本质上是HDFS客户端, 可以将RegionServer和DataNode配置在一起, 以便数据访问
16. RegionServer管理多个region, region大小可由配置参数Hbase.hregion.max.filesize设定
17. HBase提供-ROOT-和.META.表对region进行索引, 值得注意的是-ROOT-表是不会被拆分为多个region的,
  系统通过ZeeKeeper找到-ROOT-表, 由-ROOT-表定位到.META.表, 然后定位到region
18. 通过TableMapReduceUtil来实现以HBase作为数据源的mapreduce计算, JobTracker会尽量围绕region安排map任务,
  而对于reduce来说结果写入region并非同一个, 所以结果接收最好用HBase表
</pre>