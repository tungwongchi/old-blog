参考：
  [Effective MySQL: Optimizing SQL Statements](http://effectivemysql.com/book/optimizing-sql-statements/)

<pre>
1. show full processlist(5.1.7以后可以查看INFORMATION_SCHEMA.PROCESSLIST表)可以查询系统运行的sql情况.
2. 重复执行确认sql效率低下, 而不是锁表或其它系统原因导致(如果是更新语句需要重写为查询语句来验证).
3. show create table 查看表结构情况.
4. alter语句为阻塞操作, 会阻塞包括查询在内的所有操作.
5. explain查看执行计划结果中, 没有使用索引key显示为null, 处理过的行显示为rows, 被评估过的行显示为possibie_keys.
6. explain partitions(5.1以后)可以显示表分区相关信息.
7. explain extended显示filtered列, 结合show warnings可以分析一些索引失效的情况.
8. show index from 查看索引情况.
9. show table status 查看表状态情况. innoDB引擎时平均长度和行数等为估计值并且可能有很大误差.
10. 有时候全表扫描会比索引更快.
</pre>
