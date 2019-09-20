# workspace.R
R   | corresponding Python
--- | --------------------
`get_default_keyvault <- function(workspace)` | `get_default_keyvault`
`get_workspace_details <- function(workspace)` | `get_details`
`set_default_datastore <- function(workspace, datastore_name)` | `set_default_datastore`

Additional:  
* for already implemented functions, change parameter from `ws` to `workspace` for clarity & to maintain consistency across all classes

# datastore.R
R   | corresponding Python
--- | --------------------
`get_datastore <- function(workspace, datastore_name)` | `get`
`register_azure_blob_container_datastore <- function(workspace, datastore_name, container_name, account_name, sas_token = NULL, account_key = NULL, protocol = NULL, endpoint = NULL, overwrite = False, create_if_not_exists = False, skip_validation = False, blob_cache_timeout = NULL, grant_workspace_access = False, subscription_id = NULL, resource_group = NULL)` | `register_azure_blob_container`
`register_azure_file_share_datastore <- function(workspace, datastore_name, file_share_name, account_name, sas_token = NULL, account_key = NULL, protocol = NULL, endpoint = NULL, overwrite = False, create_if_not_exists = False, skip_validation = False)` | `register_azure_file_share`
`unregister_datastore <- function(datastore)` | `unregister`

Additional:  
* for already implemented functions, change parameter from `ds` to `datastore` for clarity & consistency

# compute.R
R   | corresponding Python
--- | --------------------
`list_nodes_in_aml_compute <- function(cluster)` | `list_nodes`
`list_supported_vmsizes <- function(workspace, location = NULL)` | `supported_vmsizes`
`update_aml_compute <- function(cluster, min_nodes = NULL, max_nodes = NULL, idle_seconds_before_scaledown = NULL)` | `update`

Additional:  
* rename existing function `wait_for_compute` to `wait_for_provisioning_completion`

# run.R
R   | corresponding Python
--- | --------------------
`download_file_from_run <- function(run, name, output_file_path = NULL)` | `download_file`
`download_files_from_run <- function(run, prefix = NULL, output_directory = NULL, output_paths = NULL, batch_size = 100)` | `download_files`
`get_run_details <- function(run)` | `get_details`
`get_run_details_with_logs <- function(run)` | `get_details_with_logs`
`get_run_file_names <- function(run)` | `get_file_names`
`log_accuracy_table_to_run <- function(run, name, value, description = '')` | `log_accuracy_table`
`log_confusion_matrix_to_run <- function(run, name, value, description = '')` | `log_confusion_matrix`
`log_image_to_run <- function(run, name, path = NULL, plot = NULL, description = '')` | `log_image`
`log_list_to_run <- function(run, name, value, description = '')` | `log_list`
`log_predictions_to_run <- function(run, name, value, description = '')` | `log_predictions`
`log_residuals_to_run <- function(run, name, value, description = '')` | `log_residuals`
`log_row_to_run <- function(run, name, description = '', ...)` | `log_row`
`log_table_to_run <- function(run, name, value, description = '')` | `log_table`

# experiment.R
R   | corresponding Python
--- | --------------------
`get_experiment_runs <- function(experiment, type = NULL, tags = NULL, properties = NULL, include_children = False)` | `get_runs`
