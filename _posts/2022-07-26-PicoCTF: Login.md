---
layout: post
author: The 2xdropout
---
A Legacy Blog Post From 2xdropout.xyz

![LoginCTF](https://github.com/user-attachments/assets/471eddca-6d34-48b0-ac8b-c2bd1e9210e4)

Lately I’ve been slowly working my way through the web exploitation section of PicoCTF. I came across this login CTF by BROWNIEINMOTION and I thought it could be fun to share how I went about finding the flag.

If you’re unfamiliar with what a CTF is you’re probably wondering whats a flag and why would I want to find one. Well CTF stands for capture the flag. In digital CTFs the flag that we are trying to get is just a string of text that proves that we completed a given challenge. The flags are often hidden in such a way that you have to utilize math, 
hacking, and/or programming skills to find them. PicoCTF is a wonderful free site, put together by Carnegie Mellon University, that focuses on cybersecurity CTFs. They break their CTFs into six different categories:

General Skills
Cryptography
Reversing
Web exploitation
Forensics
Binary Exploitation

So now that some prerequisite knowledge has been established lets get into the CTF! I started out by following the link and it brought me to this plain white login screen. So far there is nothing that really stands out.

![LoginPage](https://github.com/user-attachments/assets/b773737f-8558-4bec-9437-b3707e922d1f)

The first thing I did was try a very simple SQL injection login bypass (‘ or 1=1 -- and its variations) in the hopes that it would be that simple.

![SQLBypassAttempt](https://github.com/user-attachments/assets/8d616f08-7d46-44dd-9b7a-b6f426b74201)

It wasn't.

![FailedSQLBypass](https://github.com/user-attachments/assets/5b98d517-86d5-4639-8012-3301f8cb8698)

Since that didn’t work I decided to move on to opening up Burp Suite. Hopefully with it I could gain some useful information based on the outgoing requests being made. With my Burp Suite browser open and intercept on I typed in some bogus credentials and hit submit. Then, nothing happened. For a sanity check I opened up another tab in the Burp Suite browser and 
tried to go to Google. Right away we intercepted a request. So I sat there for a moment and then it dawned on me. The reason there wasn’t an outbound request was because there was nothing to request. There was no SQL database hanging out on a server somewhere that had the credentials that we were looking for. They were here, client side. Of course the SQL injection 
bypass didn’t work. There was no SQL. So I closed Burp Suite, hit F12 on my Chrome Browser and gave the index.html file a quick once over. I didn’t see anything that interesting so I moved on to the index.js file. For anyone who may not know, the js extension here represents Javascript. Javascript is the programming language used to build front end (client side) web 
applications. Javascript is what really gives your website its functionality. Looking at the index.js file we can see right away that an attempt was made to obfuscate the code by putting it all on a single line.

![IndexFile](https://github.com/user-attachments/assets/cade021e-cc2c-4a04-b74b-9f40a7158dd5)

The copied code was:```(async()=>{await new Promise((e=>window.addEventListener("load",e))),document.querySelector("form").addEventListener("submit",(e=>{e.preventDefault();const r={u:"input[name=username]",p:"input[name=password]"},t={};for(const e in r)t[e]=btoa(document.querySelector(r[e]).value).replace(/=/g,"");return"YWRtaW4"!==t.u?alert("Incorrect Username"):"cGljb0NURns1M3J2M3JfNTNydjNyXzUzcnYzcl81M3J2M3JfNTNydjNyfQ"!==t.p?alert("Incorrect Password"):void alert(`Correct Password! Your flag is ${atob(t.p)}.`)}))})();```

Doesn’t look very pretty does it? Well there is something we can do about that! We can take this copied code and toss it into a free online Javascript beautifier. I used [beautifier.io](https://beautifier.io/).

![Beuty](https://github.com/user-attachments/assets/0f3e2393-dc5b-47c3-9a2b-65af67da3e47)

Look at that! Isn’t that way easier to read? So now that I had this nice formatted Javascript in front of me I went from the beginning and started working my way through the logical flow of the code. In short, what I noticed was that the site was taking our inputs converting each character to binary and then was using the btoa() function to convert that string to a 
base64 encoded ASCII string. The code would then compare that new string against these two values.

![ValueCompare](https://github.com/user-attachments/assets/0ce01750-0d9b-4abe-b6e4-554a0931bd80)

We can see that when our encoded inputs are not equal to these seemingly random strings we get an incorrect username or password alert in the browser. To me this would imply that these random strings are not actually random and are in fact the username and password that we are looking for. So I opened up JSFiddle and wrote a few commands that should decode our 
strings for us using the atob() functions. All we have to do now is hit run and we should have what we need!

![JSFiddle](https://github.com/user-attachments/assets/20139bed-5faa-4028-b8bd-da64859bf52f)

![UandP](https://github.com/user-attachments/assets/dc0ad520-4367-4ab6-9d4a-d5e46bbfe9f7)

There it is! The administrator's username and password. A quick copy and paste and we can go test it against the login on our target site.

![SuccessAlert](https://github.com/user-attachments/assets/230d04fb-9baf-47be-b726-1804eef051b1)

Success! Our login worked and we have our flag!
