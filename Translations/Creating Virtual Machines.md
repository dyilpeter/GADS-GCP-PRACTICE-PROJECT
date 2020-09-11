# Creating Virtual Machines

## Overview
In this lab, you will explore the Virtual Machine instance options and create several VMs with different characteristics.


## Objectives
Create several standard VMs

Create advanced VMs

### Task 1: Create a utility virtual machine using the commandline below:

    gcloud beta compute --project=qwiklabs-gcp-03-6b9da6e238a9 instances create instance-1 --zone=us-central1-c --machine-type=n1-standard-4 --subnet=default --no-address --maintenance-policy=MIGRATE --service-account=715645559399-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --image=debian-10-buster-v20200910 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=instance-1 --no-shielded-secure-boot --no-shielded-vtpm --no-shielded-integrity-monitoring --reservation-affinity=any



### Task 2: Create a Windows virtual machine using the commandline below:

    gcloud beta compute --project=qwiklabs-gcp-03-6b9da6e238a9 instances create instance-2 --zone=europe-west2-a --machine-type=n1-standard-2 --subnet=default --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=715645559399-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --tags=http-server,https-server --image=windows-server-2016-dc-core-v20200813 --image-project=windows-cloud --boot-disk-size=100GB --boot-disk-type=pd-ssd --boot-disk-device-name=instance-2 --no-shielded-secure-boot --shielded-vtpm --shielded-integrity-monitoring --reservation-affinity=any

    gcloud compute --project=qwiklabs-gcp-03-6b9da6e238a9 firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server

    gcloud compute --project=qwiklabs-gcp-03-6b9da6e238a9 firewall-rules create default-allow-https --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:443 --source-ranges=0.0.0.0/0 --target-tags=https-server

###Set the password for the VM using the commandline:

    gcloud beta compute --project "qwiklabs-gcp-03-6b9da6e238a9" reset-windows-password "instance-2" --zone "europe-west2-a"




### Task 3: Create a custom virtual machine

    gcloud beta compute --project=qwiklabs-gcp-03-6b9da6e238a9 instances create instance-3 --zone=us-west1-b --machine-type=custom-6-32768 --subnet=default --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=715645559399-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --image=debian-10-buster-v20200910 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=instance-3 --no-shielded-secure-boot --no-shielded-vtpm --no-shielded-integrity-monitoring --reservation-affinity=any



### Connect via SSH to your custom VM using:

    gcloud beta compute ssh --zone "us-west1-b" "instance-3" --project "qwiklabs-gcp-03-6b9da6e238a9"


### To see information about unused and used memory and swap space on your custom VM, run the following command: 

    free

### To see details about the RAM installed on your VM, run the following command: 

    sudo dmidecode -t 17

### To verify the number of processors, run the following command: 

    nproc

### To see details about the CPUs installed on your VM, run the following command: 

    lscpu

### To exit the SSH terminal, run the following command: 

    exit
