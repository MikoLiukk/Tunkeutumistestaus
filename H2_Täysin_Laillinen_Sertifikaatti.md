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

