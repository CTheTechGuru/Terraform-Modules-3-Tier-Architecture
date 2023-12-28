

<h2 align="center"> 💻 🚀 :cloud: AWS | Terraform Module 3 Tier Architecture Project 🚀 :cloud: 💻</h6>

![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/8.jpg)





<!-- PROJECT Details-->
# About The Project
We will use terraform modules to provision an AWS account VPC with all of its networking components via code. The beauty of terraform modules is that we are able to create the aws resources on demand via code.  We will use Studio visual code as our IDE. The prerquisiates are imperative as far as following along to execute an error free lab. 


#### Overview:



## Prerequisites


* Basic understanding of AWS Services.
* Experience with Studio Visual Code.
* Download aws configure for your current OS. 
* Access to AWS (Free Tier or Paid)
* _IMPORTANT_ Create AWS user account named terraform save secret / access keys administrator access.(enable MFA on lab account)
* Dedicated terminal basic commands.
* Download reference files. ![VPC Reference File](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/vpc_reference.tf) ![S3 Backend File](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/s3_backend_reference.tf)





## 1. Setup AWS configure

* aws configure allows us to hide our credentials on our local machine without needing to use within our code.
  
* In a terminal enter ``` aws configure ``` into the shell.

You will need to enter the following 
```
AWS Access Key ID
AWS Secret Access Key
Default region name
Default output format
```

Your output should look similar to this. 

![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/aws%20configure.png)













## 2. Terraform Directory/File Configuration

* Create a folder for your terraform project.
* From studio visual code choose file open folder. 
* Within the 'terraform project folder' create a folder name modules.
* Inside the modules folder create a folder named vpc.
* Inside the vpc folder create 3 files.

 
![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/Modules%20Snapshot.PNG)

 1. main.tf - Contains code.
 2. outputs.tf - Exports value to reference in another module.
 3. variables.tf - Contains variables.














## 3. Create VPC variables in the variables.tf file under modules/vpc.

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
  

  

## 4. Create reference variables

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

4. Create Route Table & Add Public Route

Add the vpc_id - aws_vpc.vpc.id
For our cidr_block we want all access so we will enter "0.0.0.0/0"
For the gateway id use the resource and name from our igw "aws_internet_gateway" "internet_gateway"


``` aws_internet_gateway.internet_gateway.id```

tag name will be "public route table"



![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/Public%20Route.PNG)

5. Public Subnet Associations AZ1/AZ2
We will use the resource type and name for the subnet id's to public subnet 1 and 2. ``` resource "aws_subnet" "public_subnet_az1" ``` aws_subnet.public_subnet_az1.id
We will also enter the resource name and type id for the route table id. ``` "aws_route_table" "public_route_table" ``` aws_route_table.public_route_table.id

_Repeat for AZ2_

Code should look similar to this.



![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/Public%20Route%20association.PNG)

6. Private App/Data Subnets
Our private subnets will have the same as our public subnets except map on launch due to it being a private subnet we do not need on launch.
The following arguments are what correspond to each subnet. Ensure to use the correct index for the AZs
```
vpc_id = aws_vpc.vpc.id
cidr_block = var.private_app_subnet_az1_cidr
avaliability_zone = data.aws_availability_zones.avaliable_zones.names[0]
map_public_ip_on_launch = false

tags Name = "private app subnet az1"

vpc_id = aws_vpc.vpc.id
cidr_block = var.private_app_subnet_az2_cidr
avaliability_zone = data.aws_availability_zones.avaliable_zones.names[1]
map_public_ip_on_launch = false

tags Name = "private app subnet az2"

vpc_id = aws_vpc.vpc.id
cidr_block = var.private_data_subnet_az1_cidr
avaliability_zone = data.aws_availability_zones.avaliable_zones.names[0]
map_public_ip_on_launch = false

tags Name = "private data subnet az1"

vpc_id = aws_vpc.vpc.id
cidr_block = var.private_data_subnet_az2_cidr
avaliability_zone = data.aws_availability_zones.avaliable_zones.names[1]
map_public_ip_on_launch = false

tags Name = "private data subnet az2"
```

![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/Private%20App%20subnet.PNG)

![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/Private%20data%20Subnet.PNG)

## 5. Create Terraform outputs for our VPC

* Select the outputs.tf folder.
* We will enter our outputs for each of our variables and associate the value for the output file.

![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/assets/125163096/ab6d30e3-1c03-4667-a500-9b0a73247c21)




 
## 6. Create Project Folder 

* Create New Folder outside of the module folder. We will reference the module/vpc folder for our project.
  
* My folder name is TF Project, in there I will create backend.tf main.tf terraform.tfvars and variables.tf files, we will use these files to create our project. 

 ![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/Project%20Folder.png) 
 
 _Disregard the .terraform hidden file and the lockfile, we will create in the next step._





## 7. Create S3 bucket to store terraform state file


