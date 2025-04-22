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


## a) Apache easy mode. Asenna Apache, korvaa sen testisivu ja varmista, että demoni käynnistyy. 


## b) SSHouto. Lisää uusi portti, jossa SSHd kuuntelee.

Lähteet:

Karvinen, T. 2025. Läksyt: h4 Pkg-file-service. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/#laksyt Luettu: 16.4.2025

Karvinen, T. 2025. Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port. Luettavissa: https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh Luettu: 16.4.2025

VMware Inc. 2025. States tutorial, part 1 - Basic Usage. Luettavissa: https://docs.saltproject.io/en/3006/topics/tutorials/states_pt1.html?utm_source=chatgpt.com Luettu: 21.4.2025


