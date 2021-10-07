## Perform Foundational Data, ML, and AI Tasks in Google Cloud

*lab Link* (GSP323): https://google.qwiklabs.com/focuses/11044?parent=catalog

**Task 1: Run a simple Dataflow job**

* Navigation menu > Cloud Storage > Create a bucket

* Enter name as your GCP Project ID > Leave others to default > **Create**

> Run the following from the Cloud Shell:

```yaml
- bq mk lab

- gsutil cp gs://cloud-training/gsp323/lab.csv .

- gsutil cp gs://cloud-training/gsp323/lab.schema .

- cat lab.schema
```
1. Now go back to BIGQUERY,  Select **project ID** > click dropdown - lab `dataset` on three dots > Click **Open**

2. create a table inside the `lab` dataset `[Press the + icon]` and configure it as follows:

* Select `Google Cloud Storage` in Create table from dropbox

* Paste the below value in GCS bucket :
```yaml
gs://cloud-training/gsp323/lab.csv
```
* In Table name : `customers`

* In Schema turn on **Edit as text** paste the below values inside
```sql
[
        {"type":"STRING","name":"guid"},
        {"type":"BOOLEAN","name":"isActive"},
        {"type":"STRING","name":"firstname"},
        {"type":"STRING","name":"surname"},
        {"type":"STRING","name":"company"},
        {"type":"STRING","name":"email"},
        {"type":"STRING","name":"phone"},
        {"type":"STRING","name":"address"},
        {"type":"STRING","name":"about"},
        {"type":"TIMESTAMP","name":"registered"},
        {"type":"FLOAT","name":"latitude"},
        {"type":"FLOAT","name":"longitude"}
]
```
* Leave other to default and click **Create table**

[Refer]

![Schema](https://user-images.githubusercontent.com/59435839/135756331-23348ee1-ea4e-4223-8be8-020594a3b775.png)


* In Navigation menu > go to **Dataflow** > Jobs > Create Job from Template

```yaml
Job name : lab-job
In DataFlow Template : Select [ Text Files on Cloud Storage to BigQuery ] under "Process Data in Bulk (batch)"
```
> On REQUIRED PARAMETERS: 
### Make Sure you replace *YOUR_PROJECT* with your *Project ID* in the below url

|               Field                   |              Values                    |
|             ---------                 |             --------                   |
| JavaScript UDF path in Cloud Storage  | gs://cloud-training/gsp323/lab.js      |
| JSON path                             |	gs://cloud-training/gsp323/lab.schema  |
| JavaScript UDF name                   |	transform                              |
| BigQuery output table                 |	YOUR_PROJECT:lab.customers             |
| Cloud Storage input path              |	gs://cloud-training/gsp323/lab.csv     |
| Temporary BigQuery directory          |	gs://YOUR_PROJECT/bigquery_temp        |
| Temporary location                    |	gs://YOUR_PROJECT/temp                 |

[Refer]

![Required Parameters](https://user-images.githubusercontent.com/59435839/135757240-fc333c90-d3c9-4809-8389-d12aa3ce8808.png)

* **Run the Job**

> wait till the dataflow to be complete to update teh score in *Qwiklab*

**Task 2: Run a simple Dataproc job**

* In Navigation menu > go to **Dataproc** > Clusters 

* Leave all values to default and Click **Create Cluster**

* Select the Created Cluster > Go to **VM Instances** > **SSH** into cluster

* Run the following command in `SSH`: 

```yaml
hdfs dfs -cp gs://cloud-training/gsp323/data.txt /data.txt
```
* Now select **Job** from left pane > Click **Submit a job** > Configure as given:

|          Field           |                     Values                                 |
|        ---------         |                    --------                                |
| Region	                 |    us-central1                                             |
| For Cluster              |    [Select the dropdown]                                   |
| Job type	               |    Spark                                                   |
| Main class or jar	       |    org.apache.spark.examples.SparkPageRank                 |
| Jar files	               |    file:///usr/lib/spark/examples/jars/spark-examples.jar  |
| Arguments	               |    /data.txt                                               |
| Max restarts per hour    |    1                                                       |

* Click **Submit**

**Task 3: Run a simple Dataprep job**

* In Navigation menu > go to **Dataprep** > Accept the terms > `FOLLOW THE SCREEN and agree and allow` > Login with the same account

* On left pane > Cloud Storage > click the pencil > Enter the path as this: `cloud-training/gsp323/runs.csv` > continue

* Modify the table as specified in the [lab_instructions](https://google.qwiklabs.com/focuses/11044?parent=catalog#:~:text=Perform%20the%20following%20transforms%20to%20ensure%20the%20data%20is%20in%20the%20right%20state)

* and click **Run** and click **Run** on next page also

*Wait until the Dataflow job completes before you can grade this task in qwiklab*

**Task 4: AI**

> `open cloud shell run the following commands`

```yaml
- gcloud iam service-accounts create my-natlang-sa \
  --display-name "my natural language service account"

- gcloud iam service-accounts keys create ~/key.json \
  --iam-account my-natlang-sa@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com

- export GOOGLE_APPLICATION_CREDENTIALS="/home/$USER/key.json"

- gcloud auth activate-service-account my-natlang-sa@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com --key-file=$GOOGLE_APPLICATION_CREDENTIALS

- gcloud ml language analyze-entities --content="Old Norse texts portray Odin as one-eyed and long-bearded, frequently wielding a spear named Gungnir and wearing a cloak and a broad hat." > result.json

- gcloud auth login 
(Copy the token from the link provided)

- gsutil cp result.json gs://YOUR_PROJECT-marking/task4-cnl.result
      [ Replace with your project ID]
```