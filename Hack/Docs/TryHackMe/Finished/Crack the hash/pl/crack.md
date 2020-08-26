Witam!

1. Pierwsze co musimy zrobić to przygotować swoje stanowisko ja będę łamać [hashcat](https://hashcat.net/hashcat/)'em.


2. Można na 2 sposoby "łamać" hash:

a) liczyć na to, że jak go wpiszemy np w Google to nam się pokaże już złamany.

b) jak to nie zadziała musimy sami go złamać.


3. Wyróżniamy pare podstawowych ataków na hash:

a) słownikowy (wordlist) - używamy słownika z prędzej zdefiniowanymi hasłami. (a0)

b) brutalną siłą (Brute-Force) - generujemy hasło i je porównujemy w łamaniu. (a3)

c) kombinatorowy (Combinator) - mamy słownik i reguły, które definiują co ma się dziać z słowami. (a1)


4. Aby zobaczyć opcje hashcat'a i typy crackowania wpisz:

``` $ hashcat --help``` ,lub ``` $ hashcat -h ```


5. Przejdźmy do sedna. Pierwszym hash'em i przykładem reszty hashy, którego złamiemy będzie:

``` 48bb6e862e54f2a795ffc4e541caed4d ```


Jeżeli jeszcze mniej więcej na oko nie znamy typów, albo chcemy się upewnić polecam narzędzie
w kali linuxie o nazwie [hash-identifier](https://tools.kali.org/password-attacks/hash-identifier).

Wpisujemy tam nasz hash i widzimy, że jest to prosty [MD5](https://en.wikipedia.org/wiki/MD5) (m2600).

Podchodzimy do łamania. Użyjemy metody słownikowej dla hash'u MD5:

```$ hashcat -a0 -m2600 48bb6e862e54f2a795ffc4e541caed4d /usr/share/wordlists/rockyou.txt```

Jak łamanie się zakończy wystarczy dopisać do polecenia

```--show```

Aby wyświetliło nam się złamane hasło/słowo.

Hasło: ```easy```


6. Drugi typ hash'a to [SHA1](https://en.wikipedia.org/wiki/SHA-1) (m100).

Hasło: ```password123```

7. Trzeci typ hash to [SHA256](https://en.wikipedia.org/wiki/SHA-2) (m1400).

Hasło: ```letmein```

8. Następny typ hash to [Bcrypt](https://en.wikipedia.org/wiki/Bcrypt) (m3200).

Hasło: ```bleh```

9. Następny typ hash to [MD4](https://en.wikipedia.org/wiki/MD4) (m900).

Hasło: ```Eternity22```

--

10. Kolejnym typem jest [SHA256](https://www.movable-type.co.uk/scripts/sha256.html) (m1400).

Hasło: ```paule```

11. Siódmym jest [NTLM](https://en.wikipedia.org/wiki/NT_LAN_Manager) wykorzystywany w starych Windows'ach (m1000).

Hasło: ```n63umy8lkf4i```

12. Następny to [SHA512](https://medium.com/@zaid960928/cryptography-explaining-sha-512-ad896365a0c1) (m1800).

Hasło: ```waka99```

13. Ostatnim jest [HMAC-SHA1](https://en.wikipedia.org/wiki/HMAC) (m160).

Hasło: ```481616481616```

14. To byłoby na tyle. Próbuj łamać jak będziesz miał problem nie bój się pytać. Powodzenia!

// Uzupełnienie rubryk 

Zadanie 1.

1) easy

2) password123

3) letmein

4) bleh

5) Eternity22

Zadanie 2.

1) paule

2) n63umy8lkf4i

3) waka99

4) 481616481616
