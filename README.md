# Creating Application Gateway Load Balancer in Azure

## Description
This repository contains the necessary scripts and configuration files to set up an Application Gateway load balancer in Azure. The Application Gateway will be used to distribute incoming traffic among multiple virtual machines hosting a web application.

## Steps to Create Application Gateway Load Balancer

### 1. Create a Resource Group
Create a resource group named `network-labes` using Azure CLI or Azure Portal.

```bash
az group create --name network-labes --location <location>

```

### 2. Create Virtual Network
Create a virtual network (appgateway-vnet) with two subnets (subnet-appgateway and subnet-vms) using Azure CLI or Azure Portal.

```bash
az network vnet create --resource-group network-labes --name appgateway-vnet --address-prefixes 10.0.0.0/16 --subnet-name subnet-appgateway --subnet-prefix 10.10.0.0/24
az network vnet subnet create --resource-group network-labes --vnet-name appgateway-vnet --name subnet-vms --address-prefix 10.20.0.0/24
```

### 3. Create Virtual Machines
Create two virtual machines using Azure CLI or Azure Portal, both running Ubuntu Linux. Install Apache web server on each VM.
![image](https://github.com/Mouhamed-dridi/Azur_Application_Gateway/assets/53900924/5361a4fe-c64f-4edf-a54b-e9deb068bb07)


SSH into each VM and install Apache:

```bash
sudo apt update
sudo apt install apache2
```
### 4. Deploy Website
Upload your website files to both virtual machines. You can use github

### 5. Enabel the inbound firwall ruls
check the nsg ruls in the neworking setting to enebale the http and https to can esaly test appliaction by put the public ip of the vm in the browser can use only http acn also test with curl
![image](https://github.com/Mouhamed-dridi/Azur_Application_Gateway/assets/53900924/16874e0d-af3d-49d2-a160-06715608c5c3)



### 6. Create Application Gateway
Create an Application Gateway with a backend pool consisting of the two virtual machines. Configure the backend HTTP settings and health probes accordingly.

1- use v2 and multizonz if use vmms instante use 3 zonz and choosevnet that create spsifily for the appgatway
![image](https://github.com/Mouhamed-dridi/Azur_Application_Gateway/assets/53900924/d53ee3cd-05e7-4bcf-a4a8-aad249802070)

add frontend config for the app with public ip
![image](https://github.com/Mouhamed-dridi/Azur_Application_Gateway/assets/53900924/980168a4-4755-4f14-9071-75828be53741)

add backen vms pool xcan use vmms(group of vms) or can createyou vms normaly use option create without taget we gonna add the vms later 
![image](https://github.com/Mouhamed-dridi/Azur_Application_Gateway/assets/53900924/509ed97d-e3dc-4a22-b487-d211b742285e)

creae routing roules bettwen backen and frontned 
![image](https://github.com/Mouhamed-dridi/Azur_Application_Gateway/assets/53900924/680ed42b-1581-4b09-a7ef-3493b0763cab)

add lisener(frontend )
![image](https://github.com/Mouhamed-dridi/Azur_Application_Gateway/assets/53900924/9edb4d58-3bac-4abf-b5b2-69ddde45cdbe)

add backend seting
![image](https://github.com/Mouhamed-dridi/Azur_Application_Gateway/assets/53900924/be60f6f8-0fd7-4b15-a5ee-4c57222bd75d)



add the baken vms 
![image](https://github.com/Mouhamed-dridi/Azur_Application_Gateway/assets/53900924/e54a9dee-a249-4e1e-ae41-480390928274)

add your and also cann add vmss if you have 
![image](https://github.com/Mouhamed-dridi/Azur_Application_Gateway/assets/53900924/d6ee8b70-08ca-4a40-837a-983364d7c9ae)


tets by put the public ip the nevagateur 
![image](https://github.com/Mouhamed-dridi/Azur_Application_Gateway/assets/53900924/7815c8f5-736b-44a5-85c5-1812e3c8e4ca)

itswork 
![image](https://github.com/Mouhamed-dridi/Azur_Application_Gateway/assets/53900924/9785b8a9-dfdd-49ee-baa3-37d0bb59d553)


Notes
Ensure proper network security group rules are in place to allow traffic to the virtual machines from the Application Gateway.
Adjust the configurations as per your specific requirements and network architecture.
Consider enabling SSL termination and configuring other advanced features based on your needs.
For high availability and scalability, consider configuring multiple instances of the Application Gateway across different availability zones.
