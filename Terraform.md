## Automating Infrastructure on Google Cloud with Terraform: Challenge Lab # GSP345 ##

<!-- **Setup : Create the configuration files** <br/> -->
<br/> **TASK 1: Create the configuration files** <br/>
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

<br/> **TASK 2: Import infrastructure** <br/>
Navigate to _Compute Engine > VM Instances_. Click on _tf-instance-1_. Copy the _Instance ID_ down somewhere to use later. <br/>
Navigate to _Compute Engine > VM Instances_. Click on _tf-instance-2_. Copy the _Instance ID_ down somewhere to use later. <br/>
Next, navigate to _modules/instances/instances.tf_. Copy the following configuration into the file:
```
resource "google_compute_instance" "tf-instance-1" {
  name         = "tf-instance-1"
  machine_type = "n1-standard-1"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-10"
    }
  }

  network_interface {
 network = "default"
  }

metadata_startup_script = <<-EOT
#!/bin/bash
EOT

allow_stopping_for_update = true
}

resource "google_compute_instance" "tf-instance-2" {
  name         = "tf-instance-2"
  machine_type = "n1-standard-1"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-10"
    }
  }

  network_interface {
 network = "default"
  }

metadata_startup_script = <<-EOT
#!/bin/bash
EOT

allow_stopping_for_update = true
}
```
To import the first instance, use the following command, using the Instance ID for _tf-instance-1_ you copied down earlier.

```
terraform import module.instances.google_compute_instance.tf-instance-1 <FILL IN INSTANCE 1 ID>
```
To import the second instance, use the following command, using the Instance ID for _tf-instance-2_ you copied down earlier.

```
terraform import module.instances.google_compute_instance.tf-instance-2 <FILL IN INSTANCE 2 ID>
```
The two instances have now been imported into your terraform configuration. You can now run the commands to update the state of Terraform. Type _yes_ at the dialogue after you run the apply command to accept the state changes.
```
terraform plan
terraform apply
```
