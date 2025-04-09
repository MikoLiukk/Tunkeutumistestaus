# h2 Täysin Laillinen Sertifikaatti

## x) Lue/katso ja tiivistä.
- Broken Access Control (https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
  - Virheelliset käyttäjäoikeudet ovat havaittavissa noin 4% tutkituista verkkosovelluksista.
  - Paras puolustus on pienimmän oikeuden periaate.
  - API:t ja git hakemistot pitää suojata.

- Server-Side Request Forgery (https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/)
  - Harvinainen mutta vakava
  - Verkkosovellus hakee tietoa varmistamatta oikeellisuutta.
  - Ohittaa monet suojakerrokset
  - Pienimmän oikeuden periaate
  - Vain ennalta määritetty liikenne pitää olla sallittu.

- Insecure direct object references (https://portswigger.net/web-security/access-control/idor)
  - Ohittaa tunnistuksen
  - Yleensä käytetään oikeuksien nostamiseen.
  - Muuttaa käyttäjän hallittavia parametrejä, jotka johtavat Back-end pyyntöihin.

- Path traversal (https://portswigger.net/web-security/file-path-traversal)
  - Komennoilla päästään siirtymään palvelimen kansiorakenteessa.
  - ../
  - Tehokkain tapa suojautua on puhdistaa ja tarkistaa asiakkaan syöte.

- Server-side request forgery (https://portswigger.net/web-security/ssrf)
  - Syötetään verkkopohjiin haitallista koodia.
  - Hyökkäys tehdään osoitesivulta
  - Estäminen onnistuu erottamalla logiikka syötteestä.

- Cross-site scripting (https://portswigger.net/web-security/cross-site-scripting)
   - Ajetaan komentoja odottamattomista sijainneista palvelimelta.
   - Puolustaminen onnistuu suodattamalla ja suojaamalla käyttäjän komennot.
  

## a) Totally Legit Sertificate. Asenna OWASP ZAP, generoi CA-sertifikaatti ja asenna se selaimeesi.
## b) Kettumaista. Asenna "FoxyProxy Standard" Firefox Addon, ja lisää ZAP proxyksi siihen

Tein tehtävät samaan aikaan, koska tarvitsin FoxyProxya, jotta sain liikenteen ohjattua ZAP:in kautta ja näkisin liikenteen ZAP:issa.

Zaproxyn lataaminen onnistui komennolla ```sudo apt install zaproxy```.
Latauksen jälkeen avattiin ZAP.

![Näyttökuva 2025-04-07 180434](https://github.com/user-attachments/assets/829321db-abc7-43dd-bd1f-d5b49225db89)

Tämän jälkeen otettiin ZAP:issa Tools --> Options --> Networks --> Server Certificates.
Generoitiin uusi sertifikaatti ja tallennettiin se.

![Näyttökuva 2025-04-07 181407](https://github.com/user-attachments/assets/cc97818b-ebbb-4a4d-a833-99c655575aad)

Kun Zapissa on uusi sertifikaatti luotu, siirrytään Mozilla Firefoxiin, asetuksista kun haki "cert" niin löytää Certificate Managerin, jonne voi tuoda ZAP:issa luodun sertifikaatin.

![Näyttökuva 2025-04-07 181518](https://github.com/user-attachments/assets/95b445f0-65d9-47a6-86c9-d9ba30b6f7c0)

Tämän jälkeen asensin Foxyproxy lisäosan Firefoxiin ja reititin sen kautta localhost liikenteen. Lisäsin Foxyproxyyn osoitteen 127.0.0.1 ja portin 8080. Lisäsin myös Portswiggerin ja Web Security Academyn osoitteet FoxyProxyyn.

![Näyttökuva 2025-04-08 204710](https://github.com/user-attachments/assets/14e6c142-beb9-4cf3-beb5-bc665a5c89b6)

Sitten muistinkin, että kuvatkin piti saada ZAP:issa ylös tulevia tehtäviä varten, joten ZAP auki --> Tools --> Options --> Display --> Process images in HTTP requests/responses ja rasti ruutuu.

![Näyttökuva 2025-04-08 205239](https://github.com/user-attachments/assets/e2790150-76f2-4e9e-af59-2963055f84b5)

Tämän jälkeen potkaisin jo asennettua Apache2. ```sudo systemctl restart apache2```.
Firefoxiin "localhost" hakukenttään ja tadaa, sain liikenteen kaapattua ZAP:issa.

![Näyttökuva 2025-04-08 205337](https://github.com/user-attachments/assets/a4707a7d-592b-4388-a51f-0a1caca5265a)

## PortSwigger Labs. Ratkaise tehtävät.
Käytin apuna Portswiggerin ohjeita, jotka löytyivät tehtävän annossa.
### Cross Site Scripting (XSS)
Ensimmäiset 2 tehtävää olivat helpompia, koska selaimet on rakennettu luottamaan vastaanottamansa sivun HTML:ään ja JavaScripti:ään.
### c) Reflected XSS into HTML context with nothing encoded (https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded)
Tehtävän sai tehtyä kirjoittamalla hakukenttään JS koodin ```<script>alert(haluamasi viesti tähän)</script>```. Kun teki haun, viesti tuli popuppina selaimeen.

![Näyttökuva 2025-04-08 210700](https://github.com/user-attachments/assets/120c08de-8ece-4977-92a5-7c34cca5ee7b)

## d) Stored XSS into HTML context with nothing encoded (https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded)
Tehtävä oli hyvin samantapainen kuin ensimmäisen, suurin ero oli, että tällä kertaa JS syötettiin kommenttikenttään ilman verkko-osoitetta.

![Näyttökuva 2025-04-08 210944](https://github.com/user-attachments/assets/7cd5e49d-1604-4418-8f50-4fb41ecd4865)

## Path traversal
Tehtävissä tarkoitus on päästä käsiksi /etc/passwd tiedostoon. Tehtävät toimii koska selain/verkkosovellus ei rajoita käyttäjän syötettä, joten selain/verkkosovellus liittää sen suoraan tieodostopolkuun. Tulevissa tehtävissä tarvitsee ZAP:ia tai vastaavaa välimies proxya.

### e) File path traversal, simple case (https://portswigger.net/web-security/file-path-traversal/lab-simple)
Ensimmäinen onnistui, kun tajusi, että url rivillä product?product=* muuttui eri tuotteiden välillä. Oikean tuloksen sai kun url kenttään kirjoitti 
```/image?filename=../../../etc/passwd```.

ZAP:iss response headerin alla oli "salasanat"

![Näyttökuva 2025-04-08 212743](https://github.com/user-attachments/assets/69378d08-50b9-409b-afb3-4b008490c905)

![Näyttökuva 2025-04-09 0931231](https://github.com/user-attachments/assets/891a5a69-6810-4b1c-9fd5-62ed6028e803)

### f) File path traversal, traversal sequences blocked with absolute path bypass
(https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass)

Tehtävä oli todella samantapainen kuin edellinen, ero tehtävissä oli, että tässä tehtävässä "salasanat" saatiin suoralla tiedostopolulla. Lopullinen vastaus oli
```image?filename=/etc/passwd```

![Näyttökuva 2025-04-09 093431](https://github.com/user-attachments/assets/c36d7a9a-a72b-4712-9709-0f1bc31e941d)

### g) File path traversal, traversal sequences stripped non-recursively (https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively)
Tehtävän avain oli tajuta, että verkkosovellus poistaa ```../``` merkkijonot syötteestä, mutta ei huomioi muita versioita. 
Eli sovellus tulkitsee merkkijonon ```....//``` --> ```../```. 
Tehtävän vastaus oli:
```image?filename=....//....//....//etc/passwd```

![Näyttökuva 2025-04-09 093644](https://github.com/user-attachments/assets/fba1736a-f1bd-4598-ad2d-fff5095855b5)

## Insecure Direct Object Reference (IDOR) 
Tehtävässä oli tarkoitus saada ladattua edellinen chat-keskustelu, jossa carlos-käyttäjän salasana on. Tehtävä toimii, koska keskustelut tallennetaan palvelimelle tiedostoina ja tiedostot sisältävät kasvavan numeron.
### h) Insecure direct object references (https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references)
Tehtävän avain on, kun painaa "View transcript" nappia ja huomaa, että palvelin lataa 2.txt tiedoston. ZAP:issa voi tehdä requestin requester tabissa. Kun muokkaa GET pyyntöä ja vaihtaa lataus tiedostoksi 2.txt --> 1.txt todennäköisesti ensimmäinen chatlogi latautuu. Tällä kertaa tiedostossa oli Carloksen salasanan. Tämän jälkeen oli helppoa kirjautua Carloksen käyttäjälle.

![Näyttökuva 2025-04-08 220038](https://github.com/user-attachments/assets/6f20518b-1d96-45ab-9e7a-89d5face32f3)

## Server Side Request Forgery (SSRF)
Tehtävän tarkoitus on poistaa carloksen käyttäjä käyttäen suodattamatonta API kutsua ja http://localhost/admin/.
### i) Basic SSRF against the local server (https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-localhost)
Tehtävässä tarkoitus oli tarkistaa tuotteiden varasto tilanne. POST pyynnöstä löytyi stockApi=, josta ZAP:in request tabin avulla sai muokattua stockApi pyyntöä, jolla sai poistettua Carloksen käyttäjän.

![Näyttökuva 2025-04-08 220909](https://github.com/user-attachments/assets/e40b54c2-9129-4a73-8256-4cde0f10405c)

## Lähteet
https://terokarvinen.com/tunkeutumistestaus/

https://github.com/lansiri/Tunkeutumistestaus-ici001as3a-3003/blob/main/h5.md#c-insecure-direct-object-references-idor

https://github.com/panupeltola/tunkeutumistestaus/blob/main/Harjoitus%205.md

https://owasp.org/Top10/A01_2021-Broken_Access_Control/

https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/

https://portswigger.net/web-security/access-control/idor

https://portswigger.net/web-security/file-path-traversal

https://portswigger.net/web-security/ssrf

https://portswigger.net/web-security/cross-site-scripting


