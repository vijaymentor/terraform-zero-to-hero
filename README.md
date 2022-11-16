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


# tuples

- tuples is similer to a list.
- list uses elements of the same varaiable types.such as string or number.
- Tuples uses elements of various variable types.The type of variable which used in tuples are mentioned into squre brackets.



- **right**

variable kitty {
  type = tuple([string,number,bool])
  default = ["cat", 7, true]
}


- **right** **wrong  values** **need to be sure correct values**
```
variable kitty {
  type = tuple([string,number,bool])
  default = ["cat", 7, "dog"]
}
```

- this above will fail when you execute terraform plan command.coz tuple type expect string,number and boolean.

# quiz 1

Which one of the below is not a valid data type in terraform?



- set
- list
- map
- tuple


**array**

# quiz 2

Navigate to the directory /root/terraform-projects/variables. Which type does the variable called number belong to?

```
variable "name" {
     type = string
     default = "Mark"
  
}
variable "number" {
     type = bool
     default = true
  
}
variable "distance" {
     type = number
     default = 5
  
}
variable "jedi" {
     type = map
     default = {
     filename = "/root/first-jedi"
     content = "phanius"
     }
  
}

variable "gender" {
     type = list(string)
     default = ["Male", "Female"]
}
variable "hard_drive" {
     type = map
     default = {
          slow = "HHD"
          fast = "SSD"
     }
}
variable "users" {
     type = set(string)
     default = ["tom", "jerry", "pluto", "daffy", "donald", "jerry", "chip", "dale"]

  
}

  




```


- list
- string
- number
- **bool**

# quiz 3

How would you fetch the value of the key called slow from the variable called hard_drive in a terraform configuration?


This variable is defined in the file variables.tf.

```
variable "hard_drive" {
     type = map
     default = {
          slow = "HHD"
          fast = "SSD"
     }
```

- var.hard_drive.slow
- **var.hard_drive["slow"]**
- hard_drive["slow"]
- var.hard_drive.0
- var.hard_drive[0]


# quiz 4

What is the index of the element called Female in the variable called gender?

```
variable "gender" {
     type = list(string)
     default = ["Male", "Female"]
```

- **1**
- var.gender["female"]
- 0
- Female
- 3

# quiz 5

What is the type of variable called users?

```
variable "users" {
     type = set(string)
     default = ["tom", "jerry", "pluto", "daffy", "donald", "jerry", "chip", "dale"]

  
}

```

- list
- list(string)
- set
- **set(string)**

# quiz 6

However, this variable has been defined incorrectly! Identify the mistake.

```
variable "users" {
     type = set(string)
     default = ["tom", "jerry", "pluto", "daffy", "donald", "jerry", "chip", "dale"]

  
}
```


- **duplicate elements (jerry is mentioned 2 times)**
- elements should not be enclosed in double quotes
- syntax error
- type used is incorrect


# quiz 7

We have now updated the main.tf file in the same directory (/root/terraform-projects/variables) and added some resource blocks.
Inspect them.


```
main.tf

resource "local_file" "jedi" {
     filename = "/root/first-jedi"
     content = "phanius"
}

```

# quiz 8

What is the value for the argument called content used in the resource block for the resource jedi?

```
resource "local_file" "jedi" {
     filename = "/root/first-jedi"
     content = "phanius"
}
```


- **phanius**
- jedi
- yoda
- obi-wan
- first-jedi

# using variables in terraform


- ** varaiable approach 1**

```
main.tf

resource "local_file" "pet" { 
filename = var.filename 
content = var.content

}

resource "random_pet" "my-pet" { 
prefix = var.prefix
separator = var.separator 
length = var.length
}
```

```
variable.tf

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
default = 2
}

```

- ** varaiable approach 2**

```
main.tf

resource "local_file" "pet" { 
filename = var.filename 
content = var.content

}

resource "random_pet" "my-pet" { 
prefix = var.prefix
separator = var.separator 
length = var.length
}

```

```
variable.tf

variable "filename" {

}
variable "content" {

}
variable "prefix" {

}
variable "separator" {

}
variable "length" {

}

```

- **in this approach 2 whne you run terraform apply then you need to enter the values in a interactive mode.**

- if you dont want to enter each value every time..then you pass all the values like this below while running terraform apply command itself.

```
terraform apply -var "filename=/root/pets.txt" -var "content=We lovePets!"	-var "prefix=Mrs" -var "separator=." -var "length=2"
```

- ** variable approach 3**
```
export TF_VAR_filename="/root/pets.txt"
export TF_VAR_content="We love pets!"
export TF_VAR_prefix="Mrs"
export TF_VAR_separator="."
export TF_VAR_length="2"
```


```
terraform apply
```
- ** variable approach 4** **if we have lot of variables and values**

```
terraform.tfvars

filename = "/root/pets.txt" 
content = "We love pets!" 
prefix = "Mrs"
separator = "." 
length = "2"

```

```
terraform apply
```

- ** this file name can be terraform.tfvars or terraform.tfvars.json **

- **if the file name is *.auto.tfvars  or  *.auto.tfvars.json means values will be automatically loaded**

- if the file name is varaiables.tfvars then we need to run like this below

```
terraform apply -var-file variable.tfvars
```

# variable precedence [ which variable will take first priority]

```
main.tf

resource local_file pet { 
  filename = var.filename
}
```
```
variables.tf
variable filename { 
  type	= string
}
```

- ** for the above we can pass value in 4 ways**

- ** variable approach 1 called environment variables**
```
export TF_VAR_filename="/root/cats.txt" 
```
- ** variable approach 2 called terraform.tfvars**
```
terraform.tfvars
filename = "/root/pets.txt"
```

- ** variable approach 3 called auto loading auto.tfvars **
```
variable.auto.tfvars
filename = "/root/mypet.txt"
```


- ** variable approach 4 - called command line arguments**
```
terraform apply -var "filename=/root/best-pet.txt" ?

```

- **in the above 4 approach what order the terraform can give priority.


Order | Option | Description
:------|:------|:------
1 | bella | string
1 | Environment Variables | first terraform will check this
2 | terraform.tfvars | if environment variable is not available then terraform will check this
3 | *.auto.tfvars (alphabetical order) | if both are not available then terraform will check this
4 | -var or –var-file (command-line flags) | if above 3 are not available then terraform will check this
