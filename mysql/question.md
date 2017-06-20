### MySQL的复制原理以及流程，基本原理流程，3个线程以及之间的关联。
1. 主：binlog线程——记录下所有改变了数据库数据的语句，放进master上的binlog中；
2. 从：io线程——在使用start slave 之后，负责从master上拉取 binlog 内容，放进 自己的relay log中；
3. 从：sql执行线程——执行relay log中的语句；

### MySQL中myisam与innodb的区别，至少5点
1. 问5点不同；
* InnoDB支持事物，而MyISAM不支持事物
* InnoDB支持行级锁，而MyISAM支持表级锁
* InnoDB支持MVCC, 而MyISAM不支持
* InnoDB支持外键，而MyISAM不支持
* InnoDB不支持全文索引，而MyISAM支持。
2. innodb引擎的4大特性
* 插入缓冲（insert buffer),二次写(double write),自适应哈希索引(ahi),预读(read ahead)
3. 2者selectcount(*)哪个更快，为什么
* myisam更快，因为myisam内部维护了一个计数器，可以直接调取。
