参考：
  [HBase in Action](https://www.manning.com/books/hbase-in-action), 
  [source code](https://github.com/hbaseinaction)

<pre>
1. hbase.hregion.memstore.flush.size 设置memstore缓冲区大小
2. hbase将数据先写入WAL(预写日志)和memstore中, memstore写满后才会同步到磁盘生成HFile, 
3. 同一个HFile不能有多个列族, 但同一个列族可以有多个HFile
4. 系统可以从WAL中恢复上次因意外而没有同步到磁盘的数据
5. 系统优化查询使用LRU(近期最少使用算法)针对每个列族建立Blockcache缓存区, 缓存访问频繁的数据
6. deleteColumns是删除单元, deleteColumn是删除单元内的数据
7. 删除操作只是做标记, 直到major compaction时才会真正删除释放磁盘空间
8. minor compaction会将多个小HFile合并生成一个大的HFile, 可以微调性能
9. major compaction是将一个列族的所有HFile合并成一个HFile, 相当耗费资源, 但是唯一释放磁盘的机会
10. 单元中更新数据的历史数据过多, major compaction时会删除更早的数据
</pre>
