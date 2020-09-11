---
# Google Cloud Fundamentals: Getting Started with Compute Engine

---

## Overview
In this lab, you will create virtual machines (VMs) and connect to them. You will also create connections between the instances.


## Objectives
In this lab, you will learn how to perform the following tasks:

- Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.

- Create a Compute Engine virtual machine using the gcloud command-line interface.

- Connect between the two instances.

## STEPS

### Task 1: Sign in to the Google Cloud Platform (GCP) Console

1. Sign in to the Google Cloud Platform (GCP) Console 

[Sign in to the Google Console Here](https://console.cloud.google.com/) with the details provided for you

### Task 2: Create a virtual machine using the Cloud Shell
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

#### Understanding Regions and Zones

Certain Compute Engine resources live in regions or zones. A region is a specific geographical location where you can run your resources. Each region has one or more zones. For example, the us-central1 region denotes a region in the Central United States that has zones *us-central1-a, us-central1-b, us-central1-c, and us-central1-f*.

![regions_and_zones](https://cdn.qwiklabs.com/BErmNT8ZIzd5yqxO0lEJj8lAlKT3jKC%2BtI%2Byj3OSKDA%3D)

Resources that live in a zone are referred to as zonal resources. Virtual machine Instances and persistent disks live in a zone. To attach a persistent disk to a virtual machine instance, both resources must be in the same zone. Similarly, if you want to assign a static IP address to an instance, the instance must be in the same region as the static IP.

>Learn more about regions and zones and see a complete list in [Regions & Zones documentation](https://cloud.google.com/compute/docs/regions-zones/).

3. The following are the details of the virtual machine we are creating  

**(Configurations)**
- **Name**: *my-vm-1*

- **Region and Zone**: Use region and zone assigned by Qwiklabs.

- **Machine type**: Use Default

- **Boot disk image**: *Debian GNU/Linux 9 (stretch)*

- **Identity and API access**: Default

- **Firewall**: Allow HTTP traffic

- **Others**: Default

 Now go Cloud Shell and use this following commands to effect the above configurations

- Create a virtual machine in the region **us-central1** and zone **us-central1-a**:

```
gcloud compute instances create my-vm-1 --zone=us-central1-a --machine-type=n1-standard-1 \
--subnet=default --network-tier=PREMIUM --tags=http-server --image=debian-9-stretch-v20200910 \
--image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard \
--boot-disk-device-name=my-vm-1
```

- Create a firewall ingress rule that allows HTTP traffic for my-vm-1:

```
gcloud compute firewall-rules create default-allow-http \
--direction=INGRESS --priority=1000 --network=default --action=ALLOW  \
--rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server
```

4. Create another vm in the zone us-central1-b:

- To display the zones for this region, enter the **gcloud compute zones list | grep** command followed by the region for creation which, in case is **us-central1**:
```
gcloud compute zones list | grep us-central1
```

- Choose any zone of your choice from the list and set your default zone to the one you just chose using the **gcloud config set compute/zone** command followed by your zone:
```
gcloud config set compute/zone us-central1-b
```

5. Create a VM instance called my-vm-2 in that zone by executing this command:

```
gcloud compute instances create "my-vm-2" --machine-type="n1-standard-1" \
--image-project="debian-cloud" --image="debian-9-stretch-v20200910" \
--subnet="default"
```

6. SSH into **my-vm-2**:
```
gcloud compute ssh my-vm-2
```

7. Use the **ping** command to confirm that **my-vm-2** can reach **my-vm-1**
```
ping -c 3 my-vm-1
```

> Notice that the output of the ping command reveals that the complete hostname of my-vm-1 is **my-vm-1.c.PROJECT_ID.internal** , where PROJECT_ID is the name of your Google Cloud Platform project

8. Use the ssh command to open a command prompt on my-vm-1:
```
ssh my-vm-1
```

9. At the command prompt on my-vm-1, install the Nginx web server:
```
sudo apt-get install nginx-light -y
```

10. Use the nano text editor to add a custom message to the home page of the web server:
```
sudo nano /var/www/html/index.nginx-debian.html
```

11. Use the arrow keys to move the cursor to the line just below the h1 header. Add text like this, and replace YOUR_NAME with your name:
```
Hi from YOUR_NAME
```

Example:
```
Hi from Victor Adeniyi
```

- Press **Ctrl+O** and press **Enter** to save. Press **Ctrl+X** to Exit

12. Confirm that the web server is serving your new page. At the command prompt on my-vm-1, execute this command:
```
curl http://localhost/
```

> Output: The response will be the HTML source of the web server's home page, including your line of custom text.

13. To exit the command prompt on my-vm-1, execute this command:
```
exit
```

> You will return to my-vm-2 ssh

14. To confirm that my-vm-2 can reach the web server on my-vm-1, at the command prompt on my-vm-2, execute this command:
```
curl http://my-vm-1/
```

> Output: The response will again be the HTML source of the web server's home page, including your line of custom text.

15. Exit the ssh session on my-vm-2:
```
exit
```

16. View the details of **my-vm-1** instance with the command:
```
gcloud compute instances list --zone us-central1-a
```

17. Copy the External IP address for my-vm-1 and paste it into the address bar of a new browser tab.

> You will see your web server's home page, including your custom text.