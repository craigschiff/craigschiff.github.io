---
layout: post
title:  "SQL Injection  "
date:   2017-02-27 13:29:07 -0500
---

Flatiron more than anything else makes learning to code fun, and that joy is exhibited on a daily basis by our lecturer, [Steven Nunez](http://hostiledeveloper.com/), finding such genuine excitement in all of the different nuances of programming. This often results in tangents where someone gets him started on a topic he is clearly interested in and he continues down the rabbit hole for the next 10 mins. These are often the most interesting parts of the day, and I find it genuinely useful to hear about practical applications as we are learning, even if the use is many steps beyond what we are capable of applying. My favorite of these tangents was when SQL injection came up after learning the basics of SQL.

**Context** 

SQL injection is one of the oldest known hacking vulnerabilities, and over time languages have continued to protect themselves as best they can. However, the web is full of outdated websites and servers, and thus SQL injection remains one of the top vulnerabilities today. In December 2016 [Netsparker said this is still the most common vulnerability](http://www.netsparker.com/blog/web-security/sql-injection-vulnerability-history/). 

**How do they do it?**

SQL injection is a method of completing forms with a malicious command for the databse (SQL) to directly interpret. A hacker can use this to access other accounts. They can read and potentially modify sensitive information.  
A simple example is with a login field. When a user is logging into a website, the database is matching the user’s input against the users they have stored in their SQL database However, instead of inputting an applicable username or password a user can input something like ‘a’ OR 1=1--.
The OR statement allows one to proceed if either is true and the second statement will always be true. The '--' at the end tells SQL to end the command, thus a password is not even necessary. 
```
SELECT * FROM Users WHERE name = ‘a’ OR 1 = 1 --' AND password = 'xxxxx'
```

**ORM's** 

An app likely has an Object Relational Mapping system to interact with SQL. For Ruby the one we have used is ActiveRecords. This provides a basic layer of protection. However, key vulnerabilities remain, especially in scenarios where the commands “inject” or accept direct user input. This is applicable for commands such as "group_by," "where," "having," "find_by," "pluck," and many others. 
Additionally, the sequence of the course is such that we interacted with SQL first, and thus when transitioning to ActiveRecords I had a tendency to just add quotes around the ActiveRecord command and thus interpret SQL code directly, like in the below. 
```
User.where("name = '#{params[:name]'")
```

Commands like these result in a vulnerable website. BAD!

**Prevention** 

The benefit of SQL injection’s longevity is the methods are well known and languages have been able to enhance preventative capabilities. The overriding principle should be to never trust users, and confine their inputs to the maximum extent possible. The ORM is the intermediary between the user and the database. A developer needs to fully utilize this to secure their database, and this means never limiting direct interactaction with SQL, and always sanitizing inputs first.  There are two alternatives to the vulnerable code above. 
```
User.where({ name: params[:name] })
```

This method names the column rather than passing it through as a quote limiting the scope of user access. This is similarly applicable to commands like "find_by". If instead users named the key (or column of the table) in the method and just pass in the variable they would again limit the scope of user access. And finally, the best defense mechanism is to **always** parameterize inputs:

```
User.where(["name = ?", "#{params[:name]}"])
```

Using parameterized variables offers substantial protection against SQL injection. This allows for sanitization via Regular Expression before sending a command along to the database. When this is uniform across the web user's sensitive information will be much safer. 


**Resources**: 

Jess Rudder’s [fabulous talk](https://www.youtube.com/watch?v=gV7laXXBpBY) at Ruby conference in Colombia 

Netsparker has tons of resources on [SQL Injection](https://www.netsparker.com/web-vulnerability-scanner/vulnerability-security-checks-index/sql-injection/)  

[OWASP](https://www.owasp.org/index.php/SQL_Injection) has long been spreading awareness

And rails specific vulnerabilities are detailed here:  [rails-sqli](http://rails-sqli.org/)

