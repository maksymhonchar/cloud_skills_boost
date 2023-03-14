# Getting Started with Cloud Shell and gcloud

## Task 1: Configuring your environment
- Set the region:
    - gcloud config set compute/region us-east1
- View the project region setting:
    - gcloud confgi get-value compute/region
- Set the zone:
    - gcloud config set compute/zone us-east1-b
- View the project zone setting:
    - gcloud config get-value compute/zone 
---
- View project ID:
    - gcloud config get-value project
    - OR
    - (from cloud shell) gcloud compute project-info describe --project $(gcloud config get-value project)
---
- Set env variables:
    - export PROJECT_ID=$(gcloud config get-value project)
    - export ZONE=$(gcloud config get-value compute/zone)
- Self-check env variables:
    - echo -e "PROJECT ID: $PROJECT_ID\nZONE: $ZONE"
---
- Create a VM:
    - gcloud compute instances create gcelab2 --machine-type e2-medium --zone $ZONE
---
- View the list of configurations in your environment:
    - gcloud config list
    - OR
    - gcloud config list --all
- View the list of components:
    - gcloud components list

## Task 2: Filtering command-line output
- List the compute instance available in the project:
    - gcloud compute instances list
- List the gcelab2 (specific project from gcloud compute instances list) virtual machine:
    - gcloud compute instances list --filter="name=('gcelab2')"
- List the firewall rules in the project:
    - gcloud compute firewall-rules list
- List the firewall rules for the default network:
    - gcloud compute firewall-rules list --filter="network='default'"
- List the firewall rules for the default network where the allow rule matches an ICMP rule:
    - gcloud compute firewall-rules list --filter="NETWORK:'default' AND ALLOW:'icmp'"

## Task 3: Connecting to your VM instance
- Connect to your VM with SSH:
    - gcloud compute ssh gcelab2 --zone $ZONE
- To continue, type Y
- To leave the passphrase empty, press Enter twice
- Install nginx web server on to virtual machine:
    - sudo apt install -y nginx
- You don't need to do anything here; so to disconnect from SSH and exit the remote shell, run the following command:
    - exit

## Task 4: Updating the firewall
- List the firewall rules for the project:
    - gcloud compute firewall-rules list
- Try to access the nginx service running on the gcelab2 virtual machine.
    - Note: Communication with the virtual machine will fail as it does not have an appropriate firewall rule. Our nginx web server is expecting to communicate on tcp:80. To get communication working we need to:
        1. Add a tag to the gcelab2 virtual machine
        2. Add a firewall rule for http traffic
- Add a tag to the virtual machine:
    - gcloud compute instances add-tags gcelab2 --tags http-server,https-server
- Update the firewall rule to allow:
    - gcloud compute firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server
- List the firewall rules for the project:
    - gcloud compute firewall-rules list --filter=ALLOW:'80'
- Verify communication is possible for http to the virtual machine:
    - curl http://$(gcloud compute instances list --filter=name:gcelab2 --format='value(EXTERNAL_IP)')

## Task 5: Viewing the system logs
- View the available logs on the system:
    - gcloud logging logs list
- View the logs that relate to compute resources:
    - gcloud logging logs list --filter="compute"
- Read the logs related to the resource type of gce_instance:
    - gcloud logging read "resource.type=gce_instance" --limit 5
- Read the logs for a specific virtual machine:
    - gcloud logging read "resource.type=gce_instance AND labels.instance_name='gcelab2'" --limit 5

# Notes

## General notes
- Cloud Shell provides you with command-line access to computing resources hosted on Google Cloud. 
- The gcloud command-line tool and other utilities you need are pre-installed in Cloud Shell, which allows you to get up and running quickly.
