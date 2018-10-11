# 实验1教材查询语句分析：

![image](https://github.com/pyfppp/Oracle/blob/master/tree/master/test1_explanation1.png)
_语句1解释计划_

---
![image](https://github.com/pyfppp/Oracle/blob/master/tree/master/test1_explanation2.png)
_语句2解释计划_

---

在上面两张解释计划的截图中可以看到，虽然是同样的查询结果但是开销的**差距是十分巨大的**，从语句的复杂程度上来讲，有以下区别
- 1.
  语句1在```SQL where ```部分中使用and连接了两个限制条件
- 2.
  语句2的```SQL where ```部分条件虽然只有一个，但是使用了新的关键字```SQL HAVING ```  
同时在解释计划中可以看到，两条查询语句对表的 **操作**（scan）也是不一样的，这也导致产生的cost不一样。
其中包含：
- 1.
  语句1中以DEPARTMENTS的主键DEPT_ID_PK为索引, **全扫描(index full scan：索引全扫描)** 整张DEPARTMENTS表,此步操作cost为**27**
- 2.
  语句2中虽然如同语句1一样进行了 **索引扫描(index scan)**,但是语句2以 **索引范围扫描（index range scan）** 替换了索引全扫描，此步操作cost为**10**
虽然从以上列举可以看出似乎索引范围扫描对于索引全扫描的性能优化不大，但是整体来看便不难发现，导致性能差距的主要原因就是在语句1中使用**合并连接(merge join)**,而在语句2中使用的是**嵌套连接(nested join)**,两者的cost分别为**106**和**19**,一目了然。
 

# 实验1自定义查询语句:

```SQL
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
from hr.departments d，hr.employees e
where d.department_id = e.department_id
and d.department_name in ('IT'，'Sales')
GROUP BY department_name;
```

# 实验1自定义查询语句分析:
