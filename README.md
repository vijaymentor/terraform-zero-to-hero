# terraform-zero-to-hero

# terraform training

- terraform uses plugin based architecture with 100's of major platforms.publicly available in terrform.registry.io

- Official providers
- Verified providers - big-ip
- Community providers - 

# install terraform in windows

# create a directory called terraform

# create directory called hello-world



# understand the terraform

cd hello-world

vim main.tf

variable "myvar" {
   type = "string"
   default = "hello terraform"
   }
   
PS C:\terraform> cd .\hello-world\
PS C:\terraform\hello-world> terraform version
Terraform v1.3.4
on windows_amd64
PS C:\terraform\hello-world> terraform console
> var.myvar
"hello terraform"
> "${var.myvar}"
"hello terraform"


exit the console

> exit

#why terraform

resource "aws_instance" "webserver" {  
ami	= "ami-0edab43b6fa892279" 
instance_type = "t2.micro"
}

resource "aws_s3_bucket" "finance" { 
bucket = "finanace-cnl-1980" 
tags	= {
Description = "learn.share to your friends.Save Children"
}
}


resource "aws_iam_user" "admin-user" { 
name = "lucy"
tags = {
Description = "Team Leader"
}
}

# what is terraform.tfstate ?

# ensure you have the latest terraform version

terraform version


# what is resource ?

# HCL Basics ?

- HCL Syntax 

- create local directory [aim is to create local file]

mkdir terraform-local-file
cd terraform-local-file

resource "local_file" "pet" { 
filename = "pets.txt" 
content = "We love pets!"
}



# simple terrafdorm consists of 4 steps 

- write the configuration file
- run the terraform init command
- review the execution plan using terraform plan command
- once ready apply the terraform apply command

- terraform init - this command will check the configuration file and initialize the working directory contaiing the .tf file.This will understand that we are trying to use local providers in the resource block and download the necessary plugins.Then it will be start execute what mentioned in the .tf file.

- terraform plan - this command will show the actions that will be carried out by the terraform to create the resource.

- terraform apply  - this command will show the execution plan once again and ask user to confirm before provisioning.

- terraform show - this command will inspect the state file and show the resource details.



# HCL Basics

- provider --> resource_type --> Arguments
- local_provider --> local_file --> filename

# Excercise 1

resource "local_file" "games" {
  file    = "favorite-games"
  content = "FIFA 21"
}


# excercise 2

resource "local_file" "games" {
  filename     = "/root/favorite-games"
  content  = "FIFA 21"
  sensitive_content = "FIFA 21"
}


- Notice that the content of the file was not displayed when using sensitive_content instead of the content argument.
- Note: This argument is specific to the local_file resource we are using. Refer to the documentation to see all the arguments supported by this resource.

- Also note that as Terraform follows an immutable infrastructure approach, the file was recreated although the contents are the same.




# terraform basics

terraform init

- it will download local provider plugins and will initiatilse the parameters.

```
PS C:\terraform\test-hcl> terraform init    

Initializing the backend...

Initializing provider plugins...
- Reusing previous version of hashicorp/local from the dependency lock file
- Using previously-installed hashicorp/local v2.2.3

```

- the plugins will be download into .terraform directory .the directory called plugins.



$ ls /root/terraform-local-file/.terraform 
plugins


# put as many functions in one main.tf file rather multiple .tf files


vim main.tf


resource "local_file" "pet" { 

filename = "pets.txt" 
content = "We love pets!"
}

resource "local_file"	"cat" { 
filename = "cat.txt"
content = "My favorite pet is Mr. Whiskers"

}


# what are the others files can be created.

- main.tf	- Main configuration file containing resource definition
- variables.tf - Contains variable declarations
- outputs.tf - Contains outputs from resources
- provider.tf - Contains Provider definition

# how many resources used in this configuration file

