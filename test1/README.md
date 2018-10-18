实验一<br>
实验名称：分析SQL执行计划，执行SQL语句的优化指导<br>
指导老师：赵卫东<br>
实验内容：<br>
●对Oracle12c中的HR人力资源管理系统中的表进行查询与分析。<br>
●首先运行和分析教材中的样例：本训练任务目的是查询两个部门('IT'和'Sales')的部门总人数和平均工资，以下两个查询的结果是一样的。但效率不相同。<br>
●设计自己的查询语句，并作相应的分析，查询语句不能太简单。<br>
<br>
首先我执行了教材中的第二种查询语句：<br>
```SQL
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
FROM hr.departments d，hr.employees e
WHERE d.department_id = e.department_id
GROUP BY department_name
HAVING d.department_name in ('IT'，'Sales');
```
经过sqldeveloper执行后得到如下结果：<br>
![IMAGE](https://raw.githubusercontent.com/HaurchefantGreystone/oracle/master/img1.png)

经过优化指导后得到如下语句：
```SQL
SELECT d.department_name, count(e.job_id)as "部门总人数",
avg(e.salary)as "平均工资"
FROM hr.departments d, hr.employees e
WHERE d.department_id = e.department_id
GROUP BY department_name
HAVING d.department_name in ('IT','Sales')
```
由此可以看出，第二种查询语句已经没有系统可以优化的部分语句了，是相对效率高的语句。<br>
根据分析，这段语句的大概原理为：从departments,employees两表中获取部门名、所有员工的ID和工资，通过count员工ID获取每个部门的总员工数，<br>
通过avg员工工资获得平均工资，再通过groupby对部门进行分组，输出分组部门名为‘IT’‘Sales’的部门的相关信息。<br>
<br>
之后我自己编写了sql语句对数据库进行查询：
```SQL
SELECT d.department_name ,d.location_id,l.country_id
FROM hr.departments d, hr.locations l
WHERE d.location_id = l.location_id AND l.country_id='US'
```
这段语句查询了depaetments和locations两张表，并从中将location国籍为‘US’的部门相关信息输出。查询结果如下：<br>
![IMAGE](https://raw.githubusercontent.com/HaurchefantGreystone/oracle/master/img2.png)<br>
经过优化指导后，得到的语句与我自己编写的语句相同，说明此语句也达到了较高效率。
```SQL
SELECT d.department_name ,d.location_id,l.country_id
FROM hr.departments d, hr.locations l
WHERE d.location_id = l.location_id AND l.country_id='US'
```
