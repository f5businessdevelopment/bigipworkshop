Exercise 3- Day 0 BIG-IP Deployment on AWS
==========================================

We will be using Terraform to deploy BIG-IP instance on AWS in a VPC, It uses AWS 

The following topics will be covered

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


## Configure VariableSets for AWS Credentials on Terraform Cloud

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/c9118231-8063-4cc8-8602-2564b054cc25)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/86fe721d-9cee-486c-a2b7-f1de15a0ef7e)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/9b5d64ac-6d8c-4c97-af69-ac7c4fba919e)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/5522be7b-70e8-4b07-9220-5dbddc445b91)





## Execute Terraform apply
```
terraform apply -auto-approve
```

![Terraform apply](../images/apply.png)

## Check EC2 deployed on AWS Dashboard

![AWS EC2 Dashboard](../images/ec2.png)
