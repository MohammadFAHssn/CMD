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
GRANT ALL PRIVILEGES ON jetbar.* TO 'mohammad'@'localhost';

FLUSH PRIVILEGES;
```

```sql
USE jetbar;
SELECT DATABASE(); # How to determine which database is selected
SET FOREIGN_KEY_CHECKS = 0;
drop table if exists users;
SET FOREIGN_KEY_CHECKS = 1;
```

```sql
USE jetbar;
ALTER TABLE users DROP COLUMN name;
```
