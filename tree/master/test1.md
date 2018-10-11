# 实验1教材查询语句分析：

![image](https://github.com/pyfppp/Oracle/blob/master/tree/master/test1_explanation1.png)

![image](https://github.com/pyfppp/Oracle/blob/master/tree/master/test1_explanation2.png)




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