cat main.tf 
resource "local_file" "things-to-do" {
  filename     = "/root/things-to-do.txt"
  content  = "Clean my room before Christmas\nComplete the CKA Certification!"
}
resource "local_file" "more-things-to-do" {
  filename     = "/root/more-things-to-do.txt"
  content  = "Learn how to play Astronomia on the guitar!"
  
  
Now, navigate to the directory /root/terraform-projects/provider-a. We have downloaded a plugin in this directory. Identify the name and type of provider.


If the configuration files in this directory seem unfamiliar, do not worry, these are covered later in the course.

# multiple providers

# local provider and random provider

```
resource "local_file" "pet" { 
filename = "/root/pets.txt" 
content = "We love pets!"
}
```

# random provider

resource "local_file" "my-pet" {
  filename = "/root/pet-name"
  content = "My pet is called finnegan!!"
}

resource "random_pet" "other-pet" {
  prefix = "Mr"
  separator = "."
  length = "1"
}

# using input variables

- **without varaiables**
resource "local_file" "pet" { 
filename = "/root/pets.txt" 
content = "We love pets!"

}

resource "random_pet" "my-pet" { 
prefix = "Mrs"
separator = "." 
length = "1"
}

- **with variables**

vim main.tf

resource "local_file" "pet" { 
filename = var.filename 
content = var.content

}

resource "random_pet" "my-pet" { 
prefix = var.prefix
separator = var.separator 
length = var.length
}

vim variable.tf

variable "filename" {
default = "/root/pets.txt"
}
variable "content" {
default = "We love pets!"
}
variable "prefix" {
default = "Mrs"
}
variable "separator" {
default = "."
}
variable "length" {
default = "1"
}


# understanding variable block

vim variables.tf

variable "filename" { 
default = "/root/pets.txt" 
type = string
description = "the path of local file"

}
variable "content" { 
default = "I love pets!"
type = string
description = “the content of the file"

}
variable "prefix" { 
default = "Mrs"
type = string
description = "the prefix to be set"

}
variable "separator" { default = "."

# the basic variables which is acceptable in terraform is ?


Type | Example
:------|:------
Type | Example
string | "/root/pets.txt"
number | 1
bool | true/false
any | Default Value
list | ["cat", "dog"]
map | pet1 = cat pet2 = dog
object | Complex Data Structure
tuple | Complex Data Structure

# list

vim variables.tf

variable "prefix" {
default = ["Mr", "Mrs", "Sir"] 
type = list	0	1	2
}


vim main.tf

resource "random_pet" "my-pet" {
prefix	= var.prefix[0]
}

Index | Value
:------|:------
0 | Mr
1 | Mrs
2 | Sir


# Map

vim variables.tf

variable file-content {
  type = map
  default = {
      "statement1" = "we love pets1!"
	  "statement2" = "we love animals!"
	  }
	 }

vim main.tf

resource local_file my-pet { 
  filename	= "/root/pets.txt"
  content = var.file-content["statement2"]
 }
 
 
# set

vim variables.tf

- **right**

variable "prefix" {
default = ["Mr", "Mrs", "Sir"] 
type = set(string)
}


- **right** **duplicate values**

variable "prefix" {
default = ["Mr", "Sir", "Sir"] 
type = set(string)
}


- **right**

variable "prefix" {
default = ["apple", "banana"] 
type = set(string)
}


- **right** **duplicate values**

variable "prefix" {
default = ["apple", "banana" , "banana"] 
type = set(string)
}

- there shjouldnt be any duplicate values here


# objects 

Index | Value
:------|:------
0 | Mr
1 | Mrs
2 | Sir


Key | Example | Type
:------|:------|:------
name | bella | string
color | brown | string
age | 7 | number
food | ["fish", "chicken", "turkey"] | list
favorite_pet | TRUE | bool

vim variables.tf


variable	"bella"	{ 
type = object({
   name = string 
   color = string 
   age = number
   food = list(string) 
   favorite_pet = bool
})

default = {
  name = "bella" 
  color = "brown" 
  age = 7
  food = ["fish", "chicken", "turkey"] 
  favorite_pet = true
}
}


