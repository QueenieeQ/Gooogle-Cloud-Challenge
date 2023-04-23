
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

