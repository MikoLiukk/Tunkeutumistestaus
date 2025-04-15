# h3 Fuzzy
## x) Lue/katso/kuuntele ja tiivistä.
https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/
- Artikkelissa paikallinen testipalvelin, joten harjoittelu on turvallista ja laillista.
- Artikkelissa opetetaan fuzzauksen peruskomentoja ja perusteita.
https://github.com/ffuf/ffuf/blob/master/README.md
- Fuzzin githubissa käydään läpi työkalun käyttöä ja vinkkejä
- Artikkelissa myös fuzzin käyttösivu

## a) Fuzzzz. Ratkaise dirfuz-1 artikkelista Karvinen 2023
Ensimmäisellä fuzzauksella huomasin, että kaikki tiedostot olivat 154 kokoisia, joten filtteröin ne pois ```-fs 154```

![Näyttökuva 2025-04-15 185056](https://github.com/user-attachments/assets/3cfa3c88-35b6-4195-8a90-2acbdc4484e9)

Tämän jälkeen 7 kiinnosttavaa kohdetta, kokeilin ensin .git, mutta sieltä ei löytynyt mitään.
.git/ ja wp-admin:istä löytyikin halutut kohteet, joten curlasin ne ja "liput" löytyi.

![Näyttökuva 2025-04-15 185338](https://github.com/user-attachments/assets/aee9ce3f-5d9d-41a8-bf30-18da6ef578e3)

## Fuff me. Asenna FuffMe-harjoitusmaali. Seuraavassa kohdassa käydään läpi tehtävät b - i
Maalituspalvelin ja ohjeet sen asentamiseen https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/
``` 
sudo apt-get install docker.io git ffuf

git clone https://github.com/adamtlangley/ffufme
cd ffufme/
sudo docker build -t ffufme .

sudo docker run -d -p 80:80 ffufme
curl localhost

curl -si localhost|grep titl

mkdir $HOME/wordlists
cd $HOME/wordlists
wget http://ffuf.me/wordlist/common.txt
wget http://ffuf.me/wordlist/parameters.txt
wget http://ffuf.me/wordlist/subdomains.txt
cd -
````
### Nyt irti internetistä.
```
ffuf -w $HOME/wordlists/common.txt -u http://ffuf.me/cd/basic/FUZZ
```

![Näyttökuva 2025-04-15 190049](https://github.com/user-attachments/assets/12cedaf0-9cff-432f-8889-6f0b3f39715d)

## Basic
Löytyi class ja development.log

![Näyttökuva 2025-04-15 190325](https://github.com/user-attachments/assets/c59250e5-afe8-422a-ae1c-5a767844e801)

## Recursion

![Näyttökuva 2025-04-15 190431](https://github.com/user-attachments/assets/373fea72-1340-4ab7-8bde-820bd06b701e)

## File Extensions

![Näyttökuva 2025-04-15 190725](https://github.com/user-attachments/assets/cf7ad3be-b3f4-4e96-98b6-bb1af9392918)

Users.log

## 404 Status

