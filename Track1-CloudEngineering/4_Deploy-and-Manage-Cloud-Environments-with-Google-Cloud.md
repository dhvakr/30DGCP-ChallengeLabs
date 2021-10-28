## Deploy and Manage Cloud Environments with Google Cloud

*lab Link* (GSP314): https://google.qwiklabs.com/focuses/10417?parent=catalog

**Task 1: Create the production environment**
> Go to Compute Engine > **VM Instances** >  **kraken-jumphost** > Click [ SSH ]
> Run the following commands in [ SSH ]:
```yaml
- cd /work/dm

- sed -i s/SET_REGION/us-east1/g prod-network.yaml

- gcloud deployment-manager deployments create prod-network --config=prod-network.yaml

- gcloud config set compute/zone us-east1-b

- gcloud container clusters create kraken-prod \
          --num-nodes 2 \
          --network kraken-prod-vpc \
          --subnetwork kraken-prod-subnet\
          --zone us-east1-b

- gcloud container clusters get-credentials kraken-prod

- cd /work/k8s

- for F in $(ls *.yaml); do kubectl create -f $F; done
```

**Task 2 : Setup the Admin instance**

>Stay in **kraken-jumphost's SSH**, run below comands

```yaml
- gcloud config set compute/zone us-east1-b

- gcloud compute instances create kraken-admin --network-interface="subnet=kraken-mgmt-subnet" --network-interface="subnet=kraken-prod-subnet"
```
---

*Create alert* 

> Go to **Monitoring** > **Alerting** > **Create Alerting Policy**
```yaml
   Resource Type : VM Instance
   Metric : CPU utilization
   Filter : instance_name
       Value : kraken-admin
   Condition : is above
   Threshold : 50%
   For : 1 minute
```
* Click [ **Add** ]

* **Manage notification Channel** > Create a new **Email Channel** 

* Click **Done**

* give alert name as `kraken-admin`

**Task 3: Verify the Spinnaker deployment**
> Go to cloudshell, run the below commands
```yaml
- gcloud config set compute/zone us-east1-b

- gcloud container clusters get-credentials spinnaker-tutorial

- DECK_POD=$(kubectl get pods --namespace default -l "cluster=spin-deck" -o jsonpath="{.items[0].metadata.name}")

- kubectl port-forward --namespace default $DECK_POD 8080:9000 >> /dev/null &
```
---
* Go to cloudshell - **Web Preview** > **Preview on port 8080**

* Go to **applications** > **sample**

* Open **Pipelines** and manually run the pipeline if it has not already running.

* Approve the deployment to production. [ Check the production frontend endpoint (use http, not the default https) ]

* Now go to cloudshell, run to push a change
---
```yaml
- gcloud config set compute/zone us-east1-b

- gcloud source repos clone sample-app

- cd sample-app

- touch diva

- git config --global user.email "sample@gmail.com"

- git config --global user.name "student"

- git commit -a -m "change"

- git tag v1.0.1

- git push --tags
```
> Wait for some time to spinnaker deployment also retry by commiting a change if pipeline fails