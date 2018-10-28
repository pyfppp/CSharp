## 实验2

_使用PL SQLDEVELOPER,查询语句使用实验建议步骤的所有代码_<br>
![image](https://github.com/pyfppp/Oracle/blob/master/test2/insert.png)
<br>_插入数据结果_<br>

---
![image](https://github.com/pyfppp/Oracle/blob/master/test2/table.png)
<br>_创建表_<br>

---
![image](https://github.com/pyfppp/Oracle/blob/master/test2/useage.png)
<br>_数据库使用情况_<br>

上图中，可以看到结果集中出现了三种不同的表空间使用情况，分别是``` SYSUX``` ```EXAMPLE ``` ```USERS``` 和```SYSTEM```,下面是具体分析。<br>
首先是```SYSUX```从字面上并无法得知这个表空间的作用，查阅后得知<br>
1.SYSUX:该空间是辅助系统表空间，主要存放Oracle系统内部的常用样式用户的对象<br>
比如，存放某用户的表和索引等，从而，减少系统表空间的负荷，一般不存储用户的数据，由Oracle系统内部自动维护<br>
2.USER:该表是用户的空间，存放永久性用户对象的数据和私有信息，因此，也称为数据表空间，每个数据库都应该有一个用户表空间，以便在创建用户的时候，将其分配给用户。<br>
3.SYSTEM：属于系统表空间，用于存放Oracle系统内部表和数据字典的数据
比如，表名、列名和用户名等<br>
一般不建议，将用户创建的表、索引等存放在SYSTEM表空间中。<br>
4.EXAMPLE:从字面意思可以知道，该空间是Oracle安装时选择的示例数据库空间，所以大部分也是被使用的，可用空间较少<br>
_使用情况截至10.28_

---
![image](https://github.com/pyfppp/Oracle/blob/master/test2/user%26role.png)
<br>_创建角色和用户_<br>

---

