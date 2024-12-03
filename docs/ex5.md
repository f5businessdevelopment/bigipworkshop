Exercise 5- Converting BIG-IP configuration using Vscode/ACC tool to AS3 configuration
======================================================================================
We will follow these steps:-
- Open the BIG-IP UCS configuration file using the VSCode BIG-IP Extension tool (ACC).
- Navigate to the source or partition containing the configuration you wish to convert.
- Identify the specific configuration you want to convert and select it.
- Right-click on the selected configuration and choose the "Convert to AS3" option from the context menu.
- The selected configuration will be transformed into an AS3 configuration format.
- Save the converted AS3 configuration file.
- Utilize HCP Terraform to deploy the AS3 configuration to your target environment.

Ensure that you have the following extensions installed: F5 ACC Chariot, F5 Networks iApp, and The F5 Extension. You can verify this by clicking on the blocks icon in VSCode.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/7e141d17-41fd-4950-8b16-64934b47ecd9)

Click on "Open Folder" in VSCode and ensure that you have opened the "BIG-IP-Configs-to-AS3-with-Terraform" folder.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/b36c050d-cafb-48b4-9a96-74085c6147ac)

Click on the F5 icon on the left side of the VSCode app. The F5 icon which is at the extreme left (white) 
![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/4f0de423-7f83-44c5-b59c-c0068fcd090a)

Click on __"Import.conf/UCS/QKVIEW from local file"__ in the VSCode app, and select the file "App.ucs" to import. 
__Note__: Make sure you are in AS3 directory  ~/BIG-IP-Configs-to-AS3-with-Terraform/AS3

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/877490b7-48d1-4520-9579-6e84242fa8b3)

While you are in the F5 icon on VSCode, navigate to `Sources --> Partitions --> Common --> test_vs_tcl 4`. You will see the configuration app.conf on the Right side

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/64087d6d-861d-4a20-ba11-f5733fa8b0fe)

Select all the `app.conf` files as shown in the picture, then right-click to see the "Convert to AS3 with ACC" option.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/2a758fbc-c9d8-4e57-be2a-dbdb98430164)

You will see a new file named "Untitled-1" opened, which is the AS3 converted file. 

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/d9f84eef-007f-4402-8173-cd07320fdcfd)

Change the timeout value from 999 to 800 in the "Untitled-1" file, then save the file with a new name ```vs_tc1.json ```.
You have to also add at the start of the AS3 JSON you created the code snippet below and add the corresponding closing curly brace.
so basically you add an opening brace and two statements class, declaration as preamble and then closing bracket at the end of JSON.

to the JSON, make sure it looks like https://github.com/f5businessdevelopment/BIG-IP-Configs-to-AS3-with-Terraform/blob/8159c8f05d1bbc5c22a215c49eb05fdf882ccff5/AS3/vs_tc2.json#L1

Make sure you are in the __AS3__ directory and you save the newly created file in that directory.

```
cd ~/BIG-IP-Configs-to-AS3-with-Terraform/AS3
```

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/cbd2f0b2-b71d-4545-a93d-699ce2955f34)

Now, let's review the Terraform file we will be deploying. This file utilizes the BIG-IP Terraform provider and the "bigip_as3" Terraform resource. This resource will use the "vs_tc1.json" file you just converted. I have a pre-converted version of this file for you, so you can choose to use either that one or your own conversion. You need to save the file with some name
Terraform Configuration Documentation

Cloud Configuration:
---------------------
The cloud block within the Terraform configuration specifies the organization and workspaces to be used. It is structured as follows:

```
cloud {
  organization = "<Your Organisation STUDENT-XXXX>"

  workspaces {
    name = "AS3app"
  }
}
```
- organization: Specifies the organization name where resources will be provisioned. Replace "<Your Organisation STUDENT-XXXX>" with your organization's name.
- workspaces: Defines the workspace to be used for managing resources within the organization.
  - name: Specifies the name of the workspace. In this configuration, the workspace is named "AS3app".

Required Providers Configuration:
----------------------------------
The required_providers block specifies the required provider and its version to be used in the Terraform configuration.

```
required_providers {
  bigip = {
    source  = "F5Networks/bigip"
    version = "1.22.0"
  }
}
```
- bigip: Specifies the provider name, which is "bigip" in this case.
  - source: Specifies the source of the provider. In this configuration, the provider is sourced from "F5Networks/bigip".
  - version: Specifies the version of the provider to be used. The version specified is "1.22.0".

Provider Configuration:
------------------------
The provider block configures the connection details for the "bigip" provider.

```
provider "bigip" {
  address  = "https://${var.address}:${var.port}"
  username = var.username
  password = var.password
}
```
- address: Specifies the address of the BigIP device. It uses variables var.address and var.port to construct the URL.
- username: Specifies the username used to authenticate with the BigIP device. It uses a variable var.username.
- password: Specifies the password used to authenticate with the BigIP device. It uses a variable var.password.

Example Usage for JSON File:
-----------------------------
The resource block defines a resource of type "bigip_as3" named "as3-example1" and specifies the AS3 JSON configuration file to be used.
```
resource "bigip_as3" "as3-example1" {
  as3_json = file("vs_tc2.json")
}
```
- bigip_as3: Specifies the type of resource, which is "bigip_as3".
- as3-example1: Specifies the resource name.
- as3_json: Specifies the AS3 JSON configuration file to be applied to the BigIP device. It uses the file function to read the contents of the "vs_tc2.json" file.


![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/0efee389-0c47-4f6a-a32e-b341b2f75d74)

[GoTo Next Exercise6 Deploying AS3 App using TFC and terraform BIG-IP resources](ex6.md)

 [GoBack](ex4.md)
