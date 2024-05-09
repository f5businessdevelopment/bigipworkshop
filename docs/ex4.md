Exercise 4 - Day 1 Creating BIG-IP objects using TFC and BIG-IP terraform resources
===================================================================================

For Day1, our focus shifts to deploying the BIG-IP application objects using imperative Terraform resources. The main.tf file contains configuration details where we set up monitors, nodes, pools, and virtual servers using imperative BIG-IP Terraform resources, as illustrated below.

```
 # Terraform Configuration for Deploying BIG-IP Application Objects

## Cloud Configuration

cloud {
  organization = "<Your Organisation STUDENT-XXXX"

  workspaces {
    name = "day1"
  }
}
   
```
Specifies the cloud settings for the Terraform configuration, including the organization and workspace name.

### Required Providers
```
required_providers {
  bigip = {
    source  = "F5Networks/bigip"
    version = "1.22.0"
  }
}

```
Defines the required provider for interacting with the BIG-IP platform.

### Provider Configuration

```
provider "bigip" {
  address  = "https://${var.address}:${var.port}"
  username = var.username
  password = var.password
}

```
Configures the BIG-IP provider with the necessary connection details such as address, username, and password.

### Resource Definitions

```
resource "bigip_ltm_monitor" "vs_tc1" {
  name     = "/Common/test_monitor_vs_tc1"
  parent   = "/Common/http"
  send     = "GET /some/path\r\n"
  timeout  = "999"
  interval = "990"
}

resource "bigip_ltm_pool" "vs_tc1" {
  name                = "/Common/test_pool_vs_tc1"
  load_balancing_mode = "round-robin"
  monitors            = [bigip_ltm_monitor.vs_tc1.name]
  allow_snat          = "yes"
  allow_nat           = "yes"
}

resource "bigip_ltm_pool_attachment" "vs_tc1" {
  pool = bigip_ltm_pool.vs_tc1.name
  node = "10.0.0.171:80"
}

resource "bigip_ltm_pool_attachment" "vs_tc2" {
  pool = bigip_ltm_pool.vs_tc1.name
  node = "10.0.0.172:80"
}

resource "bigip_ltm_virtual_server" "vs_tc1" {
  pool                       = bigip_ltm_pool.vs_tc1.name
  name                       = "/Common/test_vs_tc1"
  destination                = "10.0.0.200"
  port                       = 8080
  source_address_translation = "automap"
}

```

Defines Terraform resources for configuring BIG-IP application objects such as monitors, pools, pool attachments, and virtual servers.

You can access the BIG-IP credentials by navigating to the app.terraform.io, then to your workspace. Click on "Outputs" to retrieve the credentials required for configuring the BIG-IP Terraform Variables Set.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/b8f3e4d9-e0ac-4a50-9b97-21a4e422b001)

---
Open app.terraform.io in a new tab, then navigate to your Organization and workspace. Click on "Settings."

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/5afd1a83-087a-4890-8e32-4f7ced33f9f4)

click on VariableSets we are adding the BIG-IP Env variables for terraform

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/968dfb7e-a697-4741-901c-ddcffce71634)


![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/1c25dc14-5c20-4295-b256-b56fe1df9c24)

You should see the BIG-IP Variables Set displayed as illustrated below. Ensure that you have selected "Terraform variables."

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/2aedc361-3214-44cb-a42e-69ad0d8aaaf3)

Before running the `terraform init/plan/apply` commands, ensure you update the Organization in the `main.tf` file with the correct prefix.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/96de93ed-f6d4-48f0-9bcd-6871491418f9)

Additionally, from your RDP session browser, navigate to the AWS console and update the ingress Security Group for the F5 instance "studentXXX-f5 instance" as depicted. Enable TCP port 8443 for 0.0.0.0/0.
This step is necessary to access the BIG-IP from the HCP Terraform Cloud.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/c1b63619-b8fa-475d-a079-f771f79db62b)

Next, execute the command `terraform init` to initialize the Terraform configuration.

Next, execute the command `terraform plan` to initialize the Terraform configuration.

Next, execute the command `terraform apply -auto-approve` to initialize the Terraform configuration.
![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/7cc0aca6-a6cf-49f5-9e6c-f32d1406977d)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/27332858-b937-4d97-ad2f-f3c724423942)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/4bddc4ea-01e4-49e3-8fb5-cf2fe0310293)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/5b36a47c-1427-4d6e-9879-bbec6d802ef2)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/3fb0ea02-b227-4640-aa16-a24c5f17fe42)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/9fa2541a-af7b-49e5-8e21-ae88d81d288a)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/d5dbcb05-e30e-4116-94eb-38d2e9157300)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/264ea0c6-6fc9-4c85-a34e-112a6b868642)



[GoTo Next Exercise1.5](ex5.md)








