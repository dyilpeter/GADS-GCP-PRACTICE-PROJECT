# Automating the Deployment of Infrastructure Using Terraform

## Overview
In this lab, you create a Terraform configuration with a module to automate the deployment of Google Cloud infrastructure.


## Objectives
Create a configuration for an auto mode network

Create a configuration for a firewall rule

Create a module for VM instances

Create and deploy a configuration

Verify the deployment of a configuration

### Task 1:
In the Cloud Console, click Activate Cloud Shell

To confirm that Terraform is installed, run the following command:
  terraform --version
Sample Result: Terraform v0.12.24

To create a directory for your Terraform configuration, run the following command:
  mkdir tfinfra

In Cloud Shell, click #Open Editor

In the left pane of the code editor, expand the #tfinfra folder.

click File > New File
Name the new file provider.tf, and then open it.

Copy the code into provider.tf:
  provider "google" {}

To initialize Terraform, run the following command:
  cd tfinfra
terraform init
Smaple Result: * provider.google: version = "~> 3.38"
Terraform has been successfully initialized!


### Task 2: Create mynetwork and its resources

In Cloud Shell, click #Open Editor

In the left pane of the code editor, expand the #tfinfra folder.

To create a new file, click File > New File.

Name the new file mynetwork.tf, and then open it.

Copy the following base code into mynetwork.tf:
  #Create the mynetwork network
resource [RESOURCE_TYPE] "mynetwork" {
name = [RESOURCE_NAME]
#RESOURCE properties go here
}

In mynetwork.tf, replace [RESOURCE_TYPE] with "google_compute_network" (with the quotes).

In mynetwork.tf, replace [RESOURCE_NAME] with "mynetwork" (with the quotes).

Add the following property to mynetwork.tf:
  auto_create_subnetworks = "true"

Verify that mynetwork.tf looks like this:
  #Create the mynetwork network
resource "google_compute_network" "mynetwork" {
name                    = "mynetwork"
auto_create_subnetworks = true
#RESOURCE properties go here
}

click File > Save.

Define a firewall rule to allow HTTP, SSH, RDP, and ICMP traffic on mynetwork.

Add the following base code to mynetwork.tf:
  #Add a firewall rule to allow HTTP, SSH, RDP and ICMP traffic on mynetwork
resource [RESOURCE_TYPE] "mynetwork-allow-http-ssh-rdp-icmp" {
name = [RESOURCE_NAME]
#RESOURCE properties go here
}

In mynetwork.tf, replace [RESOURCE_TYPE] with "google_compute_firewall" (with the quotes).

In mynetwork.tf, replace [RESOURCE_NAME] with "mynetwork-allow-http-ssh-rdp-icmp" (with the quotes).

Add the following property to mynetwork.tf:
  network = google_compute_network.mynetwork.self_link

Add the following properties to mynetwork.tf:
  allow {
    protocol = "tcp"
    ports    = ["22", "80", "3389"]
    }
allow {
    protocol = "icmp"
    }

The list of allow rules specifies which protocols and ports are permitted.

Verify that your additions to mynetwork.tf look like this:
  #Add a firewall rule to allow HTTP, SSH, RDP, and ICMP traffic on mynetwork
resource "google_compute_firewall" "mynetwork-allow-http-ssh-rdp-icmp" {
name = "mynetwork-allow-http-ssh-rdp-icmp"
network = google_compute_network.mynetwork.self_link
allow {
    protocol = "tcp"
    ports    = ["22", "80", "3389"]
    }
allow {
    protocol = "icmp"
    }
}

To save mynetwork.tf, click File > Save

To create a new folder inside tfinfra, select the tfinfra folder, and then click File > New Folder.

Name the new folder instance.

To create a new file inside instance, select the instance folder, and then click File > New File.

Name the new file main.tf, and then open it.

Copy the following base code into main.tf:
  resource "google_compute_instance" "vm_instance" {
  name = "${var.instance_name}"
  #RESOURCE properties go here
}

Add the following properties to main.tf:
    zone         = "${var.instance_zone}"
  machine_type = "${var.instance_type}"
These properties define the zone and machine type of the instance as input variables.

Add the following properties to main.tf:
   boot_disk {
    initialize_params {
      image = "debian-cloud/debian-9"
      }
  }
This property defines the boot disk to use the Debian 9 OS image. Because both VM instances will use the same image, you can hard-code this property in the module.

Add the following properties to main.tf:
   network_interface {
    network = "${var.instance_network}"
    access_config {
      # Allocate a one-to-one NAT IP to the instance
    }
  }
This property defines the network interface by providing the network name as an input variable and the access configuration.

Define the 4 input variables at the top of main.tf, and verify that main.tf looks like this, including brackets {}:
  variable "instance_name" {}
variable "instance_zone" {}
variable "instance_type" {
  default = "n1-standard-1"
  }
variable "instance_network" {}

resource "google_compute_instance" "vm_instance" {
  name         = "${var.instance_name}"
  zone         = "${var.instance_zone}"
  machine_type = "${var.instance_type}"
  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-9"
      }
  }
  network_interface {
    network = "${var.instance_network}"
    access_config {
      # Allocate a one-to-one NAT IP to the instance
    }
  }
}
By giving instance_type a default value, you make the variable optional. The instance_name, instance_zone, and instance_network are required, and you will define them in mynetwork.tf.

To save main.tf, click File > Save.

Add the following VM instances to mynetwork.tf:
  #Create the mynet-us-vm instance
module "mynet-us-vm" {
  source           = "./instance"
  instance_name    = "mynet-us-vm"
  instance_zone    = "us-central1-a"
  instance_network = google_compute_network.mynetwork.self_link
}

#Create the mynet-eu-vm" instance
module "mynet-eu-vm" {
  source           = "./instance"
  instance_name    = "mynet-eu-vm"
  instance_zone    = "europe-west1-d"
  instance_network = google_compute_network.mynetwork.self_link
}
These resources are leveraging the module in the instance folder and provide the name, zone, and network as inputs. Because these instances depend on a VPC network, you are using the google_compute_network.mynetwork.self_link reference to instruct Terraform to resolve these resources in a dependent order. In this case, the network is created before the instance.

To save mynetwork.tf, click File > Save.

It's time to apply the mynetwork configuration.

To rewrite the Terraform configuration files to a canonical format and style, run the following command:
  terraform fmt
Sample Result:
  mynetwork.tf

To initialize Terraform, run the following command:
  terraform init
Sample Result: 
  Initializing modules...
- mynet-eu-vm in instance
- mynet-us-vm in instance
...
* provider.google: version = "~> 3.37"

Terraform has been successfully initialized!

To create an execution plan, run the following command:
  terraform plan
Sample Result:
  ...
Plan: 4 to add, 0 to change, 0 to destroy.
...


To apply the desired changes, run the following command:
  terraform apply

To confirm the planned actions, type:
  yes
Sample Result:
  ...
Apply complete! Resources: 4 added, 0 changed, 0 destroyed.
