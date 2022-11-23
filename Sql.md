参考：[廖雪峰的官方网站](https://www.liaoxuefeng.com/)
1. 查询表的数据：
	+ 基本查询：
	```sql
	SELECT * FROM <表名>
	```
	​		ELECT是关键字，表示将要执行一个查询，*表示“所有列”，FROM表示将要从哪个表查询
	+ 条件查询：
	```sql
	SELECT * FROM <表名> WHERE <条件表达式>
	SELECT * FROM students WHERE score >= 80 AND gender = 'M';
	```
	​		多个条件表达式可以用`AND`或者`OR`连接`<条件1> AND/OR <条件2>`，`NOT <条件>`，表示“不符合该条件”的记录
	​		使用`<>`判断不相等，使用`=`判断相等，使用`LIKE`判断相似，`name LIKE 'ab%'`，`%`表示任意字符，例如`'ab%'`将匹配`'ab'`，`'abc'`，`'abcd'`，要组合三个或者更多的条件，就需要用小括号`()`表示如何进行条件运算，否则，优先级：`NOT>AND>OR`
	
	+ 投影查询：
	
	  ```sql
	  SELECT 列1, 列2, 列3 FROM <表名>
	  # 从students表中返回id、score和name这三列：
	  SELECT id, score, name FROM students;
	  
	  # 可以给每一列起一个别名
	  SELECT 列1 别名1, 列2 别名2, 列3 别名3 FROM <表名>
	  # 以下SELECT语句将列名score重命名为points，而id和name列名保持不变：
	  SELECT id, score points, name FROM students;
	  
	  # 投影查询同样可以接WHERE条件，实现复杂的查询：
	  SELECT id, score points, name FROM students WHERE gender = 'M';
	  
	  ```
	
	+ 排序：
		```sql
	  SELECT .......ORDER BY ...
	  # 按score从低到高：
	  SELECT id, name, gender, score FROM students ORDER BY score;
	  # 默认排序是ASC“升序”，ASC可以省略，末尾加上DESC表示“倒序”：
	  SELECT id, name, gender, score FROM students ORDER BY score DESC;
	  # 使用ORDER BY score DESC, gender表示先按score列倒序，如果有相同分数的，再按gender列排序：
	  SELECT id, name, gender, score FROM students ORDER BY score DESC, gender;
	  # 如果有WHERE子句，那么ORDER BY子句要放到WHERE子句后面：
	  SELECT id, name, gender, score
	  FROM students
	  WHERE class_id = 1
	  ORDER BY score DESC;
	  ```
	  
	  + 分页查询，聚合查询，多表查询，连接查询略
	
	2. 插入数据：
	
	   ```sql
	   INSERT INTO <表名> (字段1, 字段2, ...) VALUES (值1, 值2, ...);
	   # 向students表插入一条新记录，先列举出需要插入的字段名称，然后在VALUES子句中依次写出对应字段的值：
	   INSERT INTO students (class_id, name, gender, score) VALUES (2, '大牛', 'M', 80);
	   ```
	
	3. 修改数据：
	
	   ```sql
	   UPDATE <表名> SET 字段1=值1, 字段2=值2, ... WHERE ...;
	   # 更新students表id=1的记录的name和score这两个字段，先写出UPDATE students SET name='大牛', score=66，然后在WHERE子句中写出需要更新的行的筛选条件id=1：
	   UPDATE students SET name='大牛', score=66 WHERE id=1;
	   ```
	
	
	
	























