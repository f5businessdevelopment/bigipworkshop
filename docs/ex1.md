# Exercise 1.1 - Explore UDF and access AWS environment.


1. Prior to this workshop, student must have received an email with your login instructions and a link to access the UDF course.
   We need this to access and deploy  infrastructure on AWS for this lab. 

  ![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/290cc6c1-67e9-4c10-a097-aad0a3cb78ef)

   
   **Note:** If you are not able to find these details please ask instructor.
   
2. On clicking the link, you will enter the course lobby as shown below. Verify the course name (Dynamic Service Networking) and click Join.

   ![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/bd23d537-ea57-4fd8-8786-3d550d9922ce)

   
3. Click on DEPLOYMENT buttom on the top left to begin provisioning your personal lab access server and network in AWS.
   Here we are creating a ubuntu server to access to AWS resources

   ![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/49df6787-1c1d-41bc-b516-a5091540edb7)


4. Ensure the 'Region' is ``` us-west-2 ```
   Currently the lab is only design for AWS resources in us-west-2

   
5. Navigate to Componets -->  Systems Jumpbox --> Access --> RDP and RDP into the JumpBox

   ![alt text](../images/RDP_Jumpbox.png)
   ![alt text](../images/login.png)
    
6. Upon entering the RDP session on the Ubuntu jumpbox, locate "Activities" positioned in the top-left     corner and proceed to click on it to view the available applications.

   ![alt text](../images/activities.png)

   Click on the Vscode icon as shown below

   ![alt text](../images/vscode.png)

   change directory to ```cd AS3_TerraformCloud```

   ![alt text](../images/terminal.png)

7. On the browser, under the deployment tab, click on **Cloud Accounts** and look for the **API Key** and **API Secret** as shown below
   We will be using these AWS Secret Keys to authenticate the AWS Cloud to create infrastructure.

  ![alt text](../images/console.png)

[GoTo Next Exercise1.2](../1.2/README.md)

[GoBack](../README.md)
