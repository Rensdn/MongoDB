# MongoDB
Mongo Basic knowledge

## MongoDB
    概览
    - mongo是一个高可用、分布式、无模式的文档数据库。
    - mongo和传统关系数据库的最本质的区别在那里呢？就是其文档模型。
    - 如何考虑MongoDB 文档模式设计的基本策略呢？一般建议的是先考虑内嵌，但是有一些时候，使用引用则难以避免。
    - MongoDB是为应用程序设计的，而不是为了存储优化的。如果可以达到最高性能的话，我们甚至可以做一些反范式的东西。

    > MongoDB的特性
      1. 简单的查询语句，没有Join操作
      2. 文档型存储，其数据是用二进制的Json格式Bson存储的。其数据就像Ruby的hashes，或者Python的字典，或者PHP的数组
      3. Sharding，MongoDB提供auto-sharding实现数据的扩展性
      4. GridFS，MongoDB的提供的文件存储API
      5. 数组索引，你可以对文档中的某个数组属性建立索引
      6. MapReduce，可以用于进行复杂的统计和并行计算
      7. 高性能，通过使用mmap和定时fsync的方法，避免了频繁IO，使其性能更高

    > MongoDB的优点
      1. 高性能，速度非常快（如果你的内存足够的话）
      2. 没有固定的表结构，不用为了修改表结构而进行数据迁移
      3. 查询语言简单，容易上手
      4. 使用Sharding实现水平扩展
      5. 部署方便

    > 使用MongoDB，你得记住以下几点：
      1. MongoDB 假设你有大磁盘空间
      2. MongoDB 假设你的内存也足够大于放下你的热数据
      3. MongoDB 假设你是部署在64位系统上的（32位有2G的限制，试用还可以）
      4. MongoDB 假设你的系统是little-endian的
      5. MongoDB 假设你有多台机器（并不专注于单机可靠性）
      6. MongoDB 假设你希望用安全换性能，同时允许你用性能换安全

    > MongoDB在下面领域不太擅长
      1. 不太稳定，特别是auto-sharding目前还有很多问题
      2. 不支持SQL，这意味着你很多通过SQL接口的工具不再适用
      3. 持久化，MongoDB单机可靠性不太好，宕机可能丢失一段时间的数据
      4. 相关文档比较少，新功能都有这个问题
      5. 相关人才比较难找，这也是新功能的问题之一

    > 注意
      1.默认情况下，MongoDB 的每个数据库的命名空间保存在一个 16MB 的 .ns 文件中，平均每个命名占用约 628 字节，也即整个数据库的命名空间的上限约为 24000。
      2.每一个集合、索引都将占用一个命名空间。所以，如果每个集合有一个索引（比如默认的 _id 索引），那么最多可以创建 12000 个集合。如果索引数更多，则可创建的集合数就更少了。同时，如果集合数太多，一些操作也会变慢。
      不过，如果真的需要建立更多的集合的话，MongoDB 也是支持的，只需要在启动时加上“--nssize”参数，这样对应数据库的命名空间文件就可以变得更大以便保存更多的命名。这个命名空间文件（.ns 文件）最大可以为 2G，也就是说最大可以支持约 340 万个命名，如果每个集合有一个索引的话，最多可创建约 170 万个集合。
      还需要注意，--nssize 只设置新创建的 .ns 文件的大小，如果想改变已经存在的数据库的命名空间，在使用这个参数启动后，还需要运行 db.repairDatabase() 命令来调整尺寸。

## MongoDB 与 Noql 和 关系型数据库异同

## MongoDB 与 Redis、ufile、ufs、memCashed、CouchDB等区别

## MongoDB 读写分离方案
    1. 主从读写方案

    2. 副本集方案
## MongoDB 性能与优化

## MongoDB 设计模式与适应场景
    1. 设计模式
        mongo是一个高可用、分布式、灵活模式的文档数据库。一般建议的是先考虑内嵌，但是有一些时候，使用引用则难以避免。
    2. 适用场景
        (1).  适合实时的插入，更新与查询，并具备应用程序实时数据存储所需的复制及高度伸缩性。

         	◆ 网站数据：Mongo非常适合实时的插入，更新与查询，并具备网站实时数据存储所需的复制及高度伸缩性。

        (2).  适合作为信息基础设施的持久化缓存层。

         	◆ 缓存：由于性能很高，Mongo也适合作为信息基础设施的缓存层。在系统重启之后，由Mongo搭建的持久化缓存层可以避免下层的数据源过载。

         	◆ 大尺寸，低价值的数据：使用传统的关系型数据库存储一些数据时可能会比较昂贵

        (3).  适合由数十或数百台服务器组成的数据库。因为Mongo已经包含对MapReduce引擎的内置支持。

         	◆ 高伸缩性的场景：Mongo非常适合由数十或数百台服务器组成的数据库。Mongo的路线图中已经包含对MapReduce引擎的内置支持。

        (4).  Mongo的BSON数据格式非常适合文档化格式的存储及查询。

         	◆ 用于对象及JSON数据的存储：Mongo的BSON数据格式非常适合文档化格式的存储及查询
    3. 不适场景

        (1).  高度事务性的系统。

    	    ◆ 高度事务性的系统：例如银行或会计系统。传统的关系型数据库目前还是更适用于需要大量原子性复杂事务的应用程序。

        (2).  传统的商业智能应用。

    	    ◆ 传统的商业智能应用：针对特定问题的BI数据库会对产生高度优化的查询方式。对于此类应用,数据仓库可能是更合适的选择。

        (3).  复杂的SQL查询。

          	◆ 需要SQL的问题

    4. 针对某些MongoDB不适用的场合，有时可选用设计模式来加以应对。

        （1). 查询命令分离模式

        	在副本集中职责被分离到不同的节点。最基本的第一类节点可能也同时占据着首要地位，它只需要储存那些写入和更新所需的数据。而查询工作则交由第二类节点来执行。这一模式将提升首要节点服务器的写吞吐量，因为当写入一组对象时，需要更新及插入的数据量也随之减少，除此之外，二类节点也得益于较少的待更新数据和其自身所具有的为其工作量而优化的内存工作集。

        （2). 应用程序级事务模式

        	MongoDB不支持事务和文件内部锁定。

        （3). Bucketing模式

        	当文本含有一个不断增长的数组时，则使用Bucketing模式，例如指令。而指令线可能会扩展到超过文档大小的合理值。该模式经由编程方式处理，并通过公差计算触发。

        （4). 关系模式

        	有时，会有不能插入整个文档的情况，例如人体建模时，我们就可以使用该模式来建立关系。

        		* 确定数据是否属于该文档，即二者间是否有关系。
        		* 如果可能的话，特别是面对有用的独有（专属）数据时，插入文档。
        		* 尽可能不参考id值。
        		* 对关系中的有用部分进行反规范化处理。好的候选不会经常甚至从不更改值，并且颇为有用。
        		* 关注反规范数据的更新和关系修复。

        （5). 物化路径模式

        	在一个数据模型的树模式中，同一对象类型是该对象的子对象，这种情况下可以使用物化路径模型来以获取更高效的检索、查询。


## MongoDB 局限与不足
