---
title: 'Terraform on Windows Quick Start'
date: 2024-09-09T21:32:08-07:00
draft: true
hidden: false
externalURL: false
showDate: true
showModDate: true
showReadingTime: true
showTags: true
showPagination: true
invertPagination: true
showToC: true
openToC: false
showComments: false
showHeadingAnchors: true
tags: ['Terraform','Azure']
---
## Terraform on Windows Quick Start

This step-by-step process was originally generated using an LLM prompt, and then reviewed and enriched where it may have been necessary. You can follow along, or just jump to the prompt I used and perform the same query yourself.

### The Prompt:

<aside>
▶️ Provide a step by step process, with code examples, to install and use terraform on a windows 10 pc.
</aside>

## Enriched Response:

Here’s a step-by-step process to install and use Terraform on a Windows PC:

### Step 1: Install Terraform

**Download Terraform**

1. Go to the [Terraform download page](https://www.terraform.io/downloads.html).
2. Find the Windows version and download the ZIP file.

**Install Terraform**

1. Extract the contents of the ZIP file to a desired directory (e.g., `C:\opt\Terraform`).

**Add Terraform to the system PATH**

You can do this in a few different ways. The first is using the commandline (PowerShell in this case), and the second is manually through the Windows GUI.

**Using the commandline:**

https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_environment_variables?view=powershell-7.4#create-persistent-environment-variables-in-windows

**Using the Windows GUI:**

1. Open the Start Menu and search for “Environment Variables”, then select “Edit the system environment variables”.
    
    {{< figure src="start_menu_env_var.png">}}
    
2. In the System Properties window, click the “Environment Variables” button.
    {{< figure src="Untitled1.png">}}
    
3. In the Environment Variables window, find the “Path” variable in the “System variables” section and click “Edit”.
    {{< figure src="Untitled2.png">}}
    
4. Click “New” and add the path to the directory where you extracted Terraform (e.g., `C:\opt\Terraform`).
    {{< figure src="Untitled3.png">}}
    
5. Click “OK” to close all windows.

### Verify Installation

Open a new PowerShell window and type:

```powershell
terraform -v
```

You should see the installed version of Terraform:
{{< figure src="Untitled4.png">}}

## Step 2: Configure Azure CLI

### Install Azure CLI

1. Install the Azure CLI using the Windows Package Manager `winget`
    {{< figure src="Untitled5.png">}}
    
    I find it best to first search for the package you want to install using `winget search azurecli`
    
    Then using the `-e` option for exact id match, and the `--id` option to specify the exact id (`Microsoft.AzureCLI` in this case)
    
    *❗Alternatively, you can download the Azure CLI installer from the [Azure CLI installation page](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?tabs=azure-cli) and then run the installer and follow the on-screen instructions to complete the installation.*
    

### Login to Azure

*✳If you don’t already have your Azure account created, you can do that at https://portal.azure.com/*

Open a new PowerShell window and run:

```powershell
az login
```

This will open a new browser window where you can sign in with your Azure account.

Once signed in, you will also be presented with your Tenant and subscription selection options. In most cases you will only have 1 available, so hit enter to make the selection and complete the account login process.

For more information about this new subscription selector, see the following link: [Sign in interactively with Azure CLI - Subscription Selector](https://learn.microsoft.com/en-us/cli/azure/authenticate-azure-cli-interactively#subscription-selector)

[def]: Untitled%201.png