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