# AWS EC2 SSH Terraform module

Terraform module which creates an AWS EC2 instance which is accesible via SSH using a username and password.

## Usage

Deploy this module using standard methods to set creadentials and region (e.g., environment variables).
When deployed, an EC2 instance will be created in the given region using the latest Ubuntu 18.04 LTS image available, a randomly generated password will be set for the root user and SSH will be configured to allow login using it.
The public IP and password for the inatnce will be returned.

## Example usage

In a new shell, navigate to the module folder and set relevant environment variable to specify credentials and region:
```
export AWS_ACCESS_KEY_ID="anaccesskey"
export AWS_SECRET_ACCESS_KEY="asecretkey"
export AWS_REGION="us-west-2"
```

Apply the TF module while specifing an instance type and leaving out the ami to use:
```
terraform apply -var="instance_type=m5.large"
```
One deployment is done, outputs will be provided with the public address and password for the instance. Since the password output is sensetive, it will be hidden by default, use the following command line to retreive it:
```
terraform output -json
```
In order to connect to the instance, you will need to use the user name pre-set for the image you selected, for the default, this name is 'ubuntu'.
See below a table of default usernames for common images:

| Instance |Username |
|------|-------------|
Ubuntu | ubuntu
Redhat Linux | ec2-user
Amazon Linux | ec2-user
CentOS | centos
Debian | admin or root

to connect to the instance from a bash command line, enter the following command:

```
ssh -o StrictHostKeyChecking=no ubuntu@PUBLIC_IP
```
(note 'ubuntu' is used in the example command line above since the default image is Ubuntu)
You will be asked for the password and be provided with a bash command line on the remote instance.

## Providers

| Name | Version |
|------|---------|
| aws | >= 4.0.0 |
| random | >= 3.1.0 |
|template | >= 2.0.0 |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| instance_type | The EC2 instance type to create. when ommited a t3.micro instance is created. | `string` | `"t3.micro"` | no |
| ami | The AMI to use for the instance. when ommited defualts to the latest Ubuntu 18.04 LTS imsage available for the given region. | `string` | Ubuntu 18.04 LTS | no |

## Outputs

| Name | Description |
|------|-------------|
public_ip | The public IP address assigned to the instance.
password | The password generated for the instance.