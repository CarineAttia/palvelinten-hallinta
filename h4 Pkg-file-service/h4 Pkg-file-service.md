# h4 Pkg-file-service

Viikon 4 tehtävät:

## x) Lue ja tiivistä

## Tero Karvinen: Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port
- Suuria määriä daemoneja on mahdollista hallita Saltilla konfiguraationhallintajärjestelmän avulla
- Package-file-service on yleinen tapa: pkg.installed asentaa ohjelman, file.managed hallitsee asetustiedostoa ja service.running käynnistää palvelun uudelleen, jotta uudet asetukset saa käyttöön. Jos asetustiedosto muuttuu, palvelu käynnistyy uudelleen watch-määrityksen avulla
- Masterin asetustiedostoon määritellään esimerkiksi uusi SSH-portti, esim. Port 8888
- Aja komento masterilla: sudo salt '*' state.apply sshd
- Testaa muutokset minionilla. Tarkista että portti toimii: nc -vz localhost [portti]
- Yhdistä SSH:lla uuteen porttiin: ssh -p [portti] [käyttäjä]@localhost. Jos saat kirjautumispyynnön, olet onnistunut


## a) Apache easy mode. Asenna Apache, korvaa sen testisivu ja varmista, että demoni käynnistyy. Ensin käsin, vasta sitten automaattisesti. Kirjoita tila sls-tiedostoon. pkg-file-service. Tässä ei tarvita service:ssä watch, koska index.html ei ole asetustiedosto.

Tehtävän tarkoitus oli asentaa ja konfiguroida Apache ensin käsin ja sitten automatisoida se Saltilla käyttäen pkg-file-service -rakennetta. Tarkoituksena oli myös korvata oletussivu omalla.

Aloitin tehtävän tekemällä kaiken käsin minionilla ja annoin komennot:

    sudo apt-get update    #Päivitin pakettien listan
    sudo apt-get install apache2 -y   #Asensin Apachen

<img src="install_apache.png" width="60%">

Asennuksen jälkeen testasin selaimella, että palvelu toimii avaamalla osoitteen http://192.168.88.102, eli minionin IP-osoitteen. Sain näkyviin Apachen oletussivun, eli se toimi.

<img src="apache1.png" width="60%">

Tämän jälkeen korvasin Apachen oletussivun omalla versiollani. Annoin komennon:

    echo “Hello”! :-) | sudo tee /var/www/html/index.html   #Muutin oletussivun

<img src="indext002.png" width="60%">

Testasin, että uusi konfiguroimani sivu varmasti toimii. Katsoin taas selaimelta osoitteen http://192.168.88.102 ja sielä näkyi teksti, jonka olin määritellyt.

<img src="hello1.png" width="60%">

Onnistuin siis tekemään sen käsin ja seuraavaksi se piti automatisoida. Poistin minionilta käsin tehdyt muutokset:

    sudo rm /var/www/html/index.html    #Poistin luomani oletussivun
    sudo apt remove apache2 -y   #Poistin Apachen asennuksen

Testasin taas uudelleen selaimessa minionin IP-osoitteen ja vastaukseksi sain, että muokkaamani sivu ei ollut enää saatavilla.

<img src="apache2.png" width="60%">

Siirryin tämän jälkeen masterille automatisoinnin kimppuun. Kuten minionilla, loin oman oletussivun:

    echo “Hello”! :-) | sudo tee /var/www/html/index.html   #Loin oletussivun Salt-masterille

<img src="indext001.png" width="60%">

Tämän jälkeen loin SLS-tiedoston, joka asensi nApachen, hallitsi index.html -tiedostoa ja varmisti, että palvelu on käynnissä:

<img src="sls-file.png" width="60%">

Tallensin tiedoston ja ajoin komennon:

    Sudo salt '*' state.apply apache   #Ajoin Salt-tilan kaikilla minioneilla

Vastauksena sain onnistumisia jokaisesta vaiheesta: 

<img src="result1.png" width="60%">

<img src="result2.png" width="60%">

<img src="result3.png" width="60%">

Testasin vielä minionilla, että palvelu varmasti toimii ja on käynnissä. Siirryin taas minionille ja testasin selaimessa. Luomani testisivu oli näkyvissä, eli tehtävä onnistui. 

<img src="hello2.png" width="60%">

## b) SSHouto. Lisää uusi portti, jossa SSHd kuuntelee.

Lähteet:

Karvinen, T. 2025. Läksyt: h4 Pkg-file-service. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/#laksyt Luettu: 16.4.2025

Karvinen, T. 2025. Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port. Luettavissa: https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh Luettu: 16.4.2025

VMware Inc. 2025. States tutorial, part 1 - Basic Usage. Luettavissa: https://docs.saltproject.io/en/3006/topics/tutorials/states_pt1.html?utm_source=chatgpt.com Luettu: 21.4.2025


