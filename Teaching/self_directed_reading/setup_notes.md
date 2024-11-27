---
layout: page
title: How This Site is Configured
permalink: /teaching/self_directed_reading/setup_notes/
parent: Self-Directed Reading
grand_parent: Teaching
---
# How This Site is Configured
{: .no_toc } 

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Site Basics
This site is currently using thr Just the Docs theme. The details are available here: [https://just-the-docs.com](https://just-the-docs.com). I have included some of the basics of my setup below for reference. 

To develop the code and see the output in real time in localhost, use the following code:
```
bundle exec jekyll serve --watch
```

## Using a Table of Contents

This code ensures that a header is not included in the TOC:
```
# This is a page
{: .no_toc } 
```

This code creates a collapsable TOC in file:
```
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>
```

## Using Mermaid

I have set up Mermaid in the `_config.yml` page by using 
```
mermaid:
  version: "10.9.1"
```
Choose the version available from [https://cdn.jsdelivr.net/npm/mermaid/](https://cdn.jsdelivr.net/npm/mermaid/).  

This allows me to use mermaid as I normally would:
```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
``` 

## Using Callouts

To insert a callout into the text, use the following code:

```
This is the text of the callout  
{: .note } 
```
The output will look like this:

This is the text of the callout  
{: .note }

I have setup the callouts in the `_congig.yml` file as
```
callouts:
  note:
      title: Note
      color: purple
      opacity: 0.3
  highlight:
      title: 
      color: yellow
      opacity: 0.3
  historiography:
      title: historiography
      color: blue
      opacity: 0.3
  method:
      title: method
      color: green
      opacity: 0.3
```

### Examples:

#### Untitled Highlight
```
A highlight
{: .highlight }
```
A highlight
{: .highlight }

#### A Custom Title Callout with Multiple Paragraphs

```
{: .note-title }
> My note title
>
> A paragraph with a custom title callout
>
> Another paragraph
```

{: .note-title }
> My note title
>
> A paragraph with a custom title callout
>
> Another paragraph

## Collapsable Sections
This theme uses the `<details>` tag to create collapsable sections of text  

<details>
This is a collapsable list:
1. item 1
2. item 2
3. item 3
</details>


## Disallow AI Crawlers
I recommend using the robots.txt code found here: [https://github.com/nithinbekal/nithinbekal.github.io/blob/47b804179d111fcfe9a4914ccf6618263b0da3bd/robots.txt](https://github.com/nithinbekal/nithinbekal.github.io/blob/47b804179d111fcfe9a4914ccf6618263b0da3bd/robots.txt)


## Setting Up Social Media Icons
There are numerous ways to include social media icons in your website. The section below outlines how to use data files in Jekyll with Font Awesome. 

### Font Awesome
[Font Awesome](https://fontawesome.com) is a website that allows users to easily utlize a set of standardized icons in their websites. 

You will fitst need to set up an account with Font Awesome and get access to your icon kit. The kit will give you a line of code, which you can embed in your `head_custom.html` page in your `_includes` folder. If you do not already have a `head_custom.html` file, you can create one in the `_includes` folder. This file will not overwrite what is already in your `<HEAD>` section; it will only supplement it. 

The code provided by Font Awesome connects your website to a javascript file on the Font Awesome server. Once you have this, you can use thousands of icons. 

Note that you do not have to use Font Awesome. This is simply one way to manage your icons. 

### Implement Font Awesome on your Website
To setup your pages, Jessical Reel has a [good introduction at jreel.github.io](https://jreel.github.io/social-media-icons-on-jekyll/). The process takes place in two phases: 1) setting up a data file, 2) creating a template to display your data file; 3) displaying the template in your page. 

#### Setting Up Your Data File
Jekyll includes [support for "data files"](https://jekyllrb.com/docs/datafiles/) through the use of a `_data` folder. If you do not already have one, you can create one in your root folder. The documentation notes that Jekyll supports YAML, JSON, CSV, and TSV. In this tutorial, we will use YAML. 

The purpose of a data file is to create a single place from where you can pull information. Take the example of your email address. If you hand coded your email address on every page and then changed your email, you would need to go through every page and rewrite your new email. However, if you had it in a data file, you would need to update it only once. Data files are quite useful for data that you need to call up multiple times. 

To setup your data file for your social media icons, you can use the following format: 

```
email:
  id: 'jason@jasonmkelly.com'
  href: 'mailto:jason@jasonmkelly.com'
  title: 'Email'
  fa-icon: 'fa-envelope-square'

mastodon:
  id: '@jason_m_kelly'
  href: 'https://fediscience.org/'
  title: 'Mastodon'
  fa-icon: 'fab fa-mastodon'

github:
  id: '6500jmk4'
  href: 'https://github.com/'
  title: 'GitHub'
  fa-icon: 'fa-github-square'

instagram:
  id: 'longeighteenthcentury'
  href: 'https://www.instagram.com/'
  title: 'Instagram'
  fa-icon: 'fa-instagram'

linkedin:
  id: 'jasonmkellylinkedin'
  href: 'https://www.linkedin.com/in/'
  title: 'LinkedIn'
  fa-icon: 'fa-linkedin-square'
```

You can add as many or as few icons as you want. However, you need to be aware that not all icons follow the same format. It's simple to look them up on the Font Awesome website.

You can save this file as any name you want, but you will need to end it with the `.yml` extension. 

#### Creating a Liquid Template to Display Your Data

Now that you have your data file, you will want to pull it up in your website. To do this, we need to create a template file. You can name it anything you want, but we are following Jessica Reel and naming it `social-media-links.html`.

In your `_includes` folder, create an html file. It can have any name you like. Jessica Reel offers the following template to get you started. The template is written with [Liquid](https://jekyllrb.com/docs/liquid/), which is the templating language used by Jekyll.

```
{% if site.data.social-media %}
<div id="social-media">
    {% assign sm = site.data.social-media %}
    {% for entry in sm %}
        {% assign key = entry | first %}
        {% if sm[key].id %}
            <a href="{{ sm[key].href }}{{ sm[key].id }}" title="{{ sm[key].title }}"><i class="fa {{ sm[key].fa-icon }}"></i></a>
        {% endif %}
    {% endfor %}
</div>
{% endif %}

```

You do not have to understand Liquid, but it is important to know that the code `site.data.social-media` is the path to the YAML data file you created in the first step (social-media would be the name of the file you created).

#### Displaying the Template in Your Page
To display the template in your page, all you need to do is add the following code:
```
{% include social-media-links.html %}
```
As you will note, this simply calls up the html file we just created.



