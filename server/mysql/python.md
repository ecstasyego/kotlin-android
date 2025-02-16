## Python
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
name = "testdb"

engine = create_engine(f"mysql+pymysql://{user}:{password}@{host}:{port}/{name}")

try:
    with engine.connect() as conn:
        print("Connection Success")
except Exception as e:
    print(f"Connection Fail: {e}")
```
