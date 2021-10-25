## Build and Secure Networks in Google Cloud

*lab Link* (GSP322): https://google.qwiklabs.com/focuses/12068?parent=catalog

**Task 1 : Remove the overly permissive rules**
```yaml
gcloud compute firewall-rules delete open-access
```

**Task 2 : Start the bastion host instance**
> ### Go to Compute Engine and **Start Bastion instance**

**Task 3 : Create a firewall rule that allows SSH (tcp/22) from the IAP service and add network tag on bastion**
```yaml
- gcloud compute firewall-rules create ssh-ingress --allow=tcp:22 --source-ranges 35.235.240.0/20 --target-tags allow-ssh-iap-ingress-ql-938 --network acme-vpc

- gcloud compute instances add-tags bastion --tags=allow-ssh-iap-ingress-ql-938 --zone=us-central1-b
```

**Task 4 : Create a firewall rule that allows traffic on HTTP (tcp/80) to any address and add network tag on juice-shop**
```yaml
- gcloud compute firewall-rules create http-ingress --allow=tcp:80 --source-ranges 0.0.0.0/0 --target-tags allow-http-ingress-ql-789 --network acme-vpc

- gcloud compute instances add-tags juice-shop --tags=allow-http-ingress-ql-789 --zone=us-central1-b
```

**Task 5 : Create a firewall rule that allows traffic on SSH (tcp/22) from acme-mgmt-subnet network address and add network tag on juice-shop**
```yaml
- gcloud compute firewall-rules create internal-ssh-ingress --allow=tcp:22 --source-ranges 192.168.10.0/24 --target-tags allow-ssh-internal-ingress-ql-703 --network acme-vpc

- gcloud compute instances add-tags juice-shop --tags=allow-ssh-internal-ingress-ql-703 --zone=us-central1-b
```

**Task 6 : SSH to bastion host via IAP and juice-shop via bastion**
> In *Compute Engine* > VM Instances page, click the *SSH* button for the bastion host.
```yaml
ssh [Internal IP address of juice-shop]
```