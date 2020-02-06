**Estimator**
```python
pt_estimator = PyTorch(source_directory=project_folder,
                       compute_target=compute_target,
                       entry_script='train.py',
                       script_params={'--num_epochs': 30},
                       node_count=2,
                       distributed_training=MPI(process_count_per_node=4),
                       inputs=[dataset.as_named_input('cifar10')],
                       use_gpu=True,
                       framework_version='1.3')

run = experiment.submit(pt_estimator)
```

**RunConfig**
```python
pt_env = Environment.get(ws, name='AzureML-PyTorch-1-3-GPU'))
pt_run_config = ScriptRunConfig(source_directory=project_folder,
                                compute_target=compute_target,
                                entry_script='train.py',
                                arguments={'--num_epochs': 30},
                                node_count=2,
                                communicator=MPI(process_count_per_node=4),
                                environment=pt_env,
                                inputs=[dataset.as_named_input('cifar10')])
                                
run = experiment.submit(pt_run_config)
```
