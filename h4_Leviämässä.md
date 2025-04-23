# h4_Leviämässä.md

## x) Lue ja tiivistä
- Järjestelmät tallentavat hasheja, eikä salasanoja.
- Hash on yksinkertainen funktio
- Hashcat tarvitsee Hash-tyypin jotta toimisi.
- John the ripper pystyy sanakirja-hyökkäyksellä murtamaan salasanan
- Ei toimi vahvoihin salasanoihin. (d&/Ddfn3.aewrir#l20)
- Suojattuja zippejä vastaan voidaan hyökätä hashiin.
- Monet käyttävät oletussalasanoja tai samaa salasanaa kaikkiin laitteisiin
- Hyökkäyksen tavoite on ensimmäisenä tunnistaa käyttäjät ja sitten salasanat.
- Nykyään salasanoja on helpompi murtaa.
- File-format exploit ovat haavoittuvuuksia, jotka sijaitsevat lukijoissa.
- Saastunutta tiedostoa voidaan esimerkiksi jakaa vastaanottajan sähköpostiin.
- Active Directory on palvelu/työkalu, joka helpottaa käyttäjien, ryhmien, laitteiden ja käytänteiden hallintaa

  ## a) Asenna Hashcat ja testaa sen toiminta murtamalla esimerkkisalasana.
  Käytetty apuna Giang Le:n artikkelia samasta aiheesta. https://github.com/gianglex/Courses/blob/main/Tunkeutumistestaus/h4-leviamassa.md
  Ensimmäisenä hashcatin asennus. ```sudo apt-get install -y hashcat```. Tämän jälkeen loin kansion, jossa työskentely tapahtuu.
  Daniel Miesslerillä on sanakirja, jota tässä tapauksessa käytetään. (https://github.com/danielmiessler/SecLists/blob/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz)

  ```wget https://github.com/danielmiessler/SecLists/raw/0b739a3b00ec90f14fb1bb9a06841feffa1561f1/Passwords/Leaked-Databases/rockyou.txt.tar.gz```
  ```tar xf rockyou.txt.tar.gz```
  ```rm rockyou.txt.tar.gz```

  Komennoilla ladataan sanakirja, puretaan se ja poistetaan arkisto.

  ![Näyttökuva 2025-04-22 212812](https://github.com/user-attachments/assets/3282c175-b31c-4eb9-be56-e5078baa89f1)

  ![Näyttökuva 2025-04-22 212840](https://github.com/user-attachments/assets/fedef31d-9938-4f0d-89dc-1313a7cd84c5)

Tämän jälkeen luotiin esimerkkihashi, jotta murtautuminen onniustuu.```awk 'NR==1337 {printf "%s", $0}' rockyou.txt | sha1sum```
Tämän jälkeen tunnistettiin moodi ```hashid -m 'saatu hash'```.

Sitten vain cräkkäämään koodia.

Koska olin virtuaalikoneella, ilmeisesti koneen tehot olivat rajoittuneet, jotka rajoittivat hashcatin toimintaa.
![Näyttökuva 2025-04-22 212924](https://github.com/user-attachments/assets/ccdf4741-cbe0-4e35-9737-6c1fa6f59be6)

Sain kuitenkin ```--force -` lisäyksellä hashcatin toimimaan, hashcatista tuli hieman hitaampi. 
![Näyttökuva 2025-04-22 213555](https://github.com/user-attachments/assets/a6a7ed7b-fe3f-4e1c-b41c-c50a2774e03a)
![Näyttökuva 2025-04-22 213619](https://github.com/user-attachments/assets/1f6edba4-b49c-4d28-943f-6ebfc7d37f43)

Kun hashcat oli tehnyt tehtävänsä, sai oikean vastauksen ```cat tulos```
![Näyttökuva 2025-04-22 213651](https://github.com/user-attachments/assets/17c23e9e-64f4-493a-9f2f-ef3d5380782c)

## c) Asenna John the Ripper ja testaa sen toiminta murtamalla jonkin esimerkkitiedoston salasana.
John the Ripperin sai Kalin paketin hallinasta.```apt-cache search 'john the ripper'``` --> ```apt-cache search 'john the ripper'```

![Näyttökuva 2025-04-22 214704](https://github.com/user-attachments/assets/25840c09-a79e-4f22-957c-7d596a4c30ed)

![Näyttökuva 2025-04-22 214959](https://github.com/user-attachments/assets/28bbeeb8-0aa5-44dc-bc0d-2efe6f0f39f9)

![Näyttökuva 2025-04-22 215030](https://github.com/user-attachments/assets/d75cce0b-27fb-4c59-b654-578315cfc500)

## e) Tiedosto. Tee itse tai etsi verkosta jokin salakirjoitettu tiedosto, jonka saat auki. Murra sen salaus. (Jokin muu formaatti kuin aiemmissa alakohdissa kokeilemasi).

![Näyttökuva 2025-04-22 215156](https://github.com/user-attachments/assets/8382bbfa-03f4-4e73-aa5e-fa645aaf44b2)

![Näyttökuva 2025-04-22 215333](https://github.com/user-attachments/assets/ee6c4fd7-f7fb-4054-8e3c-1b474cfe8f36)

## f) Tiiviste. Tee itse tai etsi verkosta salasanan tiiviste, jonka saat auki. Murra sen salaus. (Jokin muu formaatti kuin aiemmissa alakohdissa kokeilemasi. Voit esim. tehdä käyttäjän Linuxiin ja murtaa sen salasanan.)
Loin testi käyttäjän Linuxiin ja salasana oli todella turvallinen qwerty. Linkissä olevan youtube videon avulla on ohjeet kuinka tein tehtävän. https://www.youtube.com/watch?v=5MLprTAxYDA&t=10s

![Näyttökuva 2025-04-23 005316](https://github.com/user-attachments/assets/79238233-6486-4ecb-85a4-c068447aec28)

![Näyttökuva 2025-04-23 005605](https://github.com/user-attachments/assets/0a4014bd-918f-46da-9628-2d9625b5f020)

## g) Tee msfvenom-työkalulla haittaohjelma, joka soittaa kotiin (reverse shell). Ota yhteys vastaan metasploitin multi/handler -työkalulla.
En saannut paketteja lähetetyksi, vaikka ohjelmat keskustelivat toistensa kanssa. Tarvitaan jatkoselvityksiä.
Tein seuraavien ohjeiden mukaan. https://www.youtube.com/watch?v=ZqWfDrD2WVY https://github.com/juliusjantti/Tunkeutumistestaus/blob/265d726276a85d300614c51b19ed066e3e04ebec/h4%20Marraskuu2024!.md
https://github.com/gianglex/Courses/blob/main/Tunkeutumistestaus/h4-leviamassa.md

![Näyttökuva 2025-04-23 010419](https://github.com/user-attachments/assets/65bcda86-3e58-457c-9a13-82031f3d10d3)
![Näyttökuva 2025-04-23 015518](https://github.com/user-attachments/assets/5180742a-1a90-4df9-b63a-ded7fbe6afb2)

## Lähteet
https://www.youtube.com/watch?v=5MLprTAxYDA&t=10s
https://www.youtube.com/watch?v=ZqWfDrD2WVY
https://github.com/juliusjantti/Tunkeutumistestaus/blob/265d726276a85d300614c51b19ed066e3e04ebec/h4%20Marraskuu2024!.md
https://terokarvinen.com/tunkeutumistestaus/
https://terokarvinen.com/2022/cracking-passwords-with-hashcat/
https://terokarvinen.com/2023/crack-file-password-with-john/
https://www.oreilly.com/videos/security-penetration-testing/9780134833989/9780134833989-sptt_00_06_00_00/
https://www.oreilly.com/library/view/metasploit-2nd-edition/9798341620032/xhtml/chapter9.xhtml#:-:text=File-Format%20Exploits
https://www.oreilly.com/library/view/the-ultimate-kali/9781835085806/Text/Chapter_12.xhtml#_idParaDest-272
https://www.oreilly.com/library/view/metasploit-2nd-edition/9798341620032/xhtml/chapter6.xhtml#toc-link_85
https://github.com/gianglex/Courses/blob/main/Tunkeutumistestaus/h4-leviamassa.md

