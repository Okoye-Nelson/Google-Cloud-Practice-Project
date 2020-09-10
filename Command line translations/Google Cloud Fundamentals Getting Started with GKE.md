# LAB: Google Cloud Fundamentals: Google Cloud Fundamentals: Getting Started with GKE

##Objectives:
In this lab, you will learn how to perform the following tasks:

  - Provision a Kubernetes cluster using Kubernetes Engine.
  - Deploy and manage Docker containers using kubectl.

##Steps:

1.	Confirm that needed APIs are enabled:
	
		1. Make a note of the name of your GCP project. This value is shown in the top bar of the Google Cloud Platform Console. It will be of the form qwiklabs-gcp- followed by hexadecimal numbers.

		2. To see a list of available services for a project, run:
			- gcloud services list --available

		3. Scroll down in the list of enabled APIs, and confirm that Kubernetes Engine API and Container Registry API are enabled. If not they are not enabled, run this command:
			- gcloud services enable container.googleapis.com containerregistry.googleapis.com


2.	Start a Kubernetes Engine cluster:

		1. For convenience, place the zone that Qwiklabs assigned you to into an environment variable called MY_ZONE:
			- export MY_ZONE=us-central1-a

		2. Start a Kubernetes cluster managed by Kubernetes Engine. Name the cluster webfrontend and configure it to run 2 nodes:
			- gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2
			
				- Outcome: It takes several minutes to create a cluster as Kubernetes Engine provisions virtual machines for you.

		3. After the cluster is created, check your installed version of Kubernetes by executing this command:
			- kubectl version
			
				- Outcome: The "gcloud container clusters create" command automatically authenticated kubectl for you.

		4. To view your running nodes, execute this command:
			- gcloud compute intances list --zone us-central1-a


3.	Run and deploy a container:

		1. Execute this command to launch a single instance of the nginx container. (Nginx is a popular web server.)
			kubectl create deploy nginx --image=nginx:1.17.10
			
				- Note: If you see any deprecation warning about future version you can simply ignore it for now and can proceed further.

		2. View the pod running the nginx container by executing this command:
			kubectl get pods

		3. Execute this command to expose the nginx container to the Internet:
			kubectl expose deployment nginx --port 80 --type LoadBalancer
			
				- Outcome: Kubernetes created a service and an external load balancer with a public IP address attached to it.

		4. To view the newly created service, execute this command:
			kubectl get services

		5. Copy the cluster's External IP address and paste it in the URL bar of your internet browser tab:
			- Outcome: You will see your web server's home page.

		6. Go back to the gcloud command line application and execute the following command to scale up the number of pods running on your service:
			kubectl scale deployment nginx --replicas 3
			
				- Outcome: This increases the number of pods running on your service to a specified number. In this case, the number of pods running on your service is increased to 3.

		7. Execute this command to confirm that Kubernetes has updated the number of pods:
			kubectl get pods

		8. Execute this command to also confirm that your external IP address has not changed:
			kubectl get services

		9. Return to the web browser tab in which you viewed your cluster's external IP address. Refresh the page to confirm that the nginx web server is still responding.