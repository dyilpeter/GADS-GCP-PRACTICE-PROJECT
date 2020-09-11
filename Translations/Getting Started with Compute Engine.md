# Google Cloud Fundamentals: Getting Started with Compute Engine

## Overview
In this lab, you will create virtual machines (VMs) and connect to them. You will also create connections between the instances.

## Objectives

Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.

Create a Compute Engine virtual machine using the gcloud command-line interface.

Connect between the two instances.

### Task 1: Create a virtual machine using the GCP Console

gcloud beta compute --project=qwiklabs-gcp-00-eaec4476875e instances create my-vm-1 --zone=us-central1-a --machine-type=n1-standard-1 --subnet=default --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=277488022162-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --tags=http-server --image=debian-10-buster-v20200910 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=my-vm-1 --no-shielded-secure-boot --no-shielded-vtpm --no-shielded-integrity-monitoring --reservation-affinity=any

gcloud compute --project=qwiklabs-gcp-00-eaec4476875e firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server



### Task 2: Create a virtual machine using the gcloud command line

In GCP console, on the top right toolbar, click the Open Cloud Shell button

Click Continue.

To see list of zones assigned to  you Enter the following into the cloud shell:

gcloud compute zones list | grep us-central1
Result:
us-central1-c              us-central1              UP
us-central1-a              us-central1              UP
us-central1-f              us-central1              UP
us-central1-b              us-central1              UP


To choose a zone Enter the following into the cloud shell:
gcloud config set compute/zone us-central1-b

To create a VM instance called my-vm-2 in that zone, execute this command:
gcloud compute instances create "my-vm-2" --machine-type "n1-standard-1" --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default"

To close the Cloud Shell, execute the following command:
exit


Task 3: Connect between VM instances