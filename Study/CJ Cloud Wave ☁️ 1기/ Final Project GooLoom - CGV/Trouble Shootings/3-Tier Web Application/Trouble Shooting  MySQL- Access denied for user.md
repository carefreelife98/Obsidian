**Access denied for user '계정'@'localhost' (using password: YES)**

- GRANT ALL ON *.* TO ’admin’@'localhost' IDENTIFIED BY 'qwer1234' WITH GRANT OPTION;

  

```
create user 'admin'@'%' identified by 'qwer1234';-- ON gooloom.item, gooloom.member, gooloom.user-- TO jdbc:mysql://localhost:3306/gooloomGRANT ALL PRIVILEGES ON *.* TO 'admin'@'%';use gooloom;
```