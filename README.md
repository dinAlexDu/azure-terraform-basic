# Azure Basic Infrastructure with Terraform
---
**A Hands-on Lab for Deploying Basic Infrastructure in Azure using Terraform**
---

This project demonstrates how to create a basic infrastructure in Azure using Terraform. It includes deploying a Resource Group, a Virtual Network (VNet) with subnets, and an Ubuntu Virtual Machine (VM) with a public IP address.

---

## Table of Contents
1. [Project Objectives](#project-objectives)
2. [Steps Implemented](#steps-implemented)
3. [Terraform Configuration](#terraform-configuration)
4. [Screenshots](#screenshots)
5. [Tools Used](#tools-used)
6. [Useful Links](#useful-links)
7. [License](#license)
8. [Contributions](#contributions)

---

## Project Objectives

- **Create a Resource Group:**  
  Organize all related Azure resources in a single container.

- **Set up a Virtual Network (VNet):**  
  Configure address spaces and subnets for network management.

- **Deploy a Virtual Machine:**  
  Provision an Ubuntu VM with a public IP and SSH access.

- **Automate with Terraform:**  
  Utilize Terraform to manage and deploy the infrastructure.

---

## Steps Implemented

1. **Initialize Terraform Configuration**  
   - Used `main.tf` as the primary Terraform configuration file to define resources such as the Resource Group, VNet, Subnets, and VM.
   - Initialized Terraform in the project directory using:
     ```bash
     terraform init
     ```

2. **Validate Configuration**  
   - Validated the syntax and structure of `main.tf`:
     ```bash
     terraform validate
     ```

3. **Generate and Apply Terraform Plan**  
   - Previewed the changes Terraform would apply:
     ```bash
     terraform plan
     ```
   - Deployed the infrastructure using:
     ```bash
     terraform apply
     ```

4. **Resources Created**  
   - **Resource Group:** `BasicInfraRG`  
   - **Virtual Network:** `BasicVNet`  
     - **Address Space:** `10.0.0.0/16`  
     - **Subnets:**  
       - `default` - `10.0.0.0/24`  
       - `BasicSubnet` - `10.0.1.0/24`  
   - **Virtual Machine:** `UbuntuVM`  
     - **Operating System:** Ubuntu Server 20.04  
     - **Size:** Standard B1s (1 vCPU, 1GB RAM)  
     - **Public IP:** Static, automatically assigned  

5. **Connect to the Virtual Machine**  
   - Retrieved the VM's public IP from the Terraform output and established an SSH connection:
     ```bash
     ssh azureuser@<PUBLIC_IP>
     ```
     Replace `<PUBLIC_IP>` with the actual IP.

6. **Clean Up Resources**  
   - To avoid unnecessary costs, destroyed the resources using:
     ```bash
     terraform destroy
     ```

---
## Terraform Configuration

The `main.tf` file contains the Terraform configuration that automates the creation of the Azure infrastructure. Below is a breakdown of the key sections with examples:  
[View the full configuration here](./main.tf).


### 1. Provider Configuration
The provider block specifies Azure as the cloud provider:
```hcl
provider "azurerm" {
  features {}
}
```
### 2. Resource Group
The resource group is the container for all Azure resources:
```hcl
resource "azurerm_resource_group" "main" {
  name     = "BasicInfraRG"
  location = "West Europe"
}
```
  - Name: BasicInfraRG (used to organize resources)
  - Location: West Europe

### 3. Virtual Network and Subnets
Defines the virtual network and two subnets for network segmentation:
```hcl
resource "azurerm_virtual_network" "main" {
  name                = "BasicVNet"
  address_space       = ["10.0.0.0/16"]
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
}

resource "azurerm_subnet" "default" {
  name                 = "default"
  resource_group_name  = azurerm_resource_group.main.name
  virtual_network_name = azurerm_virtual_network.main.name
  address_prefixes     = ["10.0.0.0/24"]
}

resource "azurerm_subnet" "basic" {
  name                 = "BasicSubnet"
  resource_group_name  = azurerm_resource_group.main.name
  virtual_network_name = azurerm_virtual_network.main.name
  address_prefixes     = ["10.0.1.0/24"]
}
```
  - Virtual Network: BasicVNet with address space 10.0.0.0/16.
  - Subnets:
  - default (10.0.0.0/24)
  - BasicSubnet (10.0.1.0/24)

### 4. Public IP and Network Interface
Associates a static public IP with the VM via a network interface:
```hcl
resource "azurerm_public_ip" "main" {
  name                = "UbuntuVM-PublicIP"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  allocation_method   = "Static"
}

resource "azurerm_network_interface" "main" {
  name                = "UbuntuVM-nic"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name

  ip_configuration {
    name                          = "internal"
    subnet_id                     = azurerm_subnet.default.id
    private_ip_address_allocation = "Dynamic"
    public_ip_address_id          = azurerm_public_ip.main.id
  }
}
```
### 5. Virtual Machine
Defines the Ubuntu VM with SSH access:
```hcl
resource "azurerm_linux_virtual_machine" "main" {
  name                = "UbuntuVM"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  size                = "Standard_B1s"

  admin_username      = "azureuser"
  disable_password_authentication = true

  admin_ssh_key {
    username   = "azureuser"
    public_key = file("~/.ssh/id_rsa.pub")
  }

  network_interface_ids = [
    azurerm_network_interface.main.id,
  ]

  os_disk {
    caching              = "ReadWrite"
    storage_account_type = "Standard_LRS"
  }

  source_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "20_04-lts-gen2"
    version   = "latest"
  }
}
```
  - VM Name: UbuntuVM
  - Size: Standard_B1s (1 vCPU, 1 GB RAM)
  - OS: Ubuntu Server 20.04
  - Authentication: SSH key (~/.ssh/id_rsa.pub)
---
### Notes:

This configuration follows best practices for Azure infrastructure:
Static IP for the VM: Ensures the VM's IP address does not change.
Network Segmentation via Subnets: Improves security and resource isolation.
SSH Authentication: Enhances security by avoiding password-based logins.
These best practices are ideal for creating scalable, secure, and cost-efficient Azure resources.


## Screenshots

Below are the screenshots that illustrate the steps:

1. **Resource Group Overview**  
   ![Resource Group Overview](images/resource_group.png)  
   *List of resources created under the `BasicInfraRG` Resource Group.*

2. **Terraform Initialization**  
   ![Terraform Initialization](images/terraform_init.png)  
   *Terraform initialized successfully for the project directory.*

3. **Terraform Plan Output**  
   ![Terraform Plan Output](images/terraform_output.png)  
   *Preview of the resources to be created, as defined in `main.tf`.*

4. **Virtual Network Overview**  
   ![Virtual Network Overview](images/vnet_screenshot.png)  
   *Visualization of the VNet topology and connected resources.*

---

## Tools Used

- **Terraform CLI:** For managing infrastructure as code.  
- **Azure CLI:** For authenticating with Azure.  
- **Visual Studio Code:** For editing `main.tf`.  
- **Azure Portal:** For resource management and validation.  


---

## Useful Links

- [Terraform Documentation](https://developer.hashicorp.com/terraform/docs)  
  Official documentation for Terraform configuration and management.

- [Azure Virtual Network Documentation](https://learn.microsoft.com/en-us/azure/virtual-network/)  
  Comprehensive guide to configuring VNets in Azure.

- [Azure Virtual Machine Documentation](https://learn.microsoft.com/en-us/azure/virtual-machines/)  
  Guide for deploying and managing Azure VMs.
---

## License

This project is licensed under the [MIT License](./LICENSE).  
See the LICENSE file for detailed terms and conditions.

---

## Contributions

Contributions are welcome!  
If you have suggestions for improvements or additional use cases, feel free to [fork this repository](https://github.com/dinAlexDu/azure-terraform-basic) and submit a pull request.  

Please adhere to our [Code of Conduct](./CODE_OF_CONDUCT.md) when contributing to this project.
