---
title: 'Hugo Deployed to Azure Static Web Apps'
date: 2024-10-14T14:41:50-07:00
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
tags: ['Hugo','Azure','GitHub']
---
Deploying your Hugo site to Azure Static Web Apps is rather straightforward with only a couple minor issues (which I’ll detail below).

This process requires using a GitHub Action, which gives you control over how and when the app is built and published.

## Prerequisites

- You must have an active subscription with Azure (you can always create one for free)
- You must have a Hugo site created
    - [See my previous post here where I explained this process](https://www.cyberwatchdoug.com/posts/install_and_configure_hugo/)
- You must also have a GitHub account that is configured with your repository for your Hugo site

## Deploy your Hugo site to Azure SWA

SWA is just the shorthand used to describe Azure Static Web Apps. To deploy your Hugo site you can follow the steps below:

### Create the new static web application in Azure


1. Login to your Azure portal at [portal.azure.com](https://portal.azure.com/)
2. Select **Create a resource**
3. Search for **`Static Web Apps`**
4. Select it and then select **Create**
5. You’ll need to provide some information for your static web app on the *Basics* tab:
    - Resource group — *(create a new one if you need to)*
    - Name — *(name your static web app)*
    - Region — *(select the region closest to you)*
    - Source — *(Select GitHub)*
6. Next select **Sign in with GitHub** to authenticate with your GitHub account (where your Hugo site repository is stored)
    - Organization — *(select desired organization)*
    - Repository — *(repository for your Hugo site)*
    - Branch — *(typically `main` )*
{{< callout >}}    
If you don’t see any repositories, then you may need to authenticate again or authorize Azure Static Web Apps on GitHub. This can be found in your GitHub repository and navigating to **Settings > Applications > Authorized OAuth Apps**
{{</ callout >}}   
7. Select **Hugo** from the *build presets* drop-down, keeping all the default values.

Then just review and create after verifying all the details! Once you select **Create** there will be a new GitHub Action creating in your Hugo site repository on GitHub. This typically runs automatically upon first creation as well.

If you are happy with your new Hugo site deployed to Azure, then feel free to skip the next section; where I detail a specific issue that cause a whole search for a particular customization option that was not immediately understood.

## Issues and Customization

If you’ve completed the above steps, chances are that you have a fully working Hugo site deployed to Azure Static Web Apps now!

But wait, something that came up for me was that the intended Hugo version was not being correctly identified, which lead to other issues.

To remedy the Hugo version required a deep dive into the details on the Azure SWA workflow file (the GitHub Action that was automatically created).

In the workflow file, there’s a line `uses: Azure/static-web-apps-deploy@v1` This is a reference to a specific GitHub Action found here: https://github.com/Azure/static-web-apps-deploy

In the details you’ll notice that the action uses https://github.com/microsoft/Oryx “to detect and build an application” before uploading it to Azure.

If you follow the link to Oryx, you’ll get a lot of information that is not particularly necessary. But within the “Get started” section, there’s a link for more information on specific choices for controlling what oryx detects. This link takes you [to the configurations.md details page](https://github.com/microsoft/Oryx/blob/main/doc/configuration.md#oryx-configuration).

The interesting values to configure here would be:

```bash
PLATFORM_NAME: "hugo"
PLATFORM_VERSION: "0.135.0"
HUGO_VERSION: "0.135.0"
```

I’m not certain if both `PLATFORM_VERSION` and `HUGO_VERSION` are needed, so you might want to include both.

**Where do you add these configuration options?**

That requires reading yet another page [found here](https://learn.microsoft.com/en-us/azure/static-web-apps/build-configuration?tabs=github-actions#environment-variables) regarding the Build configuration options for Azure Static Web Apps. In the section titled “Environment Variables” there is a piece of gold:

> You can set environment variables for your build via the `env` section of a job’s configuration
> 

So that’s where those configuration values need to go, in your GitHub Actions workflow file. You can add them to the file inside the `- name: Build and Deploy` section, at the bottom:

```bash
- name: Build And Deploy
  id: builddeploy
  uses: Azure/static-web-apps-deploy@v1
  with:
    # clipped details for more compact view
  env: # Add environment variables here
    PLATFORM_NAME: "hugo"
    PLATFORM_VERSION: "0.135.0"
    HUGO_VERSION: "0.135.0"
    ###### End of Repository/Build Configurations ######
```

This should force Oryx to use the values you set for it’s process at building your application.

There are most likely more custom configuration options you can set for Oryx, or even for your GitHub Actions workflow, but those will need to wait until next time.

I hope you found this informative and helpful. At the time of writing this, there was not a simply written solution that had all the pieces explained for me.