# My SQL 高效方案 #

----------
## 建表建议 ##
1. 数据类型尽量用数字型，数字型比字符型的快
2. 选择正确的表引擎
3. 尽量给字段加上NOT NULL
4. 一个表不要加太多索引，因为索引影响插入和更新的速度
5. 适当的使用冗余的反范式设计，以空间换时间有的时候会很高效，比如做临时表

----------
## 查询建议 ##
1. 尽量不要在数据库中做运算，少用类似max，min等函数，这是由数据库引擎做的计算，会整体拖慢查询速度
2. 使用预处理语句。用Prepared Statements(预处理语句)定义一些参数，而MySQL只会解析一次。
3. 不要在生产环境程序中使用select * from 的形式查询数据。只查询需要使用的列
4. 查询尽可能使用limit减少返回的行数，减少数据传输时间和带宽浪费
5. 所有的SQL关键字用大写，避免SQL语句重复编译造成系统资源的浪费。
	> 虽然mysql对大小写不敏感，但是sql语句在解析时，会将关键字如where等转换为WHERE，如果sql语句很多，而每次都要转换大小写，这个操作本身就是对资源的浪费，毕竟可以手动写成大写[不同的可视化工具也有相应方法设置为大写]
6. 开启慢查询日志，定期用explain或desc优化慢查询中的SQL语句
7. where后最先出现的条件，一定是过滤和排除掉更多结果的条件，第二出现的次之。
8. 对查询进行优化，尽量避免全表扫描。首先应考虑在where以及order by涉及的列上建立索引。
9. 尽量避免在where子句中对字段进行null值判断。这会进行全表扫描
10. 尽量避免在where子句中对字段进行表达式操作。这会导致引擎放弃使用索引而进行全表扫描
11. 使用连接（join）代替子查询
12. 对于OR子句，如果要利用索引，则OR之间的每个条件列都必须用到索引，如果没有索引，则应该考虑增加索引。