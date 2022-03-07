# Airflow

## Scheduling Strategies
```python
DAG(
    dag_id: str,
    description: Optional[str] = None,
    schedule_interval: Union[str, datetime.timedelta, dateutil.relativedelta.relativedelta, NoneType, Type[airflow.models.dag.ScheduleIntervalArgNotSet]] = <class 'airflow.models.dag.ScheduleIntervalArgNotSet'>,
    timetable: Optional[airflow.timetables.base.Timetable] = None,
    start_date: Optional[datetime.datetime] = None,
    end_date: Optional[datetime.datetime] = None,
    full_filepath: Optional[str] = None,
    template_searchpath: Union[str, Iterable[str], NoneType] = None,
    template_undefined: Type[jinja2.runtime.StrictUndefined] = <class 'jinja2.runtime.StrictUndefined'>,
    user_defined_macros: Optional[Dict] = None,
    user_defined_filters: Optional[Dict] = None,
    default_args: Optional[Dict] = None,
    concurrency: Optional[int] = None,
    max_active_tasks: int = 16,
    max_active_runs: int = 16,
    dagrun_timeout: Optional[datetime.timedelta] = None,
    sla_miss_callback: Optional[Callable[[ForwardRef('DAG'), str, str, List[str], List[airflow.models.taskinstance.TaskInstance]], NoneType]] = None,
    default_view: str = 'tree',
    orientation: str = 'LR',
    catchup: bool = True,
    on_success_callback: Optional[Callable[[airflow.utils.context.Context], NoneType]] = None,
    on_failure_callback: Optional[Callable[[airflow.utils.context.Context], NoneType]] = None,
    doc_md: Optional[str] = None,
    params: Optional[Dict] = None,
    access_control: Optional[Dict] = None,
    is_paused_upon_creation: Optional[bool] = None,
    jinja_environment_kwargs: Optional[Dict] = None,
    render_template_as_native_obj: bool = False,
    tags: Optional[List[str]] = None,
)
```

### Unscheduled
Manually trigger execution.
### Daily
```python
ag = DAG(
    dag_id="02_daily_schedule",
    schedule_interval="@daily",
    start_date=datetime(2019, 1, 1),
    end_date=datetime(2019, 1, 5),
)
```

## Context

| Key | Description | Example value |
| --- | --- | --- |
| conf | Provides access to Airflow configuration | `airflow.configuration.AirflowConfigParser` object |
| dag | The current DAG object | DAG object |
| dag_run | The current DagRun object | DagRun object |
| ds | execution_date formatted as %Y-%m-%d | “2019-01-01” |
| ds_nodash | execution_date formatted as %Y%m%d | “20190101” |
| execution_date | The start datetime of the task’s interval | `pendulum.datetime.DateTime object` |
| inlets | Shorthand for task.inlets, a feature to track input data sources for data lineage | [] |
| macros | airflow.macros module | macros module |
| next_ds | execution_date of the next interval (= end of current interval) formatted as %Y-%m-%d | “2019-01-02” |
| next_ds_nodash | execution_date of the next interval (= end of current interval) formatted as %Y%m%d | “20190102” |
| next_execution_date | The start datetime of the task’s next interval (= end of current interval) | pendulum.datetime.DateTime object |
| outlets | Shorthand for task.outlets, a feature to track output data sources for data lineage | [] |
| params | User-provided variables to the task context | {} |
| prev_ds | execution_date of the previous interval formatted as %Y-%m-%d | “2018-12-31” |
| prev_ds_nodash | execution_date of the previous interval formatted as %Y%m%d | “20181231” |
| prev_execution_date | The start datetime of the task’s previous interval | pendulum.datetime.DateTime object |
| prev_execution_date_success | Start datetime of the last successfully completed run of the same task (only in past) | pendulum.datetime.DateTime object |
| prev_start_date_success | Date and time on which the last successful run of the same task (only in past) was started | pendulum.datetime.DateTime object |
| run_id | The DagRun’s run_id (a key typically composed of a prefix + datetime) | “manual__2019-01-01T00:00:00+00:00” |
| task | The current operator | PythonOperator object |
| task_instance | The current TaskInstance object | TaskInstance object |
| task_instance_key_str | A unique identifier for the current TaskInstance ({dag_id}__{task_id}__{ds_nodash}) | “dag_id__task_id__20190101” |
| templates_dict | User-provided variables to the task context | {} |
| test_mode | Whether Airflow is running in test mode (configuration property) | False |
| ti | The current TaskInstance object, same as task_instance | TaskInstance object |
| tomorrow_ds | ds plus one day | “2019-01-02” |
| tomorrow_ds_nodash | ds_nodash plus one day | “20190102” |
| ts | execution_date formatted according to ISO8601 format | “2019-01-01T00:00:00+00:00” |
| ts_nodash | execution_date formatted as %Y%m%dT%H%M%S | “20190101T000000” |
| ts_nodash_with_tz | ts_nodash with time zone information | “20190101T000000+0000” |
| var | Helpers objects for dealing with Airflow variables | {} |
| yesterday_ds | ds minus one day | “2018-12-31” |
| yesterday_ds_nodash | ds_nodash minus one day | “20181231” |

