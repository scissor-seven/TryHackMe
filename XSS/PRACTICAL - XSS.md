Complete practical of tasks from room [XSS](https://tryhackme.com/r/room/axss).

# Task 5 - Vuln Web-App 1
From the knowledge gained from previous tasks, **this** lab is going to be much easier.
All you have to do is go to the website on the given **\<IP>** and do what the task says to do.
Enter the exploit code at the end of URL and your task will be done.

**NOTE**: Don't forget to put the *port number*, otherwise you won't be able to get to the site.

# Task 7 - Vuln Web-App 2
This one will require attention to follow through. First up, the site is @port80 on the given \<IP>. 
Once you go to the site, get to contacts, fill out the form but put this in the message field:
```html
<script>alert("Simple XSS")</script>
```
After this, when you login as *receptionist* (creds are given), you'll be shown the message, which you stored just above, which proves the existence of a stored xss vulnerability.

Similar to what we did above, we need to repeat the Task but this time, the payload(the thing that'll go in message field, you know! XD) will be this:
```html
<script>alert(document.cookie)</script>
```
If you know what this is going to do, then you know how much a bad and good code makes difference!