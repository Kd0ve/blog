---
title: "Portswigger Academy Day Three"
description: "Notes about third (ish) day working on portswigger academy"
date: '2024-05-15'
draft: false
lastmod: '2024-05-16'
categories:
- Learning
tags:
- portswigger
- post
license: CC BY-NC-ND
comments: true
showToc: true 
TocOpen: true
image: "portswigger-03.jpg"
---

---

This took a while for me to get through. I kept working on small sections at a time but wanted enough content for a post before I posted. So, although it says, "Day Three", it's more like day 6 or 7.

In this post, we're going to be covering part of the API Testing module. This is all about utilizing API on web apps to force changes to be made to content on the website, or potentially leak valuable information.

---

## API Recon
Basically, you need to figure out as much as you can about the API. This starts with finding endpoints that can be interacted with.

Next, you need to determine how to interact with the API. This means determining some of the following:
- What data does the API process?
- What types of requests does the API accept?
- What are the rate limits and authentication methods

## API Documentation
Another good place for recon is API documentation. It is there for developers to get a better understanding of how to utilize the API but can provide helpful information on the functionality of the API that might lead to possible exploitations.

If you are having trouble finding it publicly available, you may be able to crawl websites that use it and find the documentation there. Finding endpoints like `/api`, `/swagger/index.md`, or `/openapi.json`. 

If you find an endpoint, make sure you investigate the base path. For example, if you find the endpoint `/api`, you should explore all endpoints branching from there. 

This might be something like `/api/swagger` or `/api/swagger/v1` etc.

### Lab
This lab was straight forward you just had to go to `/api` and utilize the `DELETE` tool from the tool kit present there.

## Identifying and Interacting with API Endpoints
### Identifying
This section starts off with finding and identifying API endpoints. You'd do this by essentially just crawling applications that use the API. While doing this, you would want to look for patterns that suggest API endpoints such as `/api/`. 

You want to keep an eye out for JavaScript files particularly. These can contain calls to API endpoints that you may not have triggered yet. Burp Scanner automatically extracts some endpoints while its craws, but for more in-depth extraction, you can use JS Link Finder BApp, or you can manually review the JavaScript files.

### Interacting
To interact with API endpoints, you want to use Burp Repeater and Burp Intruder. These will allow you to observe the APIs behavior based on changes you make information sent to the API. This could be like observing what happens when you change the HTTP method and media type.

You want to keep an eye out for error messages while you do this as it could point in the direction of a valid request, which you could build and use to exploit the API.

### Identifying Supported HTTP Methods
This section basically just goes over the different HTTP methods. So `GET` retrieves resources, `PATCH` applies partial changes to resources, and `OPTIONS` gets information on the types of requests that can be used on resources.

API endpoints may support other methods such as `GET`, `POST`, and `DELETE`. Burp Intruder has a list to automatically cycle through methods.

### Identifying Supported Content Types
You can exploit APIs by changing the content type. To change the type, you can change the `Content-Type` header. You can use the Content Type converter BApp which converts data automatically between XML and JSON.

### Lab
Here, we must buy the jacket item for free. I can see that there's an API endpoint at `/api/products/1/price`. The only methods allowed are `GET` and `PATCH`. The `GET` method outputs the price and message for the item. 

My assumption is that `PATCH` needs to be used somehow, but `PATCH` outputs a message saying "Unauthorized". I think maybe I must reformat the `PATCH` request. 

You basically had to use the BApp Content Type Converter to utilize the `PATCH` method. You had to put in `{"price": 0}` to get the API to change the price of the jacket to "$0.00".

### Using Intruder To Find Hidden Endpoints
You can utilize known endpoints to find other hidden endpoints. You can use Burp Repeater to parse through different common endpoint names and find possible hidden endpoints that could be exploited.

## Finding Hidden Parameters
It's possible to find hidden undocumented API parameters that can be exploited. These can be used to change application behavior. Burp has a few tools to help find hidden API parameters.

- Burp Intruder can be used to parse through common parameter names. You should also include potential parameter names based on API recon.
- You can also use the Param Miner BApp to iterate through possible parameters.
- The Content discovery tool can help to find hidden content, including possible parameters.

---

This was all for this section. Although these are going to be labeled in order by "Days", they aren't really going to be just individual days. Each section might be a combination of multiple days of study.

This information has been valuable to learn in my opinion. I hadn't ever considered exploiting API on a website, but now that I've done it, I get the idea behind it.

In the next post, we will continue exploiting websites utilizing API. I'm looking forward to learning more about this topic, and I hope you're enjoying following along!

---

