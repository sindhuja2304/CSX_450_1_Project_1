The authentication keys named SSH keys are created using Keygen program. SSH-Keygen tool create a key pair (public and private) on our local system. Below is the process of creating SSH Keygen. The easiest way to create the key pair is to run the command without any arguments.

![ssh](https://user-images.githubusercontent.com/35319815/34912651-cf0e4b72-f8b4-11e7-87f7-b07a8cd67101.JPG)

This process will generate a id_rsa (private key) and id_rsa.pub (public key),a key pair. With this public key we will be able to SSH into the system which we wish to access using the id_rsa file.

Using below command we can verify the contents of the generated public key.
$cat ~/.ssh /id_rsa.pub
Generated output is as below

![publickey](https://user-images.githubusercontent.com/35319815/34912729-8d1a2342-f8b6-11e7-9af0-5055d72705bd.JPG)

AWS is the cloud-based service providing platform. Amazon EC2 (Elastic Compute Cloud) is a web service which provides secure and capacity in cloud. Users can try the AWS free tier which includes 750 hours of free usage on their T2.microplatform.

The process of configuring an AWS EC2 system consists of below steps
**Step1**: Configure a key pair
Go to EC2 Dashboard. Navigate Key Pairs Pane on EC2 Dashboard.
Create a new key pair if you don’t have an existing key.
Choose Import to create a new key pair. Provide a name to the key pair. Copy the generated public key of SSH to the Public key contents.
![key-pair-ubuntu](https://user-images.githubusercontent.com/35319815/34913251-0b603fd4-f8c6-11e7-9019-69cdff6ea9ef.JPG)

**Step2**: Create a New Security group
1.	Go to EC2 Dashboard. Navigate to Security Group Pane then click on Create Security Group.
2.	Provide the security group name, description and VPC.
3.	Configure the following Security Group rules for Inbound as below

**Step3**: Create a New EC2 Instance Type,
Go to EC2 Dashboard and click on Launch Instance Button.
All the necessary configurations are made as part of launching new instance.
1. In the First tab "choose AMI"(Amazon Machine Image).Choose the Ubuntu server that is free tier eligible.
2. In the Second tab "Choose Instance Type"
3. In the third tab you can "Configure Instance".This step can be ignored
4. In the fourth tab you can "Add Storage".Storage can be added upto 30 gb for free version.
5. In the fifth tab you can "Add tags".This step can be ignored.
6. In the sixth tab "Configure Security Group", is a Critical configuration. Choose the existing security group created to the instance that you are configuring. Verify that necessary ports are available in the Inbound rules at the bottom of tab.
Users might receive a warning from AWS saying “Improve your instances’ security. The security group created is now open to the world
Last step, Click "Review and Launch" button. 
Then, "Select an existing key pair or create a new key pair" will prompted. Here select key-pair created earlier and accept the acknowledgement then launch the instance.
You will be notified of the running instance. Make sure instance is running by navigating to EC2 Dashboard Instances Pane. Make a note of the IP address of the instance at the bottom of tab.

Docker Installation
1.We can configure the New EC2 Instance for Using Docker.
   $ ssh ubuntu@ 35.167.20.17

2.Docker can be installed via shell script obtained from get.docker.com and passed via pipe (|) to a shell(sh).
   ubuntu@ip-172-31-29-202:~$ curl -sSL https://get.docker.com/ | sh
       
3.Add the ubuntu user to docker group. In general the command line docker client will require sudo access in order to issue commands to the docker. You can avoid using sudo by adding Ubuntu user to the docker group.
   ubuntu@ip-172-31-29-202:~$ sudo usermod -aG docker ubuntu

4.You can reboot the system for the changes to take effect.
   ubuntu@ip-172-31-29-202:~$ sudo reboot

5.Reconnect to the system after reboot. You can verify the docker version by issuing below command.
   ubuntu@ip-172-31-29-202:~$ docker –v


Docker Image can be obtained by issuing the docker pull command as below.
ubuntu@ip-172-31-29-202:~$ docker pull jupyter/datascience-notebook. 
The image pulled is present in the docker images cache.




The below command shows all the docker images, their repository and tags and their size.

ubuntu@ip-172-31-29-202:~$ docker images –a

To run a Jupyter container, Docker will load the container from the image in your cache.
In the below command the -p flag serves to link port 80 on the host machine, your EC2 instance, to the port 8888 on which the Jupyter Notebook server is running in the Docker container.

ubuntu@ip-172-31-29-202:~$ docker run -v /home/ubuntu:/home/jovyan -p 80:8888 -d jupyter/datascience-notebook




The output from the running Jupyter Notebook server provides you with an authentication token. You can use to access the Notebook server through a browser. You can also do this using the URL by replacing the local host with Public IP address of the EC2 instance.






**A detailed budget of the costs of running a Jupyter Data Science Notebook Server for three months using at least three different kinds of EC 2 instances**
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

