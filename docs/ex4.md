Exercise 4 - Day 1 Creating BIG-IP objects using TFC and BIG-IP terraform resources
===================================================================================

For Day1, our focus shifts to deploying the BIG-IP application objects using imperative Terraform resources. The main.tf file contains configuration details where we set up monitors, nodes, pools, and virtual servers using imperative BIG-IP Terraform resources, as illustrated below.

```
 # Terraform Configuration for Deploying BIG-IP Application Objects

## Cloud Configuration

cloud {
  organization = "<Your Organisation STUDENT-XXXX>"

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
We're utilizing the private IP for BIG-IP because our plan involves configuring the Terraform TFE Agent on a VM. This agent will communicate with the HCP Terraform using a private IP connection.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/69215482-fd6e-4fc7-a1d5-f01a36cf0849)


Before running the `terraform init/plan/apply` commands, ensure you update the Organization in the `main.tf` file with the correct prefix.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/96de93ed-f6d4-48f0-9bcd-6871491418f9)

Additionally, from your RDP session browser, navigate to the AWS console and update the ingress Security Group for the F5 instance "studentXXX-f5 instance" as depicted. Enable ```SSH``` with __My IP__ as we will need to ssh into the Agent from your jumpbox

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/221fc562-027d-4f02-868b-676398453c94)

## Setup HCP Terraform Agent

You can verify the status of the Terraform agent named "big-pool" by accessing your workspace settings and navigating to the Agents section. The agent should be in an __idle__ state, and it should have both an __ID__ and an associated IP address.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/e26fe5e4-a578-4482-b18b-00371482383e)


Before configuring the Agent, it's necessary to perform `terraform init` and `terraform plan`. This step is crucial as we need to create the workspace named "day1". This workspace will utilize the "bigip-pool" Agent to communicate with the HCP Terraform. Terraform plan will fail however it will create the "day1" workspace.


![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/2d609b76-12a2-41ba-87e3-7229e8437950)


Access the Agent option by first navigating to "workspaces", then proceed to "settings", and finally select "Agents".

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/e3ed4eba-6375-47b7-92ba-73a3558f4b0f)





### Set up the day1 workspace to utilize the big-pool Agent.

To assign the workspace "day1" to utilize the agent named "big-pool," navigate to the "day1" workspace and select the "Agent" option under the Default execution mode. Then, update the Organization information accordingly.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/47c44266-b168-4b00-869a-aef078cf4cb8)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/59bf0345-d533-4fc7-b46d-37cf73e42e5f)


Next, execute the command `terraform init` to initialize the Terraform configuration.

Next, execute the command `terraform plan` to generate and review an execution plan based on the Terraform configuration.

Execute the command `terraform apply` to apply the changes defined in the Terraform configuration and provision the resources accordingly.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/7cc0aca6-a6cf-49f5-9e6c-f32d1406977d)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/27332858-b937-4d97-ad2f-f3c724423942)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/4bddc4ea-01e4-49e3-8fb5-cf2fe0310293)

You can now navigate to the workspace to verify that the day1 workspace is created and the configuration is successfully applied.


![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/5b36a47c-1427-4d6e-9879-bbec6d802ef2)

To access the configuration on the BIG-IP, you'll need to obtain the credentials from the Outputs section in the HCP Terraform. Click on the F5_ui Public IP or copy and open the same in your browser to log in to the BIG-IP interface.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/3fb0ea02-b227-4640-aa16-a24c5f17fe42)

Navigate to the traffic manager option and proceed to the virtual server. Check the status of the virtual server, pool, and pool members. You should see a green light indicating that everything is functioning correctly.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/9fa2541a-af7b-49e5-8e21-ae88d81d288a)

Additionally, to access the backend application from your RDP jump box browser, set up another security group allowing traffic on port 8080. Furthermore, from your RDP session browser, navigate to the AWS console and update the ingress Security Group for the F5 instance "studentXXX-f5 instance" to enable TCP port 8080 for 0.0.0.0/0.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/d5dbcb05-e30e-4116-94eb-38d2e9157300)

To access the backend NGINX app, issue the command `http://Public_IP_of_F5:8080` from your Jumpbox browser.


![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/264ea0c6-6fc9-4c85-a34e-112a6b868642)


You can omit the configuration for now, as in the upcoming exercise, we'll focus on creating and deploying AS3 configurations using declarative BIG-IP Terraform resources.

```
terraform destroy
```

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/b84d0cf9-c532-4005-a50e-6f1c894d3111)


[GoTo Next Exercise1.5](ex5.md)








