#### sql一些简单操作

##### 创建表

~~~sql
CREATE TABLE TEST (
ID INT(11) NOT NULL ,
NAME VARCHAR(30) DEFAULT 'WANG' COMMENT '姓名',
EMAIL VARCHAR(30) COMMENT '邮箱',
PRIMARY KEY (ID)
)ENGINE=INNODB DEFAULT CHARSET = utf8 COMMENT '测试表';

~~~

##### 删除表

~~~sql
DROP TABLE TEST;
~~~

##### 清除表数据

~~~java
RUNCATE TABLE TEST; 
~~~

##### 修改表--列操作

~~~sql
ALTER TABLE TEST ADD AGE INT ;--添加列
ALTER TABLE TEST DROP AGE;--删除列
ALTER TABLE TEST MODIFY COLUMN AGE VARCHAR(80) NOT NULL; -- 修改列的类型
ALTER TABLE TEST CHANGE AGE SEX VARCHAR(30);-- 修改列名，并增加新列名的类型

~~~

##### 修改表--默认值

~~~sql
ALTER TABLE TEST ALTER AGE SET DEFAULT '4';--修改表列的默认值
ALTER TABLE TEST ALTER SEX DROP DEFAULT;--删除列的默认值
~~~

##### 修改表--表名

~~~sql
修改表名：RENAME TABLE TEST TO TB_STUDENT; -- 修改表名
~~~

##### 修改表-键操作

~~~sql
ALTER TABLE TEST ADD PRIMARY KEY (EMAIL);-- 增加主键，但要注意主键的唯一性
ALTER TABLE TEST DROP PRIMARY KEY;-- 删除主键
ALTER TABLE TEST MODIFY SEX INT ,DROP PRIMARY KEY;
~~~



以下是基于表中数据的操作。谨记一点：**如果是对表数据进行操作的话，SQL中是不需要添加TABLE这个关键词的。**

##### 插入数据

```sql
INSERT INTO TEST VALUES('1','WANG','WANG@163.COM');
INSERT INTO TEST (ID,NAME,EMAIL) VALUE ('3','WANG3','WANG3@163.COM');
```

##### 修改表数据

~~~sql
UPDATE TEST SET AGE=AGE+1 WHERE NAME LIKE 'WANG%';
~~~



