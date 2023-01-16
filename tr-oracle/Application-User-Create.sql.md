```sql
-- Application user.
CREATE USER module_name_user IDENTIFIED BY mystrongpassword
  DEFAULT TABLESPACE users
  TEMPORARY TABLESPACE temp;

GRANT CONNECT TO module_name_user;
```