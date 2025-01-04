---
layout: post
author: The 2xdropout
---
A Legacy Blog Post From 2xdropout.xyz

![LogonHeader](https://github.com/user-attachments/assets/e23af57b-bde9-4884-b983-74cb3779bd6f)

Lately I’ve been slowly working my way through the web exploitation section of PicoCTF. I came across this login CTF by BOBSON and I thought it could be fun to share how I went about finding the flag. I don’t think I found the flag in the intended manner but ¯\_(ツ)_/¯ oh well.

If you’re unfamiliar with what a CTF is you’re probably wondering whats a flag and why would I want to find one. Well CTF stands for capture the flag. In digital CTFs the flag that we are trying to get is just a string of text that proves that we completed a given challenge. The flags are often hidden in such a way that you have to utilize math, hacking, and/or 
programming skills to find them. PicoCTF is a wonderful free site, put together by Carnegie Mellon University, that focuses on cybersecurity CTFs. They break their CTFs into six different categories:

- General Skills
- Cryptography
- Reversing
- Web exploitation
- Forensics
- Binary Exploitation

So our challenge comes from picoCTF 2019 and as can be seen in the above image indicates that, “The factory is hiding things from all of its users. Can you login as Joe and find what they’ve been looking at?” We are then provided with a link to the challenge. It is entirely preference but I am going to choose to spin up my virtual box containing a Kali Linux 
installation for this CTF.

![VMKali](https://github.com/user-attachments/assets/6dfe7664-785f-4ec3-88e3-c31a587b49cd)

Now that I have Kali up I am going to launch a tool called [ZAP](https://www.zaproxy.org/). ZAP is a network tool that functions similarly to Burp Suite, is open source, and was created by the [OWASP Foundation](https://owasp.org/). Like with Burp, Zap comes with a web browser that lets us do things like intercept http requests. To start the browser all you need to do is click on the little Firefox symbol in the 
upper right of the program. Once we have our browser open we can head on over to our target site.

![OWASPzap](https://github.com/user-attachments/assets/d3948f52-3da1-4bfc-8537-546c807bfb4f)

![logonOWASPBrwsr](https://github.com/user-attachments/assets/f0623863-99ba-473b-90d9-101a530822bf)

On the target site we encounter a simple login page. If we go down to the bottom right we can turn on our HUD. With our HUD we can see things like possible vulnerabilities that Zap has found on the page, comments (without viewing the source code), hidden elements, etc. Most importantly for us right now, we can turn on intercept mode (second from the top on the 
left hand side of the screen). With intercept mode on lets see what happens when we try and login.

Here I am attempting to login as Joe with a password of password. We have a few redirects but eventually we end up on a screen that tells us that Joe’s password is super secure.

![LogonJoesPassFail](https://github.com/user-attachments/assets/acc1ae50-86bf-4095-8ec3-34cc06ae2a1d)

Since nothing immediately stood out to me. The next thing that I tried was to leave the password and username blank and attempt to login. When we do that we are able to login, just not as Joe and we don’t seem to have the flag either.

![LogonLoginSuccess](https://github.com/user-attachments/assets/3c804c5f-1ba3-4bc5-b55a-639d0a3ba17c)

Well since we are able to login without a username and password, lets see if there is any way to exploit that. Again I turned on the intercept mode through the HUD and hit sign in. Looking at our first request header I notice something that I think could be useful. We can see that the admin status is saved as a plain text cookie. Maybe if we change this we could 
make our blank account an administrator and gain access to the flag that way.

![logonCookie](https://github.com/user-attachments/assets/b1503de4-ed8a-4a0d-a387-5450dcec6f39)

So using the request editor via the Zap HUD I changed every requests admin value to equal True. There we go! We have our flag. We may not have gotten Joe’s password but we still left with what we needed! I'll take it, we don’t always have to do things exactly in the expected way.

![LogonFlag](https://github.com/user-attachments/assets/81455523-9b05-464f-94d0-3afb50726394)
