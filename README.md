# Mr-Robot-CTF-Writeup

Mr Robot CTF Writeup (TryHackMe) By King

![image](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/5e6eb21a-a62a-4da3-a4a2-9042b4527d0a)

Hello everyone. I have done the Mr. ROBOT CTF Room and wanted to share my feedback with it.

![image](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/a30d9652-7722-4c3c-9619-f3fd0f552d49)

TryHackMe Difficulty Rating: Medium
Room Link: https://tryhackme.com/r/room/mrrobot

# Information gathering

First thing first, let’s scan the machine with Nmap to see its open ports:

![1](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/5dc678be-6cc2-4226-9245-454e41e5be05)

So while we explore it, let’s run a gobuster scan to find hidden files and directories.

![2](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/937c333c-c757-4b42-9a54-179a0acfd655)

![3](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/7c5305fc-6fc7-49bb-828c-ae1622f059bf)

The next step I will do is to check this information with a browser

![4](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/f39ba19c-4f0d-4a29-884b-589c1612eb4b)

So, we have a web page as usual. I like that it’s a crazy one. Very cool!

I checked the robots directory and got the first hint!

![5](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/20efb960-0f8c-474a-afc9-f6d1a9c0ef98)

And here we got the first key:

![6](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/3cfafe85-68eb-4631-92c2-5fdd17cdc42d)

I've verified the license directory and I got a base64 string that may be an encrypted password

![7](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/73c6c180-f36b-409b-909a-ea806473f472)

I decoded it and I got this "***********", I guess it's a username with a password probably we will use it later!

Now, we know that the website has a CMS WORDPRESS ( wp-login.php). The next step is to browse our result ;) 

![8](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/7152801a-4943-4448-99e9-8a7495165b91)

So, we have the credentials let's try to enter into the website and see what happens.

After login, this is what we get :

![9](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/83dc16e6-f6df-4a98-af00-c06c61e014e1)

Browsing the panel, we notice that we can edit themes. To do that I go to : Appearance –> Editor –> then on the right click Archives.
Let’s try to get a reverse shell that way.

![10](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/f3dc69a6-9022-4fbc-8430-6dd4cafc5dd0)

I found this php reverse shell on Github: 
``https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php``
After changing the IP and Port, let’s save the file and upload it

Now, before activating it we need to open a Netcat session on our local machine on the port configured in the script for listening:

![image](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/2bd08ad3-3c91-446a-9669-a207477425c7)

Then we load our file by acceding to: ``http://Target-IP-Address/wp-content/themes/twentyfifteen/archive.php``

![11](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/1e081831-33dd-4547-9c0d-207d411cb59e)

And then we got our shell!

![12](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/a1a4d109-79c1-4d30-915e-bc58812157eb)

I searched for the second key, and I found it at ``/home/robot`` but I couldn't see it. First, we have the MD5 password hash of robot so we need to crack it!
I cracked it (with ``https://md5hashing.net/hash/md5/``) and got the message: "*****************************" 
Now I need to switch to the robot account but our shell is not interactive so let's fix it with ``$ python -c 'import pty;pty.spawn("/bin/bash")``

![13](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/09653f00-2e82-4414-b2a3-45f74e913be1)

Here we got the second Key!

# Privileges escalation (Getting root)

Ok now onto the last flag. We need to find a way to get root to get it so let's see the SUID binaries using this command:

``$ find / -type f -perm -04000 -ls 2>/dev/null``

![14](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/c1c45140-b821-46cc-9082-f68eb922eb22)

Note that we have "nmap" as a hint for the third key. So after some investigation, I found there is a weakness in Nmap, which you can set up into interactive mode. That allows to run shell commands inside of nmap

Source: ``https://gtfobins.github.io/gtfobins/nmap/``

![15](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/c4ad4705-bd8a-48ac-ab70-1cc67d2aa5c0)

We got root! Now we just need to get the last 🔑 :

![16](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/de62b4d4-1f5e-40d6-b203-21fc07459c5b)

# I hope you enjoy it 👩‍💻

![images](https://github.com/MRKING20/Mr-Robot-CTF-Writeup-/assets/64786452/1b615ae4-281d-4728-b9b3-4483d74d288d)
