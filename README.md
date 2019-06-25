# Install-Jenkins-on-Linux-EC2
Jenkins is an open-source automation server that integrates with a number of AWS Services, such as AWS CodeCommit, AWS CodeDeploy, Amazon EC2 Spot, and Amazon EC2 Fleet. You can use Amazon Elastic Compute Cloud (Amazon EC2) to deploy a Jenkins application on AWS in a matter of minutes. This tutorial walks you through the process of deploying a Jenkins application. You will launch an EC2 instance, install Jenkins on that instance, and configure Jenkins to automatically spin up Jenkins build slave instances if build abilities need to be augmented on the instance. 
Follow following steps to get started with Jenkins:

Step 1: Set Up Prerequisites
To prepare for this tutorial, you will need an AWS account, an AWS Identity and
Access Management (IAM) user name and password, an Amazon EC2 key pair,
and a configured Virtual Private Cloud (VPC). Detailed instructions can be
found in Setting Up to Host a Web App on AWS.


Step 2: Launch an EC2 Instance
In this step you will launch a virtual server to host Jenkins. These virtual servers
are called EC2 instances. Typically, you start from a base image called an
Amazon Machine Image (AMI).

You will complete the following tasks:
• Create a Security Group for Your Amazon EC2 Instance
• Launch Your EC2 Instance

For this tutorial, you will create a security group and add the following rules:
• Allow inbound HTTP access from anywhere
Amazon Web Services – Set Up a Jenkins Build Server
• Allow inbound SSH traffic from your computer's public IP address so
that you can connect to your instance.

Make sure to select Linuz Instance.

Step 3: Install and Configure Jenkins
In this step you will deploy Jenkins on your EC2 instance by completing the
following tasks:
• Connect to Your Linux Instance
• Download and Install Jenkins
• Configure Jenkins
1. Connect to Your Linux Instance
After you launch your instance, you can connect to it and use it the way that you
would use a computer sitting in front of you.
Before you connect to your instance, get the public DNS name of the instance
using the Amazon EC2 console. Select the instance and locate Public DNS on
the Description tab.
To Connect to Your Linux Instance from Windows Using PuTTY
1. From the Start menu, choose All Programs > PuTTY > PuTTY.
2. In the Category pane, select Session, and complete the following fields:
a. In Host Name, enter ec2-user@public_dns_name.
b. Ensure that Port is 22.
3. In the Category pane, expand Connection, expand SSH, and then
select Auth. Complete the following:
a. Click Browse.
b. Select the .ppk file that you generated for your key pair, as
described in Create a Key Pair,5 and then click Open.
c. Click Open to start the PuTTY session.
4. If this is the first time you have connected to this instance, PuTTY
displays a security alert dialog box that asks whether you trust the host
you are connecting to. Click Yes. A window opens and you are connected
to your instance.
To Connect to Your Instance from Linux or Mac OS X Using SSH
1. Use the ssh command to connect to the instance. You will specify the
private key (.pem) file and ec2-user@public_dns_name.
$ ssh -i /path/my-key-pair.pem ec2-user@ec2-198-51-
100-1.compute-1.amazonaws.com
You will see a response like the following:
The authenticity of host 'ec2-198-51-100-1.compute1.amazonaws.com (10.254.142.33)' can't be
established.
RSA key fingerprint is
1f:51:ae:28:bf:89:e9:d8:1f:25:5d:37:2d:7d:b8:ca:9f:f5
:f1:6f.
Are you sure you want to continue connecting
(yes/no)?
2. Enter yes.
You will see a response like the following:
Warning: Permanently added 'ec2-198-51-100-1.compute1.amazonaws.com' (RSA) to the list of known hosts.

Download and Install Jenkins
To download and install Jenkins:
1. To ensure that your software packages are up to date on your instance,
use the following command to perform a quick software update:
[ec2-user ~]$ sudo yum update –y
2. Add the Jenkins repo using the following command:
[ec2-user ~]$ sudo wget -O
/etc/yum.repos.d/jenkins.repo http://pkg.jenkinsci.org/redhat/jenkins.repo
3. Import a key file from Jenkins-CI to enable installation from the
package:
[ec2-user ~]$ sudo rpm --import
https://pkg.jenkins.io/redhat/jenkins.io.key
4. Install Jenkins:
[ec2-user ~]$ sudo yum install jenkins -y
5. Start Jenkins as a service:
[ec2-user ~]$ sudo service jenkins start

Configure Jenkins
Jenkins is now installed and running on your EC2 instance. To configure
Jenkins:
1. Connect to http://<your_server_public_DNS>:8080 from your favorite
browser. You will be able to access Jenkins through its management
interface.
2. As prompted, enter the password found in
/var/lib/jenkins/secrets/initialAdminPassword. Use the following
command to display this password:
[ec2-user ~]$ sudo cat
/var/lib/jenkins/secrets/initialAdminPassword
3. The Jenkins installation script directs you to the Customize Jenkins page.
Click Install suggested plugins.
4. Once the installation is complete, enter Administrator Credentials, click
Save Credentials, and then click Start Using Jenkins.
5. On the left-hand side, click Manage Jenkins, and then click Manage
Plugins.
6. Click on the Available tab, and then enter Amazon EC2 plugin at the top
right.
7. Select the checkbox next to Amazon EC2 plugin, and then click Install
without restart.
8. Once the installation is done, click Go back to the top page.
9. Click on Manage Jenkins, and then Configure System.
10. Scroll all the way down to the section that says Cloud.
11. Click Add a new cloud, and select Amazon EC2. A collection of new
fields appears.
12. Fill out all the fields. (Note: You will have to Add Credentials of the kind
AWS Credentials.)
You are now ready to use EC2 instances as Jenkins build slaves.



