## Explore Machine Learning Models with Explainable AI

*lab Link* (GSP324): https://google.qwiklabs.com/focuses/12011?parent=catalog

**Start a JupyterLab Notebook instance**
* Create a bucket with **Project ID** as name > leave all to default and click **CREATE**

* From navigation bar > AI Platform > **Notebooks** > **NEW INSTANCE** > select the version of `TensorFlow Enterprise 2.3 Without GPUs` 

* Leave all default > **CREATE**

* Wait for it to be created > and click **Open Jupyterlab**
 
## Download the Challenge Notebook
* In your notebook, click the `terminal`

* Clone the repo
```yaml
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
```

* Go to the enclosing folder: `training-data-analyst/quests/dei`

* On left pane Click --> what-if-tool-challenge.ipynb.

* `Run each cells` to Download and import the dataset hmda_2017_ny_all-records_labels.

### Build and train your models

> Train your first model:

```python
model = Sequential() 
model.add(layers.Dense(200, input_shape=(input_size,), activation='relu'))
model.add(layers.Dense(50, activation='relu'))
model.add(layers.Dense(20, activation='relu'))
model.add(layers.Dense(1, activation='sigmoid'))

# The data will come from train_data and train_labels.
model.compile(loss='mean_squared_error', optimizer='adam', metrics=['accuracy'])
model.fit(train_data, train_labels, epochs=10, batch_size=2048, validation_split=0.1)
```

> Train your second model:

```python
limited_model = Sequential()
limited_model.add(layers.Dense(200, input_shape=(input_size,), activation='relu'))
limited_model.add(layers.Dense(50, activation='relu'))
limited_model.add(layers.Dense(20, activation='relu'))
limited_model.add(layers.Dense(1, activation='sigmoid'))
limited_model.compile(loss='mean_squared_error', optimizer='adam', metrics=['accuracy'])

# The data will come from limited_train_data and limited_train_labels.
limited_model.fit(limited_train_data, limited_train_labels, epochs=10, batch_size=2048, validation_split=0.1)
```
### Deploy the models to AI Platform
```
Replace `#TODO` with your Project ID
```
### Now go to AI platform -> **Models** -> **Create model**

```yaml
Set model name as: complete_model
```
Click **Create**

> now click **complete_model** -> **create a version**
```yaml
- Version Name : v1
- Python version : 3.7
- Framework : TensorFlow
- Framework version : 2.3.1
- ML Runtime version : 2.3
```
> Select Model url > [ your bucket ] > saved_complete_model > select

[Refer image to select a correct folder]

![saved_complete_model](https://user-images.githubusercontent.com/59435839/135580683-1fb01c31-6027-4529-8180-63640aab9f65.png)

### Leave other fields to default and `click > save`
--
### Now again repeat, go to AI platform -> **Models** -> **Create model**

```yaml
Set model name as: limited_model
```
Click **Create**

> now click **limited_model** -> `create a version`
```yaml
- Version Name : v1
- Python version : 3.7
- Framework : TensorFlow
- Framework version : 2.3.1
- ML Runtime version : 2.3
```
> Select Model url > your bucket > saved_limited_model > Select

[Refer image to select a correct folder]

![saved_limited_model](https://user-images.githubusercontent.com/59435839/135581647-cbad0eac-2daa-4112-af39-f0fd40cb4faa.png)

### Leave other fields to default and `click > save`
--
> *Wait till the model to build*