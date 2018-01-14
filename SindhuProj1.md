
Amazon EC 2
Security Groups
AWS Operating System
Docker Installation
Obtaining the correct Docker image
Running the correct Docker image as a container
Jupyter notebook security concerns
Anything else I may have forgotten ...
Create at least one diagram of your overall system.
A detailed budget of the costs of running a Jupyter Data Science Notebook Server for three months using at least three different kinds of EC 2 instances.
Solution

**SSH KEYS**
The authentication keys named SSH keys are created using Keygen program.SSH-Keygen tool create a key pair(public and private) on our local system.Below is the process of creating SSH Keygen.The easiest way to create the key pair is to run the command without any arguments.

![ssh](https://user-images.githubusercontent.com/35319815/34912651-cf0e4b72-f8b4-11e7-87f7-b07a8cd67101.JPG)

This process will generate a id_rsa(private key) and id_rsa.pub(public key),a key pair.With this public key we will be able to SSH into the system which we wish to access using the id_rsa file.

Using below command we can verify the contents of the generated public key.
$cat ~/.ssh/id_rsa.pub
Generated output is as below

![publickey](https://user-images.githubusercontent.com/35319815/34912729-8d1a2342-f8b6-11e7-9af0-5055d72705bd.JPG)

AWS Operating System Installation Steps:
AWS is the cloud-based service providing platform.Amazon EC2(Elastic Compute Cloud) is a web service which provides secure and capacity in cloud.Users can try the AWS free tier which includes 750 hours of free usage on their T2.microplatform.

The process of configuring an AWS EC2 system consists of below steps
Step1: Configure a key pair
Go to EC2 Dashboard.Navigate Key Pairs Pane on EC2 Dashboard.
Create a new key pair if you dont have an existing key.
Choose Import to create a new key pair.Provide a name to the key pair.Copy the generated public key of SSH to the Public key contents.
