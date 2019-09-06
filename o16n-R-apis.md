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
`environment <- function(name, version = NULL, environment_variables = NULL, cran_packages = NULL, github_packages = NULL, custom_url_packages = NULL, custom_base_image = NULL, base_image_registry = NULL)` | `Environment` constructor
`register_environment <- function(environment, workspace)` | `register`
`get_environment <- function(workspace, name, version = NULL)` | `get`

### Notes
* for `environment`, don't expose `python`, `spark`, or `databricks` parameter. Jordan to follow up on `Environment` constructor's `inferencing_stack_version` parameter & why it's there
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
R   | corresponding Python
--- | --------------------
`get_webservice <- function(workspace, name)` | `Webservice` constructor
`wait_for_deployment <- function(webservice, show_output = False)` | `wait_for_deployment`
`get_webservice_logs <- function(webservice, num_lines = 5000)` | `get_logs`
`get_webservice_keys <- function(webservice, key)` | `get_keys`
`delete_webservice <- function(webservice)` | `delete`
`invoke_webservice <- function(webservice, input_data)` | `run`
`generate_new_webservice_key <- function(webservice, key_type = c("PRIMARY", "SECONDARY"))` | `regen_key`
`get_webservice_token <- function(webservice)` | `get_token`
`serialize_webservice <- function(webservice)` | `serialize`
`deserialize_to_webservice <- function(workspace, webservice_payload)` | `deserialize`

### Notes
* don't expose the following functions until they are refactored on the Python SDK side:
  * `add_tags`
  * `remove_tags`
  * `add_properties`
* don't expose `update_deployment_state` yet - method will be changed/renamed on Python side first (@Jordan)
* for `serialize_webservice` implemention, make sure to call into the subclass AciWebservice & AksWebservice `serialize` methods and not method of parent `Webservice` class

## LocalWebservice
R   | corresponding Python
--- | --------------------
`create_local_webservice_deployment_config <- function(port = NULL)` | `deploy_configuration` 
delete
deploy_to_cloud
get_logs
reload
update
update_deployment_state



## AciWebservice
R   | corresponding Python
--- | --------------------
`create_aci_webservice_deployment_config <- function(cpu_cores = NULL, memory_gb = NULL, tags = NULL, properties = NULL, description = NULL, location = NULL, auth_enabled = NULL, ssl_enabled = NULL, enable_app_insights = NULL, ssl_cert_pem_file = NULL, ssl_key_pem_file = NULL, ssl_cname = NULL, dns_name_label = NULL)` | `deploy_configuration`
`update_aci_webservice <- function(webservice, tags = NULL, properties = NULL, description = NULL, auth_enabled = NULL, ssl_enabled = NULL, ssl_cert_pem_file = NULL, ssl_key_pem_file = NULL, ssl_cname = NULL, enable_app_insights = NULL, models = NULL, inference_config = NULL)` | `update`

### Notes
* Python SDK uses "deploy_configuration" for naming, but let's stick with "deployment_config" to align with the naming of other methods, e.g. `create_inference_config`, `create_hyperdrive_config`

## AksWebservice
R   | corresponding Python
--- | --------------------
`create_aks_webservice_deployment_config <- function(autoscale_enabled = NULL, autoscale_min_replicas = NULL, autoscale_max_replicas = NULL, autoscale_refresh_seconds = NULL, autoscale_target_utilization = NULL, auth_enabled = NULL, cpu_cores = NULL, memory_gb = NULL, enable_app_insights = NULL, scoring_timeout_ms = NULL, replica_max_concurrent_requests = NULL, max_request_wait_time = NULL, num_replicas = NULL, primary_key = NULL, secondary_key = NULL, tags = NULL, properties = NULL, description = NULL, gpu_cores = NULL, period_seconds = NULL, initial_delay_seconds = NULL, timeout_second = NULL, success_threshold = NULL, failure_threshold = NULL, namespace = NULL, token_auth_enabled = NULL)` | `deploy_configuration`
`update_aks_webservice <- function(autoscale_enabled = NULL, autoscale_min_replicas = NULL, autoscale_max_replicas = NULL, autoscale_refresh_seconds = NULL, autoscale_target_utilization = NULL, auth_enabled = NULL, cpu_cores = NULL, memory_gb = NULL, enable_app_insights = NULL, scoring_timeout_ms = NULL, replica_max_concurrent_requests = NULL, max_request_wait_time = NULL, num_replicas = NULL, tags = NULL, properties = NULL, description = NULL, models = NULL, inference_config = NULL, gpu_cores = NULL, period_seconds = NULL, initial_delay_seconds = NULL, timeout_seconds = NULL, success_threshold = NULL, failure_threshold = NULL, namespace = NULL, token_auth_enabled = NULL)` | `update`

### Notes
* for `create_aks_webservice_deployment_config` & `update_aks_webservice` don't expose `collect_model_data` parameter as `ModelDataCollector` class is getting deprecated
* for `update_aks_webservice` don't expose `image` parameter - `Image` class being deprecated

## AksCompute
R   | corresponding Python
--- | --------------------
`create_aks_compute <- function(workspace, cluster_name, agent_count = NULL, vm_size = NULL, ssl_cname = NULL, ssl_cert_pem_file = NULL, ssl_key_pem_file = NULL, location = NULL, vnet_resourcegroup_name = NULL, vnet_name = NULL, subnet_name = NULL, dns_service_ip = NULL, docker_bridge_cidr = NULL, cluster_purpose = NULL)` | `AksCompute.provisioning_configuration` & `ComputeTarget.create`
`get_aks_compute_credentials <- function(cluster)` | `get_credentials`
`attach_aks_compute <- function(resource_group = NULL, cluster_name = NULL, resource_id = NULL)` | `AksCompute.attach_configuration` & `ComputeTarget.attach`
`detach_aks_compute <- function(cluster)` | `detach`

### Notes
* in `create_aks_compute` & `attach_aks_compute`, don't expose `cluster_purpose` parameter for now - Jordan to follow up with inferencing team on this
* in `compute.R`:
  * change `delete_aml_compute` to `delete_compute` so function can also be used to delete AksCompute
  * add the following methods:
    * `serialize_compute <- function(cluster)`
    * `deserialize_to_compute <- function(workspace, compute_payload)` - Question: `cluster_payload` or `compute_payload`?
  * can we rename `wait_for_compute` to something more descriptive of what the method does?
* `refresh_state` vs `get_status`

## Additional notes
* `Image` class is being deprecated
* `ModelDataCollector` class is being deprecated
* `Run` class method `register_method` is being deprecated. Use only `Model.register` instead
* all `deploy` methods of `Webservice` class are being deprecated
* once `AksEndpoint` class changes get into SDK, add those wrappers to R SDK & remove exposure of `AksWebservice` class
