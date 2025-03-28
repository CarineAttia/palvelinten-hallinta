# h1 Viisikko


## Viikon 1 tehtävät:
x) Lue ja tiivistä (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva. Ei siis vaadita pitkää eikä essee-muotoista tiivistelmää. Lisää kuhunkin jokin oma kysymys tai huomio.).

a) Asenna Debian 12-Bookworm virtuaalikoneeseen (Poikkeuksellisesti tätä alakohtaa ei tarvitse raportoida, jos siinä ei ole mitään ongelmia).

b) Asenna Salt (salt-minion) Linuxille (uuteen virtuaalikoneeseesi)

c) Viisi tärkeintä. Näytä Linuxissa esimerkit viidestä tärkeimmästä Saltin tilafunktiosta: pkg, file, service, user, cmd. Analysoi ja selitä tulokset.

d) Idempotentti. Anna esimerkki idempotenssista. Aja 'salt-call --local' komentoja, analysoi tulokset, selitä miten idempotenssi ilmenee.


## x)
## Run Salt Command Locally
- Saltia käytetään yleensä ohjaamaan useita koneita verkon yli
- Salt-komentoja voi ajaa paikallisesti ja nähdä tuloksen heti
- hyödyllistä harjoitteluun, testaukseen ja nopeaan käyttöönottoon
- samat Salt-komennot toimivat Linuxissa ja Windowsissa
- tärkeimmät: pkg, file, service, user ja cmd
  
## Salt Quickstart - Salt Stack Master and Slave on Ubuntu Linux
- Saltilla voidaan hallita useita koneita
- Slave = ohjattavat koneet. Voi olla missä tahansa, esim. palomuurin takana, silti niitä voi ohjata
- Master = ohjaava kone. Ainoa, jonka pitää olla saavutettavissa verkosta julkisella osoitteella
- Asenna master. Jos masterilla palomuuri, pitää sallia liikenne porteissa 4505/tcp ja 4506/tcp
- Asenna slave. Slaven pitää tietää, missä master sijaitsee. Slavelle voi antaa oman nimen (id)
- Käynnistä slaven minion-palvelu uudelleen, että se ottaa uudet asetukset käyttöön ja yhdistää masteriin
- Hyväksy slaven avain masterissa
- Testaa yhteys. Jos saat vastauksen slaveilta, on ensimmäinen slave käytössäsi
- Komentojen mukana saattaa tulla turhia varoituksia, ne voi vaan ohittaa
- Kun master-slave yhteys saatu toimimaan, voit opetella state-tiedostojen kirjoittamista 
- State-tiedosto = määrittää, millaisessa tilassa slaven pitää olla, esim. mitä ohjelmia asennettu, mitä asetuksia käytössä
  
## Raportin kirjoittaminen
- Raportoinnilla tarkoitetaan, että kerrotaan tarkasti mitä on tehnyt ja mitä on tapahtunut
- Kirjoita koko ajan samalla, kun teet tehtävää
- Raportin pitää tuottaa sama tulos samassa ympäristössä kaikilla, mukaan lukien harhapolut. Raportoi myös testiympäristö selkeästi
- Kirjaa täsmällisesti, mitä komentoja on annettu ja mitä on tehty
- Merkitse kellonajat, jotta voit seurata työvaiheiden kestoa ja mahdolliset laajemmat viat
- Jos ilmenee virheitä, kuvaile ne selkeästi ja raportoi tarvittaessa
- Käytä mennyttä aikamuotoa: esim. "valitsin", "irroitin"
- Kirjoita helppolukuisesti ja huolellista kieltä. Käytä väliotsikoita tarvittaessa
- Aina viittaus lähteisiin
- Valehtelu (esim. raportoitu, että muka tehty jotain, mitä ei oikeasti ole tehty), tekstin plagiointi ja kuvien laiton kopiointi kiellettyä
  
## WMWare Inc. Salt Install Guide: Linux (DEB)
- Ohjeet kertovat, miten Salt asennetaan käyttöjärjestelmiin, jotka ovat Debianin kaltaisia:
  1. Asenna Salt Projectin repository
  2. Suorita sudo apt update 
  3. Asenna salt-minion, salt-master ja muut Saltin osat
  4. Ota Salt-services käyttöön ja käynnistä
 
## a)
Asensin Debian 12-Bookwormin virtuaalikoneelleni. Asennus onnistui ilman ongelmia.
## b)
## c)
## d)


## Lähteet:

Karvinen, Tero 2025. Palvelinten hallinta: Läksyt. https://terokarvinen.com/palvelinten-hallinta/#laksyt

Karvinen, Tero 2023. Run Salt Command Locally. https://terokarvinen.com/2021/salt-run-command-locally/

Karvinen, Tero 2018. Salt Quickstart - Salt Stack Master and Slave on Ubuntu Linux. https://terokarvinen.com/2018/03/28/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/

Karvinen, Tero 2006. Raportin kirjoittaminen. https://terokarvinen.com/2006/06/04/raportin-kirjoittaminen-4/

WMWare Inc. Salt Install Guide: Linux (DEB). https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/linux-deb.html

Karvinen, Tero. Install Debian on Virtualbox. https://terokarvinen.com/2021/install-debian-on-virtualbox/