* Open AWS Management console and go to Amazon S3 Service.
  
  ![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/S3%20Service.PNG)

* Click create bucket.

  
* Create unique name for your bucket. Choose the region for your bucket, and select Enable versioning. 

  ![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/S3%20Bucket%20Options.PNG)

* Now that our bucket is created we will create our configuration backend that will store our state file in our bucket.

* Copy and paste the code from ![S3 Backend File](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/s3_backend_reference.tf) to your backend.tf file.
  
* For the bucket variable we will enter the name we created for our S3 bucket created.
* For the key name it the same as the project.
* For region enter your region, mines is us-east-1.
* For the profile enter the AWS user which you have configured in your AWS console with the Secret/Access Keys.

 ![S3 State File Example](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/S3%20State%20file.png?raw=true)
 
* Save file.


## 8. We will now create the variables for our project folder. 
 
* Copy all the variables from your modules/vpc folder into your project variables.tf file. 


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

* Save file.



## 9. Authenticate with provider and reference VPC Module in main.tf file

* The purpopse of the main.tf file is to reference the vpc module folder and authenticate to our provider. 


* First we will authenticate provider enter the variable for region from your variables.tf file and enter the profile name.
  _Open the variables.tf file in a side panel to reference as we enter the variables and their arguments in the main.tf file._

configure aws provider - Enter your profile name in for the profile argument. 
```
provider "aws" {
 region  = var.region
 profile = "Terraform"
}

```
 

* Next under the configure provider syntax we will create our vpc. _for source enther the path of the modules/vpc folder._
* List all the variables in the variable file and enter the value for the arguments which would be var.(the name of the varable)


``` Example - region = var.region ```


Create vpc

```
module "vpc" {
  source                       = "../modules/vpc"
  region                       = var.region
  project_name                 = var.project_name
  vpc_cidr                     = var.vpc_cidr
  public_subnet_az1_cidr       = var.public_subnet_az1_cidr
  public_subnet_az2_cidr       = var.public_subnet_az2_cidr
  private_app_subnet_az1_cidr  = var.private_app_subnet_az1_cidr
  private_app_subnet_az2_cidr  = var.private_app_subnet_az2_cidr
  private_data_subnet_az1_cidr = var.private_data_subnet_az1_cidr
  private_data_subnet_az2_cidr = var.private_data_subnet_az2_cidr
}

```
* Review make sure everything entered correctly. Save File. 

  
 
## 10. Open the terraform.tfvars file, This is where we enter the values for our variables.


* Enter all the variables from the variables.tf file to the tfvars file.

* Make sure theres no space after the equal sign.

* Your code should look like the code below.
  
![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/TFvars.png)



 
## 10. Create the VPC in our AWS environment

* Right click the project folder in studio visual code and choose open in integrated termnal. 
* Check to ensure the path is your project folder. 
* Now we will initialize our aws environment.
 
* enter ``` terraform init ``` into the shell.
  
* your output should look like this.
  
 ![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/Terraform%20init.png)

* Now to create the vpc enter ``` terraform apply ```

_This command will expose any errors within your files so if you recieve an error go to the appropriate lines and modify any changes_

 Your output should look similar to below. 

 ![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/Terraform%20apply.png)

## 11. Verify Infrastructure 

* Log into the aws console and go to the VPC dashboard.

* All of your subnets should appear similar to below.

  ![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/VPC%20Subnets.PNG)

## 12. Clean-up AWS environment

* Now go back studio visual code and enter terraform destroy in the cli.
* This will remove all the vpc components and revert the aws environment to the previous state before our IAC provisioning.

![](https://github.com/CTheTechGuru/Terraform-Modules-3-Tier-Architecture/blob/main/Images/destroy.png)


<h1 align="center">Summary</h3>

Terraform 3 Tier AWS VPC Architecture: Modular, Secure, and Scalable Infrastructure

In our recent VPC project, we harnessed the power of Terraform to streamline and automate the deployment of a modular VPC architecture. Organizing our code into a Terraform module within the modules/vpc folder, we encapsulated key VPC components for enhanced reusability and maintenance. Leveraging variables and an AWS provider setup in the main.tf file, we orchestrated the creation of the VPC. To maintain state consistency, we opted for Amazon S3 to host our Terraform state file. Enhancing security, we created a dedicated user for the Terraform project, authenticating via access and secret keys. The project's variable input configuration was neatly organized in a tvars file for easy management. This approach not only provided scalability and automation but also established a secure and well-documented foundation for our cloud infrastructure. 

🚀💻 #Terraform #VPCDeployment #CloudAutomation #InfrastructureAsCode






<!-- CONTACT -->
## Contact

Cordelra Lowman - Cordelra_Lowman@yahoo.com

<h3 align="left">Follow me on Linkedin:</h3>
<p align="left">
<a href="https://linkedin.com/in/cordelra lowman" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/linked-in-alt.svg" alt="cordelra lowman" height="30" width="40" /></a>
</p>






