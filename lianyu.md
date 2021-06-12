#Lian_Yu

This is a writeup for the room [Lian_Yu by Deamon at TryHackMe](https://tryhackme.com/room/lianyu)

I used Parrot OS to complet the room, so there might be a difference of syntax in the commands!

The target IP for me was 10.10.166.110 and that is what you will be seeing through my screenshots. Just replace by the target IP given to you when you deploy the machine.

Have fun!! 

`CryptoTzipi aka CyberLola`

![alt_text](lianyu/lianyu1.png "image_tooltip")

We will start with our host enumeration by using nmap with the following command

`nmap -sC -A <IP> -T 4`

(the -T 4 switch is just to speed up things a little bit)

![alt_text](lianyu/lianyu2.png "image_tooltip")

Port 21 (FTP), 22(SSH) and 80(HTTP) are open.
I did try to check it FTP would let me login as anonymous, but it didn't :((


Next we head over the browser to check the web page...

![alt_text](lianyu/lianyu3.png "image_tooltip")

We check the page source, as always ...

![alt_text](lianyu/lianyu100.png "image_tooltip")

Nothing really useful here so let's see if we find any hidden directories via gobuster

`gobuster dir -u http://IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`


![alt_text](lianyu/lianyu500.png "image_tooltip")


And we find an interesting directory here!! By peeking at it in the browser we see

![alt_text](lianyu/lianyu4.png "image_tooltip")

Something seems missing ... but by highlighting all the text...

![alt_text](lianyu/lianyu5.png "image_tooltip")

AHA! :)

Doing further directory enumeration ...

`gobuster dir -u http://IP/island -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`

and we find ...

![alt_text](lianyu/lianyu6.png "image_tooltip")

Checking the page source for this, we see a weird comment!!

![alt_text](lianyu/lianyu7.png "image_tooltip")

OK... hehehe...this part got me nicely!! And I admittedly spent way too much time trying to figure it out, when all I had to do was look at it REALLY closely o^_^o NO, you are not supposed to go looking for a ticket somewhere!! It is a DOTticket ...".ticket" and when you finally figure out that this is a file extension, you feel REALLY silly!! Been there, done that so I am here to save you the aggravationg and to really recommend that you learn to look at this kind of think in EVERY detail possible!! I learned a lesson and I hope I can help you learn it as well :))

`gobuster dir -u http://IP/island/2100 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x ticket`

![alt_text](lianyu/lianyu8.png "image_tooltip")

Putting your new finding in the browser...

![alt_text](lianyu/lianyu9.png "image_tooltip")

Let's use Cyber Chef (awesome tool, one of my favorites!) to decode that!!

![alt_text](lianyu/lianyu10.png "image_tooltip")

EXCELLENT!! Now we have some credentials so let's attempt to login to FTP again, but now using the credentials we found

![alt_text](lianyu/lianyu11.png "image_tooltip")

SUCCESS!! We look around, see some files and transfer them into our machine

![alt_text](lianyu/lianyu12.png "image_tooltip")

Going through them..

![alt_text](lianyu/lianyu13.png "image_tooltip")

"slade" appears a lot reading this text! Hmmm..password?Username?

Checking Leave_me_alone.png answered the previous question!!

![alt_text](lianyu/lianyu200.png "image_tooltip")

But this is a password for what?? Hahaha... I did try to SSH using it, but it wouldn't be so easy now, would it? Of course it didn't work :((

Back to checking the rest of the files we took from FTP!

![alt_text](lianyu/lianyu300.png "image_tooltip")

Remember that picture we just saw? Yup, VERY useful here!! :))

![alt_text](lianyu/lianyu400.png "image_tooltip")

That last command? `cat shado` will give you the SSH password :))

Perfect! Now using the SSH credentials we found, we can finally SSH into our victim!!

![alt_text](lianyu/lianyu14.png "image_tooltip")

You know the drill for the user.txt

`cat user.txt`

Now, going for some privesc, usually my first step is checking `sudo -l` to see if the current user can run anything as root

![alt_text](lianyu/lianyu16.png "image_tooltip")

Checking the VERY useful GTFO bins repository here on github

![alt_text](lianyu/lianyu17.png "image_tooltip")

![alt_text](lianyu/lianyu18.png "image_tooltip")

YAY!!!!!

And we are DONE!! :)

![alt_text](lianyu/lianyu19.png "image_tooltip")


I hope this writeup was helpful!


Happy Hacking


                                    
                                    `CryptoTzipi aka CyberLola`








