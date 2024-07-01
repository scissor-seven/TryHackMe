#xss #web_exp #common_vulns

[TryHackMe | XSS](https://tryhackme.com/r/room/axss)

# >> Sudhanshu Chatterjee | Jun 16th '24

## TASK 1 - INTRO
There's not much to discuss in this task except for the what's and how's and since when's kind of stuff. Wanna know more about `XSS`, head over to this note --
- [[XSS - Cross_Site Scripting]]

In case you need a general definition, here's what it is:
> Cross-Site Scripting (XSS) is a common security vulnerability that allows attackers to *inject malicious scripts into web pages viewed by other users*.

The note has much more important info on all those subjects, and some pretty cool stuff as well if you ask me. Rest assured, there's nothing much here.

As far as pre-requisites are concerned, the only thing that won't be available is 
- [Intro To Cross-Site Scripting](https://tryhackme.com/room/xss).

So don't think about it much and let's move on. Lot of ground to cover. But before we move on, let me just list the `types of XSS`-
- ***Reflected XSS***
- ***Stored XSS***
- ***DOM-based XSS***

## TASK 2 - TERMINOLOGY & TYPES
**XSS bypass SOP**, consequently.

>SOP(Same-Origin Policy) - A security mechanism implemented in modern web browsers to prevent a malicious script on one web page from obtaining access to sensitive data on another page.

*XSS dodges SOP* as it is executing from the same origin.

Reviewing some essential Js(`JavaScript`) Functions:
- `alert()` - is a function to invoke Js-alert in browser. Try the damn thing, you'll understand.
- `console.log()` - is function to display a value in browser's Js-console.
- `btao() & atob()` - they *encode/decode strings of binary data to create base64-encoded ASCII string*, respectively.

I already told you about `types of XSS` and where you can read more about them.

>Another thing, I will never tell the answer which don't require practical. If the question is to be answered from extract given, read it, and answer the questions!

## TASK 3 - CAUSES & IMPLICATION
*The reason for XSS being severe threat is that exploits users' trust in a site*.

**What makes XSS possible?** - Great question to begin understanding XSS. It's a simple list - 
- **Insufficient input validation and sanitization**
- **Lack of output encoding**
- **Improper use of security headers**
- **Framework and language vulnerabilities**, etc, etc.

And more such but the implication are far worse -
- **Session hijacking**
- **Phishing and credential theft**
- **Social engineering**

and I can keep going but to what end! Point well made.

# TASK 4 - REFLECTED XSS

**Reflected XSS** is a type of XSS vulnerability where a malicious script is reflected to the user’s browser, often via a **crafted URL** or **form submission**.

Let say a search query like this
```HTML
<script>alert(document.cookie)</script>
```
won't look suspicious to many users in a URL, even if they *read-it* (assuming they don't know about it, which honestly, how many would know!)

What *this would do* is *display user cookies* in an `alert box` to user. No harm done, except now the attacker knows that the application is vulnerable.
Such user-input should be *sanitized* or *html-encoded* before being passed on.

Then there are sub-sections of on languages and frameworks. Read them all carefully, I'll only mention or explain what needs to be.
- PHP
- Js (Node.js)
- Python (Flask)
- C# (ASP.net)

#### PHP
Code snippet in `PHP`,
```php
<?php
$search_query = $_GET['q'];
echo "<p>You searched for: $search_query</p>";
?>
```
- `$_GET` - a PHP array containing values from URL query string. 
- `$_GET['q']` - refers to string query parameter `q`. For example, in
```URL
http://shop.thm/search.php?q=table
```
$\_GET['q'] has value `table`.

Possible exploit in PHP could be this, as a  `proof of concept`:
```Evil_URL
http://shop.thm/search.php?q=<script>alert(document.cookie)</script>
```
Easy fix for this case is given there as well, which is to encode any special characters, pushed by user, into HTML-entities. (I liked that word for some reason, `entities`).

After the fix, the code might look like this:
```php
<?php
$search_query = $_GET['q'];
$escaped_search_query = htmlspecialchars($search_query);
echo "<p>You searched for: $escaped_search_query</p>";
?>
```

#### JavaScript (Node.js)

A example of bad and vulnerable code in JS:
```javascript
const express = require('express');
const app = express();

app.get('/search', function(req, res) {
    var searchTerm = req.query.q;
    res.send('You searched for: ' + searchTerm);
});

app.listen(80);
```

- `req.query.q` - extracts value of `q`

And just like before, it can be tested as a proof of concept, similar to the case of PHP. A fixed code is again given there, which suggests something similar.

Instead of HTML-Encoding, *the `sanitizeHtml()` function from `sanitize-html` library removes any unsafe elements and attributes*! These include script tags and many more elements.
Another method would be using `escapeHtml()`. As name says, it skips any special characters.

#### Python (Flask)

Now that you have seen 2 examples, you'll probably find the vulnerable part in this code:
```python
from flask import Flask, request

app = Flask(__name__)

@app.route("/search")
def home():
    query = request.args.get("q")
    return f"You searched for: {query}!"

if __name__ == "__main__":
    app.run(debug=True)
```

which if you did get right, is the `request.args.get()` part, where parameter `q` gives the value `table`. A good fact here says
> `request.args` contains all the query string parameters in a dictionary-like object.

Isn't that interesting, what does it mean?

# TASK 5 - VULN WEB-APP 1
There's not much for the overview here, just check out the practical.
#### PRACTICAL
[[PRACTICAL - XSS#Task 5 - Vuln Web-App 1]].

# TASK 6 - STORED XSS

> Stored XSS, or Persistent XSS, is a web application security vulnerability that occurs when the application stores user-supplied input and later embeds it in web pages served to other users without proper sanitization or escaping.

Examples -
- Web forum posts
- Product Reviews
- User Comments, etc

Short and sweet. it occurs when a `user input` get's `stored` without proper sanitization or escaping.
Let's take a look on few `code` examples and how they can be fixed so that this does not occur!

[ Before I move on, let me make it clear, if you don't understand here, please, always give thorough read to the contents of task. It's is explained carefully there. ]

### PHP
Bad Code:
```php
// Storing user comment
$comment = $_POST['comment'];
mysqli_query($conn, "INSERT INTO comments (comment) VALUES ('$comment')");

// Displaying user comment
$result = mysqli_query($conn, "SELECT comment FROM comments");
while ($row = mysqli_fetch_assoc($result)) {
    echo $row['comment'];
}
```

Whereas here's the fixed code:
```php
// Storing user comment
$comment = mysqli_real_escape_string($conn, $_POST['comment']);
mysqli_query($conn, "INSERT INTO comments (comment) VALUES ('$comment')");

// Displaying user comment
$result = mysqli_query($conn, "SELECT comment FROM comments");
while ($row = mysqli_fetch_assoc($result)) {
    $sanitizedComment = htmlspecialchars($row['comment']);
    echo $sanitizedComment;
}
```

If you are able to spot the difference AND understand what difference it makes, you probably have the idea by now about how it works!
If you don't, no worries, just read more about it, do your own research. You'll get it eventually.

### JavaScript (Node.js)
Bad code:
```javascript
app.get('/comments', (req, res) => {
  let html = '<ul>';
  for (const comment of comments) {
    html += `<li>${comment}</li>`;
  }
  html += '</ul>';
  res.send(html);
});
```

On the contrary, here's the fixed code:
```javascript
const sanitizeHtml = require('sanitize-html');

app.get('/comments', (req, res) => {
  let html = '<ul>';
  for (const comment of comments) {
    const sanitizedComment = sanitizeHtml(comment);
    html += `<li>${sanitizedComment}</li>`;
  }
  html += '</ul>';
  res.send(html);
});
```

Similar examples are given for Python(Flask) and C#(ASP.NET). Give them a good read and you'll learn few new things!

# TASK 7 - VULN WEB-APP 2
Once again, not much to say here, but definitely a lot to show in the practical!

#### PRACTICAL
[[PRACTICAL - XSS#Task 7 - Vuln Web-App 2]].

# TASK 8 - DOM-Based XSS
Before anything, DOM
> The Document Object Model (DOM) is a programming interface for web documents that represents the structure as a tree of objects. It allows scripts to dynamically access, manipulate, and update the content, structure, and style of web pages.

Honestly though, I myself don't understand this one very clearly. It's a bit too technical for me right now, because I have a very little and limited knowledge of JavaScript and this mostly come's from there so. Can't say much about it!

But even from what I am reading and understand about it, it's mostly like where `malicious script` are executed in user-browser because `JS` of webpage does not handle the input properly.


It could be in any format, so I have a researched differentiation between these `3 XSS types`:

| Aspect                    | DOM-Based XSS                                       | Stored XSS                                                 | Reflected XSS                                     |
| ------------------------- | --------------------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------- |
| **Where Payload Resides** | In the client's browser DOM                         | Stored in the server's database                            | Reflected in the server's immediate response      |
| **Execution Location**    | Client-side (in the browser)                        | Server-side but executed client-side                       | Server-side but executed client-side              |
| **Data Source**           | Client-side sources like URL, local storage         | Database, forums, comments sections, user profiles         | HTTP request parameters (GET/POST), URL           |
| **Persistence**           | Non-persistent; only affects the current session    | Persistent; affects all users who access the infected data | Non-persistent; only affects the current request  |
| **Trigger Method**        | JavaScript manipulates the DOM directly             | Data is loaded from the server and rendered in the browser | Data is sent back in an immediate server response |
| **Typical Attack Vector** | Manipulated URLs, local storage data                | User input fields that save data (e.g., comment fields)    | Form submissions, URLs with query parameters      |
| **Example Scenario**      | A script injected via URL parameter                 | A malicious comment stored in a forum post                 | A script reflected in a search results page       |
| **Detection Complexity**  | Can be harder to detect; requires dynamic analysis  | Easier to detect; persistent in the system                 | Easier to detect; appears in server response      |
| **Example Mitigation**    | Sanitize input, use safe methods like `textContent` | Sanitize and validate input before storing                 | Sanitize and validate input before reflecting     |
I don't know if this'll help or not but, I think create somewhat of an understanding about *which is which and what is what*!

# TASK 9 - Context & Evasion

***CONTEXT***
3 popular places where the injected payload might find it's way in:
- Between HTML tags
- Within HTML tags
- Inside JavaScript

Read the content on task. It'll get you some depth on where and how these payloads should go for a successful attack! Shortening it might lose the context or clarity.

***EVASION***
This one is more about giving resources to *create/consult* for `custom payloads`!
One of the lists >>  [XSS Payload List](https://github.com/payloadbox/xss-payload-list).

There might length restrictions so  [Tiny XSS Payloads](https://github.com/terjanq/Tiny-XSS-Payloads) can be a great starting point to bypass those.

If the **payload blocking** is based off of a specific blocklist, then there are more tricks such as :
- Horizontal tab (TAB) is `9` in hexadecimal representation
- New line (LF) is `A` in hexadecimal representation
- Carriage return (CR) is `D` in hexadecimal representation

Cheat-Sheet Resource -  [XSS Filter Evasion Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html).

If you want, you can find answer to the given question here >> [ASCII text,Hex,Binary,Decimal,Base64 converter](https://www.rapidtables.com/convert/number/ascii-hex-bin-dec-converter.html). 🤭

# TASK 10 - Conclusion
Regular stuff, no such info or other source material provided ***BUT***, a good advice on keep learning more about the `behind-the-scenes` happenings to get an even better understanding!

---
# ==THANKS==

---

