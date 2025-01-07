---
layout: post
author: The 2xdropout
---
A Legacy Blog Post From 2xdropout.xyz

<iframe src="https://giphy.com/embed/FRJDGalM81snCmaPh9" width="480" height="271" style="" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/terminator-linux-cli-FRJDGalM81snCmaPh9">via GIPHY</a></p>

I recently bought an extremely cheap chromebook and just moved; so I thought it would be a great chance to show how you can still do some CTF challenges even while using something like a chromebook. Obviously Pico has a web terminal that you can access, but that's not the point. lets pretend that for whatever reason the web terminal just wasn't working for me or 
I was attempting a challenge from a site other than PicoCTF. How would I setup my chromebook to help me snag some flags on the go?

If you’re unfamiliar with what a CTF is you’re probably wondering whats a flag and why would I want to find one. Well CTF stands for capture the flag. In digital CTFs the flag that we are trying to get is just a string of text that proves that we completed a given challenge. The flags are often hidden in such a way that you have to utilize math, hacking, and/or 
programming skills to find them. PicoCTF is a wonderful free site, put together by Carnegie Mellon University, that focuses on cybersecurity CTFs. They break their CTFs into six different categories:

- General Skills
- Cryptography
- Reversing
- Web exploitation
- Forensics
- Binary Exploitation

So now that we know what a CTF is, the first thing that I would do is to hit the chromebook's dedicated search key and type in settings. Hit enter and from there we can use the settings search bar (located at the top) to then search for linux. We want to enable the linux development environment. This can take a while so make sure your charger is nearby and you 
have a reliable internet connection. Come back here when the installation is complete. Now that we have our linux environemnt installed within our chromebook we can use the apt package manager to searching for settings on the chromebookinstall any tools that we may need to complete a CTF. However, before diving into a CTF there are some quality of life improvements 
that I would recomend making. First off I would install a different terminal emulator. Theres nothing really wrong with the default emulator, however I find personally find Terminator to better fit my workflow. For me the biggest benefit of terminator is the window splitting which you can see demonstrated via the gif at the top of this post. If we want to split 
vertically 'ctrl+shift+e'. If we want to split horizontally 'ctrl+shift+o'. Terminator also allows you to have profiles for different windows which I find helpful for avoiding little mistakes like running commands on a remote machine rather than my own. If you want to give terminator a try just use apt to install:  ```sudo apt install terminator```

![search](https://github.com/user-attachments/assets/bb3b200a-2e16-4bb4-b333-997571876c13)

The next quality of life change that I would recommend is switching the default shell from bash to zsh. I prefer zsh due to all of the plugins and tools that allow you to squeeze maximum functionality and readability out of working in the terminal. Changing shells is generally pretty simple and there are plenty of easy to follow tutorials on the internet that will 
show you how to switch. For whatever reason I personally couldn't get get My changed user-lineany of the commands to work so I went into my /etc/passwd file and edited my user-line to read '/bin/zsh'. PLEASE NOTE THAT THIS METHOD IS NOT RECOMMENDED AND IF YOU DON'T KNOW WHAT YOU ARE DOING YOU CAN CAUSE DAMAGE TO YOUR SYSTEM . After you have ZSH I would then recommend 
downloading ohmyzsh which greatly expands upon the functionality of zsh.

![etcPasswd](https://github.com/user-attachments/assets/500d3b50-32ff-4989-a11f-03a2d99df221)

The final quality of life improvement that I would recommend is a pretty simple one and that is installing binutils. simply run:  ```sudo apt install binutils```

Now we should be all ready to roll. Starting off simple we are going to complete the CTF challenge, "Static ain't always noise," by SYREAL.

![StaticCTF](https://github.com/user-attachments/assets/b897e0c4-d01a-48f1-ab66-112166212ed1)

Alright, the first thing that we need to do is download our target file which is called static and our bash file. If you want to do this within the command line you can right click the links and use the wget command to download the files like so:  ```wget [your like to the file here] ```

![Downloaded](https://github.com/user-attachments/assets/d814a99a-a493-444b-b864-78f5395852f3)

lets try to run our bash file. This is the one that ends with the ".sh" extension. To run a bash file we can either use the "bash" command followed by the file name or simply precede the file with "./". Here is an example of the first method

``` bash [your file name here]```

Here is the second method:

```./[your file name here]```

That said if we were to use the second method right now we would encounter an error. 

![PermissionDenied](https://github.com/user-attachments/assets/79e3ff99-9b25-4b85-bfa0-5db46682c8ac)

To fix this we need to change our bash file's permission to allow it to be executable. We can do this by using the chmod command and including the "+x" flag like so:  ```sudo chmod +x [your file name here]```

Now we are able to run the program! However now it seems that we need to provide the program with an argument. Let's give it the file: static 

![chmod](https://github.com/user-attachments/assets/f4f4d399-8a95-4790-80c9-20aa43a7cdfe)

Now if we run the command with the argument we are told that strings have been ripped from the binary and have been placed in the newly created file "static.ltdis.strings.txt". we can now grep the file searching for "pico". To do this we can run the following: ``` grep pico ./static.ltdis.strings.txt```

![Flag](https://github.com/user-attachments/assets/b33f4574-6e9c-4d0e-8e4d-1204e4435328)

Then bam! we have our flag! 

Before you go, there was a quicker way to do this. We can get the strings from a binary file without using the bash script by using the strings command. We can pipe that into grep to find our flag with ease like so: ```strings static | grep pico```

![StringsandGrep](https://github.com/user-attachments/assets/10e07eae-61fc-49e2-8125-56048d287d6b)

Theres our flag!
