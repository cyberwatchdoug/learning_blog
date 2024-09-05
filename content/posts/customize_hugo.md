---
title: 'Customize Hugo'
date: 2024-08-30T11:28:21-07:00
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

## About page
This one is easy, compared to many other configurations. To create an About page, just issue the following command in your sites root directory:

```Bash
hugo new about/index.md
```

This will create (if not already exists) the `./content/about/` folder, and also the `index.md` page.

To use this page, you'll need to adjust your menu configuration. This will hopefully come included in some way with the theme you have chosen for your site.

In my example, I'm using the [Kayal](https://github.com/mnjm/kayal) theme.

## Archetypes
These are your templates for new content, found under the *Content Management* section on the [gohugo.io](https://gohugo.io/content-management-archetypes) website.

My first step with Hugo when I created my site was to dig into these templates so at the top of my posts I could have the same default metadata, called Front Matter.

The simplest way to get started is to copy the `default.md` file found in your chosen theme directory. In the examples provided below, I am using the [Kayal theme](https://github.com/mnjm/kayal)

First create the `archetypes` directory in your root directory for your site if it does not already exist:

```Bash
mkdir archetypes
```

Then copy over the `default.md` file from your theme's `archetypes` directory:

```Bash
cp ./themes/kayal/archetypes/default.md ./archetypes/default.md
```

Open up the `./archetypes/default.md` file you just copied and add the front matter you'd like on each newly created post.

Here is an example from my own personal file:

```Yaml
---
title: '{{ replaceRE `[-|_]` " " .File.ContentBaseName | title }}'
date: {{ .Date }}
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
tags: ['']
---
```

The file can use either yaml, toml, or json for the formatting. My example above is using yaml.

Now test it out (after saving the file!) by creating a new post:

```Bash
hugo new content post/test_new_archetype.md
```

If you followed along to this point, your new post should include the same exact text you placed in your `./archetypes/default.md` file!