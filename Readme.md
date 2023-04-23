
## Set Up and Configure a Cloud Environment in Google Cloud: Challenge Lab # GSP321 ##

 
###  TASK 1: Create development VPC manually 

```
gcloud compute networks create griffin-dev-vpc --subnet-mode custom
```

```
gcloud compute networks subnets create griffin-dev-wp --network=griffin-dev-vpc --region us-east1 --range=192.168.16.0/20
```

```
gcloud compute networks subnets create griffin-dev-mgmt --network=griffin-dev-vpc --region us-east1 --range=192.168.32.0/20
```

###  TASK 2: Create production VPC manually 

```
gsutil cp -r gs://cloud-training/gsp321/dm .

```

```
cd dm
```

```
sed -i s/SET_REGION/us-east1/g prod-network.yaml
```
```
gcloud deployment-manager deployments create prod-network \
 --config=prod-network.yaml
```

### TASK 3: Create bastion host

```
gcloud compute instances create bastion --network-interface=network=griffin-dev-vpc,subnet=griffin-dev-mgmt --network-interface=network=griffin-prod-vpc,subnet=griffin-prod-mgmt --tags=ssh --zone=us-east1-b
```

```
gcloud compute firewall-rules create fw-ssh-dev --source-ranges=0.0.0.0/0 --target-tags ssh --allow=tcp:22 --network=griffin-dev-vpc
```

```
gcloud compute firewall-rules create fw-ssh-prod --source-ranges=0.0.0.0/0 --target-tags ssh --allow=tcp:22 --network=griffin-prod-vpc
```
