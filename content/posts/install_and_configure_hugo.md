---
title: 'Install and Configure Hugo'
date: 2024-08-30T11:07:37-07:00
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
tags: ['hugo']
---

{{< callout >}}
The latest release as of this post is v0.133.1
{{< /callout >}}

## Install Hugo on Windows

The simplest way to install on Windows will be using Microsoft’s Winget package manager.

More details can be found here: https://learn.microsoft.com/en-us/windows/package-manager/

---
To install using Winget:

```PowerShell
winget install Hugo.Hugo.Extended
```
---
To confirm installation:

```Powershell
hugo version
```

## Install Hugo on Linux

The simplest way to install on linux will be using the distributions package manager.

For this guide, we will be installing on a Debian based linux distribution, which uses the `apt` pacakge manager.

---
To install using `apt`:

```Bash
sudo apt install hugo
```
---
To confirm installation:

```Bash
hugo version
```

## Create Hugo Site
You can always get help with commands by adding `help` to list available commands and `--help` for subcommands as shown below:

```Bash
hugo help
hugo server --help
```

Building your site is as simple as changing into the desired directory that you’d like to have it located, and running the `hugo` command!

But more specifically, the following will create your site in the current directory:
```Bash
hugo new site .
```

To create a new site title "my_new_site" in a new sub-directory of the same name:
```Bash
hugo new site my_new_site
```

This will build your site and place all the important published files into a directory titled public.

To test your site, use the `hugo server` command while in the project directory
This will give you output for where to point your browser to test, such as `localhost:1313/`

```Bash
hugo server
```

You can also run this command with the `-D` option to instruct hugo to build drafts for the site. This is great for testing before publishing!

## Create Content

To get started creating content, simply perform the following command while in your project directory:

```Bash
hugo new content posts/my_first_post.md
```

This will create a new file in the `/content/posts` directory titled `my_first_post.md`

Then you just need to add the content post you'd like on that page at the end of the file, using markdown format!

Unless you left it running from earlier, to test just make sure to run the `hugo server -D` command.

## Wrap-up

And that's it! You've successfully installed, created, and tested your very own Hugo framework website!