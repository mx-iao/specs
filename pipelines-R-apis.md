# ML Pipelines

## Core
[ref doc](https://docs.microsoft.com/en-us/python/api/azureml-pipeline-core/azureml.pipeline.core?view=azure-ml-py)

### `Pipeline`
R   | corresponding Python
--- | --------------------
`load_pipeline_from_yaml <- function(workspace, filename, workflow_provider = NULL, service_endpoint = NULL)` | `load_yaml`
`publish_pipeline <- function(pipeline, name = NULL, description = NULL, version = NULL, continue_on_step_failure = False)` | `publish`
`get_pipeline_service_endpoint <- function(pipeline)` | `service_endpoint`
`validate_pipeline <- function(pipeline)` | `validate`

Notes:
* for public preview, only support defining Pipelines through `load_yaml` and not `Pipeline` constructor
* only support `Experiment.submit` and not `pipeline.submit`
* in R SDK, update `submit_experiment` to `submit_experiment <- function(config, experiment, tags = NULL, ...)` to support the optional settings for pipeline experiments
* we will need to add support on Python side for defining RScriptStep in yaml
* for `publish_pipeline`, `continue_on_step_failure = False` instead of `= NULL` (inconsistency on Python side)

### `PublishedPipeline`
* a `PublishedPipeline` can be created from either a `Pipeline` or `PipelineRun` (through their respective publish methods)

R   | corresponding Python
--- | --------------------
`disable_published_pipeline <- function(pipeline)` | `disable`
`enable_published_pipeline <- function(pipeline)` | `enable`
`get_published_pipeline <- function(workspace, id)` | `get`
`save_published_pipeline_yaml_to_file <- function(pipeline, path = NULL)` | `save`
`list_published_pipelines_in_workspace <- function(workspace, active_only = True)` | `list`

Notes:  
* `Question`: name parameter `pipeline` or `published_pipeline`?

### `PipelineRun`
* `PipelineRun` object is returned when submitting a pipeline experiment
* can also be instantiated through `PipelineRun(experiment, "<pipeline_run_id>")` - can we just use a get method for this instead?

R   | corresponding Python
--- | --------------------
`get_pipeline_run_graph <- function(pipeline_run)` | `get_graph`
`publish_pipeline_from_run <- function(pipeline_run, name, description, version, continue_on_step_failure = NULL, **kwargs)` | `publish_pipeline`
`find_pipeline_step_run <- function(name)` | `find_step_run`
`get_pipeline_output <- function(pipeline_run, pipeline_output_name)` | `get_pipeline_output` returns `PortDataReference`
`wait_for_pipeline_run_completion <- function(pipeline_run, show_output = True, timeout_seconds = , raise_on_error = True)` | `wait_for_completion`

For the following methods, use the methods from the generic `Run` class:
* `cancel`
* `get_status`
* `get_tags`

Notes:  
* double check: is this also returned from submitting a published pipeline?
* `complete`, `fail`, and `child_run` methods are not supported for `PipelineRun`s. Why are these exposed in ref docs then with no mention of not supported?
* `get_pipeline_output` or `get_pipeline_run_output` better?

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

### `TimeZone`
R   | corresponding Python
--- | --------------------
`time_zone <- function(type = c("AUSCentralStandardTime", "AUSEasternStandardTime", "AfghanistanStandardTime", <add the rest of the possible timezones>))` | `TimeZone` enum

possible timezones: https://docs.microsoft.com/en-us/python/api/azureml-pipeline-core/azureml.pipeline.core.timezone?view=azure-ml-py

### Additional classes
* `StepRun`
* `StepRunOutput`
* `PipelineDraft`
* `PortDataReference`
* `OutputPortBinding`
* `InputPortBinding`
* `TrainingOutput`
* `StepSequence`
* `TimeZone`
* `PipelineEndpoint`
* `PipelineStep`
* `PipelineData`
* `Module`
* `ModuleVersion`
* `ModuleVersionDescriptor`
* `Graph`

### `PipelineDataset`
* don't expose until we support `Dataset`

### in `datastore.R`
