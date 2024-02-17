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

az network vnet create --resource-group network-labes --name appgateway-vnet --address-prefixes 10.0.0.0/16 --subnet-name subnet-appgateway --subnet-prefix 10.10.0.0/24
az network vnet subnet create --resource-group network-labes --vnet-name appgateway-vnet --name subnet-vms --address-prefix 10.20.0.0/24

3. Create Virtual Machines
Create two virtual machines using Azure CLI or Azure Portal, both running Ubuntu Linux. Install Apache web server on each VM.

az vm create --resource-group network-labes --name vm1 --image UbuntuLTS --admin-username azureuser --generate-ssh-keys
az vm create --resource-group network-labes --name vm2 --image UbuntuLTS --admin-username azureuser --generate-ssh-keys


SSH into each VM and install Apache:

sudo apt update
sudo apt install apache2

4. Deploy Website
Upload your website files to both virtual machines. You can use scp or any other file transfer method.

5. Create Application Gateway
Create an Application Gateway with a backend pool consisting of the two virtual machines. Configure the backend HTTP settings and health probes accordingly.


Creating Application Gateway Load Balancer in Azure
Description
This repository contains the necessary scripts and configuration files to set up an Application Gateway load balancer in Azure. The Application Gateway will be used to distribute incoming traffic among multiple virtual machines hosting a web application.

Steps to Create Application Gateway Load Balancer
1. Create a Resource Group
Create a resource group named network-labes using Azure CLI or Azure Portal.

bash
Copy code
az group create --name network-labes --location <location>
2. Create Virtual Network
Create a virtual network (appgateway-vnet) with two subnets (subnet-appgateway and subnet-vms) using Azure CLI or Azure Portal.

bash
Copy code
az network vnet create --resource-group network-labes --name appgateway-vnet --address-prefixes 10.0.0.0/16 --subnet-name subnet-appgateway --subnet-prefix 10.10.0.0/24
az network vnet subnet create --resource-group network-labes --vnet-name appgateway-vnet --name subnet-vms --address-prefix 10.20.0.0/24
3. Create Virtual Machines
Create two virtual machines using Azure CLI or Azure Portal, both running Ubuntu Linux. Install Apache web server on each VM.

bash
Copy code
az vm create --resource-group network-labes --name vm1 --image UbuntuLTS --admin-username azureuser --generate-ssh-keys
az vm create --resource-group network-labes --name vm2 --image UbuntuLTS --admin-username azureuser --generate-ssh-keys
SSH into each VM and install Apache:

bash
Copy code
sudo apt update
sudo apt install apache2
4. Deploy Website
Upload your website files to both virtual machines. You can use scp or any other file transfer method.

5. Create Application Gateway
Create an Application Gateway with a backend pool consisting of the two virtual machines. Configure the backend HTTP settings and health probes accordingly.

bash
Copy code
# Create backend address pool
az network application-gateway address-pool create --gateway-name app-gateway --resource-group network-labes --name myBackendPool --servers <vm1_private_ip>,<vm2_private_ip>

# Create HTTP settings
az network application-gateway http-settings create --resource-group network-labes --gateway-name app-gateway --name myHttpSettings --port 80 --protocol Http --cookie-based-affinity Disabled

# Create health probes
az network application-gateway probe create --resource-group network-labes --gateway-name app-gateway --name myHealthProbe --protocol Http --host-name-from-http-settings true --path / --interval 30 --timeout 120 --threshold 3

# Create frontend IP configuration
az network application-gateway frontend-ip create --name myFrontendIP --resource-group network-labes --gateway-name app-gateway --public-ip-address app-gateway-ip

# Create HTTP listener
az network application-gateway http-listener create --name myHttpListener --frontend-ip myFrontendIP --frontend-port 80 --resource-group network-labes --gateway-name app-gateway

# Create routing rule
az network application-gateway rule create --resource-group network-labes --gateway-name app-gateway --name myRoutingRule --http-listener myHttpListener --rule-type Basic --address-pool myBackendPool --http-settings myHttpSettings --probe myHealthProbe
6. Testing
Access the public IP address of the Application Gateway to test the load-balanced access to the web servers.

Notes
Ensure proper network security group rules are in place to allow traffic to the virtual machines from the Application Gateway.
Adjust the configurations as per your specific requirements and network architecture.
Consider enabling SSL termination and configuring other advanced features based on your needs.
For high availability and scalability, consider configuring multiple instances of the Application Gateway across different availability zones.
