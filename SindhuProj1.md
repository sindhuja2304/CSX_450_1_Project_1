 
**SYSTEM DESIGN**

![system](https://user-images.githubusercontent.com/35319815/34928757-93c09280-f98d-11e7-9904-b57d01160ed1.JPG)

**SSH KEYS**
The authentication keys named SSH keys are created using Keygen program. SSH-Keygen tool create a key pair (public and private) on our local system. Below is the process of creating SSH Keygen. The easiest way to create the key pair is to run the command without any arguments.

![ssh](https://user-images.githubusercontent.com/35319815/34912651-cf0e4b72-f8b4-11e7-87f7-b07a8cd67101.JPG)

This process will generate a id_rsa (private key) and id_rsa.pub (public key),a key pair. With this public key we will be able to SSH into the system which we wish to access using the id_rsa file.

Using below command we can verify the contents of the generated public key.

**$cat ~/.ssh /id_rsa.pub**

Generated output is as below

![publickey](https://user-images.githubusercontent.com/35319815/34912729-8d1a2342-f8b6-11e7-9af0-5055d72705bd.JPG)

**Amazon Web Services**
**AWS** is the cloud-based service providing platform. Amazon EC2 (Elastic Compute Cloud) is a web service which provides secure and capacity in cloud. Users can try the AWS free tier which includes 750 hours of free usage on their T2.microplatform.

The process of configuring an AWS EC2 system consists of below steps

**Step1**:  Create a new key pair
- Go to EC2 Dashboard. Navigate Key Pairs Pane on EC2 Dashboard.
- Create a new key pair if you don’t have an existing key.
- Choose Import to create a new key pair. Provide a name to the key pair. Copy the generated public key of SSH to the Public key contents.

![key-pair-ubuntu](https://user-images.githubusercontent.com/35319815/34913251-0b603fd4-f8c6-11e7-9019-69cdff6ea9ef.JPG)

**Step2**: Create a New Security group
1.	Go to EC2 Dashboard. Navigate to Security Group Pane then click on Create Security Group.
2.	Provide the security group name, description and VPC.
3.	Configure the following Security Group rules for Inbound as below

![securitygrp](https://user-images.githubusercontent.com/35319815/34926815-f95d7b84-f97f-11e7-859a-01ce80829a6d.JPG)

**Step3**: Create a New EC2 Instance Type,Go to EC2 Dashboard and click on Launch Instance Button.
All the necessary configurations are made as part of launching new instance.
1. In the First tab "choose AMI"(Amazon Machine Image).Choose the Ubuntu server that is free tier eligible.

![ami](https://user-images.githubusercontent.com/35319815/34927077-975cd13a-f981-11e7-914f-85cdd06359a4.JPG)
2. In the Second tab "Choose Instance Type".You can choose the free tier T2.Micro.

![chooseinstance](https://user-images.githubusercontent.com/35319815/34926847-350547f2-f980-11e7-9fe8-6d206d3ff577.JPG)

3. In the third tab you can "Configure Instance".This step can be ignored
4. In the fourth tab you can "Add Storage".Storage can be added upto 30 gb for free version.
5. In the fifth tab you can "Add tags".This step can be ignored.
6. In the sixth tab "Configure Security Group", is a Critical configuration. Choose the existing security group created to the instance that you are configuring. Verify that necessary ports are available in the Inbound rules at the bottom of tab.

![configuresecuritygroup](https://user-images.githubusercontent.com/35319815/34926867-5393e67e-f980-11e7-9e0a-ba796c348d30.JPG)
Users might receive a warning from AWS saying “Improve your instances’ security. The security group created is now open to the world.

7.In the last tab Click "Review and Launch" button. 
Then, "Select an existing key pair or create a new key pair" will be prompted.Select the existing key-pair  and accept the acknowledgement and then launch the instance.

![launchinstance](https://user-images.githubusercontent.com/35319815/34926874-5f28952a-f980-11e7-9e35-6992282f21d8.JPG)

You will be notified of the running instance. Make sure instance is running by navigating to EC2 Dashboard Instances Pane. Make a note of the IP address of the instance at the bottom of tab.

![instance](https://user-images.githubusercontent.com/35319815/34928557-68fb5770-f98c-11e7-85c5-566125e3ce06.JPG)

**Docker Installation**

1.We can configure the New EC2 Instance for Using Docker.

   $ ssh ubuntu@ 35.167.20.17
   

2.Docker can be installed via shell script obtained from get.docker.com and passed via pipe (|) to a shell(sh).

   ubuntu@ip-172-31-29-202:~$ curl -sSL https://get.docker.com/ | sh
   
![installdocker](https://user-images.githubusercontent.com/35319815/34927081-97d099f8-f981-11e7-9b49-803061020b82.JPG)
       
3.Add the ubuntu user to docker group. In general the command line docker client will require sudo access in order to issue commands to the docker. You can avoid using sudo by adding Ubuntu user to the docker group.

   ubuntu@ip-172-31-29-202:~$ sudo usermod -aG docker ubuntu

4.You can reboot the system for the changes to take effect.

   ubuntu@ip-172-31-29-202:~$ sudo reboot

5.Reconnect to the system after reboot. You can verify the docker version by issuing below command.

   ubuntu@ip-172-31-29-202:~$ docker –v
   
![dockerversion](https://user-images.githubusercontent.com/35319815/34927080-97a9b19e-f981-11e7-9946-43663bda99da.JPG)


Docker Image can be obtained by issuing the docker pull command as below.

ubuntu@ip-172-31-29-202:~$ docker pull jupyter/datascience-notebook. 
![pullingdockerimage](https://user-images.githubusercontent.com/35319815/34927082-97f26a88-f981-11e7-9571-633fad3591e9.JPG)

The image pulled is present in the docker images cache.
The below command shows all the docker images, their repository and tags and their size.

ubuntu@ip-172-31-29-202:~$ docker images –a
![dockerimage](https://user-images.githubusercontent.com/35319815/34927079-9790a8fc-f981-11e7-9274-403647e56c14.JPG)

To run a Jupyter container, Docker will load the container from the image in your cache.
In the below command the -p flag serves to link port 80 on the host machine, your EC2 instance, to the port 8888 on which the Jupyter Notebook server is running in the Docker container.

ubuntu@ip-172-31-29-202:~$ docker run -v /home/ubuntu:/home/jovyan -p 80:8888 -d jupyter/datascience-notebook
![containerid](https://user-images.githubusercontent.com/35319815/34927078-9777332c-f981-11e7-98eb-b48ccb249ec3.JPG)


You can run the below command to run the Jupyter Notebook server.

ubuntu@ip-172-31-29-202:~$ docker exec 1ea2 jupyter notebook list

![url](https://user-images.githubusercontent.com/35319815/34927800-eedbe7de-f986-11e7-926c-5da63f402e27.JPG)

The output from the running Jupyter Notebook server provides you with a security authentication token. You can use to access the Notebook server through a browser.You can also do this using the URL by replacing the local host with Public IP address of the EC2 instance.
![jupytertoken](https://user-images.githubusercontent.com/35319815/34927818-114f5f44-f987-11e7-872d-c91304403c7c.JPG)

![jupyter](https://user-images.githubusercontent.com/35319815/34929830-2344cd3a-f994-11e7-82fa-c3c03a23f1f1.JPG)


**A detailed budget of the costs of running a Jupyter Data Science Notebook Server for three months using at least three different kinds of EC 2 instances is as below**

General Purpose - Current Generation
t2.large
2 vCPU,Variable ECU,8 Memory(GiB)
Price=$0.0928 per Hour=$200.448 per three months

m4.large
2 vCPU,6.5 ECU,8 Memory(GiB)
Price=$0.1 per Hour=$216 per three months

Compute Optimized - Current Generation
c4.large
2 vCPU,8 ECU,3.75 Memory(GiB)
Price=$0.1 per Hour=$216 per three months

Memory Optimized - Current Generatio
r3.large
2 vCPU,6.5 ECU,15 Memory(GiB)
Price=$0.166 per Hour=$358.56 per three months

