# Datasets

### Dataset

 R  | corresponding Python
--- | -------------------- 
`register_dataset <- function(workspace, dataset, name, description = NULL, tags = NULL, create_new_version = FALSE)` | `register`
`unregister_dataset <- function(dataset)` | `unregister_all_versions`
`get_dataset_by_name <- function(workspace, name, version = 'latest')` | `get_by_name`
`get_dataset_by_id <- function(workspace, id)` | `get_by_id`

Source code:
- `dataset.py`
- `abstract_dataset.py`

Methods/attributes to be accessed by `$`:
- `dataset$as_named_input(...)`
- `dataset$as_mount()`
- `dataset$as_download()`
- `run$input_datasets` (this is a dict though) or `get_input_dataset_from_run <- function(name)`

### FileDataset

R   | corresponding Python
--- | --------------------
`create_file_dataset_from_files <- function(path, validate = TRUE)` | `Dataset.File.from_files`
`dataset$to_path()` | `to_path`
`download_from_file_dataset <- function(dataset, target_path = NULL, overwrite = FALSE)` | `download`
`mount_file_dataset <- function(dataset, mount_point)` | `mount`
`skip_from_file_dataset <- function(dataset, count)` | `skip`
`take_from_file_dataset <- function(dataset, count)` | `take`
`take_sample_from_file_dataset <- function(dataset, probability, seed = NULL)` | `take`
`random_split_file_dataset <- function(dataset, percentage, seed = NULL)` | `random_split`

To access methods/attributes of `MountContext`, given an `MountContext`object called `mount_context`:
- `mount_context$mount_point`
- `mount_context$start()`
- `mount_context$stop()`

### TabularDataset

R   | corresponding Python
--- | --------------------
`create_tabular_dataset_from_parquet_files <- function(path, validate = TRUE, include_path = FALSE, set_column_types = NULL, partition_format = NULL)` | `Dataset.Tabular.from_parquet_files`
`create_tabular_dataset_from_delimited_files <- function(path, validate = TRUE, include_path = FALSE, infer_column_types = TRUE, set_column_types = NULL, separator = ',', header = TRUE, partition_format = NULL)` | `Dataset.Tabular.from_delimited_files`
`create_tabular_dataset_from_json_lines_files <- function(path, validate = TRUE, include_path = FALSE, set_column_types = NULL, partition_format = NULL)` | `Dataset.Tabular.from_json_lines_files`
`create_tabular_dataset_from_sql_query <- function(query, validate = TRUE, set_column_types = NULL)` | `Dataset.Tabular.from_sql_query`
`drop_columns_from_tabular_dataset <- function(dataset, columns)` | `drop_columns`
`keep_columns_from_tabular_dataset <- function(dataset, columns, validate = FALSE)` | `keep_columns`
`random_split_tabular_dataset <- function(dataset, percentage, seed = NULL)` | `random_split`
`skip_from_tabular_dataset <- function(dataset, count)` | `skip`
`take_from_tabular_dataset <- function(dataset, count)` | `take`
`take_sample_from_tabular_dataset <- function(dataset, probability, seed = NULL)` | `take_sample`
`filter_tabular_dataset_after_time <- function(dataset, start_time, include_boundary = FALSE)` | `time_after`
`filter_tabular_dataset_before_time <- function(dataset, end_time, include_boundary = FALSE)` | `time_before`
`filter_tabular_dataset_between_time <- function(dataset, start_time, end_time, include_boundary = FALSE)` | `time_before`
`filter_tabular_dataset_from_recent_time <- function(dataset, time_delta, include_boundary = FALSE)` | `time_recent`
`define_timestamp_columns_for_tabular_dataset <- function(dataset, fine_grain_timestamp, course_grain_timestamp = NULL, validate = FALSE)` | `define_timestamp_columns`
`load_tabular_dataset_into_data_frame <- function(dataset)` | `to_pandas_data_frame`
`convert_to_file_dataset_with_csv_files <- function(dataset, separator = ',')` | `to_csv_files`
`convert_to_file_dataset_with_parquet_files <- function(dataset)` | `to_parquet_files`

- factory methods require DataType
- for from_delimited_files: "Files will read as having no header When header=False. More options can be specified using enum value of :class:`azureml.data.dataset_type_definitions.PromoteHeadersBehavior`". Do we want to support this? Thought dataset_type_definitions were getting deprecated
- `from_sql_query` will require support for SQL datastore


data_types = {
                    'ID': DataType.to_string(),
                    'Date': DataType.to_datetime('%d/%m/%Y %I:%M:%S %p'),
                    'Count': DataType.to_long(),
                    'Latitude': DataType.to_float(),
                    'Found': DataType.to_bool()
                }
                
 ### Required updates to existing methods:
 - estimator
 - register_model
                
 ### Open questions
 - should we even wrap the parquet file methods if we don't have spark support for R?
 - 'azureml-dataprep[pandas,fuse]' does this need to be installed for remote training?
