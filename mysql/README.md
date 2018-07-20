# Import/Export Large MYSQL Databases

In the following, replace  
* **[USERNAME]** with your mysql username, 
* **[DBNAME]** with your database name,  
* **[/path_to_file/DBNAME]** with the path and name of the file used for the database dump
* **[/path_to_mysql/]** with the path to mysql bin (like /Applications/MAMP/Library/bin/).

## Copy/Export a Large Database

> MYSQL has no 'Copy' function. You create a copy by dumping the database with mysqldump.  

To dump the database and gzip it at the same time, use the following. This will prompt you for your password.  
```
mysqldump -u [USERNAME] -p [DBNAME] | gzip > [/path_to_file/DBNAME].sql.gz
```
---
## Import a Large Database
> If you want to replace the database with a fresh dump created by the above process, do the following.

* First, unzip the file.
```
gzip -d [/path_to_file/DBNAME].sql.gz
```

Get to a mysql prompt (you will be asked for your password.)
```
[/path_to_mysql/]mysql -u [USERNAME] -p
```

* Then do the following to wipe out the old database and replace it with the new dump:
```
SHOW DATABASES;
DROP DATABASE [DBNAME];
CREATE DATABASE [DBNAME];
USE [DBNAME];
SOURCE [/path_to_file/DBNAME].sql;
```
---
## Conditional Dumps
> Sometimes the search index is huge and you want to omit it from the dump. Do so with:
```
mysqldump -u [USERNAME] -p [DBNAME] --ignore-table=[DBNAME].search_index | gzip > [/path_to_file/DBNAME].sql.gz 
```