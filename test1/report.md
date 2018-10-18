
# 实验1教材查询语句分析：

![image](https://github.com/pyfppp/Oracle/blob/master/test1/test1_explanation1.png)
_语句1解释计划_

---
![image](https://github.com/pyfppp/Oracle/blob/master/test1/test1_explanation2.png)
_语句2解释计划_

---

在上面两张解释计划的截图中可以看到，虽然是同样的查询结果但是开销的**差距是十分巨大的**，从语句的复杂程度上来讲，有以下区别
- 1.
  语句1在```where ```部分中使用and连接了两个限制条件。
- 2.
  语句2的```where ```部分条件虽然只有一个，但是使用了新的关键字```HAVING ```。
  
  
同时在解释计划中可以看到，两条查询语句对表的 **操作**（scan）也是不一样的，这也导致产生的cost不一样。
其中包含：
- 1.
  语句1中以DEPARTMENTS的主键DEPT_ID_PK为索引, **全扫描(index full scan：索引全扫描)** 整张DEPARTMENTS表,此步操作cost为**27**。
- 2.
  语句2中虽然如同语句1一样进行了 **索引扫描(index scan)**,但是语句2以 **索引范围扫描（index range scan）** 替换了索引全扫描，此步操作cost为**10**。
虽然从以上列举可以看出似乎索引范围扫描对于索引全扫描的性能优化不大，但是整体来看便不难发现，导致性能差距的主要原因就是在语句1中使用**合并连接(merge join)**,而在语句2中使用的是**嵌套连接(nested join)**,两者的cost分别为**7**和**5**.

那么根本上来说为什么合并连接和嵌套连接的性能差距如此巨大？下面是摘自网络的一段内容，很好的解释了相关原理。

>   Merge Join 是先将关联表的关联列各自做排序，然后从各自的排序表中抽取数据，到另一个排序表中做匹配。因为merge join需要做更多的排序，所以消耗的资源更多。 <br>通常来讲，能够使用merge join的地方，hash join都可以发挥更好的性能,即散列连接的效果都比排序合并连接要好。然而如果行源已经被排过序，在执行排序合并连接时不需要再排序了，这时排序合并连接的性能会优于散列连接。<br>可以使用USE_MERGE(table_name1 table_name2)来强制使用排序合并连接.
<br>适用情况：
<br>1.RBO模式
<br>2.不等价关联(>,<,>=,<=,<>)
<br>3.HASH_JOIN_ENABLED=false
<br>4. 用在没有索引，并且数据已经排序的情况.
<br><br><br>    Nested loops 工作方式是循环从一张表中读取数据(驱动表outer table)，然后访问另一张表（被查找表 inner table,通常有索引）。驱动表中的每一行与inner表中的相应记录JOIN。类似一个嵌套的循环。
<br>对于被连接的数据子集较小的情况，嵌套循环连接是个较好的选择。在嵌套循环中，内表被外表驱动，外表返回的每一行都要在内表中检索找到与它匹配的行，因此整个查询返回的结果集不能太大（大于1 万不适合），要把返回子集较小表的作为外表（CBO 默认外表是驱动表），而且在内表的连接字段上一定要有索引。当然也可以用ORDERED 提示来改变CBO默认的驱动表。
<br>使用USE_NL(table_name1 table_name2)可是强制CBO 执行嵌套循环连接。
<br>适用情况：
适用于驱动表的记录集比较小（<10000）而且inner表需要有有效的访问方法（Index），并且索引选择性较好的时候.
JOIN的顺序很重要，驱动表的记录集一定要小，返回结果集的响应时间是最快的。
<br>————————————摘自https://www.cnblogs.com/xqzt/p/4469673.html

# 实验1自定义查询语句:

```SQL
select e.email
from hr.departments d,hr.employees e
where d.manager_id=e.manager_id
and d.manager_id is not null
and e.email like '%A%'
```

# 实验1自定义查询语句分析:
在我的自定义查询语句中，我编写了**查询所有有领导的职员的邮箱,并且要求该邮箱中还有大写字母A**，解释计划如下图：
<br>![image](https://github.com/pyfppp/Oracle/blob/master/test1/test1_explanation_self.png)
_自定义查询语句解释计划_

---
![image](https://github.com/pyfppp/Oracle/blob/master/test1/test1_set_self.png)
_自定义语句结果集_

---
之前使用的是学校的SQL developer ，本机上安装的是pl sqldeveloper,在此说明。
性能方面并没有太多改进余地，本身该sql语句也并不算复杂，但是在呈现的方式上可以更加友好，比如使用```group-by```等语句。
并且自认为使用太多```and```会影响美观，也可以修改结构比如使用嵌套查询之类。


