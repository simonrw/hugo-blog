---
title: Python database transactions
date: 2015-09-19 14:47:23
category: tech
tags:
- python
---

## `pymysql`

* Defaults to `autocommit=False`

```python
connection = pymysql.connect(user='user', db='test')
cursor = connection.cursor()
cursor.execute('insert into test (value) values (10)')
connection.close()

connection = pymysql.connect(user='user', db='test')
cursor = connection.cursor()
cursor.execute('select value from test')
# => []
```

To commit changes to the database, `#commit()` must be called:

```python
connection = pymysql.connect(user='user', db='test')
cursor = connection.cursor()
cursor.execute('insert into test (value) values (10)')
# Call the commit line
connection.commit()
connection.close()

connection = pymysql.connect(user='user', db='test')
cursor = connection.cursor()
cursor.execute('select value from test')
# => [(10, )]
```

With a context manager on a connection, an implicit transaction is opened:

```python
with pymysql.connect(user='user', db='test') as cursor:
    cursor.execute('insert into test (value) values (10)')

with pymysql.connect(user='user', db='test') as cursor:
    cursor.execute('select value from test')
    # => [(10, )]
```

The `connect` function includes an autocommit parameter:

```python
connection = pymysql.connect(user='user', db='test', autocommit=True)
cursor = connection.cursor()
cursor.execute('insert into test (value) values (10)')
connection.close()

connection = pymysql.connect(user='user', db='test')
cursor = connection.cursor()
cursor.execute('select value from test')
# => [(10, )]
```

Multiple cursors can see any changes made within a transaction

```python
connection = pymysql.connect(user='user', db='test')
cursor = connection.cursor()
cursor2 = connection.cursor()
cursor.execute('insert into test (value) values (10)')
cursor2.execute('select value from test')
# => [(10, )]
```

Multiple connections are isolated from each other

```python
connection = pymysql.connect(user='user', db='test')
connection2 = pymysql.connect(user='user', db='test')
cursor = connection.cursor()
cursor2 = connection2.cursor()
cursor.execute('insert into test (value) values (10)')
cursor2.execute('select value from test')
# => []
```

Using a connection as a context manager starts and commits a transaction, or rolls back on error

```python
connection = pymysql.connect(user='user', db='test')
# Implicit transaction, and automatic #commit call
with connection as cursor:
    cursor.execute('insert into test (value) values (10)')
connection.close()

connection = pymysql.connect(user='user', db='test')
cursor = connection.cursor()
cursor.execute('select value from test')
# => [(10, )]
```
