## 实验3（个人用户名：new_user_pyf）

### 授权users,users02,users03访问权限

![image](https://github.com/pyfppp/Oracle/blob/master/test3/rights.png)

---
_授权截图_


![image](https://github.com/pyfppp/Oracle/blob/master/test3/orders.png)

---
_创建orders表截图

![image](https://github.com/pyfppp/Oracle/blob/master/test3/orders_detail.png)

---
_创建orders_details表截图_

![image](https://github.com/pyfppp/Oracle/blob/master/test3/usage.png)

---
_数据库使用情况分析_
![image](https://github.com/pyfppp/Oracle/blob/master/test3/do_plan.png)

---
_执行计划截图_
![image](https://github.com/pyfppp/Oracle/blob/master/test3/insert_details.png)

---
_插入user_details表截图_
![image](https://github.com/pyfppp/Oracle/blob/master/test3/orders_count.png)

---
_orders插入完成验证计数截图_
![image](https://github.com/pyfppp/Oracle/blob/master/test3/orders_detais.png)

---
_插入order_details验证计数截图_
![image](https://github.com/pyfppp/Oracle/blob/master/test3/union_search.png)

---
_联合查询情况截图_

### 关键部分SQL语句如下

```sql
...
  NOCOMPRESS NO INMEMORY  
, PARTITION PARTITION_BEFORE_2017 VALUES LESS THAN (TO_DATE(' 2017-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN')) 
  NOLOGGING 
  TABLESPACE USERS02 
  PCTFREE 10 
  INITRANS 1 
  STORAGE 
  ( 
    INITIAL 8388608 
    NEXT 1048576 
    MINEXTENTS 1 
    MAXEXTENTS UNLIMITED 
    BUFFER_POOL DEFAULT 
  ) 
  NOCOMPRESS NO INMEMORY  
, PARTITION PARTITION_BEFORE_2018 VALUES LESS THAN (TO_DATE(' 2018-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN')) 
  NOLOGGING 
  TABLESPACE USERS03 
  PCTFREE 10 
  INITRANS 1 
  STORAGE 
  ( 
    BUFFER_POOL DEFAULT 
  ) 
  NOCOMPRESS NO INMEMORY  
);

CREATE TABLE ORDER_DETAILS 
(
 ...
PCTFREE 10 
PCTUSED 40 
INITRANS 1 
STORAGE 
( 
  BUFFER_POOL DEFAULT 
) 
NOCOMPRESS 
NOPARALLEL 
PARTITION BY REFERENCE (ORDER_DETAILS_FK1) 
(
  PARTITION PARTITION_BEFORE_2016 
  LOGGING 
  TABLESPACE USERS 
  PCTFREE 10 
  INITRANS 1 
  STORAGE 
  ( 
    BUFFER_POOL DEFAULT 
  ) 
  NOCOMPRESS NO INMEMORY  
, PARTITION PARTITION_BEFORE_2017 
  LOGGING 
  TABLESPACE USERS02 
  PCTFREE 10 
  INITRANS 1 
  STORAGE 
  ( 
    BUFFER_POOL DEFAULT 
  ) 
  NOCOMPRESS NO INMEMORY  
, PARTITION PARTITION_BEFORE_2018 
  LOGGING 
  TABLESPACE USERS03 
  PCTFREE 10 
  INITRANS 1 
  STORAGE 
  ( 
    BUFFER_POOL DEFAULT 
  ) 
  NOCOMPRESS NO INMEMORY  
);
```
<br>
## 联合查询部分语句

```sql

EXPLAIN plan for
select 
    orders.order_id as AID,
    orders.customer_name as customer_name,
    order_details.order_id as BID,
    ORDER_DETAILS.PRODUCT_ID as product_id
from
    ORDERS
INNER JOIN ORDER_DETAILS ON (orders.order_id=order_details.order_id);

select * from table(dbms_xplan.display());

```
