# Azure Basic Infrastructure with Terraform
---
**A Hands-on Lab for Deploying Basic Infrastructure in Azure using Terraform**
---

This project demonstrates how to create a basic infrastructure in Azure using Terraform. It includes deploying a Resource Group, a Virtual Network (VNet) with subnets, and an Ubuntu Virtual Machine (VM) with a public IP address.

---

## Table of Contents
1. [Project Objectives](#project-objectives)
2. [Steps Implemented](#steps-implemented)
3. [Screenshots](#screenshots)
4. [Tools Used](#tools-used)
5. [Useful Links](#useful-links)
6. [License](#license)
7. [Contributions](#contributions)

---

## Project Objectives

- **Demonstrate Infrastructure as Code (IaC):**  
  Automate the creation of Azure resources using Terraform.

- **Deploy a Virtual Network (VNet):**  
  Set up a Virtual Network with custom address spaces and subnets.

- **Launch a Virtual Machine:**  
  Deploy an Ubuntu VM with SSH access and a public IP address.

- **Ensure Connectivity:**  
  Validate secure access to the VM using SSH over port 22.

---
## Steps Implemented

1. **Resources Created:**  
   - **Resource Group:**  
     - Name: `BasicInfraRG`  

   - **Virtual Network:**  
     - Name: `BasicVNet`  
     - Address Space: `10.0.0.0/16`  
     - Subnets:  
       - `default` - `10.0.0.0/24`  
       - `BasicSubnet` - `10.0.1.0/24`  

   - **Virtual Machine:**  
     - Name: `UbuntuVM`  
     - Operating System: Ubuntu Server 20.04  
     - Size: Standard B1s (1 vCPU, 1GB RAM)  
     - Public IP: Static, automatically assigned  

2. **Initialize Terraform:**  
   - Run `terraform init` to prepare the working directory.  

3. **Validate Configuration:**  
   - Use `terraform validate` to ensure the configuration is correct.  

4. **Plan and Apply Infrastructure:**  
   - Generate a plan with `terraform plan`.  
   - Apply the changes using `terraform apply`.  

5. **Test Connectivity:**  
   - Retrieve the VM's public IP from Terraform output or Azure Portal.  
   - Connect to the VM using SSH:  
     ```bash
     ssh azureuser@<PUBLIC_IP>
     ```

6. **Cleanup Resources:**  
   - Avoid unnecessary costs by destroying the infrastructure after use:  
     ```bash
     terraform destroy
     ```

---


## Screenshots

1. **Resource Group Overview**
   ![Resource Group](images/resource_group.png)  
   *Overview of the `BasicInfraRG` Resource Group, displaying associated resources such as the VNet, disk, virtual machine, and public IP.*

2. **Terraform Initialization**
   ![Terraform Init](images/terraform_init.png)  
   *Output of the `terraform init` command, which initializes the working directory for Terraform.*

3. **Terraform Execution Plan**
   ![Terraform Output](images/terraform_output.png)  
   *Output of the `terraform plan` command, showing the execution plan and resources that will be created.*

4. **Virtual Network Configuration**
   ![VNet Configuration](images/vnet_screenshot.png)  
   *Configuration of the `BasicVNet` Virtual Network, displaying its subnets and the topology view illustrating connectivity between configured resources.*

---

## Tools Used

- **Terraform CLI:** For writing and executing Infrastructure as Code (IaC).  
- **Azure CLI:** For authenticating and managing Azure resources.  
- **SSH:** For secure connectivity to the deployed VM.  

---

## Useful Links

- [Terraform Documentation](https://developer.hashicorp.com/terraform/docs)  
  Comprehensive guide for using Terraform.  

- [Azure Terraform Provider Documentation](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs)  
  Detailed information about the Azure provider for Terraform.  

- [Azure CLI Documentation](https://learn.microsoft.com/en-us/cli/azure/)  
  Reference guide for Azure CLI commands.  

- [SSH Basics](https://www.ssh.com/academy/ssh)  
  Beginner’s guide to secure remote access using SSH.  

---

## License

This project is licensed under the [MIT License](./LICENSE).  
See the LICENSE file for detailed terms and conditions.

---

## Contributions

Contributions are welcome!  
If you have suggestions for improvements or additional use cases, feel free to [fork this repository](https://github.com/dinAlexDu/azure-terraform-basic) and submit a pull request.  

Please adhere to our [Code of Conduct](./CODE_OF_CONDUCT.md) when contributing to this project.
