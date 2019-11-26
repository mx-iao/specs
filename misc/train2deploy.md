Assuming the package is called `modelpack`:

Every model that will be deployed on Azure ML is registered with the respective model file(s) and optionally a model.py file that defines a class that inherits from a type from `modelpack` (either `modelpack.PythonModel` or a strongly typed model specific to a framework, e.g. `modelpack.TensorFlowModel`, `modelpack.PyTorchModel ...).

For example, PythonModel class looks something like this:

```python
class PythonModel:
  def load(model_dir):
    ...
    
  def preprocess(request, request_type):
    ...
    
  def predict(input):
    ...
    
  def postprocess(prediction, response_type):
    ...
```

Each set of methods will have a default implementation specific to the type of model. If the user wants to override any or all of the methods, they can define their own custom class that inherits from any of the existing model classes in a model.py file that they specify during model registration (or in their training script if they train on Azure ML). The model class they inherit from must correspond to the model framework that they specify during registration (?).

Now with this implementation, the user no longer needs to provide a driver file for scoring. When deploying a web service, Azure ML will call the model's `load()` method to initialize and load the model, and when the service endpoint is invoked, Azure ML will call `preprocess()` on the request body, return the processed input which is then fed into the `predict()` call, and then postprocesses the prediction with `postprocess()`, and the return of that call is the response from the service.
