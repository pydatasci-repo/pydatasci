![PyDataSci (wide)](/images/logo_pds_wide.png)

---
*pre-alpha; in active development*

# Value Proposition
*PyDataSci* is an open source, automated machine learning (AutoML) tool for data scientists that reduces the amount of code needed to perform best practice machine learning by 95%; more science with less code.

It is a Python package that records experiments in a lightweight, file-based database that works on Mac/ Linux/ Windows without any configuration required by the user. By tracking the input (samples and settings) as well as output (models and metrics) of each experiment, it makes machine learning reproducible; less of a black box.

Users can either (a) queue many experiments on their desktop/ server, or (b) delegate them to run in the *PyDataSci* cloud if they outgrow their local resources. From there, model performance metrics can be visually compared in interactive charts. It is designed for use within Jupyter notebooks, but runs in any Python shell.

## TLDR
```python
$ pip install pydatasci
>>> import pydatasci as pds
>>> from pydatasci import aidb
```

![Model Metrics](/images/chart_boomerang.png)
<div align="center"><i>Examples of built-in charts. Seen above is the new "boomerang chart" for comparing performance across models.</i></div><br/>

![Model Metrics](/images/chart_history.png)



## Mission
* **Accelerating Research at Universities & Institutes Everywhere.**<br />We empower non-cloud users: the academic/ institute HPCers, the private clouders, the remote server SSH'ers, and everyday desktop hackers - with the same quality ML tooling as present in public clouds (e.g. AWS SageMaker). This toolset provides research teams a standardized method for ML-based evidence, rather than each researcher spending time cobbling together their own approach.<br /><br />

* **Reproducible Experiments.**<br />No more black boxes. No more screenshotting loss-accuracy graphs and hyperparameter combinations. A record of every: dataset, feature, label, sample, split, fold, parameter, model, training job, and result - is automatically persisted in a lightweight, file-based database that is automatically configured when you import the package. Submit your *aidb* database file alongside your publications/ papers and model zoo entries as a proof.<br /><br />

* **Queue Hypertuning Jobs.**<br />Design a batch of runs to test many hypotheses at once. Queue many hypertuning jobs locally, or delegate big jobs to the cloud to run in parallel by setting `cloud_queue = True`.<br /><br />

* **Visually Compare Performance Metrics.**<br />Compare models using pre-defined plots for assessing performance, including: quantitative metrics (e.g. accuracy, loss, variance, precision, recall, etc.), training histories, and confusion/ contingency matrices.<br /><br />

* **Code-Integrated & Agnostic.**<br />We don’t disrupt the natural workflow of data scientists by forcing them into the confines of a GUI app or specific IDE. Instead, we weave automated tracking into their existing scripts so that *PyDataSci* is compatible with any data science toolset.<br /><br />

![Ecosystem Banner (wide)](/images/ecosystem_banner.png)


## Functionality
*Initially focusing on tabular data before expanding to multi-file use cases.*
- [Done] Compress an immutable dataset (csv, tsv, parquet, pandas dataframe, numpy ndarray) to be analyzed.
- [Done] Split stratified samples by index while treating validation sets (3rd split) and cross-folds (k-fold) as first-level citizens.
- [Done] Generate hyperparameter combinations for model building, training, and evaluation.
- [Done] Preprocess samples to encode them for specific algorithms.
- [Done] Queue hypertuning jobs and batches based on hyperparameter combinations.
- [Done] Evaluate and save the performance metrics of each model. 
- [Done] Visually compare model metrics to find the best one.

- [ToDo] Talk to users to find out if they want: time series and image data, pytorch support, or unsupervised learning more.

- [Future] Derive informative featuresets from that dataset using supervised and unsupervised methods.
- [Future] Behind the scenes, stream rows from your datasets w generators to keep a low memory footprint.
- [Future] Scale out to run cloud jobs in parallel by toggling `cloud_queue = True`.


## Community
*Much to automate there is. Simple it must be.* ML is a broad space with a lot of challenges to solve. Let us know if you want to get involved. We plan to host monthly dev jam sessions and data science lightning talks.

* **Data types to support:** tabular, time series, image, graph, audio, video, gaming.
* **Analysis types to support for each data type:** classification, regression, dimensionality reduction, feature engineering, recurrent, generative, NLP, reinforcement.

---

