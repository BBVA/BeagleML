# Project definition file.

To define the experiments to be launched, it is needed to provide
the project definition file.

This YAML file must follow the following format:

```
name: 'logistic'
template: 'dockecompose_model_template.yaml'
experiment_timeout: 600.0
experiment_accuracy_limit: 0.85
optimization:
    type: 'grid'
    objectiveFunction: 'experimentTime'
    objectiveResult: 'min'
parameters:
  - name: C
    type: 'lineal'
    value:
      initial_value: 0.1
      final_value: 1
      interval: 0.1
  - name: HIDDEN_SIZE
    type: 'layers'
    value:
      number_layers:
       - 1
       - 2
       - 3
      number_units:
       - 20
       - 50
       - 70
       - 100
  - name: 'METRICS_TOPIC'
    type: 'absolute'
    value:
      - 'metrics'
```

There are 3 sections within the file:

#### Project:
 Project information and certain experiments constraints.

 - ***name***: Name of the project.

 - ***template***: Filename of the uploaded ```model_template```, where the model and parameters are defined.

 - ***experiment_timeout***: Timeout for each experiment.
 If reached, the scheduler stops the experiment and sets it as "timeout" in bbdd.

 - ***experiment_accuracy_limit***: Sufficient accuracy threshold for each experiment.
 If reached, the scheduler stops the experiment and sets it as "accuracy_limit_reached" in bbdd.

#### Heurisitc:
Heuristic configuration parameters.

- ***type***:
Heuristic to use (i.e., grid, random, ...)

- ***objectiveFunction***: Metric used as optimization criterion.

- ***objectiveResult***: It indicates if the objective function must be maximized or minimized
(values: "max" or "min").

#### Hyper-parameters:

List of hyper-parameters to tune, with its particular type and its possible values.

 - ***name***: Hyper-parameter name (must be specified in the ```model_template``` file).

 - ***type and value***: The field ```value``` depends on the ```type``` selected.

   There are 3 different types:

    - ***absolute***: List of values to test (can be integer, strings, float,...)

    - ***lineal***: Starts with an initial value (float), which gets incremented linearly until a final value.
    The behaviour is defined with 3 fields: ```initial-value, final-value,``` and ```interval```.

    - ***layers***: Specifically for neural networks, it is composed of:

      num_layers: Number of hidden layers.

  	  number_units: Number of neurons of each layer.

	  For instance, ```num_layers=[1,2] and number_units=[10,20]```,
	  results in these combinations ```[(10),(20),(10,10), (10, 20), (20, 10), (20,20)]```.

### Disclaimer
* Due to OpenShift naming rules, it is not recommended to use dots (".") in names.
