# Postgres

## psycopg2

```python
import psycopg2

conn = psycopg2.connect(**{
            'dbname': 'my_db',
            # 'sslmode': 'verify-full',
            'user': 'my_user',
            'password': 'my_password',
            'host': 'localhost',
            'port': 5432,
            'target_session_attrs': 'read-write'
        })
cur = conn.cursor()
cur.execute("CREATE TABLE test (id serial PRIMARY KEY, num integer, data varchar);")
cur.execute("INSERT INTO test (num, data) VALUES (%s, %s)", (101, "test"))

SELECT_PHONES_IN_SELECTED_COUNTRY = """
    SELECT id
    FROM test
    WHERE data=%(some)s;
"""

# cur.execute("SELECT * FROM test;")
cur.execute(SELECT_PHONES_IN_SELECTED_COUNTRY, {"some": "test' or '1'='1"})
print(cur.fetchall())

print('test')
```
