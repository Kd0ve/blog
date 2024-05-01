---
title: "Portswigger Academy Day One"
description: "Notes about first day working on portswigger academy"
date: '2024-04-24'
lastmod: '2024-04-25'
categories:
- Learning
tags:
- portswigger
- post
license: CC BY-NC-ND
comments: true
showToc: true 
TocOpen: true
image: "portswigger-01.png"
---

---

Today I'm making my first entry in what I hope will be a series of posts on my daily learning at Portswigger Academy! Please let me know if you enjoyed this post, and without further ado, here are my notes for day one!

## Path Traversal
Starting off I'm going down the server-side vulnerabilities path, which covers a variety of common server-side exploits. 

Starting off, they went over path traversal, which essentially is a vulnerability that allows you to access different areas of the server than you otherwise should be able to. The example that Portswigger Academy gives is there being a path traversal exploit within the image viewing.

This would look like `url-here.com/file?image=image.jpg`, where you can then put in something like `url-here.com/file?image=../../../etc/passwd`. The `../` is a valid file path, and you would put it there 3 times to traverse the path `/var/www/site`.

---

## Access Control
There are many vulnerabilities that can be abused when it comes to access control. 

### Vertical Privilege Escalation
Here, we looked at being able to traverse to other accounts using settings shown in the url, such as `url.com/login?id=123` being able to be changed to a different ID number, or being able to find an exposed area of the website in something like the robots.txt file.

### Unpredictable URL
This is where they may obfuscate the url path of something, such as the admin panel, to prevent people being able to access them. However, often these obfuscated URLs are exposed within the HTML of the website or scripts running on the website.

### Parameter-based Access Control
This one was interesting. Often, a cookie will be set to determine access rights. The thing about this is that cookies can be easily edited, which can allow the user to change their own privileges on the website.

In the example, they had us login to an authenticated account, which had an admin cookie set to false. You can easily use a cookie editor and change the admin setting in that cookie to "true".

### Horizontal Privilege Escalation
This relates to traversing different accounts using things like user IDs, often GUIDs. Although sometimes these ID settings are in a predictable pattern, such as being set to the username of the user, often they are a random string of numbers and letters used to identify different accounts.

### Unpredictable ID
This is a similar idea to the unpredictable URL, where the ID is unpredictable. In the example lab, we had to go to the user account from their blog posts, which would leak the GUID in the URL, which can be input in the "My Account" panel, which will go to their account, where the password was exposed in the HTML.

---

## Authentication Vulnerabilities
These are vulnerabilities where authentication can be easily bypassed. Authentication is the process of verifying that the user is who they claim to be, while the similar term, Authorization, is verifying that the user is *allowed* to do something. This would be related to user permissions like being an administrator account.

### Brute Force
This is where credentials are rapidly attempted on a login page. There are small indications in something like Burpsuite that the brute force attack has found something. Particularly with usernames, websites will often specify if the username or password is incorrect, which is a security flaw as this can indicate a user that exists on the website. You can tell the difference by the response size.

With passwords, once you find a username to test against, there will usually be a 302 status code returned, although paying attention to response time, response size, and possible other status codes that vary from the one consistently being returned is a good thing to do as well.

### Bypassing Two-factor Authentication
Sometimes, even though two factor authentication is implemented, if a user has already authenticated with their password, sometimes other areas of the website that should be restricted to only be accessed when logged in are still accessible, even though the two-factor authentication wasn't done.

## Server-side Request Forgery (SSRF)
These are attacks where the attacker may be able to get the server-side application to make requests to unintended locations. This would be like making the server connect to internal-only services, or making the server connect to an arbitrary external system, which could leak sensitive data.

In the lab, the requirement was to just request `http://localhost/admin`, however that wouldn't delete the user you needed. You needed to find out what the URL was that deleted the user required, which was `http://localhost/admin/delete?username=carlos`, and put that in the `stockApi` request. It was much easier to do in burpsuite.

### Against Other Back-end Systems
Here, instead of doing something like `http://localhost/admin`, you would replace `localhost` with an internal address such as `192.168.0.68`. So, the request would look like `stockApi=http://192.168.0.68/admin`, which can give you access to the administrative panel of this internal system.

This one is a pain. You had to iterate through all the IPs in the `192.168.0.*` range which takes forever on the community version of burpsuite. Ther's probably a different way to do it faster outside of burpsuite, but anyway, once you find the one that pops the 200 status, you just find the URL for deleting carlos and boom you've got it. 

Same thing as last time essentially but you use the internal address you find.

## File Upload Vulnerabilities
These types of vulnerabilities occur in file upload areas of a website. This is where a file upload doesn't verify the filetype, which means you are able to upload code that may open shells within the system. The task in this lab is to upload a php script and gain access to the contents of a certain file. The php code I'm going to use is:

```
<html>
    <body>
    <form method="GET" name="<?php echo basename($_SERVER['PHP_SELF']); ?>">
    <input type="TEXT" name="cmd" autofocus id="cmd" size="80">
    <input type="SUBMIT" value="Execute">
    </form>
    <pre>
        <?php
            echo file_get_contents('/home/carlos/secret');
        ?>
    </pre>
    </body>
</html>

```
This shows the contents of that file, which is where the required flag is.

### Exploiting Flawed File Upload Validation
Sometimes, even when a webpage tries to validate the file-type, it doesn't do a very in-depth job of verifying. Because of this, you can use burpsuite to change the `Content-Type` header to match the MIME type that would allow your file. This is able to happen when the "header is implicitly trusted by the server" according to portswigger academy.

The lab here had you upload the same shell but intercept it using burpsuite. Once you intercepted it, you had to change the content type to image/jpeg, which would allow the file to be uploaded.

--- 

This is all of what I did on my first day going through this free course. If any of you want to try the course out, go to [https://portswigger.net/web-security](https://portswigger.net/web-security) and register an account. There are a bunch of learning paths, so if you already feel versed in certain areas, you can go to others and try them out. 

I'm hoping to continue to update each day as I work on these. I'm going to use these posts to take notes, and then post them at the end of the day with all the content I've learned. Hopefully this entry piqued your interest, and you look into doing these courses yourself! 

If you made it this far, I appreciate you reading. You can email me if you have anything you want to share, feel free to comment below! I'm hoping these note posts will keep me accountable with doing these pathways, and that it'll be a good way to document my journey trying to get into web app exploitation!