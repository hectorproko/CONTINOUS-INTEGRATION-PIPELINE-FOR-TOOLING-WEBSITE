# CONTINOUS-INTEGRATION-PIPELINE-FOR-TOOLING-WEBSITE

**Project 9:** Enhance the architecture prepared in  [**Project 8:** LOAD-BALANCER-SOLUTION-WITH-APACHE](https://github.com/hectorproko/LOAD-BALANCER-SOLUTION-WITH-APACHE) by adding a Jenkins server, configure a job to automatically deploy source codes changes from Git to NFS server.

## Provision AWS Server
	
        
 1. Create an Ubuntu Server 20.04  
[Creating Ubuntu instance in AWS (**EC2**)](https://github.com/hectorproko/RepeatableSteps_tutorials/blob/main/AWS_Ubuntu_Instnace.md)

2. Open TCP port **8080** creating an **Inbound Rule** in Security Group.  
[Open Ports in AWS (**EC2**)](https://github.com/hectorproko/RepeatableSteps_tutorials/blob/main/OpenPortAWS.md)

## Install Jenkins
``` bash
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'

sudo apt update
sudo apt-get install jenkins
```

From your browser access http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080
You will be prompted to provide a default admin password

## Image: Unlock

We retrieve the password from the specified path
``` bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

``` bash
*************************************************************

Jenkins initial setup is required. An admin user has been created and a password generated.
Please use the following password to proceed to installation:

8cba9d4d4dcc4658931ba22fe9435521

This may also be found at: /root/.jenkins/secrets/initialAdminPassword

*************************************************************
```

Then you will be asked which plugings to install – choose suggested plugins.
## Image: Suggested

Once plugins installation is done – create an admin user and you will get your Jenkins server address.
The installation is completed!



## Configure Jenkins to retrieve source codes from GitHub using Webhooks
In this part, you will learn how to configure a simple Jenkins job/project (these two terms can be used interchangeably). This job will will be triggered by GitHub webhooks and will execute a ‘build’ task to retrieve codes from GitHub and store it locally on Jenkins server.

### 1. Create Jeknkins Job
Go to Jenkins web console, click "New Item" and create a "Freestyle project"
Pres ok at the bottom
## Image: Job

It will prompt you for the configuration of the job
Go to Source Code management tab pick Git put the URL of tooling repo
https://github.com/hectorproko/tooling.git
## Image: Source Code

Click save

Issue:
When specifying repo, Jenkins uses master even though my local repo shows main, if I change to main I get error
ERROR: Couldn't find any revision to build. Verify the repository and branch configuration for this job.
Finished: FAILURE
From <http://18.204.202.156:8080/job/tooling_github/3/console> 

It will take you to the Jobs Dashboard where you will "Build Now" to build 
We test it
## Image: Build Now


Going to configure to trigger whenever there is a change in the sourcecode/Github's Repo

Configure, "Build Triggers" Tab, check 
GitHub hook trigger for GITScm polling

Archive the artifacts for now all *

## Image: Buildtriggers

addin webhooks in github

Go to the setting of the repo you wnat to add webhook Setttings > Webhooks > Add webhook

## Image: Webhook1

On Payload URL, we put the public IP of our Jenkins instance followed by 8080 port and /github-webhhook/


## Image: Webhook2

Now we can commit something to our repo to test the webhook
Here Im creating a test file

## Image: commit

This triggers a 5th build

## Image: Build5

If I look at the conosole output of the build 5 i can see that the job was trigger by Github push

## Image: Conosoleoutput


