Exercise 5- Converting BIG-IP configuration using Vscode/ACC tool to AS3 configuration
======================================================================================

1. Open the BIG-IP UCS configuration file using the VSCode BIG-IP Extension tool (ACC).
2. Navigate to the source or partition containing the configuration you wish to convert.
3. Identify the specific configuration you want to convert and select it.
4. Right-click on the selected configuration and choose the "Convert to AS3" option from the context menu.
5. The selected configuration will be transformed into an AS3 configuration format.
6. Save the converted AS3 configuration file.
7. Utilize HCP Terraform to deploy the AS3 configuration to your target environment.

Ensure that you have the following extensions installed: F5 ACC Chariot, F5 Networks iApp, and The F5 Extension. You can verify this by clicking on the blocks icon in VSCode.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/7e141d17-41fd-4950-8b16-64934b47ecd9)

Click on "Open Folder" in VSCode and ensure that you have opened the "BIG-IP-Configs-to-AS3-with-Terraform" folder.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/b36c050d-cafb-48b4-9a96-74085c6147ac)

Click on the F5 icon on the left side of the VSCode app.
![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/4f0de423-7f83-44c5-b59c-c0068fcd090a)

Click on "Import.conf/UCS/QKVIEW from local file" in the VSCode app, and select the file "App.ucs" to import.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/877490b7-48d1-4520-9579-6e84242fa8b3)

While you are in the F5 icon on VSCode, navigate to `Sources --> Partitions --> Common --> test_vs_tcl 4`. You will see the configuration app.conf on the Right side

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/64087d6d-861d-4a20-ba11-f5733fa8b0fe)

Select all the `app.conf` files as shown in the picture, then right-click to see the "Convert to AS3 with ACC" option.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/2a758fbc-c9d8-4e57-be2a-dbdb98430164)


![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/d9f84eef-007f-4402-8173-cd07320fdcfd)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/cbd2f0b2-b71d-4545-a93d-699ce2955f34)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/0efee389-0c47-4f6a-a32e-b341b2f75d74)

[GoTo Next Exercise1.6](ex6.md)
