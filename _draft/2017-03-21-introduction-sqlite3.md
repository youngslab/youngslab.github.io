
# Tutorials

* [An Introduction To The SQLite C/C++ Interface](https://www.sqlite.org/cintro.html)
* [SQLite Wrapper for C++](http://www.cplusplus.com/articles/jL18T05o/)
* http://gywn.net/2013/08/let-me-intorduce-sqlite/



# Sqlite 특징

* Nested Loop
* File Based Processing - 데이터베이스 단위로 잠금이 발생
* Transaction (Rollback Journal / WAL mode) -         

# Installation 

`sudo apt-get install sqlite3 libsqlite3-dev`

```
> sqlite3
SQLite version 3.8.2 2013-12-06 14:53:30
Enter ".help" for instructions
Enter SQL statements terminated with a ";"
sqlite>

```

# [Core Interfaces](https://www.sqlite.org/cintro.html)

아래 2개의 core object가 있다.  

- **`sqlite3           `**: the database connection object
- **`sqlite3_stmt      `** : the prepared statment object

core object를 이용하여 db engine이 동작하는 방식은 아래와 같다. 

1. **`sqlite3_open     `**: database connection object를 생성한다. 
2. **`sqlite3_prepare  `**: sql text를 byte-code로 compile한다. 
3. **`sqlite3_step     `**: evaluation. to the next result row.
4. **`sqlite3_column   `**: 현재 결과 row에 대한 column값.
5. **`sqlite3_finalize `**: statement object를 해제한다. 
6. **`sqlite3_close    `**: conection object를 해제한다. 

추가로 설명해야 할 api들은 아래와 같다. 

* Convenience wrappers =  prepare + stop + column + finalize

  * **`sqlite3_exec`**: callback에 의해 결과를 전달.
  * **`sqlite3_get_table`**: callback이 아닌 heap memory에 결과를 저장. 
  
* Binding Parameters and Reusing Prepared Statements
  
  * **`sqlite3_reset`**: statement를 처음으로 되돌린다. 다시 statement를 prepare 하지 않도록 도와준다. 
  * **`sqlite3_bind`**: 

* Configuring SQLite

  **`sqlite3_config`**

* Extending SQLite


# Code Snippets. 

1. Open database. 
  
  ```c++
   /* Open database */
   sqlite3 *db;
   rc = sqlite3_open("test.db", &db);
   if( rc ){
      fprintf(stderr, "Can't open database: %s\n", sqlite3_errmsg(db));
      return(0);
   }else{
      fprintf(stdout, "Opened database successfully\n");
   }
  ```

2. Create sql statement.
  
  ```c++
   /* Create SQL statement */
   sql = "CREATE TABLE COMPANY("  \
         "ID INT PRIMARY KEY     NOT NULL," \
         "NAME           TEXT    NOT NULL," \
         "AGE            INT     NOT NULL," \
         "ADDRESS        CHAR(50)," \
         "SALARY         REAL );";
  ```

3. Execute the statement.
  
  ```c++
   /* Execute SQL statement */
   rc = sqlite3_exec(db, sql, callback, 0, &zErrMsg);
   if( rc != SQLITE_OK ){
   fprintf(stderr, "SQL error: %s\n", zErrMsg);
      sqlite3_free(zErrMsg);
   }else{
      fprintf(stdout, "Table created successfully\n");
   }   
  ```

4. close database. 
  ```c++
   sqlite3_close(db);
  ```



