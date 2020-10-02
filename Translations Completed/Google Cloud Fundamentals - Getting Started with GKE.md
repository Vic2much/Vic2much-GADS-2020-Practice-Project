# Google Cloud Fundamentals: Getting Started with GKE

---

## Overview
In this lab, you create a Google Kubernetes Engine cluster containing several containers, each containing a web server. You place a load balancer in front of the cluster and view its contents.

## Objectives
In this lab, you learn how to perform the following tasks:

- Provision a [Kubernetes](http://kubernetes.io/) cluster using [Kubernetes Engine](https://cloud.google.com/container-engine).

- Deploy and manage Docker containers using `kubectl`.

## STEPS

### Task 1: Sign in to the Google Cloud Platform (GCP) Console
1. Sign in to the Google Cloud Platform (GCP) Console 

[Sign in to the Google Console Here](https://console.cloud.google.com/) with the details provided for you

### Task 2: Confirm that needed APIs are enabled
1. In GCP console, on the top right toolbar, click the Open Cloud Shell button.

![Select the Cloud Shell Icon](https://cdn.qwiklabs.com/vdY5e%2Fan9ZGXw5a%2FZMb1agpXhRGozsOadHURcR8thAQ%3D)

2. Click Continue

![Continue](https://cdn.qwiklabs.com/lr3PBRjWIrJ%2BMQnE8kCkOnRQQVgJnWSg4UWk16f0s%2FA%3D)

It takes a few moments to provision and connect to the environment. When you are connected, you are already authenticated, and the project is set to your *PROJECT_ID*. For example:

![Example](https://cdn.qwiklabs.com/hmMK0W41Txk%2B20bQyuDP9g60vCdBajIS%2B52iI2f4bYk%3D)

`gcloud` is the command-line tool for Google Cloud. It comes pre-installed on Cloud Shell and supports tab-completion.

If the *PROJECT_ID* has not been assigned, use the following commands to assign it. Remember to replace *[PROJECT_ID]* with the real **project ID**

```
gcloud config set project [PROJECT_ID]
```

You can list the active account name with this command:

```
gcloud auth list
```

(Output)

>*Credentialed accounts:*
>*- <myaccount>@<mydomain>.com (active)*
 
 (Example output)

>*Credentialed accounts:*
 >*- google1623327_student@qwiklabs.net*
 
You can list the project ID with this command:

gcloud config list project

(Output)

>*[core]*
>*project = <project_ID>*

(Example output)

>*[core]*
>*project = qwiklabs-gcp-44776a13dea667a6*

3. To confirm that needed APIs are enabled, type the following commands

```
gcloud services list --enabled
```

Check maybe you can see 

- Kubernetes Engine API

- Container Registry API

in the list of enabled APIs. If you can't find it 

Use the following commands to enable Kubernetes Engine API

```
gcloud services enable container.googleapis.com
```
and 

Use the following commands to enable Container Registry API

```
gcloud services enable containerregistry.googleapis.com
```

### Task 3: Start a Kubernetes Engine cluster
1. For convenience, place the zone that Qwiklabs assigned you to into an environment variable called MY_ZONE. At the Cloud Shell prompt, type this partial command:

```
export MY_ZONE=
```

followed by the zone that Qwiklabs assigned to you. Your complete command will look similar to this:

```
export MY_ZONE=us-central1-a
```

2. Start a Kubernetes cluster managed by Kubernetes Engine. Name the cluster **webfrontend** and configure it to run 2 nodes:

```
gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2
```

It takes several minutes to create a cluster as Kubernetes Engine provisions virtual machines for you.

3. After the cluster is created, check your installed version of Kubernetes using the `kubectl version` command:

```
kubectl version
```

The `gcloud container clusters` create command automatically authenticated `kubectl` for you.

4. View your running nodes in the GCP cloud shell

To list all the node pools of a cluster, run the `gcloud node-pools` list command:

```
gcloud container node-pools list --cluster cluster-name
```

To view details about a specific node pool, run the `gcloud node-pools describe` command:

```
gcloud container node-pools describe pool-name \
    --cluster cluster-name
```
**NOTE**: Change *pool-name* to the real pool's name and change *cluster-name* to the real cluster's name 


### Task 4: Run and deploy a container
1. From your Cloud Shell prompt, launch a single instance of the nginx container. (Nginx is a popular web server.)

```
kubectl create deploy nginx --image=nginx:1.17.10
```

In Kubernetes, all containers run in pods. This use of the `kubectl create` command caused Kubernetes to create a deployment consisting of a single pod containing the nginx container. A Kubernetes deployment keeps a given number of pods up and running even in the event of failures among the nodes on which they run. In this command, you launched the default number of pods, which is 1.

>Note: If you see any deprecation warning about future version you can simply ignore it for now and can proceed further.

2. View the pod running the nginx container:

```
kubectl get pods
```

3. Expose the nginx container to the Internet:

```
kubectl expose deployment nginx --port 80 --type LoadBalancer
```

Kubernetes created a service and an external load balancer with a public IP address attached to it. The IP address remains the same for the life of the service. Any network traffic to that public IP address is routed to pods behind the service: in this case, the nginx pod.

4. View the new service:

```
kubectl get services
```

You can use the displayed external IP address to test and contact the nginx container remotely.

It may take a few seconds before the **External-IP** field is populated for your service. This is normal. Just re-run the `kubectl get services` command every few seconds until the field is populated.

5. Open a new web browser tab and paste your cluster's external IP address into the address bar. The default home page of the Nginx browser is displayed.

6. Scale up the number of pods running on your service:

```
kubectl scale deployment nginx --replicas 3
```

Scaling up a deployment is useful when you want to increase available resources for an application that is becoming more popular.

7. Confirm that Kubernetes has updated the number of pods:

```
kubectl get pods
```

8. Confirm that your external IP address has not changed:

```
kubectl get services
```

Return to the web browser tab in which you viewed your cluster's external IP address. Refresh the page to confirm that the nginx web server is still responding.



