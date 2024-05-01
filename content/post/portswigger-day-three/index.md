---
title: "Portswigger Academy Day Three"
description: "Notes about third (ish) day working on portswigger academy"
date: '2024-04-27'
draft: true
lastmod: '2024-04-27'
categories:
- Learning
tags:
- portswigger
- post
license: CC BY-NC-ND
comments: true
showToc: true 
TocOpen: true
image: "portswigger-02.jpg"
---

This time we're doing API hacking woooo

## API Recon
Basically, you need to figure out as much as you can about the API. This starts with finding endpoints that can be interacted with.

Next, you need to determine how to interact with the API. This means determining some of the following:
- What data does the API process?
- What types of requests does the API accept?
- What are the rate limits and authentication methods

### API Documentation
Another good place for recon is API documentation. It is there for developers to get a better understanding of how to utilize the API, but can provide helpful information on the functionaility of the API that might lead to possible exploitations.

If you are having trouble finding it publicly available, you may be able to crawl websites that use it and find the documentation there. Finding endpoints like `/api`, `/swagger/index.md`, or `/openapi.json`. 

If you find an endpoint, make sure you invesitgate the base path. For example, if you find the endpoint `/api`, you should explore all endpoints branching from there. 

This might be something like `/api/swagger` or `/api/swagger/v1` etc.

### Lab
This lab was pretty straight forward you just had to go to `/api` and utilize the `DELETE` tool from the tool kit present there.