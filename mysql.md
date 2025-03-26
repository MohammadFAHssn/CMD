```sql
CREATE DATABASE jetbar;
```

```sql
SHOW DATABASES;
```

```sql
CREATE USER 'mohammad'@'localhost' IDENTIFIED BY 'MLMFAha13455474758302!';
```

```sql
mysql> GRANT ALL PRIVILEGES ON jetbar.* TO 'mohammad'@'localhost';

mysql> FLUSH PRIVILEGES;
```

```sql
SET FOREIGN_KEY_CHECKS = 0;
drop table if exists users;
SET FOREIGN_KEY_CHECKS = 1;
```

```sql
USE jetbar;
SELECT DATABASE(); # How to determine which database is selected
ALTER TABLE users DROP COLUMN name;
```
