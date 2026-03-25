# Executing the table output in Airflow Logs

## Activate the virtual environment
```bash
source myvenv/bin/activate
cd airflow
mkdir dags
cd dags
```
All our script will be inside the dags

## Now we have to create a new connection in Airflow 
```bash
airflow connections add postgres_conn \
    --conn-type postgres \
    --conn-host localhost \
    --conn-login my_user \
    --conn-password my_pass \
    --conn-schema mydb \
    --conn-port 5432
```
Here my_user and my_pass will be the user and password you have created in postgress configuration for airflow

Now we will run our first command by creating a connections.py in dags directory
```bash
vi connections.py
```
Paste the below code
```bash
from airflow import DAG
from airflow.operators.python import PythonOperator
from airflow.providers.postgres.hooks.postgres import PostgresHook
from datetime import datetime

def conn_func():
    hook = PostgresHook(postgres_conn_id='postgres_conn')
    print(hook)
    print("Hook is running")

    # 1. this prints only metadata
    airflow_conn = hook.get_connection('postgres_conn')
    print("Airflow connection object:")
    print(airflow_conn)

    # 2. This is the REAL psycopg2 connection
    pg_conn = hook.get_conn()
    print(pg_conn)
    cursor = pg_conn.cursor()

    # 3. Run your query
    cursor.execute("SELECT table_name FROM information_schema.tables WHERE table_schema = 'school';")

with DAG(
    dag_id='connection_dag',
    start_date=datetime(2026, 3, 1),
    schedule_interval=None,
    catchup=False
) as dag:

    view_task = PythonOperator(
        task_id='call_function',
        python_callable=conn_func
    )
```
## Now you can execute the connections.py dag and view the resultant output in logs
