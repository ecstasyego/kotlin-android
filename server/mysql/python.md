## Python

### MySQL
```bash
$ sudo mysql
```
```mysql
mysql> CREATE DATABASE testdb;
mysql> USE mysql;
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'temppw';
mysql> FLUSH PRIVILEGES;
mysql> EXIT;
```

### PyMySQL

```bash
$ pip install pymysql sqlalchemy pandas
```

`PyMySQL`
```python
import pymysql

user = "root"        
password = "temppw"  
host = "localhost"   
port = 3306          

try:
    connection = pymysql.connect(
        host=host,
        user=user,
        password=password,
        port=port
    )
    print("Connection Success")

    cursor = connection.cursor()    
    cursor.execute("""CREATE DATABASE IF NOT EXISTS testdb;""")
    
    cursor.close()
    connection.close()
except pymysql.MySQLError as e:
    print(f"Connection Fail: {e}")
```

### SQLAlchemy

`SQLAlchemy`
```python
from sqlalchemy import create_engine

user = "root"        
password = "temppw"  
host = "localhost"   
port = 3306          
dbname = "testdb"

try:
    engine = create_engine(f"mysql+pymysql://{user}:{password}@{host}:{port}/{dbname}")
    with engine.connect() as conn:
        print("Connection Success")
except Exception as e:
    print(f"Connection Fail: {e}")
```
