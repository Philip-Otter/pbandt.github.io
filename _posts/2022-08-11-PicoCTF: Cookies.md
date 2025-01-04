---
layout: post
author: The 2xdropout
---
A Legacy Blog Post From 2xdropout.xyz

![CookiesCTF](https://github.com/user-attachments/assets/3f25853d-a0d5-47a7-a764-dba8510a876f)

Lately I’ve been slowly working my way through the web exploitation section of PicoCTF. My goal has been to try and complete at least one CTF a night after I get off of work. On one of those nights I came across this CTF by MADSTACKS and I thought it could be fun to share how I went about finding the flag. Going into it I could see that it had a user score of 
60% and no hints so I really wasn't sure what to expect.

If you’re unfamiliar with what a CTF is you’re probably wondering whats a flag and why would I want to find one. Well CTF stands for capture the flag. In digital CTFs the flag that we are trying to get is just a string of text that proves that we completed a given challenge. The flags are often hidden in such a way that you have to utilize math, hacking, and/or 
programming skills to find them. PicoCTF is a wonderful free site, put together by Carnegie Mellon University, that focuses on cybersecurity CTFs. They break their CTFs into six different categories:

- General Skills
- Cryptography
- Reversing
- Web exploitation
- Forensics
- Binary Exploitation

Anyway lets get started. When we first open up the link to the target site we are presented with a screen that contains a text-box and a search button. With the text box we are able to enter cookie names and see how the author feels about them. The default cookie seems to be snickerdoodle.

![Home](https://github.com/user-attachments/assets/d53a6a7e-e079-4902-8635-95b07b76b7b7)

When we type snickerdoodle in and hit enter we are presented with a screen that says, "I love snicerdoodle cookies!" However, this page also informs us that while snickerdoodle is a cookie, it is not special. So I think this means that what we are looking for a special cookie value.

![Doodle](https://github.com/user-attachments/assets/48b00ff6-cb63-4486-9b29-c5e256da7ffb)

So the next thing that I tried to put into the text box was just something random. I did this just to see what would happen. I was then informed that what I had entered was not a valid cookie

![Rand](https://github.com/user-attachments/assets/67a43f49-e04c-45e2-a566-6ce9afd715d5)

![NotValid](https://github.com/user-attachments/assets/6038f077-a38a-4cb7-a27e-bd6b70ba20d2)

At this point I assumed that I had gotten all the information from just looking at the website. I had also inspected the associated index.html file and didn't find anything of use to me. So I decided to open up Burp Suite and take a look at the HTTP requests and responses. While looking at the headers I noticed that our first GET request resulted in us being 
redirected. So our second GET request is the one that we would want to edit to start looking for our special cookie. My first attempt at finding the cookie via editing the headers was to use burp intruder and a list of real life cookies I pulled from a food website to automatically reassign the name value with a cookie from our list. After playing around with our 
name values I came to realize that our cookie name attribute is what was linked to different cookie names. duh 🤦.

![Name](https://github.com/user-attachments/assets/23f780f8-1934-40d3-bf11-07e494ee29de)

This was great news! We could try and enumerate our way through all the cookie name values until we found our special cookie. So I went ahead and sent my request over to the burp intruder and set up a sniper attack to do just that. I set it to test values from 4-15 in increments of 1. I started at 4 because I had already fround the values up through 3 at this point. 
I ended at 15 arbitrarily. If the value was greater than 15 we would just start our next attack at 16 and pick another larger arbitrary end value. 

![Payload](https://github.com/user-attachments/assets/dfb43550-9270-4734-b845-875190ff179d)

As it turns out, 15 was not a high enough end value! So I ended up increasing our ending value and ran another attack from 16-30. After that ended I looked through our responses and right there, with a name value of 18, was our flag!

![IntruderResults](https://github.com/user-attachments/assets/2d9f484e-07c1-49ea-8c2b-b0b111a1a8d1)
