# ML Pipelines

## Core
[ref doc](https://docs.microsoft.com/en-us/python/api/azureml-pipeline-core/azureml.pipeline.core?view=azure-ml-py)

### `Pipeline`
R   | corresponding Python
--- | --------------------
`load_pipeline_from_yaml <- function(workspace, filename)` | `load_yaml`
`publish_pipeline <- function(pipeline, name = NULL, description = NULL, version = NULL, continue_on_step_failure = False)` | `publish`
`get_pipeline_service_endpoint <- function(pipeline)` | `service_endpoint`
`validate_pipeline <- function(pipeline)` | `validate`

Notes:
* for public preview, only support defining Pipelines through `load_yaml` and not `Pipeline` constructor
* only support `Experiment.submit` and not `pipeline.submit`
* in R SDK, update `submit_experiment` to `submit_experiment <- function(config, experiment, tags = NULL, ...)` to support the optional settings for pipeline experiments
* we will need to add support on Python side for defining RScriptStep in yaml
* for `publish_pipeline`, `continue_on_step_failure = False` instead of `= NULL` (inconsistency on Python side)
* Q: for `publish_pipeline`, name, description, & version parameters default to NULL while in `publish_pipeline_from_run` those parameters do not have default values - we should make this consistent 

### `PublishedPipeline`
* a `PublishedPipeline` can be created from either a `Pipeline` or `PipelineRun` (through their respective publish methods)

R   | corresponding Python
--- | --------------------
`disable_published_pipeline <- function(pipeline)` | `disable`
`enable_published_pipeline <- function(pipeline)` | `enable`
`get_published_pipeline <- function(workspace, pipeline_id)` | `get`
`save_published_pipeline_yaml_to_file <- function(pipeline, path = NULL)` | `save`
`list_published_pipelines_in_workspace <- function(workspace, active_only = True)` | `list`

### `PipelineRun`
* `PipelineRun` object is returned when submitting a pipeline experiment
* can also be instantiated through `PipelineRun(experiment, "<pipeline_run_id>")` - use the get method for this instead?

R   | corresponding Python
--- | --------------------
`get_pipeline_run <- function(experiment, run_id)` | `get`
`get_pipeline_runs <- function(workspace, pipeline_id)` | `get_pipeline_runs`
`publish_pipeline_from_run <- function(pipeline_run, name, description, version, continue_on_step_failure = False, **kwargs)` | `publish_pipeline`
`save_pipeline_yaml_to_file <- function(pipeline_run, path = NULL)` | `save`
`wait_for_pipeline_run_completion <- function(pipeline_run, show_output = True, timeout_seconds = 9223372036854775807, raise_on_error = True)` | `wait_for_completion`

For the following methods, use the methods from the generic `Run` class:
* `get`
* `cancel`

Notes:  
* don't expose `find_step_run`, `get_steps`, `get_graph`, `get_pipeline_output` methods for now (until we support `StepRun`, `Graph`, and `PortDataReference` classes)
* for `cancel`, use the `cancel_run` method in run.R

### `PipelineParameter`
* needed to pass to `submit_experiment` method `pipeline_parameters` parameter

R   | corresponding Python
--- | --------------------
`pipeline_parameter <- function(name, default_value)` | `PipelineParameter` constructor

### `ScheduleRecurrence`
R   | corresponding Python
--- | --------------------
`schedule_recurrence <- function(frequency, interval, start_time = NULL, time_zone = NULL, hours = NULL, minutes = NULL, week_days = NULL, time_of_day = NULL)` | `ScheduleRecurrence` constructor
`validate_schedule_recurrence <- function(schedule_recurrence)` | `validate`

### `Schedule`
R   | corresponding Python
--- | --------------------
`create_pipeline_schedule <- function(workspace, name, pipeline_id, experiment_name, recurrence = NULL, description = NULL, pipeline_parameters = NULL, wait_for_provisioning = False, wait_timeout = 3600, datastore = NULL, polling_interval = 5, data_path_parameter_name = NULL, continue_on_step_failure = False, path_on_datastore = NULL)` | `create`
`disable_pipeline_schedule <- function(schedule, wait_for_provisioning = False, wait_timeout = 3600)` | `disable`
`enable_pipeline_schedule <- function(schedule, wait_for_provisioning = False, wait_timeout = 3600)` | `enable`
`get_pipeline_schedule <- function(workspace, schedule_id)` | `get`
`get_last_pipeline_run <- function(schedule)` | `get_last_pipeline_run`
`get_pipeline_runs <- function(schedule)` | `get_pipeline_runs`
`get_schedules_for_pipeline_id <- function(workspace, pipeline_id)` | `get_schedules_for_pipeline_id`
`list_pipeline_schedules_in_workspace <- function(workspace, active_only = True, pipeline_id = NULL)` | `list`
`load_pipeline_schedule_info_from_yaml <- function(workspace, filename)` | `load_yaml`
`update_pipeline_schedule <- function(name = NULL, recurrence = NULL, description = NULL, pipeline_parameters = NULL, status = NULL, wait_for_provisioning = False, wait_timeout = 3600, datastore = NULL, polling_interval = NULL, data_path_parameter_name = NULL, continue_on_step_failure = False, path_on_datastore = NULL)` | `update`

### `TimeZone`
R   | corresponding Python
--- | --------------------
`time_zone <- function(type = c("AUSCentralStandardTime", "AUSEasternStandardTime", "AfghanistanStandardTime", <add the rest of the possible timezones>))` | `TimeZone` enum

possible timezones: https://docs.microsoft.com/en-us/python/api/azureml-pipeline-core/azureml.pipeline.core.timezone?view=azure-ml-py

### in `datastore.R`

### `PipelineEndpoint`
R   | corresponding Python
--- | --------------------
`add_pipeline_to_endpoint <- function(pipeline, endpoint)` | `add`
`archive_pipeline_endpoint <- function(endpoint)` | `archive`
`disable_pipeline_endpoint <- function(endpoint)` | `disable`
`enable_pipeline_endpoint <- function(endpoint)` | `enable`
`get_pipeline_endpoint <- function(workspace, id = NULL, name = NULL)` | `get`
`get_pipeline_from_version <- function(endpoint, version = NULL)` | `get_pipeline`
`list_pipeline_endpoints_in_workspace <- function(workspace, active_only = True)` | `list`
`list_pipelines_in_endpoint <- function(endpoint, active_only = True)` | `list_pipelines`
`publish_pipeline_endpoint <- function(workspace, name, description, pipeline)` | `publish`
`reactivate_pipeline_endpoint <- function(name)` | `reactivate`

Notes:  
* implement `PipelineEndpoint` class post public preview
* Q: for `publish` method, name: `publish_pipeline_endpoint`, `publish_pipeline_to_endpoint`, or `create_pipeline_endpoint`?
* Q: expose `get_default_version` or just have user do `endpoint$default_version`
* `list_versions` returns a list of `PipelineVersion` while `endpoint.pipeline_version_list` returns a list of `PipelineIdVersion`. it's kind of confusing to have two options for getting slight variations of the same thing. propose to just have users do `endpoint$pipeline_version_list` since `PipelineIdVersion` encapsulates more info than `PipelineVersion`
* instead of having to support both `add` and `add_default`, propose to modify `add` method on R side to be: `add_pipeline_to_endpoint <- function(pipeline, pipeline_endpoint, set_as_default = False)`
