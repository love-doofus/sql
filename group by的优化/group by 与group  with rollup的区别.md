#### group by 与group by with rollup

##### group by

~~~sql
EXPLAIN SELECT u.LOGINNAME,u.`NAME`,COUNT(u.LOGINNAME) FROM TB_USER u GROUP BY u.CUSTOMER_TYPE;
~~~

执行计划：

![group by执行计划](E:\sql\sql\group by的优化\group by执行计划.png)

##### group by with rollup

~~~sql
EXPLAIN SELECT u.LOGINNAME,u.`NAME`,COUNT(u.LOGINNAME) FROM TB_USER u GROUP BY u.CUSTOMER_TYPE WITH ROLLUP;
~~~

执行计划：

![img](file:///E:/sql/sql/group%20by%E7%9A%84%E4%BC%98%E5%8C%96/group%20by%20with%20rollup.png?lastModify=1494320509?lastModify=1494320528?lastModify=1494320528)



虽然都是全表扫描，但是Extra是不同的。group by使用了Using temporary就是说使用了临时表，执行的时候，先把 SELECT u.LOGINNAME,u.`NAME`,SUM(u.LOGINNAME) FROM TB_USER u的结果执行放到一个临时表中，然后在进行分组。但是需要注意的是，临时表是不能使用索引的。而mysql执行group by with rollup的时候，直接使用了文件排序(Using filesort)，不需要使用临时表，所以这种有利于节省磁盘空间的。

##### 注意点

使用group by with rollup 的时候不能配合order  by，因为两者是互斥的。当然，如果你的by后面的字段建立了索引，那么两者的执行计划是相同的。