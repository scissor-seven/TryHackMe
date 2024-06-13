#_2021_ #common_vulns #web_exp 
# >> Sudhanshu Chatterjee | Jun 6th' 24
## -> TASK 1 - INTRODUCTION
OWASP Top 10 vulnerabilities; the 10 most critical web security risks, as of 2021 :

| 1. ***Broken Access Control***                      | [[#-> TASK 3 - 1. BROKEN ACCESS CONTROL]]                       |
| --------------------------------------------------- | --------------------------------------------------------------- |
| 2. ***Cryptographic Failures***                     | [[#-> TASK 5 - 2. CRYPTOGRAPHIC FAILURES]]                      |
| 3. ***Injection***                                  | [[#TASK 9 - 3. INJECTION]]                                      |
| 4. ***Insecure Design***                            | [[#-> TASK 11 - 4. INSECURE DESIGN]]                            |
| 5. ***Security Misconfiguration***                  | [[#-> TASK 12 - 5. SECURITY MISCONFIGURATION]]                  |
| 6. ***Vulnerable and Outdated Components***         | [[#-> TASK 13 - 6. VULNERABLE AND OUTDATED COMPONENTS]]         |
| 7. ***Identification and Authentication Failures*** | [[#-> TASK 16 - 7. IDENTIFICATION AND AUTHENTICATION FAILURES]] |
| 8. ***Software and Data Integrity Failures***       | [[#-> TASK 18 - 8. SOFTWARE AND DATA INTEGRITY FAILURES]]       |
| 9. ***Security Logging & Monitoring Failures***     | [[# -> TASK 21 - 9. SECURITY LOGGING & MONITORING FAILURES]]    |
| 10. ***Server-Side Request Forgery (SSRF)***        | [[# -> TASK 22 - SERVER-SIDE REQUEST FORGERY (SSRF)]]           |
### Proceeding further assuming that we have no previous knowledge of security!
My Take: Pretty standard and informative.
##  -> TASK 2 - ACCESSING THE MACHINE
Nothing as such to do in this one, just access the machine for completing tasks lying ahead!

My Take: Standard Procedure
##  -> TASK 3 - 1. BROKEN ACCESS CONTROL
#BAC [[BAC - Broken Access Control]] 
Basically, what ***Broken Access Control*** means is that the *user of a website having access to protected pages/content of the website or being able to mess with the site management which the user should not be able to access!*

Failing to prevent #BAC can lead to the following:
	- Being able to view sensitive information from other users
	- Accessing unauthorized functionality

There is a paragraph in this task, about a **vuln** found in #_2019_ where a user could request frames from a **private video** and hence reconstruct the whole video! 😱

My Take: [[BAC - Broken Access Control]]  allows attackers to **bypass authorisation**, allowing them to *view sensitive data or perform tasks they aren't supposed to.*
##  -> TASK 4 - BROKEN ACCESS CONTROL (IDOR Challenge)
[[BAC - Broken Access Control]] #idor [[IDOR - Insecure Direct Object Reference]]
### Insecure Direct Object Reference
Probably thinking, *What the hell does this mean!?* Short 'n sweet, being able to access an object within server which being directly referenced by another specific object.

For example, let say you have a URL to your back account which looks like `https://bank.thm/account?id=111111`, and you can see someone else's back account by, let say, changing the `id=222222`, then it can be said as an **IDOR**.

#### --> PRACTICAL
[[PRACTICALS - OWASP Top 10 vulnerabilities (2021)#TASK 4 - IDOR CHALLENGE]] 
>`Log into the website on the provided <MACHINE_IP> with given credentials and try doing what the above example said, you will find the FIRST flag within 5 mins! `

My Take: This example was hypothetical and no such URL exists, also, this was a very simple and efficient manner to explain IDOR. But you can consider other examples which suit your understanding.
##  -> TASK 5 - 2. CRYPTOGRAPHIC FAILURES
#cryptography #common_vulns #web_exp [[ENCRYPTION-DECRYPTION]]
**This refers to any vulnerability arising from the misuse (or lack of use) of cryptographic algorithms for protecting sensitive information.**
Its a basic necessity of any web-app to provide confidentiality at any required level. Failing to do so ends up putting sensitive data, mostly linked to customers *(e.g. names, financial info, personal records, etc)* but more importantly, which everyone fears, *usernames*, *passwords* and more such technical information in open or for sale on dark web.

Complex attacks such as ***[[MITM - Man In The Middle]]*** are advantageous for criminals in this case, as it **would force user connection through a device controlled by them,** furthering their advance with the help of weak encryption on transmitted data. Sometimes, *sensitive data can be found on web server itself, where accessing it would not require any advanced knowledge of networking and such, depending on case.*
##  -> TASK 6 - CRYPTOGRAPHIC FAILURES (SUPPORTING MATERIAL 1)
#SQL #database
*The most common way to store a large amount of data in a format easily accessible from many locations is in a **database**.* Database engines use SQL. **TADA!**

Unlike being usually set up on dedicated servers, sometimes you can find these *databases as files*, stored somewhere, referred to as `flat-file databases`, since they are in *a single file*.

**Why? Because we are lazy**, and it is **easier than setting up and entire database server**. Therefore only seen in smaller web applications. And since it is out-of-scope for us to access a database-server, we'll be going on with `flat-file databases`. (Lazy folks!)

Let say, we do this with our web-app, all good for that, but what about storing it under *root directory* of our website *(i.e. one of the files accessible to the user connecting to the website)?* If fuzzed out, one can download it, look into it and have access to all the information present in `database file`.

Now the challenge, *how to query the `flat-file` database we acquired?* No worries, **most common and simplest format, SQLite Database**. Interaction with these can be made with *any programming languages and with a dedicated client*, **sqlite3** in our case, which is **a default in** many **Linux Distribution**.

You can read the syntax and example part in the room itself. Quite nicely explained over there!

My take: Very important and still quite common if you are updated on hacking related news and happening going on in the world! Go read some news, understood!
##  -> TASK 7 - CRYPTOGRAPHIC FAILURES (SUPPORTING MATERIAL 2)
#hashcracking #encryp_decrypt 
This one is more so about cracking hashes that were shown to be obtained in previous task (or so it says!).

Now that it has come to some **cool part,** here's where *`Kali` can show-off its vast collection of tools 'n tricks for cracking hashes.* **Unfortunately**, they are out-of-scope as well for this room, therefore we're using an online tool called [**CrackStation**](https://crackstation.net/) .
Like, there are many out there, which are better or worse than this one, but this one is being used for this task!

My Take: Just go through the task, you don't have to do anything, just read it and get an idea!
## -> TASK 8 - CRYPTOGRAPHIC FAILURES (CHALLENGE)
#### --> PRACTICAL
[[PRACTICALS - OWASP Top 10 vulnerabilities (2021)#TASK 8 - CRYPTOGRAPHIC FAILURES (CHALLENGE)]]

## TASK 9 - 3. INJECTION
*~ Injection flaws are very common in applications today.* Very well said! These occur when a developer forgets to put some sort of sanitization method for user-input. ***Lazy folks!*** Anyway, injection attacks depend on what technologies are used and how these technologies interpret the input.

2 of the common examples include:
- **SQL Injection** - *This occurs when user-controlled input is passed to SQL queries*. As a result, an attacker can pass in SQL queries to manipulate the outcome of such queries, ending up granting the attacker unchecked, absolute control and access over database.
- **Command Injection** - Similar in some essence to above, except here, attacker *can send arbitrary system commands over application server*, potentially resulting in system takeover. 

Common prevention methods include **sanitization of user-controlled input**, stopping it from being interpreted as *query* or *command*, which can be done in different ways such as:
- **Using an allow list** - Comparing the user-input with a list of allowed/valid input, moving forward only if the input is considered safe.
- **Stripping input** - Unlike previous one, if input contains dangerous/invalid characters, they are removed/sanitized before processing.

***This is that and that is this, its the same damn thing!***
One good this with this which makes it easy is there are *libraries* to perform this so called 'allow-list' or 'stripping' for you!
## -> TASK 10 - 3.1 COMMAND INJECTION
*Command Injection occurs when server-side code (like PHP) in a web application makes a call to a function that interacts with the server's console directly.* An attacker **can run operating system commands,** arbitrarily, using this vulnerability. Further possibilities are endless.
- Worst case scenario, once an attacker takes over the server, now they'll probably enumerate systems to pivot around and get further into the network!

Beyond this point in this task are examples which I highly suggest reading [there](https://tryhackme.com/r/room/owasptop102021) because making conclusions of them would defeat the point of `concept elaboration and explanation!`
#### --> PRACTICAL - Exploiting Command Injection
[[PRACTICALS - OWASP Top 10 vulnerabilities (2021)#TASK 10 - COMMAND INJECTION]].

## -> TASK 11 - 4. INSECURE DESIGN
**Insecure design** refers to *vulnerabilities* which are *inherent to the application's architecture*. They are *not vulnerabilities regarding bad implementations or configurations, but the idea behind the whole application (or a part of it) is flawed from the start*.

**In simple terms, you had a bad start and messed things up right from the beginning!** Reason? Improper threat modelling and assessments during planning phase, go have a read on **[[SDLC - Software Development Lifecycle ]]** & **[[SSDLC - Secure Software Development Lifecycle]]**. Or sometimes it can be lack of discipline like let say, disabling OTP validation of an application during development and testing but then forgetting to re-enable it afterwards.

**Insecure Password Resets**
This one has a great example told on [the room itself](https://tryhackme.com/r/room/owasptop102021), tells about a vulnerability found on [Instagram a while ago](https://thezerohack.com/hack-any-instagram) where an attacker could *`brute-force`* the 6-digit OTP for password reset request, *if they had around 4k IP's* since *each IPv4 had a limit of 250 OTP attempts!* And getting all those IP's is not a big problem since many *cloud services make it easily available for a small cost!* **Damn scary ain't it!😨** 

Since the flaw is in the design itself, often times the only way to resolve this is with rebuilding the vulnerable parts, which were ignored in early stages from scratch! **Yep, that's what you get for being lazy!** The best way to be safe from this, *stop being lazy in the early stages of development and do it right from the start!* Wanna read how to do that - [TryHackMe | SSDLC](https://tryhackme.com/r/room/securesdlc), or you can read wherever you like to read about such topics!
#### --> Practical Example
Do the exercise present in this task and get a feel for it!
[[PRACTICALS - OWASP Top 10 vulnerabilities (2021)#TASK 11 - INSECURE DESIGN]].

## -> TASK 12 - 5. SECURITY MISCONFIGURATION
**This one can be said as one of the worst mistakes a security engineer can make** Yes! I said it, that's all it is. This occurs when *security could have been appropriately configured but was not*, which makes sense only in 2 cases -
- Either you are an accomplice of a criminal organization i.e. you did this on purpose,
- or you are just dumb, lazy and disconcerted professional!

If it's case 1, **congrats**! Job well done, promotion secured!, if case 2, just.... find something else to do, you ain't made for this!
Jokes apart, some of the common misconfiguration are included in the `task-text`, not gonna write them here, go read [there](https://tryhackme.com/r/room/owasptop102021).

**Debugging Interfaces**
And another one the **stupidest mistakes** one can make is **leaving debugging features exposed** in production software! Provided by some *programming frameworks*, debugging functionalities can be exploited by hackers with malicious intent.

#### --> Practical Example
One such example was when  [Patreon got hacked in 2015](https://labs.detectify.com/2015/10/02/how-patreon-got-hacked-publicly-exposed-werkzeug-debugger/) using this vulnerability. Definitely recommended to give a thorough read at the article. Also read what it says in [the room](https://tryhackme.com/r/room/owasptop102021) about how a researcher *found an open debug interface for Werkzeug console, a vital component for python-based web applications*, which could be simply accessed via URL on `/console`, from where the attacker could execute arbitrary system commands.

[[PRACTICALS - OWASP Top 10 vulnerabilities (2021)#TASK 12 - SECURITY MISCONFIGURATION]].

## -> TASK 13 - 6. VULNERABLE AND OUTDATED COMPONENTS
This one *occurs due to lack of research and knowledge on what components you are using*. **It may not happen often but that's no excuse if the company you work get attacked because of it!**

It does not take much to do, just *stay updated* and keep track of what kinds of updates you are getting, *if anything* you use is *out-dated or have any kind of vulnerability present*, if it is there at all, *research about them*, etc. It might seem negligible but remember, even in these times of such quick advancement in technology, people still get hacked with simple [[SQL Injections]]

The [task](https://tryhackme.com/r/room/owasptop102021) provides with an example, read about it, it'll show you its importance.
## -> TASK 14 - VULNERABLE AND OUTDATED COMPONENTS - EXPLOIT
The room says a great thing - *Recall that since this is about known vulnerabilities, most of the work has already been done for us.* Apart from quoting that, there's not much to explain since most of it is an example. I'd rather suggest you read there and get a good understanding of how it's explained over there!
[TryHackMe | OWASP Top 10 - 2021](https://tryhackme.com/r/room/owasptop102021).
## -> TASK 15 - VULNERABLE AND OUTDATED COMPONENTS - LAB
#### --> PRACTICAL
[[PRACTICALS - OWASP Top 10 vulnerabilities (2021)#TASK 15 - VULNERABLE / OUTDATED COMPONENTS]]

## -> TASK 16 - 7. IDENTIFICATION AND AUTHENTICATION FAILURES
*Funny thing, when I got into this domain (Cybersecurity) and started reading about it, `I used to think that` [[BAC - Broken Access Control]] &* **this** *`are identical`, just different ways of saying the same thing, but as I got deep into it, I realized that they're absolutely not same in any aspect!* 
Flaws in identification and authentication mechanisms are what allows attackers to gain access to user accounts. **This is failure is known as what the task says**, (it's too long to write!).
Before we move any further, I suggest take a good read on the [task](https://tryhackme.com/r/room/owasptop102021)about what is being said. It's basics!

Some common flaws include: *(Attack **|** Prevention)*
1. Brute-force attacks **|** enforce auto-lock after certain number of attempts
2. Use of weak credentials **|** ensure strong password policy
3. Weak session cookies! **|** implement MFA, the stronger the better!

*My Take: Read more about it from articles by all kinds of researchers and better your understanding!*
## -> TASK 17 - IDENTIFICATION AND AUTHENTICATION FAILURES PRACTICAL
#### --> PRACTICAL
[[PRACTICALS - OWASP Top 10 vulnerabilities (2021)#TASK 17 - IDENTIFICATION & AUTHENTICATION FAILURES]]

## -> TASK 18 - 8. SOFTWARE AND DATA INTEGRITY FAILURES
#hashes #hashcracking 

Q. What is **Integrity**?
A. *When talking about integrity, we refer to the capacity we have to ascertain that a piece of data remains unmodified.* I personally think that was simplest answer if you can understand English to a point.
To accomplish this, **hashes** are sent alongside the files to prove whether the integrity the obtained file is kept or not!

> **Hash** - A hash or digest is simply a number that results from applying a specific algorithm over a piece of data.

There's a whole lot of archive out there for hashes, but very often we'll encounter the most common of all of them, such as `MD5, SHA1, SHA256, AES` or many more such.

The failure here occurs when software or data being used by the code/infrastructure lacks any kind of integrity checks! Therefore, an attacker might mess with the data while it is in transit. There's mainly 2 type:
- Software Integrity Failure
- Data Integrity Failure
## -> TASK 19 - SOFTWARE INTEGRITY FAILURES
#### --> PRACTICAL
[[PRACTICALS - OWASP Top 10 vulnerabilities (2021)#TASK 19 - SOFTWARE INTEGRITY FAILURES]].

## -> TASK 20 - DATA INTEGRITY FAILURES
#### --> PRACTICAL
[[PRACTICALS - OWASP Top 10 vulnerabilities (2021)#TASK 20 - DATA INTEGRITY FAILURES]].

## ->TASK 21 - 9. SECURITY LOGGING & MONITORING FAILURES
This arises when any web application, which is supposed to log user activity over the website, **does not!** *Logging is one of the important tasks to be performed by any web application, as it comes in handy when things go south quickly!* Without this, it becomes significantly harder to trace step of any attacker about what and how they did to gain access! `2 more significant impacts include:`
1. Regulatory Damage: Endangering user accounts could lead to lawsuits on website owners
2. Risk of further attacks: Undetected presence of attackers, allowing them to get further into the network, stealing credentials of owners, attacking infrastructure of code, etc.

The information stored in logs should include the following:
- HTTP status codes
- Time Stamps
- Usernames
- API endpoints/page locations
- IP addresses

So far, logging helps only after an incident, but what about before? Hence, a system has to be placed to detect suspicious activities being done, and alarming before the incident occurs, either completely stopping or reducing the impact of the attack! Hence, comes **monitoring!**
Common examples of suspicious activity are included in task, go read there! **Lazy dude!**
Not only detecting, but rating the activity based on what kind of impact it will have on the application! Based on those ratings, response times should be decided for higher-impact actions, raising alarms to get appropriate attention.

#### --> PRACTICAL

## -> TASK 22 - SERVER-SIDE REQUEST FORGERY (SSRF)
**This is one of my favourites!** *Server-Side Request Forgery (SSRF) is a type of security vulnerability where an attacker can trick a server into making requests to unintended locations, often internal systems that are not accessible from the outside. This can lead to unauthorized actions or access to sensitive data.*
For more on `SSRF` specifically, refer to -> [[SSRF - Server-Side Request Forgery]].
To put it in a bit more layman language, an attacker forces a website to send `requests` on their behalf, to arbitrary destinations, all the while keeping control of the request itself!
**Funny thing, this occurs when web application use third-party services!** The task explains this with a good and simple example, read [there](https://tryhackme.com/r/room/owasptop102021). Actually, I'd suggest you to read it there, it's more collected and easier to understand from there!

#### --> PRACTICAL

[[PRACTICALS - OWASP Top 10 vulnerabilities (2021)#TASK 22 - SERVER-SIDE REQUEST FORGERY (SSRF)]]. 