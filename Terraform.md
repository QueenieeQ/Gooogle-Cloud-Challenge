## Automating Infrastructure on Google Cloud with Terraform: Challenge Lab # GSP345 ##

**Setup : Create the configuration files** <br/>
Make the empty files and directories in _Cloud Shell_ or the _Cloud Shell Editor_.


```
touch main.tf
touch variables.tf
mkdir modules
cd modules
mkdir instances
cd instances
touch instances.tf
touch outputs.tf
touch variables.tf
cd ..
mkdir storage
cd storage
touch storage.tf
touch outputs.tf
touch variables.tf
cd
```
 Configuration files and a directory structure that resembles the following

```
main.tf
variables.tf
modules/
└── instances
    ├── instances.tf
    ├── outputs.tf
    └── variables.tf
└── storage
    ├── storage.tf
    ├── outputs.tf
    └── variables.tf
```
Add the following to the each _variables.tf_ file, and fill in the _GCP Project ID_:
```
variable "region" {
 default = "us-east1"
}

variable "zone" {
 default = "us-east1-a"
}

variable "project_id" {
 default = "<FILL IN PROJECT ID>"
}
```
Add the following to the _main.tf_ file:
```
terraform {
  required_providers {
    google = {
      source = "hashicorp/google"
      version = "4.53.0"
    }
  }
}

provider "google" {
  project     = var.project_id
  region      = var.region
  zone        = var.zone
}


module "instances" {
  source     = "./modules/instances"
}
```
Run "_terraform init_" in Cloud Shell in the root directory to initialize terraform.
```
terraform init
```
