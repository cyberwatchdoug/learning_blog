---
title: 'Custom Domain Using Azure CLI'
date: 2024-09-04T20:55:01-07:00
draft: false
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
tags: ['Azure CLI','DNS']
---

## Prompt Example

{{< callout >}}
Make sure you proof-read and spot check the generated response!
{{< /callout >}}

Below is an example prompt you can use in your favorite LLM to generate a similar process.

Provide a step by step process, with code examples, to create a dns zone in azure with 
a custom domain and configure a static web app to use the custom domain. Do this using 
the azure CLI and python if necessary.


## Prerequisites

1. Active Azure subscription
2. Azure CLI installed on your computer
3. A purchased domain name that is ready to use

## Step 1: Create a Resource Group

If you don’t already have a resource group created where the DNS zone and/or Static Web App will be stored, then the following will create that for you:

```bash
az group create --name newResourceGroup --location eastus
```

## Step 2: Create the DNS Zone

The DNS Zone in Azure is the data resource that contains the DNS records for a domain name. With a DNS zone you can manage the DNS records for your domain, and point your registrars name servers to the Azure DNS IP’s for your DNS zone.

To create a DNS zone for your custom domain:

```bash
az network dns zone create --resource-group newResourceGroup --name mydomain.com
```

## Step 3: Retrieve the Name Servers

These will be required for your domain registrars website. Essentially, your domain requests will be pointed to these listed name servers so that Azure DNS handles the DNS queries to your domain name.

```bash
az network dns zone show --resource-group newResourceGroup --name mydomain.com `
--query "nameServers" --output table
```

## Step 4: Add a CNAME Record to the DNS Zone

The CNAME is needed because it maps the alias, such as the subdomain, to the apex or root domain name. In this guide, we are mapping the `www` subdomain for our Static Web App.

First create the cname record:

```powershell
az network dns record-set cname create --resource-group newResourceGroup `
--zone-name mydomain.com --name www
```

Then set the value of the CNAME record:

```powershell
az network dns record-set cname set-record --resource-group newResourceGroup `
--zone-name mydomain.com --record-set-name www --cname www.mydomain.com
```

## Step 5: Configure Custom Domain in your Static Web App

{{< callout >}}
If you have not yet created your Static Web App in Azure, check out [Deploy a Hugo site to Azure Static Web Apps](https://learn.microsoft.com/en-us/azure/static-web-apps/publish-hugo)
{{< /callout >}}

Now set the custom domain to your Static Web App:

```bash
az staticwebapp hostname set --resource-group newResourceGroup --name myStaticWebApp `
--hostname www.mydomain.com
```

## Step 6: Verify Domain Ownership

Due to the nature of Azure providing SSL certificates for your Static Web App, there might be times when you’ll need to verify that you do in-fact own your custom domain name. 

To verify ownership of the domain, you must add a TXT record to the DNS zone, which is provided by Azure. I know this may sound confusing, since it’s the same place we are configuring but hold steady here.

Retrieve the verification token:

```bash
az staticwebapp hostname show --resource-group newResourceGroup --name myStaticWebApp `
--hostname www.mydomain.com --query "validationToken"
```

Then add the TXT record to the DNS zone:

```bash
az network dns record-set txt add-record --resource-group newResourceGroup \
--zone-name mydomain.com --record-set-name asuid --value "your_validationToken"
```

## Step 7: Verify DNS Propagation

Use tools such as `nslookup` or `dig` or online DNS resolvers/checkers to verify the DNS changes. This process can take some time to complete after changes are made to your DNS records.

## Resources

My favorite place to learn more would be tool references, such as the [Azure CLI A-Z Reference list](https://learn.microsoft.com/en-us/cli/azure/reference-index?view=azure-cli-latest)