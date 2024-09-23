---
layout: page
title: AWS Setup
description: for AWS Academy Learner Lab
nav_order: 8
---

# Using AWS Academy, EC2, and Linux Shell

## FAQs

* If you have trouble signing up AWS Academy due to the term item not
showing up, it's because of the browser compatibility issue. Try
updating your Firefox and then try logging in using Firefox. (I will
contact the AWS Academy support to see how this can be addressed as
this is obviously a bug!)
* If you work with WSL, make sure to store your SSH key file under your
Ubuntu home directory. Windows FS does not work well with `chmod
400`. How to locate your `/home/user` directory is by 
typing `cd` and that will automatically bring you to your home dir. 
* Accessing the SSH key file stored under a cloud drive (OneDrive,
Dropbox, or Google Drive) might have permission issue. If that
happens, move `vockey.pem` to a local directory not under any
cloud-managed folder and that should fix the permission issue. 
* For Windows user, we recommend installing WSL (the Windows Subsystem for Linux).
You can find the installation documentation
[here](https://learn.microsoft.com/en-us/windows/wsl/install). With
WSL, you can use the familiar Linux commands for SSH, file
operations, and all other kinds of
[shell-related](https://linuxcommand.org/lc3_lts0010.php) tasks. 
* To copy your submission file from remote EC2 to local, you can use 
`scp` (SSH copy). To do so:
```sh
scp -i vockey.pem ubuntu@YOUR_ADDRESS:PATH_TO_YOUR_FILE .
```
Note that calling scp from EC2 to copy file out will not work as your computer
is behind NAT so it is almost not possible to be directly addressable. You need to run scp from your computer to copy file in from a remote EC2, which is addressable. 



## Overview

We supply you with a cloud-native Linux environment via [AWS
Academy](https://www.awsacademy.com/vforcesite/LMS_Login).
Through AWS Academy, you can get access to Ubuntu 22.04 LTE on [Amazon Web
Services](https://aws.amazon.com) (AWS). This is for those who only have
access to a Windows OS-hosted shell environment. This is to avoid subtle bugs caused by
Windows shell environments. 
However, **it is not required to use AWS EC2 for 
all lab assignments, especially if you already have a native Linux envionment**.

If you haven't worked with AWS Academy before, you need to go through
the AWS Academcy onboarding process. 


This tutorial will walk you through the AWS Academy registration
process and show you how to use AWS and Linux EC2 from AWS Academy to
setup an environment for upcoming programming assignments. 



## Learning outcomes

After completing this assignment, you should be able to:

* Create and SSH into an EC2 virtual machine (EC2) instance.
* Create a remote Jupyter Notebook server running on AWS EC2.
* Get familiar with basic Linux shell commands (browsing file system, installing packages, etc.).



## Part 0: Register an AWS Academy account

You will receive a course invitation email sent to your `virginia.edu` email. 
You will need to register with AWS Academy Canvas (note this is different from UVA Canvas) before you can participate in the AWS Academy Learner Lab. 
To register, click on `Get Started` in the invitation email. It will bring you to the `Welcome Aboard` web page. Enter a password under your email, select `Eastern Time (US & Canada)` as the Time Zone, and click on `Register`. 

After successfully registered, you will have access to your AWS Academy portal. You will see the AWS Academy Learner Lab under `Dashboard`. Click on `Accept` to accept the invitation to join AWS Academy Learner Lab if you see a message at the very top of the web page. 

Click on AWS Academy Learner Lab at the Dashboard to enter. Then
click on `Modules` to view the available modules. 
Before start using AWS resources, I highly encourage you to go through the 
`AWS Academy Learner Lab Student Guide` to get you familiar with how to use
Learner Lab (including how to start a lab, how to track your credit, etc.). 

To start using the AWS resources, click on `Launch AWS Academy Learner Lab` under
`Module`, which will bring you to the Terms and Conditions. Click on
`I agree` and enter the lab session. 

![AWS Academy Setting]({{site.baseurl}}/assets/images/aws_academy_guide_launch_lab.jpg)

The lab session is a web terminal interface with limited
functionality. To start the lab, click on `Start Lab` at the top of
the web terminal interface. The starting process should take a few
minutes (if this is the very first time you start the lab, as in the
background AWS Academy creates a temporary AWS account for you under
the Learner Lab). At the same time, you can see your available AWS
cloud credit with a total amount of $100 and how much you have used.
When the signal light at the top left corner turns green, it
indicates the lab has started. 

![AWS Academy Setting]({{site.baseurl}}/assets/images/aws_academy_dash1.png)

Click on the green signal light to access your AWS Console Home web
page. 

A started lab session has a session duration of up to 
four hours (see the timer `04:00` in the figure above). When the lab
session timer runs to `0:00`, the session will automatically end, but
any data and resources that you created in the AWS account will be
retained. If you later launch a new session (for example, the next
day), you will find that your work is still in the lab environment.
Running EC2 instances will be stopped and then automatically
restarted the next time you start a session. 

> **IMPORTANT:** Monitor your lab budget in the lab interface above. 
Whenever you have an active lab session, the latest known remaining
budget information will display at the top of this screen. This data
comes from AWS Budgets which typically updates every 8 to 12 hours.
Therefore the remaining budget that you see may not reflect your most
recent account activity.  **If you exceed your lab budget your lab
account will be disabled and all progress and resources will be lost.**
As a best practice, we **highly recommend** frequently pushing your
progress to online repositories such as [GitHub](https://github.com/)
to avoid losing data. Therefore, it is important for you to manage
your spending. When you use GitHub or any other online repository service,
make sure to create a **private** repository; **DO NOT** share any of 
your code to other students and the internet.

At the AWS Console Home page, click on `Services` at the top left
corner and click on `Compute` to view all the available computing
services that you may use. Click on `EC2` to start creating new EC2
VM (virtual machine) instances. 


## Part 1: Create and access EC2 instances

### Step 1: Create a name and choose an OS image for your VM
Under `EC2`, click on `Launch instances` to start creating a new VM.
Type a name to label your VM under `Name and tags`. For example, name
your first instance `vm0`.

We recommend using Ubuntu Server 22.04 LTS as the OS image and
`64-bit (x86)` as the architecture. You can also specify the number of
instances to launch at this time. To start off, choose 1. 

![EC2 instances]({{site.baseurl}}/assets/images/ec2_create_vm0.png)

### Step 2: Choose an EC2 instance type
Next choose an EC2 instance type.  To test, you can always create one
or multiple `t2.micro` or `t1.micro`, both of which are free tier
eligible, meaning the resource is free of charge. 
There are, however, a limited range of EC2 instances that you can
choose from. We recommend using the `t3.large` instance type that comes
with 2 vCPU cores and 8 GB of memory.

![EC2 instances]({{site.baseurl}}/assets/images/ec2_create_vm1.png)

### Step 3: Choose an SSH key pair
What is important when creating new VMs is that you choose a key pair
for SSH login. Under `Key pair (login)`, click on the drop down menu
and select `vockey (type rsa)`. 

![EC2 instances]({{site.baseurl}}/assets/images/ec2_create_vm2.png)

### Step 4: Check HTTP/HTTPS options
Under `Network settings`, you may check `Allow HTTPs traffic from the
internet` and `Allow HTTP traffic from the internet` so that you can
access the web servers hosted on your EC2 VM. (Apache Spark comes
with a web-based dash, and would require HTTP/HTTPS traffic.) 

![EC2 instances]({{site.baseurl}}/assets/images/ec2_create_vm3.png)

### Step 5: Configure storage
You may also increase the storage capacity of the `Root volume` under
`Configure storage`. By default you will be allocated a small 8-GB
root disk. We recommend increasing it to 100 GB so that you have
sufficient disk storage capacity for dependency installation and
storing datasets. Optionally, you can also add a new EBS volume of
100GB just in case the 100GB root volume runs out of capacity (it
runs out of space very fast considering the installation of quite a
few large software packages and datasets).

![EC2 instances]({{site.baseurl}}/assets/images/ec2_create_vm4.png)

### Step 6: Configure the subnet availability zone
We recommend launching all your EC2 instances in the same
availability zone. Any US east zones should be fine. This can be
configured in Network settings. Once choosing one, always stick with
it when creating new EC2 instances. This is to guarantee the best
network performance among your EC2 instances.
For example, you may choose to use a subnet within an availability
zone of `us-east-1a` and stick with it.

![EC2 instances]({{site.baseurl}}/assets/images/ec2_create_vm5.png)

### Step 7: Launch the EC2 instance
After finalizing the EC2 VM configuration, click on the `Launch
instance` button on the right side to launch the configured VM. It
may take a few minutes to start the VM. 

### Step 8: Download the SSH private key
Now go back to the Learner Lab web page. This is where you can
download the SSH key. Click on the `AWS Details` tab, you will see
the information you need to login to your computing resources. Under
`SSH key`, click on `Download PEM` to download the SSH key to your
local computer. It comes with a default name of `labsuser.pem`.

![AWS Academy Setting]({{site.baseurl}}/assets/images/aws_academy_ssh_key.png)

### Step 9: Login to the EC2 instance through SSH
Open a terminal (if you use a MacBook, open the `Terminal` software;
if you use a Windows, we recommend [installing WSL](#01242023)),
locate your private key file that you have just downloaded locally,
change its name to `vockey.pem` and update its permission, if
necessary, to ensure your key is not publicly viewable:

```bash
% mv labsuser.pem vockey.pem
% chmod 400 vockey.pem
```

Then, connect to your instance using the following command:

```bash
% ssh -i "vockey.pem" ubuntu@[public_IPv4_DNS_address_of_your_EC2_instance]
``` 

To view the connection instruction, click on the instance ID from
your AWS Console, then click on `Connect` at the top right corner of
the page to view the public DNS address of your EC2 instance. An
example name looks something like this:
`ubuntu@ec2-11-222-3-190.compute-1.amazonaws.com`, where `ubuntu` is
your default username on any EC2 instances that you created. 

![AWS Academy Setting]({{site.baseurl}}/assets/images/ec2_create_vm6.png)


Great! So far you should be able to SSH login to a remote Linux VM computer
that you have just created on AWS running on somewhere in some North Virginia
datacenter! 


## Part 2: Install Go

Simply follow Go's [Download and install](https://go.dev/doc/install) documentation.
After downloading, select the `Linux` tab on that page and follow the rest of the
instructions for Go installation. 


## Part 3: Do data backup on your own

Your AWS resources under the Academy Learner Lab will be permamently
removed once the $100 AWS credit is fully depleted. 
You are responsible for backing up all your lab-associated data. You
may use any tools that you like. Examples include, but not limited
to:

* `scp`: where you need to manually `scp` all your data regularly.
* GitHub: **Note that if you choose to use GitHub, you should not make your GitHub repo publicly available.** Making your repos publicly available is considered cheating.




