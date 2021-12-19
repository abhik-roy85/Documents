<a id='Top'></a>
# <u>SQLITE Cheat-sheet</u>

# Content:

* [**Installation:**](#Installation)
    * [Update apt repository cache](#Update_apt_repository_cache)
    * [Update apt-cache](#Update_apt-cache)
    * [Install SQLite 3](#Install_SQLite_3)
    * [Verify the installation](#Verify_the_installation)
    * [Removing SQLite 3](#Removing_SQLite_3)
* [**Create Operation:**](#Create_Operation)
    * [Creating and opening a database in the SQLite](#Creating_and_opening_a_database_in_the_SQLite)
    * [CREATE a new table](#CREATE_a_new_table)
* [**Write Operation**](#Write_Operation)
    * [Column ADD Operation:](#Column_ADD_Operation)
        * [Add column in table](#Add_column_in_table)
        * [Update one column](#Update_one_column)
    * [Column DELETE Operation:](#Column_DELETE_Operation)
        * [Deleting a specific column from a table](#Deleting_a_specific_column_from_a_table)
    * [Row ADD Operation:](#Row_ADD_Operation)
        * [Adding a Row](#Adding_a_Row)
        * [Adding multiple rows for multiple column at once](#Adding_multiple_rows_for_multiple_column_at_once)
        * [Adding multiple rows to one column at once](#Adding_multiple_rows_to_one_column_at_once)
        * [Updating multiple rows of a column](#Updating_multiple_rows_of_a_column)
    * [Row DELETE Operation:](#Row_DELETE_Operation)
        * [Deleting a specific row](#Deleting_a_specific_row)
        * [Deleting multiple rows](#Deleting_multiple_rows)
        * [Deleting multiple rows with range](#Deleting_multiple_rows_with_range)
        * [Deleting multiple rows with condition](#Deleting_multiple_rows_with_condition)
* [**Read Operation**](#Read_Operation)
    * [Table Read Operation:](#Table_Read_Operation)
        * [Check info about the table](#Check_info_about_the_table)
        * [List all available tables](#List_all_available_tables)
        * [Viewing the whole table](#Viewing_the_whole_table)
    * [Column Read Operation:](#Column_Read_Operation)
        * [Read a specific column of the table](#Read_a_specific_column_of_the_table)
        * [Read a multiple columns of the table](#Read_a_multiple_columns_of_the_table)
    * [Row Read Operation:](#Row_Read_Operation)
        * [Read a specific row of the table](#Read_a_specific_row_of_the_table)
        * [Read multiple rows with Index range](#Read_multiple_rows_with_Index_range)
        * [Read multiple rows with condition](#Read_multiple_rows_with_condition)
        * [Read multiple rows with range](#Read_multiple_rows_with_range)
        * [Counting number of rows of a table](#Counting_number_of_rows_of_a_table)
            * [Count number of rows with condition](#Count_number_of_rows_with_condition)
    * [Grouping Operation:](#Grouping_Operation)
        * [List the non-duplicate entries of a column in a table](#List_the_non-duplicate_entries_of_a_column_in_a_table)
        * [List the non-duplicate entries of a column in a table with count](#List_the_non-duplicate_entries_of_a_column_in_a_table_with_count)

---

A database is composed of tables, which can be visualized as spreadsheets. A table contains a series of rows (called records) and columns. The intersection of a row and the column it's connected to is called a field.

---

<a id='Installation'></a>
# <u>Installation:</u>

<a id='Update_apt_repository_cache'></a>
## Update apt repository cache
```
sudo apt update
```

<a id='Update_apt-cache'></a>
## Update apt-cache
```
sudo apt upgrad
```

<a id='Install_SQLite_3'></a>
## Install SQLite 3
```
sudo apt install sqlite3
```

<a id='Verify_the_installation'></a>
## Verify the installation
```
(base) abroy@BAN-L170815:/mnt/c/AllWorkData/Work/MyBrain/Education/Database/SQL$ sqlite3 –version;
SQLite version 3.33.0 2020-08-14 13:23:32
Enter ".help" for usage hints.
sqlite> ^D
```
Note: ctrl+d  to exit sqlite

<a id='Removing_SQLite_3'></a>
## Removing SQLite 3
```
sudo apt --purge remove sqlite3
```

[Top](#Top)

<a id='Create_Operation'></a>
# <u>Create Operation:</u>

<a id='Creating_and_opening_a_database_in_the_SQLite'></a>
## Creating and opening a database in the SQLite
```
sqlite3 <Database name>
sqlite3 mydatabase.db
```
OR
```
sqlite> .open <Database name>
sqlite> .open mydatabase.db
```

SQLite won't run your command unless it terminates with a semi-colon (;).

<a id='CREATE_a_new_table'></a>
## CREATE a new table
```
sqlite> CREATE TABLE IF NOT EXISTS <Table name>
   ...> (<Column name> <Data type> <Initial value>,
   ...> <Column name> <Data type> <Initial value>);


sqlite> CREATE TABLE IF NOT EXISTS member
   ...> (name TEXT NOT NULL,
   ...> datestamp DATETIME DEFAULT CURRENT_TIMESTAMP);

OR

sqlite> CREATE TABLE IF NOT EXISTS linux (distro TEXT NOT NULL);
   
```


[Top](#Top)

---

<a id='Write_Operation'></a>
# <u>Write Operation:</u>

<a id='Column_ADD_Operation'></a>
## <u>Column ADD Operation:</u>


<a id='Add_column_in_table'></a>
### Add column in table
```
ALTER TABLE <table_name>
  ADD <new_column_name> <column_definition>;

sqlite> ALTER TABLE linux ADD varsion INTEGER;
```

<a id='Update_one_column'></a>
### Update one column
```
sqlite> UPDATE <Table name>
   ...> SET <Column name> = <Value>
   ...> WHERE <another Column name> = <Some pattern>
   ...> ;

sqlite> UPDATE linux
   ...> SET varsion = 1
   ...> WHERE distro = 'Slackware'
   ...> ;
```

<a id='Column_DELETE_Operation'></a>
## <u>Column DELETE Operation:</u>


<a id='Deleting_a_specific_column_from_a_table'></a>
### Deleting a specific column from a table
```
sqlite> CREATE TEMPORARY TABLE linux_backup(distro);
sqlite> .tables
linux              member             temp.linux_backup
sqlite>
sqlite> INSERT INTO linux_backup SELECT distro FROM linux;
sqlite> DROP TABLE linux;
sqlite> CREATE TABLE linux(distro);
sqlite> INSERT INTO linux SELECT distro FROM linux_backup;
sqlite> DROP TABLE linux_backup;
sqlite> .tables
linux   member
```
**Note:** There's no way to directly delete a column from a table. Although DROP COLUMN is supported in SQL database but not in sqlite.


<a id='Row_ADD_Operation'></a>
## <u>Row ADD Operation:</u>


<a id='Adding_a_Row'></a>
### Adding a Row
```
sqlite> INSERT INTO <Table name> (<Column name>) VALUES (<Entry value>);

sqlite> INSERT INTO member (name) VALUES ('Alice');
sqlite> INSERT INTO member (name) VALUES ('Bob');
```

<a id='Adding_multiple_rows_for_multiple_column_at_once'></a>
### Adding multiple rows for multiple column at once
```
sqlite> INSERT INTO <Table name> (<Column name1>, <Column name2>, ...) VALUES (<Value1>, <Value2>, ...);

INSERT INTO linux (distro, id) VALUES ('Debian', 3);
```

<a id='Adding_multiple_rows_to_one_column_at_once'></a>
### Adding multiple rows to one column at once
```
sqlite> INSERT INTO <Table name> (<Column name>) VALUES (<Value1>), (<Value2>), (<Value3>), (<Value4>);

sqlite> INSERT INTO linux (distro) VALUES ('Slackware'), ('RHEL'), ('Fedora'),('Debian');
```

<a id='Updating_multiple_rows_of_a_column'></a>
### Updating multiple rows of a column
```
sqlite> UPDATE <Table name>
   ...> SET <Column name> = <Value>
   ...> WHERE <another Column name> IN (<Pattern1>, <Pattern2>, <Pattern3>);
sqlite>

sqlite> UPDATE linux
   ...> SET varsion = 3
   ...> WHERE distro IN ('RHEL', 'Fedora', 'Debian');
sqlite>
```

<a id='Row_Delete_Operation'></a>
## <u>Row Delete Operation:</u>

<a id='Deleting_a_specific_row'></a>
### Deleting a specific row
```
DELETE FROM <Table name> WHERE <Column name>=<Pattern>;
DELETE FROM linux WHERE distro='RHEL';
```

<a id='Deleting_multiple_rows'></a>
### Deleting multiple rows
```
DELETE FROM <Table name> WHERE <Column name> IN (<Pattern1>, <Pattern2>, <Pattern3>);
```

<a id='Deleting_multiple_rows_with_range'></a>
### Deleting multiple rows with range
```
sqlite> DELETE FROM <Table name> WHERE <Column name> BETWEEN <start identifier> AND <end identifier>;
sqlite> DELETE FROM linux WHERE id BETWEEN 2 AND 4;
```
**Note:** the above delete operation includes the start and end id.


<a id='Deleting_multiple_rows_with_condition'></a>
### Deleting multiple rows with condition
```
DELETE FROM <Table name> WHERE <Condition>;
DELETE FROM linux WHERE id > 1 AND id < 4;
```

[Top](#Top)

---

<a id='Read_Operation'></a>
# <u>Read Operation</u>

<a id='Table_Read_Operation:'></a>
## <u>Table Read Operation:</u>


<a id='Check_info_about_the_table'></a>
### Check info about the table
```
sqlite> PRAGMA table_info(<Table name>);

sqlite> PRAGMA table_info(member);
0|name|TEXT|1||0
1|datestamp|DATETIME|0|CURRENT_TIMESTAMP|0
```

<a id='List_all_available_tables'></a>
### List all available tables
```
sqlite> .table
linux   member
```

<a id='Viewing_the_whole_table'></a>
### Viewing the whole table
```
sqlite> SELECT * FROM <Table name>;

sqlite> SELECT * FROM member;
Alice|2021-12-16 11:49:40
Bob|2021-12-16 11:49:47
```

<a id='Column_Read_Operation'></a>
## <u>Column Read Operation:</u>


<a id='Read_a_specific_column_of_the_table'></a>
### Read a specific column of the table
```
SELECT <Coulmn name> FROM <Table name>;

sqlite> SELECT distro from linux;
Slackware
Fedora
Debian
RHEL
Ubuntu
```

<a id='Read_a_multiple_columns_of_the_table'></a>
### Read a multiple columns of the table
```
SELECT <Coulmn1>, <Coulmn2>, <Coulmn3> FROM <Table name>;

sqlite> SELECT distro, id from linux;
Slackware|1
Fedora|2
Debian|3
RHEL|4
Ubuntu|5
sqlite>
```

<a id='Row_Read_Operation'></a>
## <u>Row Read Operation:</u>
        

<a id='Read_a_specific_row_of_the_table'></a>
### Read a specific row of the table
```
sqlite> SELECT <Coulmn1>, <Coulmn2>, <Coulmn3> FROM <Table name> WHERE <Condition>;

sqlite> SELECT * FROM linux WHERE id = 4;
RHEL|4
sqlite> SELECT distro FROM linux WHERE id = 4;
RHEL
sqlite> SELECT distro, id FROM linux WHERE id = 4;
RHEL|4

```

<a id='Read_multiple_rows_with_Index_range'></a>
### Read multiple rows with Index range
```
sqlite> SELECT * FROM <Table name> LIMIT <Start index>, <End index>;

sqlite> SELECT * FROM linux LIMIT 0, 3;
Slackware|1
Fedora|2
Debian|3
```

<a id='Read_multiple_rows_with_condition'></a>
### Read multiple rows with condition
```
sqlite> SELECT <Coulmn1>, <Coulmn2>, <Coulmn3> FROM <Table name> WHERE <Condition>;

sqlite> SELECT distro, id FROM linux WHERE id < 4;
Slackware|1
Fedora|2
Debian|3
```

<a id='Read_multiple_rows_with_range'></a>
### Read multiple rows with range
```
sqlite> SELECT distro, id FROM linux WHERE id < 5 AND id > 1;
Fedora|2
Debian|3
RHEL|4
```

<a id='Counting_number_of_rows_of_a_table'></a>
### Counting number of rows of a table
```
SELECT COUNT(*) FROM <Table name>;
or
SELECT COUNT(<Column name>) FROM linux;

SELECT COUNT(*) FROM linux;
SELECT COUNT(distro) FROM linux;
```
**Note:** The COUNT(//<Column name//>) returns the number of non-null values.


<a id='Count_number_of_rows_with_condition'></a>
#### Count number of rows with condition
```
SELECT COUNT(<Column name>) FROM <Table name> WHERE <Condition>;

SELECT COUNT(*) FROM linux WHERE id > 2;
```

<a id='Grouping_Operation'></a>
## <u>Grouping Operation:</u>


<a id='List_the_non-duplicate_entries_of_a_column_in_a_table'></a>
### List the non-duplicate entries of a column in a table
```
SELECT <Column name> FROM <Table name> GROUP BY <Column name>;

SELECT distro FROM linux GROUP BY distro;
```

<a id='List_the_non-duplicate_entries_of_a_column_in_a_table_with_count'></a>
### List the non-duplicate entries of a column in a table with count
```
SELECT <Column name>, COUNT(*) FROM <Table name> GROUP BY <Column name>;

SELECT distro, COUNT(*) FROM linux GROUP BY distro;
```


[Top](#Top)
