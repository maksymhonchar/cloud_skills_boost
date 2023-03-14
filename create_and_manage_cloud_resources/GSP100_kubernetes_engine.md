# Kubernetes Engine: Qwik Start

## Task 1: Set a default compute zone
- Set the default compute region:
    - gcloud config set compute/region us-central1
- Set the default compute zone:
    - gcloud config set compute/zone us-central1-a

## Task 2: Create a GKE cluster
- Create a cluster
    - gcloud container clusters create --machine-type=e2-medium --zone=us-central1-a lab-cluster

## Task 3: Get authentication credentials for the cluster    
- Authenticate with the cluster
    - gcloud container clusters get-credentials lab-cluster

## Task 4: Deploy an application to the cluster
- Create a new Deployment object "hello-server" from the "hello-app" container image:
    - kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0
- Create a Kubernetes service - a Kubernetes resource that lets you expose your application to external traffic:
    - kubectl expose deployment hello-server --type=LoadBalancer --port 8080
- Inspect the hello-server:
    - kubectl get service
- View the application from your web browser, open a new tab and enter the following address, replacing [EXTERNAL IP] with the EXTERNAL-IP for hello-server:
    - http://[EXTERNAL-IP]:8080

## Task 5: Deleting the cluster
- Delete the cluster:
    - gcloud container clusters delete lab-cluster

# Notes

## General notes
- GKE:
    - Google Kubernetes Engine (GKE) provides a managed environment for deploying, managing, and scaling your containerized applications using Google infrastructure.
    - Google Kubernetes Engine (GKE) clusters are powered by the Kubernetes open source cluster management system.
- Kubernetes:
    - The Kubernetes Engine environment consists of multiple machines (specifically Compute Engine instances) grouped to form a container cluster.
    - Kubernetes provides the mechanisms through which you interact with your container cluster.
    - You use Kubernetes commands and resources to deploy and manage your applications, perform administrative tasks, set policies, and monitor the health of your deployed workloads.
    - Kubernetes draws on the same design principles that run popular Google services and provides the same benefits: automatic management, monitoring and liveness probes for application containers, automatic scaling, rolling updates, and more.
- Benefits of advanced cluster management features that Google Cloud provides:
    - Load balancing for Compute Engine instances
    - Node pools to designate subsets of nodes within a cluster for additional flexibility
    - Automatic scaling of your cluster's node instance count
    - Automatic upgrades for your cluster's node software
    - Node auto-repair to maintain node health and availability
    - Logging and Monitoring with Cloud Monitoring for visibility into your cluster
- Cluster:
    - A cluster consists of at least one cluster master machine and multiple worker machines called nodes.
    - Nodes are Compute Engine virtual machine (VM) instances that run the Kubernetes processes necessary to make them part of the cluster.
    - Cluster names must start with a letter and end with an alphanumeric, and cannot be longer than 40 characters.
    - After creating your cluster, you need authentication credentials to interact with it: use gcloud container clusters get-credentials cluster-name
