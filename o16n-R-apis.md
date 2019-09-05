# O16N R APIs 

## Model
R   | corresponding Python
--- | --------------------
`get_model <- function(workspace, name = NULL, id = NULL, tags = NULL, properties = NULL, version = NULL, run_id = NULL)` | `Model` constructor
`register_model <- function(workspace, model_path, model_name, tags = NULL, properties = NULL, description = NULL, child_paths = NULL)` | `register`
`download_model <- function(model, target_dir = '.', exist_ok = False, exists_ok = NULL)` | `download`
`serialize_model <- function(model)` | `serialize`
`deserialize_to_model <- function(workspace, model_payload)` | `deserialize`
`delete_model <- function(model)` | `delete`
`deploy_model <- function(workspace, name, models, inference_config, deployment_config = NULL, deployment_target = NULL)` | `deploy`
`package_model <- function(workspace, models, inference_config, generate_dockerfile = False)` | `package`
`profile_model <- function(workspace, profile_name, models, inference_config, input_data)` | `profile`

### Notes
* for `get_model`, don't expose `model_framework` parameter
* for `register_model`, don’t expose `datasets`, `model_framework`, or `model_framework_version` parameters
* don't expose `add_dataset_references` method right now since we don't have `Datasets` support yet
* don't expose model profiling yet until profiling supports `InferenceConfig` with `Environments`
* instead of exposing `get_model_path`, we will use environment variable approach instead
* the following methods are getting refactored on the Python SDK side. We will hold off on wrapping these until the changes are in:
  * `update`
  * `update_tags_properties`
  * `add_tags`
  * `remove_tags`
  * `add_properties`
  
## ModelPackage
R   | corresponding Python
--- | --------------------
`get_model_package_container_registry <- function(package)` | `get_container_registry`
`get_model_package_creation_logs function(package, decode = True, offset = 0)` | `get_logs`
`pull_model_package_image <- function(package)` | `pull`
`save_model_package_files <- function(package, output_directory)` |`save`
`wait_for_model_package_creation <- function(package, show_output = False)` | `wait_for_creation`

### Notes
* `get_model_package_container_registry` returns a `ContainerRegistry` object. User can access container registry details like so:
```r
cr = get_model_package_container_registry(package)
address = cr$address
username = cr$username
pw = cr$password
```

## ModelProfile
R   | corresponding Python
--- | --------------------
`get_profiling_results <- function(profile)` | `get_results`
`serialize_model_profile <- function(profile)` | `serialize`
`wait_for_profiling(profile, show_output = False)` | `wait_for_profiling`

### Notes
* don't expose `update_creation_state` yet - method will be changed/renamed on Python side first
* don't do wrappers for `ModelProfile` until profiling supports `Environments`

## InferenceConfig
R   | corresponding Python
--- | --------------------
`create_inference_config <- function(entry_script, source_directory = NULL, description = NULL, environment = NULL)` | `InferenceConfig` constructor

## Environment
R   | corresponding Python
--- | --------------------
`create_environment <- function(name, version = NULL, environment_variables = NULL, cran_packages = NULL, github_packages = NULL, custom_url_packages = NULL, custom_base_image = NULL, base_image_registry = NULL)` | `Environment` constructor
`register_environment <- function(environment, workspace)` | `register`
`get_environment <- function(workspace, name, version = NULL)` | `get`

### Notes
* for `create_environment`, don't expose `python`, `spark`, or `databricks` parameter. Jordan to follow up on `Environment` constructor's `inferencing_stack_version` parameter & why it's there
* methods we won't expose for `Environment` class:
  * `Environment.from_conda_specification`
  * `Environment.from_existing_conda_environment`
  * `Environment.from_pip_requirements`
  * `Environment.add_private_pip_wheel`
  * `save_to_directory`
  * `Environment.load_from_directory`
 
## ContainerRegistry
R   | corresponding Python
--- | --------------------
`container_registry <- function(address = NULL, username = NULL, password = NULL)` | `ContainerRegistry` constructor

## Webservice

## AciWebservice

## AksWebservice

## AksCompute
