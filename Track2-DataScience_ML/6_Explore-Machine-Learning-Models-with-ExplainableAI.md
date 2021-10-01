## Explore Machine Learning Models with Explainable AI

*lab Link* (GSP324): https://google.qwiklabs.com/focuses/12011?parent=catalog

**Start a JupyterLab Notebook instance**
* Storage > Bucket > Create Bucket > Enter **Project ID** as name > CREATE

* AI Platform > Notebooks > NEW INSTANCE > select the version of `TensorFlow Enterprise 2.3 Without GPUs` 

* Leave all default > CREATE

* Wait for it to be created > click **Open Jupyterlab**
 
## Download the Challenge Notebook
* In your notebook, click the terminal

* Clone the repo
```yaml
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
```

* Go to the enclosing folder: `training-data-analyst/quests/dei`

* Open the notebook file > what-if-tool-challenge.ipynb.

* Download and import the dataset hmda_2017_ny_all-records_labels.

### Build and train your models

> Modify the block 10 as:

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

* Enter the correct information in block 19.
```
Replace `#TODO` with your GCP Project ID
```
```
> Go to AI platform -> Models -> `Create the Complete AI Platform model`:
> Repeat the above same steps and create another model name as `limited_model`
```
> now click **complete_model** -> `create a version`
```yaml
- Version Name : v1
- Python version : 3.7
- Framework : TensorFlow
- Framework version : 2.3.1
- ML Runtime version : 2.3
```
> Select Model url > your bucket > saved_complete_model > Select
[Refer]
![Model URL](https://user-images.githubusercontent.com/59435839/135580683-1fb01c31-6027-4529-8180-63640aab9f65.png)

> now click **limited_model** -> `create a version`
```yaml
- Model Name : complete_model
- Version Name : v1
- Python version : 3.7
- Framework : TensorFlow
- Framework version : 2.3.1
- ML Runtime version : 2.3
```
> Select Model url > your bucket > saved_limited_model > Select
[Refer]
![Model URL](![lm](https://user-images.githubusercontent.com/59435839/135581647-cbad0eac-2daa-4112-af39-f0fd40cb4faa.png))

### Use the What-If Tool to explore biases

Just run the code for What-If Tool add the WitConfigBuilder code from qwiklab instruction