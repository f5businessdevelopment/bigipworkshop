Exercise 2- Create a New Organisation and Workspace on HCP Terraform
====================================================================

## Access the lab documentation 

You can open a new tab on the jump box browser and click https://github.com/f5businessdevelopment/bigipworkshop
to access documentation

To access Terraform Cloud, go to [https://app.terraform.io](https://app.terraform.io) from your Jump Box. Your Jump Box is set up to authenticate with HCP Terraform. If you encounter any access issues, log in using your HCP account. The account and password are saved in the browser, so you just need to click "Login using HCP account" without entering credentials.

If you are not authenticated with https://app.terraform.io click on __Continue with HCP account__ as shown below

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/52d96284-c5b3-44a4-8316-1dd881ced43f)

select the user ID as shown below ( __Note__: PLEASE DON'T CREATE NEW ID JUST USE SHOWN BELOW)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/822545eb-b2ed-481a-81be-5e311244e34d)

Click continue and use the password stored in the browser to login

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/ca7a5c23-789c-432a-a83f-e610a883b804)


At the start, it's important to set up the Terraform Cloud organization and workspace. This prepares you to utilize Terraform Cloud for deploying the initial BIG-IP configuration on AWS at a later stage.
The organization name will follow a format like __STUDENT-XXX__  where "__XXXX__" represents a unique prefix assigned to each student.
Additionally, a workspace will be generated with a name patterned as "__workspace-XXXX__," where "__XXXX__" corresponds to the unique prefix assigned to each student.

"Let's take a look at the existing `tfe.tf` file in your directory."

```
# Declare required provider
terraform {
  required_providers {
    tfe = {
      version = "~> 0.54.0"
    }
  }
}

# Define local variables
locals {
  # Generate a random prefix
  prefix = random_string.pre.result
}

# Generate a random string
resource "random_string" "pre" {
  length  = 4
  special = false
}

# Configure Terraform Cloud provider
provider "tfe" {
  hostname = "app.terraform.io"
}

# Create an organization in Terraform Cloud
resource "tfe_organization" "Org" {
  name  = "STUDENT-${local.prefix}"
  email = "s.shitole@f5.com"
  # Additional organization settings...
}

# Create a workspace in Terraform Cloud
resource "tfe_workspace" "myworkspace" {
  name         = "workspace-${local.prefix}"
  organization = tfe_organization.Org.name
  tag_names    = ["workshop"]
}

# Define outputs
output "Your_Workshop_HCP_Org" {
  value = "STUDENT-${local.prefix}"
}

output "Your_Workspace_configured" {
  value = "workspace-${local.prefix}"
}

output "prefix" {
  value = random_string.pre.result
}

# Generate Terraform Cloud variables file
data "template_file" "tfcvar" {
  template = file("day0/tfcvariables.tf.example")
  vars = {
    org    = "STUDENT-${local.prefix}"
    works  = "workspace-${local.prefix}"
    prefix = "${local.prefix}"
  }
}

resource "local_file" "tfcvar" {
  content  = data.template_file.tfcvar.rendered
  filename = "day0/tfcvariables.tf"
}

# Generate cloud configuration file
data "template_file" "cloud" {
  template = file("day0/cloud.tf.example")
  vars = {
    org   = "STUDENT-${local.prefix}"
    works = "workspace-${local.prefix}"
  }
}

resource "local_file" "cloud" {
  content  = data.template_file.cloud.rendered
  filename = "day0/cloud.tf"
}

```

This Terraform configuration sets up Terraform Cloud organization and workspace, utilizing random strings for organization and workspace names. It also generates files (tfcvariables.tf and cloud.tf) based on templates provided in the day0 directory.


Let's run terraform init. This will ensure that all the necessary Terraform dependencies for Terraform resources are downloaded.

```
terraform init
```

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/16827506-d0cf-4359-8b6b-904ce12b2559)

```
terraform plan
```

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/f286b7e4-f525-4716-a10d-9d6143f47660)

```
terraform apply -auto-approve
```
This action will generate a distinctive organizational name and workspace for you within HCP Terraform.


![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/67329155-2d4c-448e-94b4-f079f04f6212)


Navigate to the HCP Terraform app at https://app.terraform.io in your browser. Click on the Terraform icon located in the upper left corner, then proceed to the 'Organizations' section. Select your correct Org name, typically labeled as __STUDENT-XXX__. Ensure that you choose your specific Org name and check for the appropriate __PREFIX__ following the '__STUDENT__' word. The __PREFIX__ should be provided by the terraform apply command you executed in the previous step. Avoid selecting any other Org

__Note__ Make sure to refresh the browser to see you Tenant Prefix

## Please check your PREFIX following STUDENT-XXXX and restrict your access to your Organization on Terraform Cloud.

to check you PREFIX and ORG  use the below code

```
cd ~/BIG-IP-Configs-to-AS3-with-Terraform
terraform output
```

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/1ab39f9e-dfc7-402f-a244-220c728cea75)

You'll also notice that your __workspace-XXXX__ has been created correctly. Feel free to review the details.


![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/a6758fec-8d4a-48c0-8279-2dc1a4ce83a0)
You can explore the workspace and learn how to initialize it. Take note of the example code provided, which references your organization name and corresponding workspace. We'll utilize this in all our workspaces when managing the BIG-IP platform for __Day0__, __Day1__, and __AS3__ deployment. However, dedicated workspaces will be created for __Day1__ and __AS3__ tasks.


![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/fb44b33a-959e-4e0b-a9e0-6085c6721df3)


[GoTo Next Exercise3 Day 0 BIG-IP Deployment on AWS](ex3.md)

[GoBack](ex1.md)
