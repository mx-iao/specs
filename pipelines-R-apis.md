# ML Pipelines

## Core

[ref doc](https://docs.microsoft.com/en-us/python/api/azureml-pipeline-core/azureml.pipeline.core?view=azure-ml-py)
### `PipelineRun`
### `StepRun`
### `StepRunOutput`
### `PipelineStep`
  * for step types see list of steps in below section
### `PipelineData`
  * intermediate data (or output of a Step)

### `Pipeline`
  * use this constructor to instantiate a Pipeline object

R   | corresponding Python
--- | --------------------
`pipeline <- function(workspace, steps, description = NULL, default_datastore = NULL, default_source_directory = NULL, resolve_closure = True, workflow_provider = NULL, service_endpoint = NULL)` | `Pipeline` constructor
`load_pipeline_from_yaml <- function(workspace, filename, workflow_provider = NULL, service_endpoint = NULL)` | `load_yaml`
`publish_pipeline <- function(pipeline, name = NULL, description = NULL, version = NULL, continue_on_step_failure = NULL)` | `publish`
`get_pipeline_service_endpoint <- function(pipeline)` | `service_endpoint`
`submit_pipeline_experiment <- function(pipeline, experiment_name, pipeline_parameters = NULL, continue_on_step_failure = False, regenerate_outputs = False, parent_run_id = NULL)` | `submit`
`validate_pipeline <- function(pipeline)` | `validate`

Notes:

* `Experiment.submit` has `**kwargs` for the optional settings for a `Pipeline` run. Will need to make sure this is supported on R side.
* `Pipeline` constructor parameters `_workflow_provider=None`, `_service_endpoint=None` - why are they prefixed with underscore? Same with `load_yaml` method
* `publish` - should `continue_on_step_failure` default value be `None`? or just either T/F? `publish` method has default of False
* should we support both `Experiment.submit` and `pipeline.submit` or just one?
* FYI `Pipeline` has a `graph` attribute of type `Graph`

### `PublishedPipeline`
* a `PublishedPipeline` can be created from either a `Pipeline` or `PipelineRun` (through their respective publish methods)

R   | corresponding Python
--- | --------------------
`disable_published_pipeline <- function(pipeline)` | `disable`
`enable_published_pipeline <- function(pipeline)` | `enable`
`get_published_pipeline <- function(workspace, id, _workflow_provider = NULL, _service_provider = NULL)` | `get`
`get_published_pipeline_graph <- function(pipeline, _workflow_provider = NULL)` | `get_graph`
`get_published_pipeline_step_names <- function(pipeline, _workflow_provider = NULL` | `get_step_names`
`save_published_pipeline_yaml <- function(pipeline, path = NULL, _workflow_provider = NULL)` | `save`

Notes:

* if we do want to support `submit`, use the same `submit_pipeline_experiment` method of `Pipeline`
* name parameter `pipeline` or `published_pipeline`?
* same questions re: `_workflow_provider` and `_service_provider`
* `get_all` is getting deprecated, don't expose

### `PipelineParameter`
  * represents parameters to pipeline for use when user wants to resubmit a published pipeline
  * can we get away with not wrapping this class for now if we support `Pipeline` creation from yaml file?

* `PortDataReference`
* `OutputPortBinding`
* `InputPortBinding`
* `TrainingOutput`
* `StepSequence`
* `Schedule`
* `ScheduleRecurrence`
* `TimeZone`
* `PipelineEndpoint`

### `Module`
### `ModuleVersion`
### `ModuleVersionDescriptor`

### `PipelineDataset`
* don't expose until we support `Dataset`

### * `PipelineDraft`
* what exactly is the point of `PipelineDraft` and how does this compare to `Pipeline`?

### `Graph`?


## Steps

[ref docs](https://docs.microsoft.com/en-us/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py)
* `AdlaStep`
* `DatabricksStep`
* `DataTransferStep`
* `PythonScriptStep`
* `EstimatorStep`
* `MpiStep`
* `HyperDriveStep`
* `HyperDriveStepRun`
* `AzureBatchStep`
* `ModuleStep`

## Questions
* what is the order of priority of Step types to be supported?
* will we need to add ADLA Compute support?
* what is `DataPath`
* will we need to expose `DataReference` class to support Pipelines?
* which additional `Datastore` types to support?
* how often are people defining the Pipeline steps through the SDK? Is it enough to just support loading from yaml file?
 * does yaml file support the pre-build module steps?
* clarify difference between `ModuleStep` and the pre-built ones like `PythonScriptStep`
* clarify usage of `Graph`
