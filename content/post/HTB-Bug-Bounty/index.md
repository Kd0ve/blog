---
title: "HTB Bug Bounty - HTTP Fundamentals"
description: ""
date: 2024-06-06T20:24:43-04:00
lastmod: 2024-06-06T20:24:43-04:00
draft: false
categories:
tags:
license: CC BY-NC-ND
comments: true
image: 'hack-the-box-e1694085620820.png'
---

---

This is just some notes I took while going through the Bug Bounty course in Hack The Box Academy. I figured I would post it in case it would help others learn.

Apologies if it seems everywhere! I didn't go through and clean anything up :3

---

## HTTP Fundamentals
### HyperText Transfer Protocol (HTTP)
**Fully Qualified Domain Name (FQDN)** - essentially the domain name of the website
**Uniform Resource Locator** - this includes the FQDN as well as different parts like the top-layer domain, subdomain, query string, and fragment.

There are different parts of a URL
*Scheme* - this is like `http://` or `https://`
*User info* - this would sometimes be a part of the url, but not always. It looks like `admin:password@` and then the main domain name.
*Host* - this includes the domain name and sub domain. It looks like `hackthebox.com`
*Port* - sometimes included, but not always necessary. Will look like `:80` after the host.
*Path* - this immediately follows either the host or the port if the port is specified. Looks like `/dashboard.php`
*Query String* - this is also sometimes included if needed to look up something. Will look like `?login=true` or similar
*Fragments* - this is to specify a section within the main resource. Will look something like `#status`

**HTTP Flow**
User -> DNS -> IP
DNS sends IP -> User
User -> Send HTTP Request to IP/Web Server
IP/Web server -> Send HTTP Response to User

> Basically, the user requests the IP of the domain specified to the Domain Name Server. The DNS sends the IP of the web server to the User. The User then sends an HTTP or HTTPS request to that IP address. That web server at that IP address sends back an HTTP or HTTPS response.
> Request: GET /HTTP/1.1
> Response: HTTP/1.1 200 OK

> **Side Note**
> Typically, on a Linux system, the browser will first look in `/etc/hosts` to find the IP address associated with the domain name. You can use this to specify domain names for IP addresses that don't necessarily have one assigned with a DNS. HTB wants this to be done quite a bit :)

**cURL**
This is a really important command, as you can send HTTP requests and see the response in terminal. You can also do stuff like getting the `index.html` file that is rendered when you first request the main website. You can do this using the `-O` flag in curl. You can also use the `-o` flag to specify just a general output file. 

Calling the `index.html` file for say `hackthebox.com` would look like `curl -O hackthebox.com/index.html`, and you should have created a copy of that domain's `index.html` file that's been requested. You can also do the `-s` flag to silence the status and the `-O` flag in conjunction to just straight grab the `index.html` file without the status.

### HyperText Transfer Protocol Secure (HTTPS)
One drawback to HTTP is that it sends everything over plaintext, which is pretty dangerous. This is why HTTPS was created. It utilizes TLS/SSL to encrypt traffic sent over the protocol. This even includes login credentials if they aren't hashed client-side. 

If someone intercepts HTTPS traffic, they won't be able to see the data within that request. You can tell if a website is using HTTPS as it will have `https://` in the beginning of the URL. Most browsers will also warn you now before you visit a website only using HTTP.

HTTPS is where the three-way-handshake is employed. This is to make sure that the communication is secure and stable before transferring any information between the client and host. There are different response codes depending on the status of the requested page as well.

**cURL For HTTPS**
Using curl with HTTPS, there might be an issue where there will be an SSL Certificate problem saying that the certificate is invalid. To bypass this, when you are doing the cURL command, you can add on the `-k` flag to the curl command. 

### HTTP Requests and Responses
HTTP requests and responses are structured in particular ways that are consistent. The send request information will cause certain responses, so they must be formatted correctly to get the intended response back. Browsers will craft these automatically.


**HTTP Request**
In a request, there are three main aspects of the header. There will be the `method`, `path`, and `version`. The `method` will be something like `GET`, `POST`, `DELETE`, and more. There are a lot of different kinds of methods.

The `path` was somewhat covered earlier, but generally will look like `/index.html` if you're calling that particular file. The path is whatever resource you are requesting. The `version` is the version of HTTP being used.

Following these, there tend to be a lot more parameters, but the important thing for now is those main three request parts. 

**HTTP Response**
With this, in the first line, there are two main fields returned. That will be the HTTP `version`, and the response code. The response code denotes what ended up happening with the request. Typically, a `200 OK` response means that the page was returned successfully. Something like a `404 ERROR` response will be that the page doesn't exist.

Again, there will be a lot more perameters in the header, but these two are the main things to pay attention to.

**cURL**
Utilizing the `-v` flag will show what is send more clearly. You can also use `-vvv`, which will increase verbosity, or the information you are shown about the exchange. 

**Browser DevTools**
Basically, we're talking about inspect element. I typically like using the FireFox DevTools as you can much more easily edit requests right there in the toolkit. For Chrom or FireFox, you can press `CTRL+SHIFT+I` or click `F12` to pull up the toolkit.

You'll mainly want to focus on the `Network` tab to grab files being transferred and requests being performed. Here is where you'll be able to find the requests to alter. You can apparently use the `Filter` to filter out and search certain requests if a website loads too many to parse through.

### HTTP Headers
Since we didn't discuss them before, they're covered here thankfully (somewhat). We're going to go over a few different kinds of headers that you'll typically encounter. There are general, entity, response, request, and security headers. 

For general headers, you'll either encounter the date, or the connection status. The date is well, the date. The connection status will either be `close` or `keep-alive`. It will commonly look like `Connection: close` or `Connection: keep-alive`.

For entity headers, there are quite a few. There's content-type, media-type, boundary, content-length, and content-encoding. The content-type specifies the kind of content accepted. The media-type defines the data that's being transferred. The boundary acts as a marker to seperate content. 

The content length contains the size of what's being passed. The content-encoding defines the kinds of transformations the data goes through. This means that, if the data being transferred is large, it will denote that the content-encoding was compressed, and what kind of compression was used.

