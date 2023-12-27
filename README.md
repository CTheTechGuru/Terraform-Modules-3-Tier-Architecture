 Terraform-Module-3-Tier-Architecture

<h1 align="center">AWS | Terraform Module 3 Tier Architecture Project</h3>

![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/8.jpg)





<!-- PROJECT Details-->
# About The Project
We will use terraform modules to provision an AWS account VPC with all of its networking components via code. The beauty of terraform modules is that we are able to use modules to create the same resources on demand. 
We will use Studio visual code as our IDE to write the code 


#### Overview:



## Prerequisites


* Basic understanding of AWS Services.
* Experience with Studio Visual Code.
* Access to AWS (Free Tier or Paid)
* Create AWS user account named terraform save secret / access keys administrator access.(enable MFA on lab account)
* Dedicated terminal basic commands.
* Download reference files. ![VPC Reference File](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/vpc_reference.tf) ![S3 Backend File](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/s3_backend_reference.tf)



### 1. Terraform Directory/File Configuration

* Create a folder for your terraform project.
* From studio visual code choose file open folder. 
* Within the 'terraform project folder' create a folder name modules.
* Inside the modules folder create a folder named vpc.
* Inside the vpc folder create 3 files.

 
![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/Modules%20Snapshot.PNG)

 1. main.tf - Contains code.
 2. outputs.tf - Exports value to reference in another module.
 3. variables.tf - Contains variables.














### 2. Create VPC variables in the variables.tf file under modules/vpc.

* In order for us to specify values and arguments, we need to create variables.
* Copy the following code and paste into the variables.tf file and save.

```
variable "region" {}
variable "project_name" {}
variable "vpc_cidr" {}
variable "public_subnet_az1_cidr" {}
variable "public_subnet_az2_cidr" {}
variable "private_app_subnet_az1_cidr" {}
variable "private_app_subnet_az2_cidr" {}
variable "private_data_subnet_az1_cidr" {}
variable "private_data_subnet_az2_cidr" {}
```
  

  

### 3. Create reference variables

* To reference our variables we will type var. then the name of the variable in quotations in our variables.tf file.

Example -

```
var.region
var.project_name
var.vpc_cidr
```


1. Create VPC

For the cidr_block argument enter the variable created for the cidr. 
To call the variable again we will put var.vpc_cidr
For instance_tenancy we will put "default" for value.
To enable dns hostname enter true.

For the tags name value enter var.project_name in the brackets to reference our project name. Your code should look like this. 

 ![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/Create%20VPC.PNG)

2. Create IGW

The first argument to enter will be our vpc id.
The value is our resource type and name of our vpc. Which is "aws_vpc" "vpc".
We will remove the double quotes and add .id at the end. 
under tags enter var.project_name
Your code should look like this. 

 ![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/Create%20IGW.PNG)

3. Create Public Subnets AZ1/AZ2

We will enter our vpc_id again like previously, aws_vpc.vpc.id

For the availability zone argument we will use the resource and name from data "availibility_zones" avaliable_zones"

Enter - data.aws_availability_zones.availibility_zones.name[0]

```this index references the first AZ due to indexing 1 is the second, so forth so on. ```

For cidr_block enter the variable created for public subnet az1 cidr. 

```var.public_subnet_az1_cidr```

map_public_ip_on_launch = true

For our tag name we will give it "public subnet az1"

_REPEAT FOR PUBLIC SUBNET 2 WITH THE CORRESPONDING ARGUMENTS. INDEX 1 FOR THE SECOND AZ._

Your code should look like this.

 ![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/Public%20Subnets.PNG)

5. Create Route Table

![](  

5. Public Subnet Association

![](

6. Private Subnets

![](

## 4.

*
*
*
*
*


 
## 5.

*
*
*
*
*
*




## 6.  

*
*
*
*
*
*
*


## 7. 
 
*
*
*
*


## 8. 

*
*
*
*
*


  
 
## 9. 

*
*
*
*
*




 
## 10.

*
*
*
*
*
*


<h1 align="center">Summary</h3>







<!-- CONTACT -->
## Contact

Cordelra Lowman - Cordelra_Lowman@yahoo.com

<h3 align="left">Follow me on Linkedin:</h3>
<p align="left">
<a href="https://linkedin.com/in/cordelra lowman" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/linked-in-alt.svg" alt="cordelra lowman" height="30" width="40" /></a>
</p>






