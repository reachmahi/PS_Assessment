Terraform is an open-source tool that codifies APIs into declarative configuration files. Team members can share the configuration files amongst the team. You and your team can treat the Terraform configuration files as code: edit, review, and; version them.

Problem statement:
Cloud
•	Using your favorite configuration management tool (Terraform) write the code to:
1.	Provision a virtual machine on your preferred cloud provider (AWS)
2.	Install the components necessary to run your favourite webserver on it
3.	Write the code to push/install new application code onto the server

Solution:

Pre Requisites:

An AWS account with powers to create resources. If you don’t have an AWS account, a free tier account is available.
An EC2 instance with Terraform installed.
An installed and configured AWS CLI.
Related:
Building an AWS VPC with Terraform Step-by-Step
Using a Terraform Provisioner, in combination with PowerShell, can help automate infrastructure creation. In this guide, you will learn how to use Terraform Provisioners to create infrastructure for you

Steps to execute 

1) SSH into ec2 instance and ensure terraform is installed in ubuntu instance and ensure git is also installed in it
2.a Create a main.tf file which is the main configuration file and it has below contents in it

When you check the file’s contents, you can see that it is responsible for creating an EC2 VM (aws_instance) and giving it many properties. Such as:

The image you will install on your VM: data.aws_ssm_parameter.webserver-ami.value.
A public IP address: associate_public_ip_address = true.
The VM instance type: t2.micro.
The key pair you want to use for this VM: aws_key_pair.webserver-key.key_name
A few security groups: aws_security_group.sg.id.

2.b The second file in the directory is the setup.tf file. Again, run the cat command below to check the contents of the setup.tf file

The file contains a few configurations worth examining. As you don’t want to miss anything, a few screenshots will come next, highlighting different relevant parts.

The primary focus of this guide is Terraform provisioners. So you will want to pay special attention to the provisioner block present in setup.tf.

When inspecting the code, you will see the remote-exec keyword, meaning this Terraform provisioner is a remote one. The remote-exec keyword allows you to execute commands on the remote host: your web server, an EC2 instance.

Next, you will see the inline section, which lists commands to run on your newly created VM. For this example, the commands will be for:

Installing Apache (install httpd) and starting the webserver (systemctl start httpd);
Creating an index.html file that will have its contents displayed when testing the server. The file contains the text to be shown: My Test Website With Help From Terraform Provisioner;
Moving the index.html file to /var/www/html/ – Apache web root directory – after provisioning.

Next, comes the connection block: this block tells Terraform what kind of connection to make when it’s running these commands. In this case, the connection type is ssh, and the username is ec2-user.

The private_key is of the typical .ssh/id_rsa format from Linux. This line creates a file called ~/.ssh/id_rsa and puts the AWS EC2 user’s PEM-encoded RSA key in it. Since you are SSHing into the EC2 with the PEM-encoded RSA key, the host has access to execute commands. Terraform will grab the contents from the variable declared earlier (associate_public_ip_address = true).

3) This helps in the infrastructure set up

Work in progress 
To have these checks integrated with the existing code 

1.	Network only allows secured HTTP access from the outside.
2.	Versioning, audit trails are enabled on file/object stores
3.	Logs are sent to an appropriate location
4.	All data at rest for any databases/volumes are encrypted
 




