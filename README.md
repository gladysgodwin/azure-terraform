# Azure Infrastructure Setup

## Overview
This project provisions infrastructure in Azure using the Infrastructure as Code (IaC) tool, Terraform. It includes the following components:

- **Virtual Network (VNet):** A virtual network for isolating resources.
- **Bastion:** Provides secure remote access to Windows Server instances.
- **Windows Servers:** Provisions both Windows Server 2019 and 2022 for various tasks.
- **Key Vault (KeyVault):** Stores secrets securely and integrates with other services.
- **SQL Database (SQL):** Managed SQL server, accessed securely via Managed Identity.
- **Storage Account:** Persistent data storage using Azure Storage.
- **Managed Identity (Managed ID):** Enables secure access between services like Key Vault and SQL.

All components are deployed within a resource group, interconnected through a VNet for network isolation.

## Resources Created

- **Virtual Network (VNet):** Used for securely connecting resources. An existing VNet is utilized.
- **Bastion Host:** Provides secure remote access to Windows Server instances.
- **Windows Server 2019 & 2022:** Deployed for specific applications and tasks.
- **Azure Key Vault:** Stores secrets and enables secure access for other resources (e.g., SQL Database).
- **SQL Database:** A managed SQL server, securely accessed using Managed Identity.
- **Managed Identity:** Used by Windows Servers and Key Vault for secure communication between resources.
- **Azure Storage Account:** Provides scalable storage for various data needs.

## Project Structure

The Terraform project is modularized for reusability and flexibility.

### 1. **components/core**

This folder contains the core files that initialize Terraform and call the necessary modules to create the resources.

#### Files in this folder:
- `init.tf`: Initializes Terraform, sets up the backend for storing the state, and initializes any provider configurations.
- `input.tf`: Defines input variables used throughout the core configuration across multiple modules or resources.
- `main.tf`: Primary configuration file that calls the necessary modules to create the resources (VNet, Bastion, Windows servers, Key Vault, etc.).

### 2. **environments**

This folder stores environment-specific configurations.

#### Subfolders:
- **dev/**: Contains files specific to the development environment.

#### File in `dev/`:
- `dev.tfvars`: Defines environment-specific variables for the development environment.

### 3. **modules**

This folder contains reusable Terraform modules. Each module sets up specific parts of the infrastructure:

- **bastion.tf**: Configures Azure Bastion for secure RDP/SSH access to virtual machines.
- **flexible-server.tf**: Configures Azure Flexible Servers (e.g., MySQL) with scaling and backup options.
- **input-common-vars.tf**: Defines common variables shared across multiple modules or environments.
- **input-flexible-server.tf**: Defines variables for configuring flexible servers.
- **input-kv.tf**: Defines input variables for configuring Azure Key Vault.
- **input-win-server.tf**: Defines input variables for configuring Windows Servers.
- **kv.tf**: Configures Azure Key Vault and integrates with managed identities.
- **managed-id.tf**: Sets up Managed Identity for secure communication between resources.
- **networking.tf**: Defines networking components, including VNets and subnets.
- **password-gen.tf**: Automatically generates strong passwords for resources.
- **rg.tf**: Configures the Azure Resource Group where all infrastructure is deployed.
- **storage-account.tf**: Configures an Azure Storage Account for diagnostics logs, VM disks, and application data.

## Prerequisites

Before running the Terraform configuration, ensure the following:

- **Terraform Installed**: [Install Terraform](https://www.terraform.io/downloads.html).
- **Azure CLI Installed**: [Install Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).
- **Service Principal or User Authentication**: Use a service principal or user account with sufficient permissions to manage Azure resources.

## How to Run Manually

1. **Initialize Terraform**: Run the following command to initialize the Terraform configuration:

    ```bash
    terraform init
    ```

2. **Validate**: Run the command below to validate the Terraform configuration:

    ```bash
    terraform validate
    ```

3. **Plan the Infrastructure**: To see what resources will be created or modified, run:

    ```bash
    terraform plan
    ```

4. **Apply the Configuration**: To create the infrastructure, run:

    ```bash
    terraform apply
    ```

    You will be prompted to confirm the execution.

5. **Destroy Resources (Optional)**: To destroy the resources created, run:

    ```bash
    terraform destroy
    ```

## Deploy Infrastructure using Azure DevOps Pipeline

An Azure DevOps pipeline automates the execution of Terraform. Below is a summary of the process:

### Pipeline Structure

The pipeline is designed to:
- Install Terraform.
- Authenticate with Azure using a Service Principal or Managed Identity.
- Run Terraform scripts to provision the infrastructure.

### Pipeline Steps

1. Clone the repository and navigate to the Terraform directory.
2. Initialize the Terraform backend.
3. Run `terraform validate` to validate the configuration.
4. Run `terraform plan` to display planned infrastructure changes.
5. Apply the configuration with `terraform apply` to create or update resources.
6. Store the Terraform state file in a backend (Azure Storage).

### Triggering the Pipeline

The pipeline triggers manually on the `dev` branch. To trigger it:

- Navigate to the Azure DevOps pipeline section.
- Click "Run Pipeline."
- Select the branch and click "Run."

