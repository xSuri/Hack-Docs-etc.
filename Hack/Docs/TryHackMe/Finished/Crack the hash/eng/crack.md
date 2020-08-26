Hi!

Let's start!

1. The first thing we have to do is prepare your tools to cracking. I use [hashcat](https://hashcat.net/hashcat/).

2. You can "crack" hash do 2 ways:

1) count on the fact that when we enter it, for example in Google, it will show up already cracked.

2) if it doesn't work, we have to crack it ourselves.

3. There are a few basic hash attacks:

1) Wordlist - we use a dictionary with earlier defined terms (words). (a0)

2) Brute-Force - we generate the password and compare it in cracking. (a3)

3) Combinator - we have a dictionary and rules that define what happens to words. (a1)

4. To see hashcat options and crack types, type:

``` $ hashcat --help``` ,lub ``` $ hashcat -h ```

5. Let's cut to the chase. The first hash and an example of the rest of the hash that we crack will be:

``` 48bb6e862e54f2a795ffc4e541caed4d ```

If we do not know the types more or less, or we want to make sure, I recommend the tool
in kali linux named [hash-identifier](https://tools.kali.org/password-attacks/hash-identifier).

We enter our hash and see simple [MD5](https://en.wikipedia.org/wiki/MD5) (m2600).

We approach crack. We will use the dictionary method for the MD5 hash:

```$ hashcat -a0 -m2600 48bb6e862e54f2a795ffc4e541caed4d /usr/share/wordlists/rockyou.txt```

When the breaking is finished, just add to the command

``` --show```

To display the broken password / word.

6. The second type of hash is [SHA1](https://en.wikipedia.org/wiki/SHA-1) (m100).

Password: ``` password123```

7. The third type of hash is [SHA256](https://en.wikipedia.org/wiki/SHA-2) (m1400).

Password: ```letmein```

8. The next hash type is [Bcrypt](https://en.wikipedia.org/wiki/Bcrypt) (m3200).

Password: ```bleh```

9. The next hash type is [MD4](https://en.wikipedia.org/wiki/MD4) (m900).

Password: ``` Eternity22```

--

10. Another type is [SHA256](https://www.movable-type.co.uk/scripts/sha256.html) (m1400).

Password: ``` paule```

11. The seventh is [NTLM](https://en.wikipedia.org/wiki/NT_LAN_Manager) used in old Windows (m1000).

Password: ``` n63umy8lkf4i```

12. The next one is [SHA512](https://medium.com/@zaid960928/cryptography-explaining-sha-512-ad896365a0c1) (m1800).

Password: ``` waka99```

13. The last one is [HMAC-SHA1](https://en.wikipedia.org/wiki/HMAC) (m160).

Password: ``` 481616481616 ```


// Completing the headings

Task 1.

1) easy

2) password123

3) letmein

4) bleh

5) Eternity22

Task 2.

1) paule

2) n63umy8lkf4i

3) waka99

4) 481616481616