# Installation
Requires Python 3+ (check your deep learning library's Python requirements). You will only need to perform these steps the first time you use the package. 

Enter the following commands one-by-one and follow any instructions returned by the command prompt to resolve errors should they arise.

_Starting from the command line:_
```bash
$ pip install --upgrade pydatasci
$ python
```
_Once inside a Python shell:_
```python
>>> import pydatasci as pds
>>> pds.create_folder()
>>> pds.create_config()

>>> from pydatasci import aidb
>>> aidb.create_db()
```

> PyDataSci makes use of the Python package, `appdirs`, for an operating system (OS) agnostic location to store configuration and database files. This not only keeps your `$HOME` directory clean, but also helps prevent careless users from deleting your database. 
>
> The installation process checks not only that the corresponding appdirs folder exists on your system but also that you have the permissions neceessary to read from and write to that location. If these conditions are not met, then you will be provided instructions during the installation about how to create the folder and/ or grant yourself the appropriate permissions. 
>
> We have attempted to support both Windows (`icacls` permissions and backslashes `C:\\`) as well as POSIX including Mac and Linux including containers & Google Colab (`chmod letters` permissions and slashes `/`). Note: due to variations in the ordering of appdirs author and app directories in different OS', we do not make use of the appdirs `appauthor` directory, only the `appname` directory.
>
> If you run into trouble with the installation process on your OS, please submit a GitHub issue so that we can attempt to resolve, document, and release a fix as quickly as possible.
>
> _Installation Location Based on OS_<br />`import appdirs; appdirs.user_data_dir('pydatasci');`:
> * Mac: <br />`/Users/Username/Library/Application Support/pydatasci`<br /><br />
> * Linux - Alpine and Ubuntu: <br />`/root/.local/share/pydatasci`<br /><br />
> * Windows: <br />`C:\Users\Username\AppData\Local\pydatasci`
>
> `create_db()` is equivalent to a *migration* in Django or Rails in that it creates the tables found in the Object Relational Model (ORM). We use the [`peewee`](http://docs.peewee-orm.com/en/latest/peewee/models.html) ORM as it is simpler than SQLAlchemy, has good documentation, and found the project to be actively maintained (saw same-day GitHub response to issues on a Saturday). With the addition of Dash-Plotly, this will make for a full-stack experience that also works directly in an IDE like Jupyter or VS Code.


### Deleting & Recreating the Database
When deleting the database, you need to either reload the `aidb` module or restart the Python shell before you can attempt to recreate the database.

```python
>>> from pydatasci import aidb
>>> aidb.delete_db(True)

>>> from importlib import reload
>>> reload(aidb)
>>> create_db()
```

---

# Usage

If you've already completed the *Installation* section above, then let's get started.


## I. Built-In Examples.
```python
import pydatasci as pds
from pydatasci import aidb
from pydatasci.aidb import examples
```

There are a few KB of tabular `demo_files` included with the package that are well suited for classification (multi & binary) as well as regression.
```
examples.list_demo_files()

df = examples.demo_file_to_pandas('iris.tsv')
```

If you want to skip the process of preparing sample data and jump right into the hyperparameter tuning, then take a look at the `demo_batches`. All examples include a validation split and 

```python
examples.list_demo_batches()

batch = examples.make_demo_batch(batch_name:str)

batch.run_jobs()

batch.metrics_to_pandas()

# `metric_2` is 'accuracy' for classification models or 'r^2' for regression models.
batch.plot_performance(max_loss=3.0, min_metric_2=0.0)
```
Reference the section titled '12. Visually compare the performance of your hypertuned Algorithms.' for more plots.


## II. Beginner (high-level API).

```python
import pydatasci as pds
from pydatasci import aidb
from pydatasci.aidb import examples
from sklearn.preprocessing import *
```
A `DataPipeline` boils down all of the procedures involved in preparing samples to be fed into a machine learning model into a single step. Its `.make()` method allows for customization such as stratified validation splits, stratified (categorical & continuous) cross-validation folds, and encoding samples.

```python
datapipeline = aidb.DataPipeline.make(
	dataFrame_or_filePath = examples.demo_file_to_pandas('iris_10x.tsv')
	, label_column = 'species'
	, size_test = 0.30
	#, size_validation:float #skipping due to use of folds.
	, fold_count = 5
	, encoder_features = StandardScaler()
	, encoder_labels = OneHotEncoder(sparse=False)
)
```
Now you are ready to build your model from the template below. Anytime you see an argument like `samples_*`, know that the appropriate samples (splits and/or folds) will be provided to your model automatically. We'll specify `**hyperparameters` in a minute, so that you can specify any `hyperparameters['variable_name']` you like when defining your model. Welcome to hypertuning!

```python
def function_model_build(**hyperparameters):
	model = Sequential()
	model.add(Dense(hyperparameters['neuron_count'], input_shape=(4,), activation='relu', kernel_initializer='he_uniform'))
	model.add(Dropout(0.2))
	model.add(Dense(hyperparameters['neuron_count'], activation='relu', kernel_initializer='he_uniform'))
	model.add(Dense(3, activation='softmax'))

	opt = keras.optimizers.Adamax(hyperparameters['learning_rate'])
	model.compile(
		loss = 'categorical_crossentropy'
		, optimizer = opt
		, metrics = ['accuracy']
	)
	return model

def function_model_train(model, samples_train, samples_evaluate, **hyperparameters):
	model.fit(
		samples_train["features"]
		, samples_train["labels"]
		, validation_data = (
			samples_evaluate["features"]
			, samples_evaluate["labels"]
		)
		, verbose = 0
		, batch_size = hyperparameters['batch_size']
		, epochs = hyperparameters['epoch_count']
		, callbacks=[History()]
	)
	return model

algorithm = aidb.Algorithm.make(
	library = "keras"
	, analysis_type = "classification_multi"
	, description = "dense, 2 layers, medium height"
	, function_model_build = function_model_build
	, function_model_train = function_model_train
)
```
> If either `function_model_predict` or `function_model_loss` are left blank, then they will be automatically populated for you based on the `library` and `analysis_type` that you specified.

Create an `Experiment` to keep track of all of the models generated by combinations of your hyperparameters. 

```python
hyperparameters = {
	"neuron_count": [9, 12]
	, "batch_size": [3]
	, "learning_rate": [0.03, 0.05]
	, "epoch_count": [30, 60]
}

experiment = algorith.make_experiment(
	datapipeline_id = datapipeline.id
	, hyperparameters = hyperparameters
	, description = "primarily comparing epochs and lr"
)

experiment.batch.run_jobs()

experiment.batch.metrics_to_pandas()

# `metric_2` is 'accuracy' for classification models or 'r^2' for regression models.
batch.plot_performance(max_loss=3.0, min_metric_2=0.0)
```
Reference the section titled '12. Visually compare the performance of your hypertuned Algorithms.' for more plots.



## III. Advanced (low-level API).

Within the `/notebooks` folder of this repository, there are notebooks that you can follow along with for:

- Multi-class Classification.
- Binary Classification.
- Regression.

### 1. Import the Library
```python
import pydatasci as pds
from pydatasci import aidb
```

### 2. Ingest a `Dataset` as a compressed file.

Supported tabular file formats include CSV, [TSV](https://stackoverflow.com/a/9652858/5739514), [Apache Parquet](https://parquet.apache.org/documentation/latest/).

```python
# From a file.
dataset = aidb.Dataset.from_file(
	path = 'iris.tsv' # files must have column names as their first row
	, file_format = 'tsv'
	, name = 'tab-separated plants'
	, perform_gzip = True
)

# From an in-memory data structure.
dataset = aidb.Dataset.from_pandas(
	dataframe = df
	, file_format = 'csv'
	, name = 'comma-separated plants'
	, perform_gzip = False
)

dataset = aidb.Dataset.from_numpy(
	ndarray = arr
	, file_format = 'parquet'
	, name = 'chunking plants'
	, perform_gzip = True
	, dtype = None # feeds pd.Dataframe(dtype)
	, column_names = None # feeds pd.Dataframe(columns)
)
```
> Apart from `read_numpy()`, it's best if you provide your own column names ahead of time as the first row of your files and DataFrames that you want to ingest.

> The bytes of the data will be stored as a BlobField in the SQLite database file. Storing the data in the database not only (a) provides an entity that we can use to keep track of experiments and link relational data to but also (b) makes the data less mutable than keeping it in the open filesystem.

> You can choose whether or not you want to gzip compress the file when importing it with the `perform_gzip=bool` parameter. This compression not only enables you to store up to 90% more data on your local machine, but also helps overcome the maximum BlobField size of 2.147 GB. We handle the zipping and unzipping on the fly for you, so you don't even notice it.

> Optionally, `dtype`, as seen in [`pandas.DataFrame.astype(dtype)`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.astype.html), can be specified as either a single type for all columns, or as a dict that maps a specific type to each column name. This encodes features for analysis. We read NumPy into Pandas before persisting it, so `columns` and `dtype` are read directly by `pd.DataFrame()`.

> At this point, the project's support for Parquet is extremely minimal.

> If you leave `name` blank, it will default to a human-readble timestamp with the appropriate file extension (e.g. '2020_10_13-01_28_13_PM.tsv').

#### Fetch a `Dataset` with either **Pandas** or **NumPy**.

Supported in-memory formats include [NumPy Structured Array](https://numpy.org/doc/stable/user/basics.rec.html) and [Pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html). 

```python
# Implicit IDs
df = dataset.to_pandas()
df.head()

arr = dataset.to_numpy()
arr[:4]

# Explicit IDs
df = aidb.Dataset.to_pandas(id=dataset.id)
df.head()

arr = aidb.Dataset.to_numpy(id=dataset.id)
arr[:4]
```

> For the sake of simplicity, we are reading into NumPy via Pandas. That way, if we want to revert to a simpler ndarray in the future, then we won't have to rewrite the function to read NumPy.

### 3. Select the `Label` column(s).

From a Dataset, pick the column(s) that you want to train against/ predict. If you are planning on training an unsupervised model, then you don't need to do this.

```python
# Implicit IDs
label_column = 'species'
label = dataset.make_label(columns=[label_column])

# Explicit IDs
label = aidb.Label.from_dataset(
	dataset_id=dataset.id
	, columns=[label_column]
)
```

> Again, read a Label into memory with `to_pandas()` and `to_numpy()` methods.

> Labels accept multiple columns for situations like one-hot encoding (OHE).


### 4. Select the `Featureset` column(s).

Creating a Featureset won't duplicate your data! It simply records the `columns` to be used in training. 

Here, we'll just exclude a Label column in preparation for supervised learning, but you can either exlcude or include any columns you see fit.

```python
# Implicit IDs
featureset = dataset.make_featureset(exclude_columns=[label_column])

# Explicit IDs
featureset = aidb.Featureset.from_dataset(
	dataset_id = dataset.id
	, include_columns = None
	, exclude_columns = [label_column]
)

>>> featureset.columns
['sepal length (cm)',
 'sepal width (cm)',
 'petal length (cm)',
 'petal width (cm)']

>>> featureset.columns_excluded
['species']
```

Again, read a Featureset into memory with `to_pandas()` and `to_numpy()` methods.

> The `include_columns` and `exclude_columns` parameters are provided to expedite column extraction:
> - If both `include_columns=None` and `exclude_columns=None` then all columns in the Dataset will be used.
> - If `exclude_columns=[...]` is specified, then all other columns will be included.
> - If `include_columns=[...]` is specified, then all other columns will be excluded. 
> - Remember, these parameters accept *[lists]*, not raw *strings*.


### 5. Generate `splits` of samples.

Divide the `Dataset` rows into `Splitsets` based on how you want to train, validate (optional), and test your models.

Again, creating a Splitset won't duplicate your data. It simply records the samples (aka rows) to be used in your train, validation, and test splits. 

```python
splitset_train70_test30 = featureset.make_splitset(label_id=label.id)

splitset_train70_test30 = featureset.make_splitset(
	label_id = label.id
	, size_test = 0.30
)

splitset_train68_val12_test20 = featureset.make_splitset(
	label_id = label.id
	, size_test = 0.20
	, size_validation = 0.12
)

splitset_unsupervised = featureset.make_splitset()
```

> Label-based stratification is used to ensure equally distributed label classes for both categorical and continuous data.

> The `size_test` and `size_validation` parameters are provided to expedite splitting samples:
> - If you leave `size_test=None`, it will default to `0.30` when a Label is provided.
> - You cannot specify `size_validation` without also specifying `size_test`.
> If you want more control over stratification of continuous features, then you can specify the number of `continuous_bin_count` for grouping.

Again, read a Splitset into memory with `to_pandas()` and `to_numpy()` methods. Note: this will return a `dict` of either data frames or arrays.

```python
>>> splitset_train68_val12_test20.sizes
{
	'train': {
		'percent': 0.68, 	
		'count': 102},

	'validation': {
		'percent': 0.12,
		'count': 18}, 

	'test':	{
		'percent': 0.2, 	
		'count': 30}
}

>>> splitset_train68_val12_test20.to_numpy()
{
	'train': {
		'features': <ndarray>,
		'labels': <ndarray>},

	'validation': {
		'features': <ndarray>,
		'labels': <ndarray>},

	'test': {
		'features': <ndarray>,
		'labels': <ndarray>}
}
```

### 6. Optionally, create `Folds` of samples for cross-fold validation.
*Reference the [scikit-learn documentation](https://scikit-learn.org/stable/modules/cross_validation.html) to learn more about folding.*
![Cross Folds](/images/cross_folds.png)

As seen in the image above, different cuts through the training data are produced while the test data remains untouched. The training data is divided into stratified folds where one fold is left out each time, while the test data is left untouched. 

We refer to the left out fold as the `fold_validation` and the remaining training data as the `folds_train_combined`. The samples of the validation fold are still recorded if you wanted to generate performance metrics against them.

> In a scenario where a validation split was specified in the original Splitset, the validation split is also untouched. Only the training data is folded. The implication is that you can have 2 validations in the form of the validation split and the validation fold.

```python
foldset = splitset_train68_val12_test20.make_foldset(fold_count=5)
#generates `foldset.fold[0:4]`
```

Again, read a Foldset into memory with `to_pandas()` and `to_numpy()` methods. Note: this will return a `dict` of either data frames or arrays.


### 7. Optionally, create a `Preprocess`.
If you want to either encode, standardize, normalize, or scale you Features and/ or Labels - then you can make use of `sklearn.preprocessing` methods.

```python
preprocess = aidb.Preprocess.from_splitset(
    splitset_id = splitset_id
    , description = "standard scaling on features"
    , encoder_features = encoder_features
    , encoder_labels = encoder_labels
)
```

### 8. Create an `Algorithm` aka model.


#### Create variations of hyperparameters.

For each parameter that you want to tune, simply provide lists of the values that you want to test. These will get passed as kwargs, `**hyperparameters`, to your model functions in the next steps.

```python
hyperparameters = {
    "l2_neuron_count": [9, 13, 18]
    , "optimizer": ["adamax", "adam"]
    , "epochs": [66, 99]
}
```

#### Create a function to build the model.

Here you can see we specified the variables `l1_neuron_count`, `l2_neuron_count`, and `optimizer`. We also made an entire layer optional with a simple *if* statement on `l2_exists`!

In a moment, we will make a dictionary of these variables, and they will be fed in via the `**hyperparameters` parameter.

```python
def function_model_build(**hyperparameters):
    model = Sequential()
    model.add(Dense(13, input_shape=(4,), activation='relu', kernel_initializer='he_uniform'))
    model.add(Dropout(0.2))
    model.add(Dense(hyperparameters['l2_neuron_count'], activation='relu', kernel_initializer='he_uniform'))
    model.add(Dense(3, activation='softmax', name='output'))

    model.compile(optimizer=hyperparameters['optimizer'], loss='categorical_crossentropy', metrics=['accuracy'])
    
    return model
```

#### Create a function to train the model.

The appropriate training and evaluation samples are automatically made available to you behind the scenes based on how you designed your Splitset and/ or Foldset.

```python
def function_model_train(model, samples_train, samples_evaluate, **hyperparameters):
    model.fit(
        samples_train["features"]
        , samples_train["labels"]
        , validation_data = (
            samples_evaluate["features"]
            , samples_evaluate["labels"]
        )
        , verbose = 0
        , batch_size = 3
        , epochs = hyperparameters['epochs']
        , callbacks=[History()]
    )
    return model
```
> If you created a Foldset, then your `samples_train` will point to your Fold's `folds_train_combined` and your `samples_evaluate` will point to your `fold_validation`. If you didn't create a Foldset, but did specify `test` and/ or `validation` splits in your Splitset, then `samples_evaluate` will point to those splits with a preference for the `validation` split. If your model doesn't provide an evaluation history or you only specified a `train` split in your Splitset then you don't need to worry about it.

#### Create a function to predict.

For most libraries, classification algorithms output probabilities as opposed to actual predictions when running `model.predict()`. Notice in the `return` line that we return both the `predictions` as well as the `probabilities` and the order matters. We will use both objects behind the scenes when generating performance metrics.

Additionally, all classification `predictions`, both mutliclass and binary, must be returned in ordinal format. 

```python
def function_model_predict(model, samples_predict):
    # Returns a nested array [[prob,prob,prob],] of probabilities for multiclass/ OHE samples.
    probabilities = model.predict(samples_predict['features'])
    # This is the official Keras replacement for multiclass `.predict_classes()`
    # Returns a flat, ordinal array: `[0, 1, 2, 3]`
    predictions = np.argmax(probabilities, axis=-1)
    
    return predictions, probabilities
```
> Calculating performance metrics performance metrics will not work in OHE format.
> For classification models both 


#### Create a function to calculate loss.

Most deep learning models provide an `.evaluate()` method in order to calculate loss.

```python
def function_model_loss(model, samples_evaluate):
    loss, _ = model.evaluate(samples_evaluate['features'], samples_evaluate['labels'], verbose=0)
    return loss
```
> In contrast to openly specifying a loss function, for example `keras.losses.<loss_fn>()`, the use of `.evaluate()` is consistent because it comes from the compiled model. Also, although `model.compiled_loss` would be more efficient, it requires making encoded `y_true` and `y_pred` available to the user, whereas `.evaluate()` can be called with the same arugments as the other `function_model_*` and many deep learning libraries support this approach. 

#### Pull it all together in creating the Algorithm.

```python
algorithm = aidb.Algorithm.create(
    description = "dense with 1 or 2 layers"
    , library = "Keras"
	, analysis_type = "classification_multi"
	, function_model_build = function_model_build
	, function_model_train = function_model_train
	, function_model_predict = function_model_predict
	, function_model_loss = function_model_loss
)
```


### 9. Optionally, create combinations of `Hyperparamsets` for our model.

Remember those variables we specified in our model functions? Here we provide values for them, which will be used in `**hyperparameters`.

In the future, we will provide different strategies for generating and selecting parameters to experiment with.

```python
hyperparameter_lists = {
	"l1_neuron_count": [9, 18]
	, "l2_neuron_count": [9, 18]
	, "optimizer": ["adamax", "adam"]
	, "epochs": [30, 60, 90]
}

hyperparamset = aidb.Hyperparamset.from_algorithm(
	algorithm_id = algorithm.id
	, description = "experimenting with neuron count, layers, and epoch count"
	, hyperparameter_lists = hyperparameter_lists
)
```

### 10. Create a `Batch` of `Job`s to keep track of training.
```python
batch = aidb.Batch.from_algorithm(
	algorithm_id = algorithm.id
	, splitset_id = splitset.id
	, hyperparamset_id = hyperparamset.id
	, foldset_id = foldset.id
	, preprocess_id = preprocess.id
)
```

#### When you are ready, run the Jobs.
The jobs will be asynchronously executed on a background process, so that you can continue to code on the main thread. You can poll the job status.

```python
batch.run_jobs()
batch.get_statuses()
```

You can stop the execution of a batch if you need to, and later resume it. If your kernel crashes then you can likewise resume the execution.

```python
batch.stop_jobs()
batch.run_jobs()
```

### 11. Assess the `Results`.
The following artifacts are automatically written to `Job.results[0]` after training:
```python
class Result(BaseModel):
	model_file = BlobField()
	history = JSONField()
	predictions = PickleField()
	probabilities = PickleField()
	metrics = PickleField()
	plot_data = PickleField()

	job = ForeignKeyField(Job, backref='results')
```

#### Fetching the trained model.

```python
compiled_model = batch.jobs[0].results[0].get_model()
compiled_model

<tensorflow.python.keras.engine.sequential.Sequential at 0x14d4f4310>
```


### 12. Visually compare the performance of your hypertuned Algorithms.

There are several methods for automated plotting that pull directly from the persisted metrics of each `Job`.

These vary based upon the `analysis_type` of the `Algorithm`.

#### Classification metrics:

Charts about aggregate performance metrics for the whole `Batch`.
- `batch.plot_performance(max_loss=0.3, min_metric_2=0.85)`

Charts about individual performance metrics for a `Job`.
- `batch.jobs[0].results[0].plot_learning_curve()`

Charts specific to classification `Algorithms`.
- `batch.jobs[0].results[0].plot_roc_curve()`
- `batch.jobs[0].results[0].plot_precision_recall()`
- `batch.jobs[0].results[0].plot_confusion_matrix()`

---

# PyPI Package

### Steps to Build & Upload

```bash
$ pyenv activate pydatasci
$ pip3 install --upgrade wheel twine
$ python3 setup.py sdist bdist_wheel
$ python3 -m twine upload --repository pypi dist/*
$ rm -r build dist pydatasci.egg-info
# proactively update the version number in setup.py next time
$ pip install --upgrade pydatasci; pip install --upgrade pydatasci
```

---

Shoutout to the [ORM](http://docs.peewee-orm.com/en/latest/index.html). Glad we took a chance on this SQLAlchemy alternative. Check out [neomodel](https://neomodel.readthedocs.io/en/latest/getting_started.html) for a graph database ORM; biology doesn't perform `joins`.