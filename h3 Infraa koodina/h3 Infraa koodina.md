# h3 Infraa koodina

Viikon 3 tehtävät:

## x) Lue ja tiivistä.

## Tero Karvinen: Hello Salt Infra-as-code
- Artikkelissa kuvataan, miten luodaan Salt-konfiguraatio, jolla varmistetaan, että tietty tekstitiedosto on olemassa
- Asenna Salt
- Luo kansio "hello" moduulillle. Moduulit asentavat, konfiguroivat ja käynnistävät asioita. Luotu kansio jaetaan myöhemmin kaikille minion-koneille
- Kirjoita infraa koodina
- Aja koodi. Saat selosteen siitä, mitä tapahtui
- Tarkista, että tiedosto on olemassa
- Kun koodin ajaa ensimmäisen kerran, se luo tiedoston. Seuraavilla kerroilla Salt tarkistaa, onko tiedosto jo olemassa. Koska se on jo luotu, se ei tee mitään = idempotenssi
- Tärkeimmät tilat ovat pkg, file, service, user ja cmd

## Salt overview
- Salt käyttää oletuksena YAML-renderöintiä tiedostoissa. Renderöinti tarkoittaa
- YAML:n perussäännöt:
  - data esitetään key: value -pareina
  - Kaksoispiste ja yksi välilyönti (": ")
  - Avainten arvot voi olla eri muodoissa
  - Kaikki avaimet ovat case-sensitive, eli kirjainkoolla on merkitystä
  - Ei tabulaattorin käyttöä, vain välilyöntejä
  - Kommentit alkavat risuaidalla (#)
- YAML koostuu kolmesta peruselementistä:
  - Scalaarit (Scalars)
  - Listat (Lists)
  - Sanakirjat (Dictionary)
- YAML perustuu lohkorakenteisiin, jotka määräytyvät sisennysten perusteella
- Ominaisuudet ja listat täytyy sisentää vähintään yhdellä välilyönnillä, yleensä käytetään kahta
- Kokoelma, mikä on lista tai sanakirjojen alkio alkaa väliviivalla ja välilyönnillä ("- ")

## a) Hei infrakoodi! Kokeile paikallisesti (esim 'sudo salt-call --local') infraa koodina. Kirjota sls-tiedosto, joka tekee esimerkkitiedoston /tmp/ -kansioon.

Tarkoituksenani oli kokeilla infrasktruktuurin hallintaa Saltin avulla. Tehtävänä oli luoda SLS-tiedosto, joka luo esimerkkitiedoston /tmp/ -hakemistoon ja ajaa sen paikallisesti.

Avattuani koneeni loin uuden hakemiston ja SLS-tiedoston sen sisälle:

    sudo mkdir -p /srv/salt/hello/   #Loin uuden hakemiston
    cd /srv/salt/hello/   #Siirryin uuteen hakemistoon
    sudoedit init.sls   #

Tiedoston sisälle kirjoitin:

    /tmp/hellocarine:
      file.managed

Tallensin tiedoston ja ajoin komennon:

    sudo salt-call --local state.apply hello    #

Vastaukseksi sain tilan onnistuneen.

Result: True = tila suoritettiin onnistuneesti

Changes: new = uusi tiedosto /tmp/hellocarine luotiin
    

## b) Aja esimerkki sls-tiedostosi verkon yli orjalla.



## c) Tee sls-tiedosto, joka käyttää vähintään kahta eri tilafunktiota näistä: package, file, service, user. Tarkista eri ohjelmalla, että lopputulos on oikea. Osoita useammalla ajolla, että sls-tiedostosi on idempotentti.




Lähteet:

Karvinen, T. 2025. Läksyt: h3 Infraa koodina. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/#laksyt Luettu: 15.4.2025

Karvinen, T. 2024. Luettavissa: https://terokarvinen.com/2024/hello-salt-infra-as-code/ Luettu: 15.4.2025

VMware, Inc. Salt overview. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml Luettu: 15.4.2025


