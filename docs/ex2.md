Exercise 2- Create New Organisation on HCP Terraform
====================================================

At the start, it's important to set up the Terraform Cloud organization and workspace. This prepares you to utilize Terraform Cloud for deploying the initial BIG-IP configuration on AWS at a later stage.
The organization name will follow a format like "STUDENT-XXXX," where "XXXX" represents a unique prefix assigned to each student.
Additionally, a workspace will be generated with a name patterned as "workspace-XXXX," where "XXXX" corresponds to the unique prefix assigned to each student.

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

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/16827506-d0cf-4359-8b6b-904ce12b2559)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/f286b7e4-f525-4716-a10d-9d6143f47660)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/67329155-2d4c-448e-94b4-f079f04f6212)



![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/1ab39f9e-dfc7-402f-a244-220c728cea75)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/a6758fec-8d4a-48c0-8279-2dc1a4ce83a0)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/fb44b33a-959e-4e0b-a9e0-6085c6721df3)


