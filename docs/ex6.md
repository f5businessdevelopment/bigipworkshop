Exercise 6- Deploying AS3 App using TFC and terraform BIG-IP resources
======================================================================

Make sure you are in __AS3__ directory or use below

```
cd ~/BIG-IP-Configs-to-AS3-with-Terraform/AS3
```
In this exercise, we'll explore deploying AS3 (Application Services 3) JSON blobs using BIG-IP Terraform resources. We'll utilize the configuration we converted from legacy settings to AS3. Once more, we'll employ the same big-pool agent for this task.

Let's examine the `main.tf` file and ensure that the organization is set to STUDENT-XXXX. Please verify your prefix and adjust the number accordingly.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/a4a2d69e-b965-4db7-bf75-a4765ea45d6e)

You can confirm your prefix or organization number by revisiting the "day0" section and inspecting the `cloud.tf` file.
```
cat  ../day0/cloud.tf
```
![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/43ed8802-e43e-4a6a-bbce-726ff2145b6b)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/77b0df9e-040b-4307-bccd-072497adc171)

## Make sure  Default Execution Mode to Agent

Navigate to workspaces --> Setting --> Agent

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/a3f4bb2e-fee2-4f10-8174-c2cb1573e8ef)

Select Execution mode as Agent select big-pool as shown below and click update

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/8764db91-6978-41bd-8c3f-8a5a64b0a054)


Let's execute terraform now

```
terraform init
```


![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/218b3419-e4b1-41e5-8501-c0dff731b91c)

```
terraform plan
```

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/a9b16439-b4ae-4157-af3e-644bfce7f9bf)

```
terraform apply -auto-approve
```


![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/2b1c8cf4-9516-4a9c-8837-33843f069fbc)

You'll encounter this if you haven't removed the configuration from day1, as the previous setup remains intact on BIG-IP.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/944b5ff6-cac0-499e-aa70-3ada8a90cbb8)

going back to day1 now

```
cd ../day1

```

```
terraform destroy

```



![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/94a9048e-171c-4046-b813-2b467aeb6450)

 
Now do Terraform apply again

```
terraform apply -auto-approve
```

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/b58c45cf-9627-4327-a926-3355136d445c)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/8bebd6a2-d942-4459-9eaa-e92412861d87)

Ensure that you validate all the workspaces and confirm that you are within your Organization.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/78eca5b4-ba1c-4f68-97e4-d943a04264ff)

Try accessing HTTP://YOUR_PUBLIC_IP:8080 in your browser; you should receive a response from the NGINX backend server.

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/38e06954-9e18-40c7-957e-08411100800e)

### Clean UP

Prior to cleaning up, ensure that your workspace is not linked to the big-pool agent. Instead, opt for the Remote option. Also go to the __day0__  directory and run terraform destroy from there.

```
terraform destroy
```
![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/38b04cf1-c87d-48f4-9648-33930920d33c)


![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/64d1ea2b-4d1f-4dbd-b153-848e8e26ed21)

![image](https://github.com/f5businessdevelopment/bigipworkshop/assets/13858248/5940e1e2-c361-48fe-8688-1f726da25217)

[GoTo Home](../README.md)

 [GoBack](ex5.md)
