第一步：连接oracle，创建角色并分配权限空间1
```sql
$ sqlplus system/123@pdborcl
SQL> CREATE ROLE test;
Role created.
SQL> GRANT connect,resource,CREATE VIEW TO test;
Grant succeeded.
SQL> CREATE USER test2 IDENTIFIED BY 1234 DEFAULT TABLESPACE users TEMPORARY TABLESPACE temp;
User created.
SQL> ALTER USER test2 QUOTA 50M ON users;
User altered.
SQL> GRANT test TO test2;
Grant succeeded.
SQL> exit
```

第二步：test2与数据库连接并建新表和视图myview
```sql
$ sqlplus new_user/123@pdborcl
SQL> show user;
USER is "TEST2"
SQL> CREATE TABLE mytable (id number,name varchar(50));
Table created.
SQL> INSERT INTO mytable(id,name)VALUES(1,'zhang');
1 row created.
SQL> INSERT INTO mytable(id,name)VALUES (2,'wang');
1 row created.
SQL> CREATE VIEW myview AS SELECT name FROM mytable;
View created.
SQL> SELECT * FROM myview;
NAME
--------------------------------------------------
zhang
wang

SQL> GRANT SELECT ON myview TO hr;
Grant succeeded.
SQL>exit
```

第三步：hr用户查询myview
```sql
$ sqlplus hr/123@pdborcl
SQL> SELECT * FROM new_user.myview;
NAME
--------------------------------------------------
zhang
wang
zhang
wang
zhang
wang
SQL> exit
```
获取与其他同学的数据共享（此时发现查询得到的数据中有其他同学的数据），实验结束。
