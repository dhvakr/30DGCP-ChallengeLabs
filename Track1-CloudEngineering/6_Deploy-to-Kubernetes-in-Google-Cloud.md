## Deploy to Kubernetes in Google Cloud

*lab Link* (GSP318): https://google.qwiklabs.com/focuses/10457?parent=catalog

**Task 1 : Create a Docker image and store the Dockerfile**
```yaml
- gcloud auth list

- gsutil cat gs://cloud-training/gsp318/marking/setup_marking.sh | bash

- gcloud source repos clone valkyrie-app

- cd valkyrie-app

- cat > Dockerfile <<EOF
FROM golang:1.10
WORKDIR /go/src/app
COPY source .
RUN go install -v
ENTRYPOINT ["app","-single=true","-port=8080"]
EOF

- docker build -t valkyrie-app:v0.0.1 .

- cd ..
  cd marking

- ./step1.sh
```

**Task 2: Test the created Docker image**
```yaml 
- cd ..
  cd valkyrie-app

- docker run -p 8080:8080 valkyrie-app:v0.0.1 &

- cd ..
  cd marking

- ./step2.sh
```

**Task 3: Push the Docker image in the Container Repository**
```yaml
- cd ..
  cd valkyrie-app

- docker tag valkyrie-app:v0.0.1 gcr.io/$GOOGLE_CLOUD_PROJECT/valkyrie-app:v0.0.1

- docker push gcr.io/$GOOGLE_CLOUD_PROJECT/valkyrie-app:v0.0.1

- sed -i s#IMAGE_HERE#gcr.io/$GOOGLE_CLOUD_PROJECT/valkyrie-app:v0.0.1#g k8s/deployment.yaml
```

**Task 4: Create and expose a deployment in Kubernetes**
```yaml
- sed -i s#IMAGE_HERE#gcr.io/$GOOGLE_CLOUD_PROJECT/- valkyrie-app:v0.0.1#g k8s/deployment.yaml

- gcloud container clusters get-credentials valkyrie-dev --zone us-east1-d

- kubectl create -f k8s/deployment.yaml

- kubectl create -f k8s/service.yaml

- git merge origin/kurt-dev 
```
> Enter vim editor to increase replicas from 1 to 3
```yaml
kubectl edit deployment valkyrie-dev
```
> *Press "i" to get into insert mode and change "replicas" from 1 to 3 in start and end. Press "Esc" --> [type] ":wq" to exit Vim editor*

**Task 5: Update the deployment with a new version of valkyrie-app**
```yaml
- docker build -t gcr.io/$GOOGLE_CLOUD_PROJECT/valkyrie-app:v0.0.2 .

- docker push gcr.io/$GOOGLE_CLOUD_PROJECT/valkyrie-app:v0.0.2

- kubectl edit deployment valkyrie-dev
```
> *Press 'i' to edit and change image to "image: gcr.io/YOUR_PROJECT_ID/valkyrie-app:v0.0.2" in both. Press "Esc" --> [type] ":wq" to exit Vim*
```yaml
docker ps
```

**Task 6: Create a pipeline in Jenkins to deploy your app**
```yaml
- docker kill [ container_id ]

- export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/component=jenkins-master" -l "app.kubernetes.io/instance=cd" -o jsonpath="{.items[0].metadata.name}")

- kubectl port-forward $POD_NAME 8080:8080 >> /dev/null &   

- printf $(kubectl get secret cd-jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo

- gcloud source repos list
```
> Now open Jenkins web view -> Click **`Preview on port 8080`** in cloud shell
```yaml
   Username : admin
   Password : {Enter password from which you got from above {printf} command}
```
> Follow the steps to start jenkins

### Follow the steps below:

* On Left Pane > Manage Jenkins > Manage Credentials > Global > add credentials > Kind: Google Service Account from metadata > OK

* Back to DashBoard > On left pane > New Item -> Name : valkyrie-app > select " Pipeline " > OK

* Pipeline > Definition: Pipeline script from SCM > SCM: Git

* Repository URL: {Url from previous command} > Credentials: {Project id}

* Apply > Save

## In cloud shell , run below code

```yaml
- sed -i "s/green/orange/g" source/html.go

- sed -i "s/YOUR_PROJECT/$GOOGLE_CLOUD_PROJECT/g" Jenkinsfile

- git config --global user.email "$(gcloud config get-value account)"                

- git config --global user.name "student..."                       

- git add .

- git commit -m "built pipeline init"

- git push
```

*In Jenkins, click Build Pipeline and wait untill the build to complete to update the score in qwiklab*