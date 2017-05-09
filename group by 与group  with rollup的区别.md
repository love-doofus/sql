#### group by 与group by with rollup



##### group by

by 后面没有建立索引的时候：

~~~sql
EXPLAIN SELECT * FROM TB_USER u GROUP BY u.USER_TYPE;
~~~

执行计划：





##### group by with rollup

~~~sql
EXPLAIN  SELECT * FROM TB_USER u GROUP BY u.USER_TYPE WITH ROLLUP;
~~~

执行计划：



##### 相同点

如果by后面有索引的时候，两者的执行计划是一样的。所以没有区别，

##### 不同点

当by后面的字段没有索引列的时候，由两者执行计划可以看出，group by with rollup少了一个尽量选择**group by with rollup**,但是需要注意的一点是：group by with rollup不能喝order by共同使用，两者是互斥的。

