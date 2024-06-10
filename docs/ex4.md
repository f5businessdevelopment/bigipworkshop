Exercise 4 - Day 1 Creating BIG-IP objects using TFC and BIG-IP terraform resources
===================================================================================

Change directory to Day1

```
cd ~/BIG-IP-Configs-to-AS3-with-Terraform/day1
```
For Day 1, our focus shifts to deploying the BIG-IP application objects using imperative Terraform resources. The main.tf file contains configuration details where we set up monitors, nodes, pools, and virtual servers using imperative BIG-IP Terraform resources, as illustrated below.

__Note__: You don't need to create this configuration, this is just for reference, file `main.tf` already has that.

## Review TF files for Day1

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
__Note__: You don't need to create this configuration, this is just for reference, file `main.tf` already has that.
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
__Note__: You don't need to create this configuration, this is just for reference, file `main.tf` already has that.
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

### Review BIG-IP Credentials created  in HCP Terraform

You can access the BIG-IP credentials by navigating to the https://app.terraform.io  then to your workspace. Click on "Outputs" to retrieve the credentials required for configuring the BIG-IP Terraform Variables Set.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/b8f3e4d9-e0ac-4a50-9b97-21a4e422b001)

## Create Variable Set as BIG-IP env on HCP Terraform
---
Open https://app.terraform.io in a new tab, then navigate to your Organization and workspace. Click on "Settings."

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/5afd1a83-087a-4890-8e32-4f7ced33f9f4)

click on VariableSets we are adding the BIG-IP Env variables for terraform


![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/968dfb7e-a697-4741-901c-ddcffce71634)

```
BIG-IP Env
```
![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/5196bf03-a743-495b-a90d-495af196ccb4)


![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/1c25dc14-5c20-4295-b256-b56fe1df9c24)

One by one start adding the parameters __address, port, username, password__ and so on, Ensure that you have selected "Terraform variables."....finally create a variable set

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/e7e3045b-32bd-4dbd-a009-3a6cc8e85aa0)


You can copy the variables name from below

```
address
```
```
port
```
```
username
```
```
password
````
You should see the BIG-IP Variables Set displayed as illustrated below. 
We're utilizing the private IP for BIG-IP because our plan involves configuring the Terraform TFE Agent on a VM. This agent will communicate with the HCP Terraform using a private IP connection.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/69215482-fd6e-4fc7-a1d5-f01a36cf0849)

Make sure you are in __day1__ directory by changing the directory to

```
cd ~/BIG-IP-Configs-to-AS3-with-Terraform/day1
```
Before running the `terraform init/plan/apply` commands, ensure you update the Organization in the `main.tf` file with the correct prefix.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/96de93ed-f6d4-48f0-9bcd-6871491418f9)

 

## Setup HCP Terraform Agent

You can verify the status of the Terraform agent named "big-pool" by accessing your workspace settings and navigating to the Agents section. The agent should be in an __idle__ state, and it should have both an __ID__ and an associated IP address.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/e26fe5e4-a578-4482-b18b-00371482383e)


Before configuring the Agent, it's necessary to perform `terraform init` and `terraform plan`. This step is crucial as we need to create the workspace named "day1". This workspace will utilize the "bigip-pool" Agent to communicate with the HCP Terraform. Terraform plan will fail however it will create the "day1" workspace.


![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/2d609b76-12a2-41ba-87e3-7229e8437950)


Access the Agent option by first navigating to "workspaces", then proceed to "settings", and finally select __"Agent"__.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/e3ed4eba-6375-47b7-92ba-73a3558f4b0f)





### Set up the Workspaces to utilize the big-pool Agent in your ORG.

To assign the workspaces  to utilize the agent named "big-pool," navigate to the "day1" workspace and select the "Agent" option under the Default execution mode. Then, update the Organization information accordingly.

- Go to Workspaces --> Settings 


- Click on __Agents__ and select Default execution mode as Agent and update Organisation.
![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/59bf0345-d533-4fc7-b46d-37cf73e42e5f)

- Go to the __Agents__ Click on __big-pool__  the one which is above Agent name, to get into a new window 
  ![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/b0f021de-1db4-4a21-8933-2008cd4a5f5e)

- make sure __Grant to All workspaces in the Org__ is selected and Hit __SAVE__

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/15a92643-f389-4609-8cfe-757de312fe48)



## AWS Security Group Configuration

__Note__: Make sure you are in us-west-2 Oregon region 

1. Go to AWS Console from the Jump Box browser and select __studentXXXX-f5__ instance
2. Click on the security group go __studentXXX-f5__ and edit inbound rules

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/17a6db5b-c238-42f5-b69c-e3fe0821fe70)

3. Update the inbound rule by selecting __All TCP__ and __My_IP__ and save the rule
   
![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/09116a91-4c97-41af-9e94-1dfe4b337faf)

It should look like this, obviously, the IP addresses will be different in your case

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/9d340c17-8d74-4eae-bba2-cd41dcf8c1ef)




__Note__ IF security group is not configured terraform plan will fail.
Go to VSCode 

Next, execute the command `terraform plan` to generate and review an execution plan based on the Terraform configuration.

Execute the command `terraform apply` to apply the changes defined in the Terraform configuration and provision the resources accordingly.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/7cc0aca6-a6cf-49f5-9e6c-f32d1406977d)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/27332858-b937-4d97-ad2f-f3c724423942)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/4bddc4ea-01e4-49e3-8fb5-cf2fe0310293)

You can now navigate to the workspace to verify that the day1 workspace is created and the configuration is successfully applied.


![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/5b36a47c-1427-4d6e-9879-bbec6d802ef2)


To access the configuration on the BIG-IP, you'll need to obtain the credentials from the Outputs section in the HCP Terraform. Click on the F5_ui Public IP or copy and open the same in your browser to log in to the BIG-IP interface.

Make sure to update the Security Group of your F5 instance to include your __My IP__

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/3fb0ea02-b227-4640-aa16-a24c5f17fe42)

Navigate to the traffic manager option and proceed to the virtual server. Check the status of the virtual server, pool, and pool members. You should see a green light indicating that everything is functioning correctly.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/9fa2541a-af7b-49e5-8e21-ae88d81d288a)

## Access the backend application from your RDP jump box browser

To access the backend NGINX app, issue the command `http://Public_IP_of_F5:8080` from your Jumpbox browser.


![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/264ea0c6-6fc9-4c85-a34e-112a6b868642)


You can omit the configuration for now, as in the upcoming exercise, we'll focus on creating and deploying AS3 configurations using declarative BIG-IP Terraform resources.

```
terraform destroy
```

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/b84d0cf9-c532-4005-a50e-6f1c894d3111)


[GoTo Next Exercise5 Converting BIG-IP configuration using Vscode/ACC tool to AS3 configuration](ex5.md)

 [GoBack](ex3.md)






