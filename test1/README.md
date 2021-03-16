# 实验1：SQL语句的执行计划分析与优化指导
## 2018软件工程1班 彭嘉绮 201810215310
## 实验目的：
分析SQL执行计划，执行SQL语句的优化指导。理解分析SQL语句的执行计划的重要作用。
## 实验内容：
1. 对Oracle12c中的HR人力资源管理系统中的表进行查询与分析。
2. 设计自己的查询语句，并作相应的分析，查询语句不能太简单。
## 分析教材查询：

### sql查询语句1：
```sql
SELECT d.department_name,count(e.job_id)as "部门总人数",
avg(e.salary)as "平均工资"
from hr.departments d,hr.employees e
where d.department_id = e.department_id
and d.department_name in ('IT','Sales')
GROUP BY d.department_name;
```
### sql查询语句1结果：
![pic1](D:\oracleHome\oracle\test1\pic1.png)
### sql查询语句1oracle优化指导：
查找结果：通过创建一个或多个索引可以改进此语句的执行计划。
建议：考虑运行可以改进物理方案设计的访问指导或者创建推荐的索引。
原理：创建推荐的索引可以显著地改进此语句的执行计划。但是, 使用典型的 SQL 工作量运行 “访问指导” 可能比单个语句更可取。通过这种方法可以 获得全面的索引建议案, 包括计算索引维护的开销和附加的空间消耗。

### sql查询语句2：
```sql
SELECT d.department_name,count(e.job_id)as "部门总人数",
avg(e.salary)as "平均工资"
FROM hr.departments d,hr.employees e
WHERE d.department_id = e.department_id
GROUP BY d.department_name
HAVING d.department_name in ('IT','Sales');
```
### sql查询语句2结果：
![pic2](D:\oracleHome\oracle\test1\pic2.png)
### sql查询语句1oracle优化指导：
查找结果：无
建议：无
原理：无
![pic3](D:\oracleHome\oracle\test1\pic3.png)

## 结论：sql查询语句2最优

## 自建查询：
### 目的：
1. 该查询主要是对HR人力资源管理系统中的表进行查询与分析
部门总人数 平均工资
1. 该查询主要是对department区域的Marketing和Finance部门进行查询
2. 同时查询出对应部门的人数
3. 同时查询出对应部门的平均工资
### 自己设计查询语句：
```sql
SELECT d.department_name, count(e.job_id)as "部门总人数",
avg(e.salary)as "平均工资"
from hr.departments d
JOIN hr.employees e
on d.department_id = e.department_id
and d.department_name ='IT' OR d.department_name = 'Sales'
GROUP BY d.department_name;
```
### 自己设计查询语句结果：
![pic4](D:\oracleHome\oracle\test1\pic4.png)
### 分析自己设计查询语句：
查找结果：在执行计划的行 ID 4 处发现开销很大的笛卡尔积操作。
建议：考虑从此语句中移去断开连接的表或视图, 或者添加引用它的联接条件。
原理：应尽可能避免笛卡尔积操作, 因为它的开销很大, 并且会产生大量数据。
![pic5](D:\oracleHome\oracle\test1\pic5.png)
