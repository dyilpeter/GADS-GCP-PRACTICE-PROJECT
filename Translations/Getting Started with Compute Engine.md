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


### Task 3: Connect between VM instances

To open a command prompt on the my-vm-2 instance, click SSH in its row in the VM instances list:
  gcloud beta compute ssh --zone "us-central1-b" "my-vm-2" --project "qwiklabs-gcp-00-eaec4476875e"

Use the ping command to confirm that my-vm-2 can reach my-vm-1 over the network:
  ping my-vm-1

Notice that the output of the ping command reveals that the complete hostname of my-vm-1 is my-vm-1.c.PROJECT_ID.internal, where PROJECT_ID is the name of your Google Cloud Platform project. GCP automatically supplies Domain Name Service (DNS) resolution for the internal IP addresses of VM instances.

Press Ctrl+C to abort the ping command.

Use the ssh command to open a command prompt on my-vm-1:
  ssh my-vm-1


If you are prompted about whether you want to continue connecting to a host with unknown authenticity, enter yes to confirm that you do.

At the command prompt on my-vm-1, install the Nginx web server:
  sudo apt-get install nginx-light -y

Use the nano text editor to add a custom message to the home page of the web server:
  sudo nano /var/www/html/index.nginx-debian.html

Use the arrow keys to move the cursor to the line just below the h1 header. Add text like this, and replace YOUR_NAME with your name:
  Hi from Dyil

Press Ctrl+O and then press Enter to save your edited file, and then press Ctrl+X to exit the nano text editor.

Confirm that the web server is serving your new page. At the command prompt on my-vm-1, execute this command:
  curl http://localhost/
The response will be the HTML source of the web server's home page, including your line of custom text.

To exit the command prompt on my-vm-1, execute this command:
  exit
You will return to the command prompt on my-vm-2


To confirm that my-vm-2 can reach the web server on my-vm-1, at the command prompt on my-vm-2, execute this command:
  curl http://my-vm-1/
The response will again be the HTML source of the web server's home page, including your line of custom text.
