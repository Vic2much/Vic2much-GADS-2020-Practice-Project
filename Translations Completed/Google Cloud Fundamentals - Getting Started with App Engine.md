---
# Google Cloud Fundamentals: Getting Started with App Engine

---

## Overview
In this lab, you create and deploy a simple App Engine application using a virtual environment in the Google Cloud Shell.

## Objectives
In this lab, you learn how to perform the following tasks:

- Initialize App Engine.

- Preview an App Engine application running locally in Cloud Shell.

- Deploy an App Engine application, so that others can reach it.

- Disable an App Engine application, when you no longer want it to be visible.

## STEPS

### Task 1: Sign in to the Google Cloud Platform (GCP) Console
1. Sign in to the Google Cloud Platform (GCP) Console 

[Sign in to the Google Console Here](https://console.cloud.google.com/) with the details provided for you

### Task 2: Open Cloud Shell
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

Full documentation of **gcloud** is available on [Google Cloud gcloud Overview](https://cloud.google.com/sdk/gcloud).

### Task 3: Initialize App Engine
1. Initialize your App Engine app with your project and choose its region:
```
gcloud app create --project=$DEVSHELL_PROJECT_ID
```

When prompted, select the [region](https://cloud.google.com/appengine/docs/locations) where you want your App Engine application located.

2. Clone the source code repository for a sample application in the **hello_world** directory:
```
git clone https://github.com/GoogleCloudPlatform/python-docs-samples
```

3. Navigate to the source directory:
```
cd python-docs-samples/appengine/standard_python3/hello_world
```


### Task 4: Run Hello World application locally
In this task, you run the Hello World application in a local, virtual environment in Cloud Shell.

Ensure that you are at the Cloud Shell command prompt.

1. Execute the following command to download and update the packages list.
```
sudo apt-get update
```

2. Set up a virtual environment in which you will run your application. Python virtual environments are used to isolate package installations from the system.
```
sudo apt-get install virtualenv
```

>If prompted [Y/n], press `Y` and then `Enter`.
```
virtualenv -p python3 venv
```

3. Activate the virtual environment.
```
source venv/bin/activate
```

4. Navigate to your project directory and install dependencies.
```
pip install  -r requirements.txt
```

5. Run the application:
```
python main.py
```

>Please ignore the warning if any.

6. In **Cloud Shell**, click **Web preview** (![Web Preview](https://cdn.qwiklabs.com/7b9oXblGsiFuNK7hmDZjFB%2B7Lrwdv5T64bbmo8X9FAo%3D)) > **Preview on port 8080** to preview the application.
>To access the **Web preview** icon, you may need to collapse the **Navigation menu**.

Result:
![Result](https://cdn.qwiklabs.com/vTRhzjVoW3LX%2BaFG6ox7ZExJHDQvTdMK8fAyRGBQCDQ%3D)

7. To end the test, return to Cloud Shell and press Ctrl+C to abort the deployed service.


### Task 4: Disable the application
App Engine offers no option to **Undeploy** an application. After an application is deployed, it remains deployed, although you could instead replace the application with a simple page that says something like "not in service."

However, you can disable the application, which causes it to no longer be accessible to users.

- In the Cloud Console, on the **Navigation menu** (![Navigation menu](https://cdn.qwiklabs.com/LyLHJ5I3gtYdRN1pHDZ2JK2vbd1sM6W2viT0OzyRPTs%3D)), click **App Engine** > **Settings**.

- Click **Disable application**.

- Read the dialog message. Enter the **App ID** and click **DISABLE**.

**Or use cloud shell *(using any of the following command-lines as deem fit)***

### gcloud app versions stop

#### NAME
*gcloud app versions stop - stop serving specified versions*

#### SYNOPSIS

`gcloud app versions stop` `VERSIONS` [`VERSIONS` …][`--service`=`SERVICE`, `-s` `SERVICE`] [`GCLOUD_WIDE_FLAG …`]

#### DESCRIPTION
This command stops serving the specified versions. It may only be used if the scaling module for your service has been set to manual.

#### POSITIONAL ARGUMENTS
`VERSIONS` [`VERSIONS` …]

The versions to stop (optionally filtered by the --service flag).

#### FLAGS
`--service`=`SERVICE`, `-s` `SERVICE`

If specified, only stop versions from the given service.

#### GCLOUD WIDE FLAGS
These flags are available to all commands: *[--account](https://cloud.google.com/sdk/gcloud/reference#--account), [--billing-project](https://cloud.google.com/sdk/gcloud/reference#--billing-project), [--configuration](https://cloud.google.com/sdk/gcloud/reference#--configuration), [--flags-file](https://cloud.google.com/sdk/gcloud/reference#--flags-file), [--flatten](https://cloud.google.com/sdk/gcloud/reference#--flatten), [--format](https://cloud.google.com/sdk/gcloud/reference#--format), [--help](https://cloud.google.com/sdk/gcloud/reference#--help), [--impersonate-service-account](https://cloud.google.com/sdk/gcloud/reference#--impersonate-service-account), [--log-http](https://cloud.google.com/sdk/gcloud/reference#--log-http), [--project](https://cloud.google.com/sdk/gcloud/reference#--project), [--quiet](https://cloud.google.com/sdk/gcloud/reference#--quiet), [--trace-token](https://cloud.google.com/sdk/gcloud/reference#--trace-token), [--user-output-enabled](https://cloud.google.com/sdk/gcloud/reference#--user-output-enabled), [--verbosity](https://cloud.google.com/sdk/gcloud/reference#--verbosity).*

Run  $*[gcloud help](https://cloud.google.com/sdk/gcloud/reference)* for details.

#### EXAMPLES
To stop a specific version across all services, run:
```
gcloud app versions stop v1
```

To stop multiple named versions across all services, run:
```
gcloud app versions stop v1 v2
```

To stop a single version on a single service, run:
```
gcloud app versions stop --service=servicename v1
```

To stop multiple versions in a single service, run:
```
gcloud app versions stop --service=servicename v1 v2
```

Note that that last example may be more simply written using the services stop command (see its documentation for details).

#### NOTES
This variant is also available:
```
gcloud beta app versions stop
```

#### After disabling the application
If you refresh the browser window you used to view to the application site, you'll get a 404 error.
![error 404](https://cdn.qwiklabs.com/jVzvehqMDLGdJxcGG6aHjrT1zG6SRd443bZo%2BTO383I%3D)


