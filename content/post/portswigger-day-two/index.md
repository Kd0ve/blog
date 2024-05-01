---
title: "Portswigger Academy Day Two"
description: "Notes about second day working on portswigger academy"
date: '2024-04-26'
lastmod: '2024-04-26'
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

---

Today is my second day going through the PortSwigger Academy learning paths! We're continuing with the Server-side Vulnerabilities pathway, where we will finish up with OS Command Injection and SQL Injection.

---

## OS Command Injection
This is a shell attack that allows the execution of OS commands, which can be used to fully compromise a system running the application at hand. 

It also gives an avenue for potentially changing internal permissions, allowing for higher privileges within the system than you may have initially been able to gain access to.

### Useful Commands
Here are some useful commands to execute to help identify there being an OS command injection vulnerability:
|**Purpose of command**|**Linux**    |**Windows**    |
|----------------------|-------------|---------------|
|Name of current user  |`whoami`     |`whoami`       |
|Operating system      |`uname -a`   |`ver`          |
|Network configuration |`ifconfig`   |`ipconfig /all`|
|Network connections   |`netstat -an`|`netstat -an`  |
|Running processes     |`ps -ef`     |`tasklist`     |

### Injecting OS Commands
Being able to execute these is like an SQL injection. In the URL, often a parameter will be established to be set to an input variable. 

You can exploit this by doing something like `& echo aiwefwlguh &` which will make the system execute the `echo` command and echo the specified string. 

This is a good way to test if OS Command Injection is a possible vulnerability that can be exploited.

### OS Command Injection, Simple Case
This lab had us do something that wasn't shown in the instructions beforehand. Rather than doing something like `& whoami &`, the lab wanted the input to be `| whoami` put after the input storeID.

I'm assuming they wanted us to look up other ways to do the injection, but it's kind of weird they show one way to do the injection method, but have us physically do a different method.

Anyway, the result looked like `productId=1&storeId=1 | whoami` which outputs the name of the user.

---

## SQL Injection
I referred to this earlier because I'm relatively familiar with SQL injections. This type of vulnerability takes advantage of a website using an SQL database where input isn't filtered.

If there is an input form where the results are passed to an SQL query, there are times when you can put in an SQL command with some slight character manipulation, and it will pass the query as an actual SQL query.

According to the course material, SQL injections can allow for DDoS attacks, which I was not aware of and find very interesting.

### Detect SQL Injection
According to Portswigger, the following are methods that can be used to test for the potential of an SQL injection being possible:

> - The single quote character ' and look for errors or other anomalies.
> - Some SQL-specific syntax that evaluates the base (original) value of the entry point, and to a different value, and looks for systematic differences in the application responses.
> - Boolean conditions such as OR 1=1 and OR 1=2 and look for differences in the application's responses.
> - Payloads designed to trigger time delays when executed within a SQL query and look for differences in the time taken to respond.
> - OAST payloads are designed to trigger an out-of-band network interaction when executed within a SQL query and monitor any resulting interactions.

### Retrieving Hidden Data
Doing some tests such as '+OR+1=1 at the end of a URL performing an SQL query can result in the dropping of an entire database, even hidden elements.

Portswigger gives the following warning, which I think is important to remember:

> **Warning**
>
> Take care when injecting the condition OR 1=1 into an SQL query. Even if it appears to be harmless in the context you're injecting into, it's common for applications to use data from a single request in multiple different queries. If your condition reaches an UPDATE or DELETE statement, for example, it can result in an accidental loss of data.  

### SQL Injection Lab
This one was straightforward. You just did the '+OR+1=1-- that was talked about earlier in the learning and it dropped all the products.

### Subverting Application Logic
This one is cool. You can try to subvert the SQL logic that is written out by inserting certain test characters into certain fields.

For example, if you wanted to log in as a user without needing to provide a password, and the SQL logic looked like `SELECT * FROM users WHERE username = 'user' AND password = 'pass'` if you just put in `user'--` into the username field, it would allow you to bypass the password check.

### Subverting Application Logic Lab
I got kind of confused here because I thought you had to leave the password field blank, but when you would do that the submission thing would freak out.

What you had to do was input a password and then alter the username to have `'--` at the end, which would then make the password field blank and not necessary to log in.

---

That's all for day two! And that is the end of the Server-side Exploitation path. Next time, I'll be moving onto the API testing pathway, where I'll learn about how to exploit API.

Thank you all for reading, and I hope to continue providing my notes as I go along on these learning paths! They've been informative so far, and I feel like I am learning a lot.