## Dependencies

If a task (eg `clean_weather`) that depends on the product of another task (eg `fetch_weather`), there is a dependancy which you can represent as so

`fetch_weather >> clean_weather`

If you have a task that is dependant on multiple other tasks, you can indicate that as so

`[clean_weather, clean_sales] >> join_datasets`

### Conditional Branching within a DAG

To define branching logic, you can use the `BranchPythonOperator`, which takes at least the parameters `task_id` (the name of the task) and a `python_callable` (which will be a python function containing the branching logic), and will return the `task_id` for the downstream task to advance to.

If this new branching regime creates multiple paths to a task (eg if there are different data sources that were standardized/cleaned into a common format then joined with other data and you only want to run the cleaning for one of those sources pre-join), you can set a different triggering rule in the task that only needs one branch to complete.

```python
join_datasets = PythonOperator(
    ...,
    trigger_rule="none_failed",
)
```

### Trigger Rules 

Per [Airflow documentation](https://airflow.apache.org/docs/apache-airflow/1.10.2/concepts.html?highlight=trigger#trigger-rules
* all_success: (default) all parents have succeeded
* all_failed: all parents are in a failed or upstream_failed state
* all_done: all parents are done with their execution
* one_failed: fires as soon as at least one parent has failed, it does not wait for all parents to be done
* one_success: fires as soon as at least one parent succeeds, it does not wait for all parents to be done
* none_failed: all parents have not failed (failed or upstream_failed) i.e. all parents have succeeded or been skipped
* dummy: dependencies are just for show, trigger at will

### Sharing data between tasks

#### XComs

Airflow has some data service called XComs that allows you to push data into xcoms in one function call in a task and pull that data in another function call (in a separate task). 

#### Taskflow API (introduced in Airflow 2.0)
This allows us to define functions as tasks within DAGs via a decorator, and these functions can just return values.

```python
from airflow import DAG
from airflow.decorators import task
from airflow.operators.dummy import DummyOperator


with DAG(
    dag_id="taskflow_demo",
    start_date=airflow.utils.dates.days_ago(3),
    schedule_interval="@daily",
) as dag:
    fetch_sales = DummyOperator(task_id="fetch_sales")
    clean_sales = DummyOperator(task_id="clean_sales")

    fetch_sales >> clean_sales

    @task
    def train_model():
        model_id = str(uuid.uuid4())
        return model_id

    @task
    def deploy_model(model_id: str):
        print(f"Deploying model {model_id}")

    model_id = train_model()
    deploy_model(model_id)

    clean_sales >> model_id
```

