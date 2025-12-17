## HOST UTILITY

`host` is a small DNS lookup utility that translates domain names to IP addresses (and vice-versa) and queries specific DNS record types (A, MX, TXT, NS, etc.). It’s designed for quick, human-readable DNS checks.

READ HOST MANUAL
```
man host
```

SIMPLE HSOT COMMAD
```
host -t MX google.com
```

__________
## ROBOTS.TXT

`CHECH - /ROBOTS.TXT`


_________________

## SITE_MAP.XML

### The Technical Definition

A `sitemap.xml` is a file on a website that provides a list of all the pages, videos, and other files on the site, along with optional metadata about each one. This file is written in a specific code language called XML (eXtensible Markup Language) that search engines can easily read.

Its primary purpose is to **help search engines discover and understand the content on your website more effectively.**

`CHECK /SITE_MAP.XML OR SITE_MAPS.XML`

_________________
## WHATWEB UTILITY

INBUILT IN KALI

**WhatWeb** is a website fingerprinting tool, a next-generation version of older tools like `httprint`. Its primary purpose is to **recognize web technologies** by analyzing a target website's responses.

It doesn't just identify the obvious things like the CMS (e.g., WordPress); it digs deeper to find specific versions, email addresses, plugin use, module IDs, and much more. It is a cornerstone tool for the **Information Gathering** phase of a penetration test or security assessment.

```
whatweb http://target.ine.local
```

OUTPUT

```
http://target.ine.local [200 OK] Apache[2.4.41], Country[RUSSIAN FEDERATION][RU], HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.41 (Ubuntu)], IP[192.124.197.3], MetaGenerator[WordPress 6.5.3 - FL@G2{704d19c49c00424aa2ec9aedb15324a1}], Script[importmap,module], Title[INE], UncommonHeaders[link], WordPress
```

______________
## What is HTTrack?

**HTTrack** is a free, open-source **website copier** and **offline browser** tool. Its core function is to download a website from the internet to your local computer, replicating the site's directory structure and all its assets (HTML, images, PDFs, CSS, JavaScript, etc.). Once downloaded, you can browse the site locally on your machine without an active internet connection.

It is often called a **"web scraper"** or **"website mirroring"** tool.

INSTALL IN KAIL
```
sudo apt-get install webhttrack
```

_________

## ADD-ONS OR EXTENSIONS TO USE


### What is BuiltWith?

First, it's essential to understand the core **BuiltWith** service. BuiltWith is a powerful **B2B lead generation and competitive intelligence tool**. Its primary function is to "look under the hood" of any website and tell you:

- **What technology it uses:** This includes everything from the web server (Apache, Nginx) and programming frameworks (React, WordPress, Shopify) to analytics tools (Google Analytics), advertising networks (Google AdSense), widgets (live chat), and payment gateways (Stripe, PayPal).
    
- **Who uses a specific technology:** You can find all the websites that use a particular tech, e.g., "find all e-commerce sites in the UK that use Magento and Hotjar."


## **Wappalyzer**
 A **technology detection library and discovery tool**. It's for **identification** (e.g., "What is this website built with?").


