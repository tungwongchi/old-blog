参考：
  [用Solr构建垂直搜索引擎](https://fliaping.gitbooks.io/create-your-vertical-search-engine-with-solr/content/introduction-of-vertical-search-engine.html)
  
<pre>
1. 搜索引擎按其工作方式主要可分为三种, 分别是全文搜索引擎(Full Text Search Engine), 
  垂直搜索引擎(Vertical Search Engine)和元搜索引擎(Meta Search Engine). 垂直搜索是
  针对某一行业的专业搜索引擎, 解决了通用搜索中信息量大, 查询不准确, 深度不够等问题.  
  但其信息范围局限于某一特定领域、人群、需求. 元搜索引擎在接受用户查询请求时, 同时在
  其他多个引擎上进行搜索, 并将结果返回给用户. (感觉有点像代理)
2. solr特性: Advanced Full-Text Search Capabilities(高级全文搜索能力), 
  Optimized for High Volume Traffic(大数据性能优化), Easy Monitoring(易于监控), 
  Standards Based Open Interfaces - XML, JSON and HTTP(标准的XML,JSON,HTTP接口),
  Comprehensive Administration Interfaces(综合管理界面), Near Real-Time Indexing(近乎实时的索引)
  Highly Scalable and Fault Tolerant(高可扩展和容错力), Extensible Plugin Architecture(可扩展的插件构架)
  Flexible and Adaptable with easy configuration(通过简单配置带来灵活性和适应性)
3. solr目录:
  bin: 存放的是solr为了方便使用所编写好的脚本
  contrib: 存放的大多是一些第三方提供的类库，使用这些类库能够极大扩展solr的功能
  dist: 存放的是solr自身所用到的一些核心类
  docs: 存放solr的文档，功能介绍，一些api的介绍
  example: 存放的是solr的例子
  licenses: 许可相关
  server: 是solr自带的jetty
4. SolrHome是solr运行的主目录, 包含多个SolrCore目录. SolrCore目录中就solr实例的运行配置文件和数据文件.
5. solr5.0+开始官方不再支持Tomcat的集成. ${solr}/server/lib/ext中的jar全部复制到
  ${tomcat}/webapps/solr/WEB-INF/lib 目录中. 将${solr}/server/solr-webapp/WEB-INF下
  web.xml中<env-entry-value>中的内容改成你的${solr_home}路径.
6. 在${solr_home}目录下创建自定义SolrCore目录, ${solr_home}/configsets/basic_configs/目录下
  的conf目录复制到自定义SolrCore目录下
7. Heritrix具有良好的可扩展性, 方便用户通过配置文件实现自己的抓取逻辑. crawler-beans.cxml可以
  主导整个Heritrix的抓取, 采用spring来管理.
8. Jsoup是一款Java的HTML解析器, 有类似jQuery选择器.
9. WebMagic是一个简单灵活的Java爬虫框架.
10. solrconfig.xml文件是solr的主配置文件, 主要配置高亮, 数据源, 索引大小, 索引合并等所有的索引策略.
  <luceneMatchVersion> 声明使用的lucene的版本
  <lib> 配置solr用到的jar包
  <dataDir> 自定义目录来存放所有索引数据
  <directoryFactory> 索引文件的类型, 默认solr.NRTCachingDirectoryFactory
  <indexConfig> 主要索引相关配置
  <writeLockTimeout> IndexWriter写锁过时的时间, 默认1000
  <maxIndexingThreads> 最大索引的线程数, 默认8
  <useCompoundFile> 是否使用混合文件, Lucene默认是true, solr默认是false
  <ramBufferSizeMB> 使用的内存的大小, 默认100
  <ramBufferdDocs> 内存中最大的文档数, 默认1000
  <mergePolicy> 索引合并的策略, 默认TiereMergePolicy
  <mergeFactor> 每次合并索引的时候获取多少个段, 默认10. 等同于同时设置了maxMergeAtOnce和segmentsPerTier两个参数
  <mergeScheduler> 段合并器, 背后有一个线程负责合并, 默认ConcurrentMergeScheduler
  <lockType> 文件锁的类型, 默认native, 使用NativeFSLockFactory
  <unlockOnStartup> 默认false
  <termIndexInterval> Lucene每次加载到内存的terms数, 默认128
  <reopenReaders> 如果是true时, IndexReaders能够被reopened, 而不是先关闭再打开, 默认true
  <deletionPolicy> 删除策略, 用户可以自己定制, solr默认的是SolrDeletionPolicy
  <infoStream> 为了调试, Lucene提供了这个参数, 如果是true的话, IndexWriter会像设置的文件中写入debug信息
  <updateHandler> 更新的Handler，默认DirectUpdateHandler2
  <updateLog><str name="dir"> 配置更新日志的存放位置
  <autoCommit> 硬自动提交, 可以配置maxDocs即从上次提交后达到多少文档后会触发自动提交, maxTime时间限制, 
    openSearcher. 如果设为false, 导致索引变化的最新提交, 不需要重新打开searcher就能看到这些变化, 默认false
  <autoSoftCommit> 如自动提交, 与前面的<autuCommit>相似, 但是它只是让这些变化能够看到, 
    并不保证这些变化会同步到磁盘上. 这种方法比硬提交要快, 而且更接近实时更友好
  <listerner event> update时间监听器配置, postCommit每一次提交或优化命令后触发
  <query> 配置检索词相关参数以及缓存配置参数
  <maxBooleanClauses> 每个BooleanQuery中最大Boolean Clauses的数目, 默认1024
  <filterCache> 为IndexSearcher使用, 当一个IndexSearcher Open时, 可以被重新赋于原来的值, 
    或者使用旧的IndexSearcher的值, 例如使用LRUCache时, 最近被访问的Items将被赋予IndexSearcher. 
    solr默认是FastLRUCache
  <queryResultCache> 缓存查询的结果集的docs的id
  <documentCache> 缓存document对象, 因为document中的内部id是transient, 
    所以autowarmed为0, 不能被autowarmed
  <fieldValueCache> 字段缓存
  <enableLazyFieldLoading> 保存的字段, 如果不需要的话就懒加载, 默认true
  <useColdSearcher> 当一个检索请求到达时, 如果现在没有注册的searcher, 
    那么直接注册正在预热的searcher并使用它. 如果设为false则所有请求都要block, 直到有searcher完成预热
  <maxWarmingSearchers> 后台同步预热的searchers数量
  <requestDispatcher handleSelect="false"> solr接受请求后如何处理, 推荐新手使用false
11. managed-schema在solr5之前叫schema.xml, 文件主要配置索引和查询的字段信息, 
  定义了所有的数据类型和各索引字段的信息(如类型, 是否建立索引, 是否存储原始信息等)
  fields块配置: name-名称. type-类型从的fieldType中取. indexed-是否索引. stored-是否保存.
    required-是否必须. multiValuer-在同一篇文档中可以有多个值. omitNorms-true的话忽略norms.
    termVectors-默认false. 如果是true的话. 要保存字段的term vector. termPositions-保存term vector的位置信息.
    termOffects-保存term vector的偏移信息. default-字段的默认值
  <dynamicField>动态字段, 当不确定字段名称时采用这种配置 <CopyField>
  types块配置: name-类型名称, class-对应于solr fieldtype类. sortMissingLast=true|false, 如果设置为true, 
    那么对这个字段排序的时候, 包含该字段的文档就排到不包含该字段的文档前面. sortMissingFirst=true|false, 
    如果设置为true, 那么对这个字段排序的时候, 没有该字段的文档排在包含该字段的文档前面. precisionStep-默认值是4.
    positionIncrementGap和multiValued一起使用, 设置多个值之间的虚拟空白的数量
12. solr.xml主要是配置solr主目录中的索引库即SolrCore
13. 分词算法:
  mmseg4j: 用[MMSeg算法](http://technology.chtsai.org/mmseg/)实现的中文分词器, 并实现lucene的analyzer和
  solr的TokenizerFactory以方便在Lucene和Solr中使用. 
  IKAnalyzer: 采用了特有的正向迭代最细粒度切分算法, 具有60万字/秒的高速处理能力. 采用了多子处理器分析模式, 
  支持英文字母, 数字, 中文词汇等分词处理, 兼容韩文, 日文字符. 
  paoding: 中文分词具有极高效率和高扩展性. 引入隐喻, 采用完全的面向对象设计, 构思先进.
14. solr集成mmseg4j: 引入jar包并运行bin/solr create -c trip, 配置managed-schema
  
</pre>
