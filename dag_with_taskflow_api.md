-  TaskFlow API was introduced in version 2.0 to make it simple for you to write your DAGs using Python code. 
-  So if most of your DAGs use plain Python code, you can eliminate a lot of the boilerplate involved with Python operators by using the TaskFlow API. 
-  The TaskFlow API is a new way to define workflows using a more Pythonic and intuitive syntax and it aims to simplify the process of creating complex workflows by providing
a higher-level abstraction compared to the traditional DAG-based approach

```
import time
from datetime import datetime, timedelta

from airflow.utils.dates import days_ago
from airflow.decorators import dag, task
default_args = {
    'owner': 'loonycorn'
}

```
- ### we make use of the @dag and @task decorators to identify the DAG and tasks in our workflow.
- This @dag decorator identifies the function dag_with_taskflow_api as containing the definition of a workflow.
- Each of these functions is annotated using the @task decorator.
- The @task decorator is what identifies these functions as tasks within our workflow.
- in order to run this entire DAG, you need to invoke the function dag_with_taskflow_api.
```
@dag(dag_id='dag_with_taskflow',
     description = 'DAG using the TaskFlow API',
     default_args = default_args,
     start_date = days_ago(1),
     schedule_interval = '@once',
     tags = ['dependencies', 'python', 'taskflow_api'])
def dag_with_taskflow_api():
    
    @task
    def task_a():
        print("TASK A executed!")
    
    @task
    def task_b():
        time.sleep(5)
        print("TASK B executed!")
    
    @task
    def task_c():
        time.sleep(5)
        print("TASK C executed!")
    
    @task
    def task_d():
        time.sleep(5)
        print("TASK D executed!")
    
    @task
    def task_e():
        print("TASK E executed!")
    
    task_a() >> [task_b(), task_c(), task_d()] >> task_e()


dag_with_taskflow_api()
```
