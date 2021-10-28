## Perform Foundational Infrastructure Tasks in Google Cloud

*lab Link* (GSP3150): https://google.qwiklabs.com/focuses/10379?parent=catalog

**Task 1: Create a bucket**
```yaml
1. On Navigation menu > Cloud Storage > Create Bucket
2. Name your bucket > [ Enter your Project ID ] --> Leave all to default and click -> Create
```

**Task 2: Create a Pub/Sub topic**

```yaml
1. Navigation menu > Pub/Sub
2. Create Topic > Name: diva > Create Topic 
```
> Make sure you remember the topic name, which will be used in Cloud Function.

**Task 3: Create the Cloud Function**
```yaml
1. Navigation menu > Cloud Functions
2. Click **Create function**
```
|    Field    |     Values                             |
|  ---------  |    --------                            |
| Name        | thumbnail                              |
| Region      | us-east1                               | 
| Trigger     | Cloud Storage                          |
| Event type  | Finalize/Create                        |
| Bucket      | BROWSE > Select the bucket you created |

Now click -> **Save** and Click the `Runtime, build, connections and security settings` Dropdown
> On Autoscaling change the **Maximum number of instance to `5`** Then click Next
 ```yaml 
3. Create the thumbnail Cloud Function

    Select Runtime: Node.js 14
    Entry point: thumbnail 

4. Copy the code for [ index.js ] and [ package.json ] from QwikLab and paste it.
```
[Click here to redirect to copy the codes from (Qwiklab_Page)](https://google.qwiklabs.com/focuses/10379?parent=catalog#:~:text=const%20topicName%20%3D%20%22MyTopic%22%3B-,index.js%3A,-/*%20globals%20exports%2C%20require)

 > Make sure you replace the text **REPLACE_WITH_YOUR_TOPIC** with the topic **diva**, in line 15 of **index.js**

 ```yaml
 5. Download the below image
 ```
 ![GCP](https://user-images.githubusercontent.com/59435839/139296274-3b11d33c-8221-485c-99d0-0cd6ea3f2114.jpg)

```yaml
6. Now goto your bucket that created at Task1 > Upload the downloaded image
```

**Task 4: Remove the previous cloud engineer**

```yaml
1. Navigation menu > IAM & Admin
2. Search for the "USERNAME 2" > Click the pencil icon > Delete Role > Save
```