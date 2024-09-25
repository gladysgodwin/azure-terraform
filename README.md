## Azure Infrastructure setup

#### Infrastructure Architectural Design
![infrastructure design]()
#### Overview
This project is designed to provision infrastructure in Azure using an Iac tool Terraform, which includes:

- Virtual Network (VNet): A virtual network for isolating resources.
- Bastion: Secure remote access to Windows Server instances.
- Windows Servers: Both Windows Server 2019 and Windows Server 2022 are provisioned for different tasks.
- Key Vault (KeyVault): Used to store secrets securely and provides integration with other services.
- SQL Database (SQL): Managed SQL server, linked with Managed Identity for secure access.
- Storage Account: Azure Storage Account for persistent data storage.
- Managed Identity (Managed ID): Enables secure access between services like Key Vault and SQL.

This infrastructure is deployed within a resource group, with all components integrated through a VNet for network isolation.

#### Resources Created
- Virtual Network (VNet):
  Used for connecting different resources in a secure manner. We are using an existing Vnet.
- Bastion Host:
  Provides secure remote access to Windows Server instances.
- Windows Server 2019 & 2022:
  Windows Server instances deployed for specific applications and tasks.
- Azure Key Vault:
  Stores secrets and provides secure access to other resources (e.g., SQL Database).
- SQL Database:
  Managed SQL server for data storage, accessed securely using Managed Identity.
- Managed Identity:
  Used by the Windows Servers and Key Vault for secure resource communication.
- Azure Storage Account:
  Provides scalable storage for various data needs.

The Terraform project is modularized for reusability and flexibility.

### Prerequisites
Before running the Terraform configuration, ensure the following:

- Terraform Installed: Install Terraform by following the official guide.
- Azure CLI Installed: You need the Azure CLI for authentication. Follow the installation guide if not already installed.
- Service Principal or User Authentication: Use a service principal or user account with sufficient permissions to create and manage resources in Azure.

You can either create the infrastructure manually or run the pipeline to create the infrastructure.

#### How to Run manually

1. Initialize Terraform: Run the following command to initialise the Terraform configuration:

```bash
   terraform init
```
2. Validate: Run the command below to validate the terraform configuration.
   
```bash
   terraform validate
```
3. Plan the Infrastructure: To see what resources will be created or modified, run the following:

```bash
   terraform plan
```
4. Apply the Configuration: To apply the changes and create the infrastructure, use the following command:

```bash
   terraform apply
```
You will be prompted to confirm the execution.

5. Destroy Resources (Optional): If you want to destroy the resources created, run:

```bash
  terraform destroy
```

#### Deploy Infrastructure using Azure DevOps Pipeline.

An Azure DevOps pipeline is created to automate the execution of Terraform. Below is a summary of how it works:

#### Pipeline Structure: The pipeline is set up to:
- Install Terraform.
- Authenticate with Azure using a Service Principal or Managed Identity.
- Run the Terraform scripts to provision the infrastructure.

#### Steps in the Pipeline:
- Step 1: Clone the repository and navigate to the Terraform directory.
- Step 2: Initialise the Terraform backend.
- Step 3: Run terraform validate to validate the configuration
- Step 4: Run terraform apply to show the planned infrastructure changes.
- Step 5: Apply the Terraform configuration using terraform apply to create or update the resources.
- Step 6: Store the Terraform state file in a backend (Azure Storage).

### Triggering the Pipeline: 
The pipeline is set to trigger manually on the dev branch. You can trigger it manually by navigating to the Azure DevOps pipeline section and clicking "Run Pipeline", select the branch and click on run.
