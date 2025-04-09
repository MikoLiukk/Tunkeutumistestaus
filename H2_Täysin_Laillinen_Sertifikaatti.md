# h2 Täysin Laillinen Sertifikaatti


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

