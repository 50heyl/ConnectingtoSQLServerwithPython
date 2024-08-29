# Connecting to SQL Server with Python

## Introduction

This guide will walk you through the steps to connect to a SQL Server database using Python. I'll cover how to establish connections using both **Windows Authentication** and **SQL Server Authentication**. 
The `pyodbc` library is used to facilitate these connections, which is a popular choice for working with databases in Python.

## Prerequisites

- **Python**: Ensure that you have Python installed on your machine. You can download it from [here](https://www.python.org/downloads/).
- **SQL Server**: Make sure that SQL Server is installed and running on your machine or accessible over the network.
- **SQL Server Management Studio (SSMS)**: Useful for managing and querying your SQL Server databases.
- **pyodbc Library**: This can be installed using pip.

```
pip install pyodbc
```

## Connecting to SQL Server

### 1. Windows Authentication

Windows Authentication allows you to connect to SQL Server using your Windows credentials. This method is simple and secure, especially in a domain environment.

#### Code Example:

```
import pyodbc

# Define the connection string for Windows Authentication
conn_str = (
    r'DRIVER={ODBC Driver 17 for SQL Server};'
    r'SERVER=YourServerName;'  # Replace with your server name
    r'DATABASE=YourDatabaseName;'  # Replace with your database name
    r'Trusted_Connection=yes;'
)

# Establish a connection to the database
try:
    conn = pyodbc.connect(conn_str)
    print("Connection Successful using Windows Authentication")
except pyodbc.Error as e:
    print(f"Error: {e}")

# Close the connection
finally:
    conn.close()
```

### 2. SQL Server Authentication

SQL Server Authentication requires you to provide a username and password. This method is useful when connecting to SQL Server from a different environment or when Windows Authentication is not available.

#### Code Example:

```
import pyodbc

# Define the connection string for SQL Server Authentication
conn_str = (
    r'DRIVER={ODBC Driver 17 for SQL Server};'
    r'SERVER=YourServerName;'  # Replace with your server name
    r'DATABASE=YourDatabaseName;'  # Replace with your database name
    r'UID=YourUsername;'  # Replace with your SQL Server username
    r'PWD=YourPassword;'  # Replace with your SQL Server password
)

# Establish a connection to the database
try:
    conn = pyodbc.connect(conn_str)
    print("Connection Successful using SQL Server Authentication")
except pyodbc.Error as e:
    print(f"Error: {e}")

# Close the connection
finally:
    conn.close()
```

### 3. Fetching Data from SQL Server

Once connected, you can execute SQL queries and fetch data. Below is an example that retrieves data from a table.

#### Code Example:

```python
import pyodbc

# Replace the connection string with your details
conn_str = (
    r'DRIVER={ODBC Driver 17 for SQL Server};'
    r'SERVER=YourServerName;'
    r'DATABASE=YourDatabaseName;'
    r'Trusted_Connection=yes;'  # Or use SQL Server Authentication credentials
    # r'UID=YourUsername;'
    # r'PWD=YourPassword;'
)

# Establish a connection to the database
try:
    conn = pyodbc.connect(conn_str)
    cursor = conn.cursor()

# Execute a SQL query
cursor.execute("SELECT * FROM YourTableName")  # Replace with your table name

# Fetch all results from the executed query
rows = cursor.fetchall()

# Print the fetched data
for row in rows:
  print(row)

except pyodbc.Error as e:
print(f"Error: {e}")

# Close the connection
finally:
    conn.close()
```

### 4. Inserting Data into SQL Server

Here’s an example of how to insert data into a table.

#### Code Example:

```python
import pyodbc

# Replace the connection string with your details
conn_str = (
    r'DRIVER={ODBC Driver 17 for SQL Server};'
    r'SERVER=YourServerName;'
    r'DATABASE=YourDatabaseName;'
    r'Trusted_Connection=yes;'  # Or use SQL Server Authentication credentials
    # r'UID=YourUsername;'
    # r'PWD=YourPassword;'
)

# Establish a connection to the database
try:
    conn = pyodbc.connect(conn_str)
    cursor = conn.cursor()

    # Define the SQL INSERT query
    insert_query = """
    INSERT INTO YourTableName (Column1, Column2)
    VALUES (?, ?)  # Replace with your columns and data
    """

    # Data to be inserted
    data = ('Value1', 'Value2')  # Replace with your actual data

    # Execute the query
    cursor.execute(insert_query, data)

    # Commit the transaction
    conn.commit()

    print("Data Inserted Successfully")

except pyodbc.Error as e:
    print(f"Error: {e}")

# Close the connection
finally:
    conn.close()
```

## Error Handling

It's important to handle potential errors that may occur during the connection or execution of SQL queries. The `try-except` block in Python is commonly used for this purpose, as shown in the examples above.

### Common Errors:

- **Invalid Connection String**: Ensure that your connection string is correctly formatted.
- **Authentication Failure**: Verify your credentials if using SQL Server Authentication.
- **SQL Syntax Errors**: Check your SQL queries for any syntax errors.

## Conclusion

This guide covered the basics of connecting to SQL Server using Python with both Windows Authentication and SQL Server Authentication. By following these examples, you should be able to establish connections, execute queries, and manage your SQL Server databases from a Python environment.

You can expand on this by incorporating more complex queries, handling stored procedures, or even building Python applications that interact with SQL Server.

## Additional Resources

- [pyodbc Documentation](https://github.com/mkleehammer/pyodbc/wiki)
- [Microsoft SQL Server Documentation](https://docs.microsoft.com/en-us/sql/sql-server/?view=sql-server-ver15)
