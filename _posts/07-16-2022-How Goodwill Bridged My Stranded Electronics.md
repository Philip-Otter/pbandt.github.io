A Legacy Blog Post From 2xdropout.xyz
<br></br>
So the other day I was looking through the electronics section of a Goodwill when an old wifi router caught my eye.
![Router](https://github.com/user-attachments/assets/739b1836-cd3a-4279-81ab-2077cc31a30c)
I have some Pis and old computers in my room that are unable to connect to the network via wifi and I unfortunately do not have any ethernet 
jacks in my room. I could certainly fix this problem by using PCIe devices, adapters, HATs or by running an ethernet line directly to my room 
(my landlord may not approve of this). However, why would I bother with any of that when I have a solution staring me right in the face at a $6.99
price point.
<br></br>
Have you ever heard of DD-WRT? DD-WRT can provide the owner of a consumer wireless router with more features than what they may have access to 
while using router's default firmware. More details and a full list of features can be found here. The feature that I am interested in, and 
think might be the solution to my problem, is the ability to put certain routers into client and client bridge mode. Client and client bridge 
mode will allow our router to work as a client device and connect to an already existing wireless network while not broadcasting our own network. 
The difference between client bridge and client mode is that the former allows for DHCP(Dynamic Host Configuration Protocol) to assign IP 
addresses to the devices behind the client bridge router. In client mode the WLAN and the LAN will be on two seperate subnetworks. We can still 
have ip addresses assigned within the subnetworks but not without extra work.
<br></br>
Before I left the store with the new router I went to the DD-WRT site and ensured that the device I was about to purchase was in fact supported. 
If you have a device that you would like to try this on, you can find the router database here. The Linksys E2500 that I was looking at was in 
fact listed as being supported.
![ddwrtRouterSupport](https://github.com/user-attachments/assets/b08175c2-43f5-4588-8b96-906a01f2dc43)
After purchasing my device (and finding out it was 75% off!) I took it home and got to work. The DD-WRT documentation can feel overwhelming at 
times but it is important that you RTFM! Changing the firmware on your device is no joke and if done improperly can even cause your device to 
become bricked. So the very first thing that I did was read my way all the way through the instructions for my device.
![ddwartInstructions](https://github.com/user-attachments/assets/86b8df44-f58e-4cd9-971c-4bd1de6561d6)
Notice how on step three we are guided to perform a hard reset. Also take note of the fact that "hard reset" is a hyperlink and again comes up 
on step nine as hyperlink. This should indicate to us that there is some background knowledge that the author of the documentation wants to make 
sure that we understand. RTFM isn't limited to just what is physically inside of the documentation. If we click on that hyperlink we are brought 
[here](https://wiki.dd-wrt.com/wiki/index.php/Hard_reset_or_30/30/30). Upon accessing this page we are bombarded with big scary red text that 
warns of things that we can do that will cause issues with or even brick our device. By reading this page we find out that a hard reset refers 
to a 30-30-30 reset in which we hold the reset button (located at the bottom of my device)
![ddwrtRouterSupport](https://github.com/user-attachments/assets/0c176f24-438d-4432-b35c-5be927427952)
for 30 seconds while the device is plugged in. Then we unplug the device, while still holding the reset button, for 30 more seconds. Finally 
we plug the device back in while holding the reset button for another 30 seconds. If done right the reset button should be held down 
continuously for a total of 90 seconds.
<br></br>
Now that we have read the instructions we can go ahead and begin taking the actions listed within the instructions. After following all of 
the steps I was greeted with the following screen:
![ddwart firmware login](https://github.com/user-attachments/assets/c7031147-2f27-49c2-9ed7-06a4b8d27914)
I set up my username and password and then I was in! Unfortunately whenever I would try to make any changes to the settings I was met with a 
failure page. After some googling I found out that DD-WRT could have issues if you failed to close and re-open your browser post firmware 
flashing. After doing this I am now able to apply my desired changes! As you can see here I changed my GUI style to a nice yellow!
![ddwrtYellow](https://github.com/user-attachments/assets/5a48bcd0-363a-4fa2-ba67-37b6052bb397)
Now all have to do to enable client bridge mode is head on over to the wireless tab and select it from the dropdown.
![ddwartClienBridgeMode](https://github.com/user-attachments/assets/c42df1d0-9f57-4b50-a1b0-5595862a6eda)
Just kidding, if only it was that easy (don't worry it's still pretty easy). What I had to do after changing the device to client bridge mode 
was follow the instructions located on the DD-WRT website [here](https://wiki.dd-wrt.com/wiki/index.php/Client_Bridge). After completing all the steps and rebooting the router I had a stable internet 
connection! So it seemed time to test it on my Pis and old computers.
![ddwrtInternetConnected](https://github.com/user-attachments/assets/669e1b45-202a-4916-9c36-7c7e99f4abf9)
As you can be seen below, I was able to SSH into my Raspberry Pi 4 running Kali and check the repositories for any package updates.
![ddwrtConnectPi](https://github.com/user-attachments/assets/82c82719-7311-4f9c-b0c6-c51a0d2586c9)
