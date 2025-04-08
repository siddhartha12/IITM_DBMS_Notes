# 7.4 - Python and PostgreSQL
Python modules:
	psycopg2
## Steps to access PostgreSQL from Python using Psycopg
* Create Connection
* Create cursor
* Execute query
* Commit/rollback
* Close the cursor
* Close the connection

In code:
```
# Create Connecition
psychopg2.connect(database='mydb',user='myuser',password='mypass')

# Creating cursor
cursor = connection.cursor()

# Executing cursor
cursor.execute(sql [,optional parameters])

# Cursor Execute many
cursor.executemany(sql, seq_of_parameters)

# returns number of rows modified, inserted or deleted by the last query
cursor.rowcount()

cursor.fetchone()
# Fetches the next row of query result

cursor.fetchmany([size=cursor.arraysize])

cursor.fetchall()

connection.commit()

connection.rollback()
# Rolls back any changes to database since the last call to commit()

cursor.close()

connection.close()
```

