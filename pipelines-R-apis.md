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

### `PublishedPipeline`
### `PipelineParameter`
  * represents parameters to pipeline for use when user wants to resubmit a published pipeline

* `PortDataReference`
* `OutputPortBinding`
* `InputPortBinding`
* `TrainingOutput`
* `StepSequence`
* `Schedule`
* `ScheduleRecurrence`
* `TimeZone`
* `PipelineEndpoint`
* `Module`
* `ModuleVersion`
* `ModuleVersionDescriptor`
* `PipelineDataset`
* `PipelineDraft`


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
