# Creating AWS S3-bucket as the remote backend for Terraform

Configuring a backend for any kind of Terraform project is always recommended. Backends are primarily meant to manage the information contained in the state files. State files hold the mapping of the Terraform configuration with their real-world deployments. The state files are created at the very first execution of the Terraform configuration. All the runs after that will always be validated with the same state files to make sure the intended changes are applied.

## Features

* Secure storage
* Locking



## Step 1: Create an s3-bucket

![image](https://github.com/Akshay-Gk/Terraform-remote-backend-s3/assets/112197849/08a6d3fb-0b1c-42d8-b848-d7514b06ba7c)


* Enter name

![image](https://github.com/Akshay-Gk/Terraform-remote-backend-s3/assets/112197849/5095b8e5-5560-488c-a5d6-4807c4752c68)


* Enable versioning
> `Helps to keep multiple versions of an object in one bucket so that you can restore objects that are accidentally deleted or overwritten`
 

![image](https://github.com/Akshay-Gk/Terraform-remote-backend-s3/assets/112197849/fa7e9a74-75cc-4e2b-b5db-d11cd77f39cd)


> **We can see the created bucket**


![image](https://github.com/Akshay-Gk/Terraform-remote-backend-s3/assets/112197849/4341fd20-ed39-4f2d-a62d-75381cf0b6dc)


## Step 3: Create an IAM policy


* Creating an IAM policy for s3-bucket

![image](https://github.com/Akshay-Gk/Terraform-remote-backend-s3/assets/112197849/bac2f5a4-2de4-4b66-9f0f-12a1d9c018a3)



```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::terraform.akshay.com"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject"
            ],
            "Resource": "arn:aws:s3:::terraform.akshay.com/terraform.tfstate"
        }
    ]
}

```

> **We can see the created policy with the name "terraform-backend-policy"**

![image](https://github.com/Akshay-Gk/Terraform-remote-backend-s3/assets/112197849/a3a9e38e-0b44-46ed-926d-c38961642f70)



## Step 3: Create an IAM role

> `An IAM role is an identity you can create with specific permissions with valid credentials for short durations. Roles can be assumed by entities that you trust`


  ![image](https://github.com/Akshay-Gk/Terraform-remote-backend-s3/assets/112197849/f9ead5e0-5eb0-47b2-8763-bba5275171e0)

* We are creating for AWS service entity type. Choose ec2, Click "next"

![image](https://github.com/Akshay-Gk/Terraform-remote-backend-s3/assets/112197849/b55aadc4-5cf7-4aa6-aee8-f0bcef14c50b)

* Attach ***"terraform-backend-policy" and "AmazonEC2FullAccess"***

![image](https://github.com/Akshay-Gk/Terraform-remote-backend-s3/assets/112197849/eb049b17-ae8b-4faa-8c8a-2d70dd5298f3)

> **We can see created "terraform-s3" role**

![image](https://github.com/Akshay-Gk/Terraform-remote-backend-s3/assets/112197849/198df694-4d70-4c85-af43-ae76334d4c96)



## Step 4: Attach role to an ec2 instance

* Action > security > Modify IAM role

![image](https://github.com/Akshay-Gk/Terraform-remote-backend-s3/assets/112197849/20e343d4-1981-4bde-b7c4-96cde17bf653)


* Attach "terraform-s3" role


![image](https://github.com/Akshay-Gk/Terraform-remote-backend-s3/assets/112197849/42a93345-4cd4-4b5e-bd21-a60d405e4fb6)


## Step 4: Install terraform in ec2

```
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
sudo yum -y install terraform
```


## Step 5: Create terrafom files in ec2

> `Note : The terrafom files must be created with .tf extension`



### Create a provider file

> `Note: Terraform relies on plugins called "providers" to interact with remote systems. Terraform configurations must declare which providers they require, so that Terraform can install and use them. I'm using AWS as provider`

```
provider "aws" {

region = "ap-south-1"

}

terraform {

  backend "s3" {
    bucket = "terraform.akshay.com"
    key    = "terraform.tfstate"
    region = "ap-south-1"
  }
}
```
> `Note: Give desired AZ`

### Terraform initialize

 *The terraform init command initializes a working directory containing Terraform configuration files*

> `terraform init`

* 


# Conclusion

Here is a simple document on how to create AWS S3-bucket as the remote backend for Terraform
