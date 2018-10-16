# 实验一
## 分析oracle12c教材上的查询语句
- 语句一：
```
SELECT e.EMPLOYEE_ID,e.FIRST_NAME,e.MANAGER_ID,
(SELECT M.FIRST_NAME FROM employees m 
WHERE m.EMPLOYEE_ID=e.MANAGER_ID)
AS MANAGER_NAME FROM hr.employees e ORDER BY e.EMPLOYEE_ID;
``` 
![查询一的图片](select1.png "查询一的图片")



分析：
语句一先执行子查询，这里是一个主键唯一性索引。消耗1个cpu资源.
然后执行主查询语句，这里是一个全表扫描。消耗4个cpu资源
总查询一共消耗21个cpu资源

    

- 语句二：

```
SELECT e.EMPLOYEE_ID,e.FIRST_NAME,e.MANAGER_ID,m.FIRSTNAME
AS MANAGER_NAME FROM employees e,employees m WHERE
e.MANAGER_ID=m.EMPLOYEE_ID(+)ORDER BY e.EMPLOYEEE_ID;
```
![查询二的图片](select2.png "查询二的图片")



分析：
语句二是先做的查询，快速索引查询出别名为e和m的要查询的信息。消耗2个cpu资源
然后做右外连接，全表扫描，消耗5个cpu资源
总语句一共消耗6个cpu资源。

    
## 给出的实验语句分析

- 语句一：

```
SELECT d.department_name，count(e.job_id)as "部门总人数"，
   avg(e.salary)as "平均工资"
   from hr.departments d，hr.employees e
   where d.department_id = e.department_id
   and d.department_name in ('IT'，'Sales')
   GROUP BY department_name;
```
![查询一的图片](select3.png "查询一的图片")

分析：


分析：该语句的含义是查出名字为'IT','Sales'的部门的名字，以及每个部门的部门总人数和平均工资
并以部门名字分组。


- 语句二：
```
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
FROM hr.departments d，hr.employees e
WHERE d.department_id = e.department_id
GROUP BY department_name
HAVING d.department_name in ('IT'，'Sales');
```
![查询而的图片](select4.png "查询二的图片")

分析：
语句二是先做的查询，快速索引查询出别名为d和e的要查询的信息。消耗3个cpu资源
然后做连接，全表扫描，消耗3个cpu资源
总语句一共消耗7个cpu资源。


## 自定义语句

- 语句一：


- 语句二：