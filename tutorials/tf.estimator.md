# tf.estimator
The Estimator is a high-level API that is officially provided by TensorFlow. One of the advantages of using `tf.estimator` is that the code automatically builds a graph and a tensorboard itself. A user only needs to specify required arguments, such as features, optimizer, and labels, and plug into the estimator class. 
Just like PyTorch, the Estimator can easily switch to training, evaluation, or test modes without creating individual methods. 
Also, estimators are built upon the `tf.keras.layers` which simplifies the customization.
It is preferred to know the Estimator usage because TensorFlow offers `TPUEstimator` which extends to the Estimator class.

The entire structure of constructing estimator model is shown below.

```py
def model_fn(features, targets, mode, params):
   # Logic to do the following:
   # 1. Configure the model via TensorFlow operations
   # 2. Define the loss function for training/evaluation
   # 3. Define the training operation/optimizer
   # 4. Generate predictions
   return predictions, loss, train_op
```

It is always easy to understand the new classes with the relevant example code.


## Example code: [linear_regression.py](https://github.com/tensorflow/tensorflow/blob/r1.11/tensorflow/examples/get_started/regression/linear_regression.py)

1. We need a certain container that stores features of the data. In this example, we create a dictionary, `feature_columns` that represents keys and values of features.

```py
feature_columns = [
	# "curb-weight" and "highway-mpg" are numeric columns.
	tf.feature_column.numeric_column(key="key1"),
	tf.feature_column.numeric_column(key="key2"),
]
```

`tf.feature_column.numeric_column` represents real-valued or numerical features with designated key value.  `key1` and `key2` are categorical features of the provided data.

2. We build a simple linear regression estimator based on the created dictionary.

```py
model = tf.estimator.LinearRegressor(feature_columns=feature_columns)
```

The created model represents a linear regression function with the `feature_columns`.
Other estimator models are described below.

* [linear_regression.py](https://www.github.com/tensorflow/tensorflow/blob/r1.11/tensorflow/examples/get_started/regression/linear_regression.py)
* [linear_regression_categorical.py](https://www.github.com/tensorflow/tensorflow/blob/r1.11/tensorflow/examples/get_started/regression/linear_regression_categorical.py)
* [dnn_regression.py](https://www.github.com/tensorflow/tensorflow/blob/r1.11/tensorflow/examples/get_started/regression/dnn_regression.py)
* [custom_regression.py](https://www.github.com/tensorflow/tensorflow/blob/r1.11/tensorflow/examples/get_started/regression/custom_regression.py): to train a customized DNN regression model.

3. Train the model.

```py
# By default, the Estimators log output every 100 steps.
model.train(input_fn=input_train, steps=STEPS)
```

`input_train` is a customized generator including batch and shuffle features containing train data. At every 100 steps, the Estimators compute the output.

4. Evaluate how the model performs on data it has not yet seen.

```py
eval_result = model.evaluate(input_fn=input_test)
```

`input_test` is another customized generator without shuffle feature, because a validation usually processes with a regular dataset.

5. The evaluation returns a Python dictionary. The "average_loss" key holds the Mean Squared Error (MSE).

```py
average_loss = eval_result["average_loss"]
```

6. Run the model in prediction mode.

```py
input_dict = {
	"curb-weight": np.array([2000, 3000]),
	"highway-mpg": np.array([30, 40])
}
predict_input_fn = tf.estimator.inputs.numpy_input_fn(
	input_dict, shuffle=False)
predict_results = model.predict(input_fn=predict_input_fn)
```

Use the Estimator to predict the test data.


## ModeKeys and EstimatorSpec
Both are the fundamental classes defined at  [tensorflow/python/estimator/model_fn.py](https://www.github.com/tensorflow/tensorflow/blob/r1.11/tensorflow/python/estimator/model_fn.py). By calling `tf.estimator.ModeKeys.<ModeKeys>`, the Estimator performs corresponded operations designated at each modes. `ModeKeys` has 3 modes and each requires certain fields.

* **TRAIN** requires `loss` and `train_op`.
* **EVAL** requires `loss`.
* **PREDICT** requires `predictions`.

`tf.estimator` provides 3 kinds of regressors and classifiers.
* [tf.estimator.DNNRegressor](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor): for deep models that perform multi-class regression.
* [tf.estimator.DNNClassifier](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNClassifier): for deep models that perform multi-class classification.
* [tf.estimator.DNNLinearCombinedRegressor](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNLinearCombinedRegressor): for wide & deep models.
* [tf.estimator.DNNLinearCombinedClassifier](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNLinearCombinedClassifier): for wide & deep models.
* [tf.estimator.LinearRegressor](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor): for regressors based on linear models.
* [tf.estimator.LinearClassifier](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearClassifier): for classifiers based on linear models.

The use of `DNNClassifier`, for instance, is shown below.

```py
# Build a DNN with 2 hidden layers and 10 nodes in each hidden layer.
classifier = tf.estimator.DNNClassifier(
	feature_columns=my_feature_columns,
	# Two hidden layers of 10 nodes each.
	hidden_units=[10, 10],
	# The model must choose between 3 classes.
	n_classes=3)
```

All the graph, tensorboard, and model specifications are automatically performed using `tf.estimator`, without specifying `tf.variable_scope()` or building models. Then, the specified operations and objects returned from a `model_fn` are passed through EstimatorSpec which is defined [here](https://github.com/tensorflow/tensorflow/blob/r1.11/tensorflow/python/estimator/model_fn.py#L67). 

`EstimatorSpec` fully defines the model to be run by an Estimator. `EstimatorSpec` takes required fields regarding specified `ModeKeys`.

```py
tf.estimator.EstimatorSpec(
	mode=mode,
	predictions=predictions,
	loss=loss,
	train_op=train_op)
```

With `ModeKeys` and `EstimatorSpec`, models' train/eval/test functions are automatically inferred without creating each separated functions. 

## Next:
* More details of Estimator classes
* TPUEstimator
