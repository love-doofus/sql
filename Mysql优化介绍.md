MySql优化

mysql优化步骤：

1. show status 了解各种sql执行的频率。可以了解当前数据库是以插入为主，还是查询为主。

2. 通过EXPLAIN查看SQL的执行计划。

   主要看type  ,possible_key,  key_len  ,   ref   , rows ,  extra(Using filesort  ,Using temporry)。

   从i以上两个步骤基本就知道sql的具体情况了，知道哪些需要优化了。

   那么优化分为两个部分：

   * 从SQL语句本身来优化

     1. 分清连接之间的关系以及mysql执行连接时候的原理。

     2. 如果where  后面的条件是字符串的一定要加引号，否则mysql执行的时候就把当做整数，然后mysql自动转换，但是之后并不使用索引了。

     3. 对于group by的优化：

        ~~~sql
        EXPLAIN SELECT * FROM TB_USER U GROUP BY U.INVESTORTYPE ORDER BY NULL;
        ~~~

        这样可以避免排序，extra为：Using temporary。

     4. join 优化

        有时候可以将子查询优化为join查询。因为子查询是把数据放到临时表中，而临时表是不能使用索引之类的。

        ~~~SQL
        EXPLAIN SELECT * FROM TB_USER U WHERE U.ID NOT IN (SELECT AU.USER_ID FROM TB_AGENT_USER AU );

        EXPLAIN SELECT * FROM TB_USER U LEFT JOIN TB_AGENT_USER AU ON AU.USER_ID = U.ID WHERE AU.USER_ID = NULL

        ~~~

        当AU中USER_ID添加了索引的话，性能更好。而且join查询的话，不需要临时表。

   * 使用索引进行优化：索引的话，分为单列索引，多列索引。而且需要考虑到索引的长度。 最终要的是能够以一个全局的观点来思考如何进行索引优化。而且索引不能建立太多，因为一般我们建立的索引是Btree的，索引在插入的时候，可能需要通过分裂叶子节点。这样当数据插入或者修改的时候，也需要对BTREE进行维护，消耗性能。

   * 优化表的数据类型

     char(length)   varchar(length),char是固定长度，varchar(length)时可变长度的，而且如果length长度大于256，则需要一个字节保存数据的长度，否则，需要两个字节保存数据的长度。

   * 数据库读写分离。但是对于数据一致性和及时性比较高的情况下，因为读写数据库需要时间来同步，所以读库可能存在延迟，因此在这种情况下，就直接走写库。对于，一致性和及时性要求不高的情况下，我们可以读写分离。

   * 数据进行缓存，避免频繁访问数据库，也避免高并发引起的数据库雪崩。

   ​

   ​