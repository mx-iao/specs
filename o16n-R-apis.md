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


## ModelProfile

## InferenceConfig
