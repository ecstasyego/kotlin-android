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

```python
import pymysql
import pandas as pd
from sqlalchemy import create_engine

user = "root"        
password = "temppw"  
host = "localhost"   
port = 3306          
dbname = "testdb"

engine = create_engine(f"mysql+pymysql://{user}:{password}@{host}:{port}/{dbname}")

try:
    with engine.connect() as conn:
        print("Connection Success")
except Exception as e:
    print(f"Connection Fail: {e}")
```
