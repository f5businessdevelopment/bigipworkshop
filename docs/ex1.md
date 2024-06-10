# Exercise 1.1 - Explore UDF and access AWS Environment.


1. Prior to this workshop, student must have received an email with your login instructions and a link to access the UDF course. We need this to access and deploy  infrastructure on AWS for this lab. 

  ![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/290cc6c1-67e9-4c10-a097-aad0a3cb78ef)

   
   **Note:** If you are not able to find these details please ask the instructor.
   
2. On clicking the link, you will enter the course lobby as shown below. Verify the course name  and click Join.

   ![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/bd23d537-ea57-4fd8-8786-3d550d9922ce)

   
3. Click the __DEPLOYMENT__ button on the top left to begin provisioning your personal lab access server and network in AWS. Here we are creating an Ubuntu server to access  AWS resources

   ![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/49df6787-1c1d-41bc-b516-a5091540edb7)


4. 'Region' is ``` us-west-2 ``` Currently, the lab is only designed for AWS resources in the us-west-2

__Note__: If you're using a Mac and haven't yet downloaded the Remote Desktop App, you can do so by clicking
  https://apps.apple.com/us/app/microsoft-remote-desktop/id1295203466?mt=12
   
5. Navigate to Components -->  __Systems Jumpbox__ --> __Access__ --> __RDP__ and RDP into the JumpBox

![alt text](../images/RDP_Jumpbox.png)

   
  You'll be presented with a range of resolution options. Feel free to choose whichever suits your 
  needs. If you don't make a selection, it will automatically default to the highest resolution
  Please use username as ```student``` and password as ```workshop```
  
   ![alt text](../images/login.png)

   __Note__ if you get this window please cancel and move forward

   ![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/c8f218c5-ffd3-45ad-a778-fe10a2766159)


    
6. Upon entering the RDP session on the Ubuntu jump box, locate "__vsCode__" and __Firefox__  positioned in the top- 
left corner and proceed to click on it to view the available applications.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/40a9b086-7bbb-433e-a0f2-6d4b55b604bc)


  Locate the VScode icon and click on it. Similarly, locate the browser icon and click on it. Both 
  applications will then launch. Please allow a few seconds for them to appear, especially since 
  you're using RDP
  
   If you get this window enter the password as ```workshop```

   ![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/300ebf15-a103-411c-8b08-a7e8142e3229)


   On the VSCode open a Terminal and change directory to
   ```
   cd BIG-IP-Configs-to-AS3-with-Terraform

   ```
   We're navigating to the directory to ensure we're in the right location to begin interacting 
   with HCP Terraform for deploying the Org and workspace later

   ![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/d2d587e7-fc2c-4937-b101-96aef292656e)



7. On the browser, under the deployment tab, click on **Cloud Accounts** and look for the **API Key** and **API Secret** as shown below
We will be using the AWS key ID and  AWS Secret Keys and configure these in Terraform Cloud for authenticating  the AWS Cloud to create infrastructure.

  ![alt text](../images/console.png)

   We'll require this information to input as AWS credentials into HCP Terraform, as all tasks will 
   be executed through HCP Terraform.

[GoTo Next Exercise2 Create a New Organisation and Workspace on HCP Terraform](ex2.md)

[GoBack](../README.md)
