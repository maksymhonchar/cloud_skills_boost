# Create a VM

## Task 1: Create a new instance from the Cloud Console
- In the Cloud Console, on the Navigation menu (Navigation menu icon), click Compute Engine > VM Instances.
- To create a new instance, click CREATE INSTANCE.
- There are many parameters you can configure when creating a new instance. Use the following for this lab:
    - Name	gcelab
    - Region	<filled in at lab start>	
    - Zone	<filled in at lab start> *
    - Series	E2	
    - Machine Type	2 vCPU	
    - Boot Disk	New 10 GB balanced persistent disk OS Image: Debian GNU/Linux 11 (bullseye)	
    - Firewall	Allow HTTP traffic	
- Click Create.
- To use SSH to connect to the virtual machine, in the row for your machine, click SSH.

## Task 2: Install an NGINX web server
- Update the OS:
    - sudo apt-get update
- Install NGINX:
    - sudo apt-get install -y nginx
- Confirm that NGINX is running:
    - ps auwx | grep nginx
- To see the web page, return to the Cloud Console and click the External IP link in the row for your machine, or add the External IP value to http://EXTERNAL_IP/ in a new browser window or tab.

## Task 3. Create a new instance with gcloud
- In the Cloud Shell, use gcloud to create a new virtual machine instance from the command line:
    - gcloud compute instances create gcelab2 --machine-type e2-medium --zone 
- To see all the defaults, run:
    - gcloud compute instances create --help
- To exit help, press CTRL + C.
 -In the Cloud Console, on the Navigation menu, click Compute Engine > VM instances. Our 2 new instances should be listed.
- You can also use SSH to connect to your instance via gcloud. Make sure to add your zone, or omit the --zone flag if you've set the option globally:
    - gcloud compute ssh gcelab2 --zone  
- Type Y to continue.
- Press ENTER through the passphrase section to leave the passphrase empty.
- After connecting, disconnect from SSH by exiting from the remote shell:
    - exit

# Notes

## General notes
- Compute Engine lets you create virtual machines that run different operating systems, including multiple flavors of Linux (Debian, Ubuntu, Suse, Red Hat, CoreOS) and Windows Server, on Google infrastructure. You can run thousands of virtual CPUs on a system that is designed to be fast and to offer strong consistency of performance.
- GCloud CLI guide: https://cloud.google.com/sdk/gcloud
- Regions and zones docs: https://cloud.google.com/compute/docs/regions-zones/
- Cloud Shell is a Debian-based virtual machine loaded with all the development tools you'll need (gcloud, git, and others) and offers a persistent 5-GB home directory.

## Activate Cloud Shell
- "Activate Cloud Shell" icon is at the top of the Google Cloud console.
- First output from Cloud Shell: "Your Cloud Platform project in this session is set to YOUR_PROJECT_ID"
- List project ID:
    - gcloud config list project
- Get active account name:
    - gcloud auth list
- Set active account:
    - gcloud config set account `ACCOUNT`

## Understanding Regions and Zones
- Certain Compute Engine resources live in regions or zones.
- A region is a specific geographical location where you can run your resources. Each region has one or more zones.
- Examples:
    - Region: us-central1
    - Zone: us-central1-a, us-central1-b, ...
- Resources that live in a zone are referred to as zonal resources. Virtual machine Instances and persistent disks live in a zone.
