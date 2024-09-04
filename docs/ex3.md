Exercise 3- Day 0 BIG-IP Deployment on AWS
==========================================

This exercise uses  Terraform files responsible for setting up an isolated VPC for your deployments, complete with security groups. It deploys a __BIG-IP__ instance along with two __NGINX__ instances. These actions occur within your exclusive __STUDENT-XXX__ Organization and workspace, labeled as __"Day0"__. Upon completion, it provides the Public IP address and password for the __BIG-IP__ instance. These credentials are crucial for configuring the BIG-IP on __Day1__, requiring you to set them as Terraform variables in HCP Terraform for your subsequent __Day1__ deployment.


## Note:- Please check your PREFIX following STUDENT-XXXX and restrict your access to your Organization on Terraform Cloud.

to check you PREFIX and ORG  use the below code

```
cd ~/BIG-IP-Configs-to-AS3-with-Terraform
terraform output
```


## Create a SSH Key 
Before executing the Terraform script, it's essential to generate an SSH key pair. We'll require this in the subsequent section to configure the HCP Terraform agent on a VM machine. 

Change directory to day0

```
cd ~/BIG-IP-Configs-to-AS3-with-Terraform/day0
```

Now generate the key
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com" -f work
```
### Note: Hit Enter multiple times to create the key, let everything be default
Before executing the Terraform commands, ensure that you're in the __day0__ directory, as shown in the picture.


## Execute Terraform init
```
terraform init
```
![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/6829986f-5d7a-465b-ae20-40ac8e0e107d)


## Execute Terraform plan
```
terraform plan
```
![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/cda54e23-fcc4-41ee-96dd-ce480da54137)

An error occurs because AWS credentials, such as AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY, haven't been configured in HCP Terraform. Let's address this in our next step.

## Configure VariableSets for AWS Credentials on Terraform Cloud

Navigate to  https://app.terraform.io and click on the Terraform icon. Head to the Organisations tab; ensure you're in the correct organization. Then, move to the Variables Sets on the left-hand side and choose "__Create New Variable Set__."

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/c9118231-8063-4cc8-8602-2564b054cc25)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/86fe721d-9cee-486c-a2b7-f1de15a0ef7e)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/9b5d64ac-6d8c-4c97-af69-ac7c4fba919e)

Make sure you're in the right organization, aligning the prefix number with your organization's designation. Then, provide an appropriate name for the variable set.

You can provide your name as 
```
AWS Credentials
```


![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/5522be7b-70e8-4b07-9220-5dbddc445b91)


Next, proceed to configure the AWS_ACCESS Key and Secret. Ensure you've selected the Environmental variable option. Then, navigate to Key and copy-paste the Key name from here into the Key field.
```
AWS_ACCESS_KEY_ID
```

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/d6c2107c-f9be-417e-af7c-8f3575c9d28d)

Next, navigate to your UDF cloud deployment and locate Cloud Accounts, as illustrated in the image below. This step is essential for obtaining AWS credentials such as the Secret Key and Key ID.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/2bb753fa-bc87-4f87-9cc4-3593fc167c43)

Add the AWS secret key name and update it with the secret key provided.

```
AWS_SECRET_ACCESS_KEY
```

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/6ca490a0-d53f-4461-9b2c-04750a6c734c)

Save the variable set, ensuring that the "env" option is selected.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/d52b1e5c-54d1-40f6-b675-7bf7bc9f1963)

Now run the command 

```
terraform apply -auto-approve
```

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/d80d6c43-1bee-46b6-81c4-da0250caf51a)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/8f7ab970-6075-4527-8860-8c509687a0c3)

The BIG-IP deployment will take approximately a couple of minutes. You can verify the output for the public IP and password, which will be necessary during the Day1 deployment when configuring the Application objects.

You can visit app.terraform.io go to _YOUR_ workspace and check the configuration is being applied

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/9948c464-6c52-498e-9099-1a85ebcf1908)

Access the AWS Console by logging in from the __RDP Jumpbox__ instead of your laptop. Use the login ID __udf__ and copy the password from the UDF Cloud account. We need to use the Jumpbox browser as the Terraform script uses the Public IP of that machine.

```
login: udf
password: <Copy from the UDF Cloud Account>
```
![image](https://github.com/user-attachments/assets/fa765ed1-0013-4270-b3f6-1c68df4c892c)

## Make sure you are in us-west-2 region Oregon from the AWS Console

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/3c50d081-1bf6-4428-83d4-80001e20a60c)


![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/e6bfc70a-a916-47a3-a688-22f47ca4eceb)

---
Now, visit https://app.terraform.io and ensure you're in your appropriate organization. Navigate to your __workspace-XXX__, where you can view the run details as illustrated below.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/583d9e2e-2e5a-41a3-a929-5be7f353308b)

----
![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/b1e7fcd9-d744-47dc-bcb0-995c4a9be2f7)

Great! We've successfully deployed the BIG-IP in the workspace. Let's proceed to the next exercise, which focuses on Day1 operations, such as deploying the BIG-IP objects.

 [GoTo Next Exercise4 Day 1 Creating BIG-IP objects using TFC and BIG-IP terraform resources](ex4.md)

 [GoBack](ex2.md)
