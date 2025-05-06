# h5 Miniprojekti

Viikon tehtävä: Oma miniprojekti

Tässä projektissa toteutin tiedostojen automaattisen aiivouksen ja raportoinnin  Saltilla. Yli 7 päivää vanhat tiedostot poistetaan automaattisesti ja niistä luodaan raportti päivittäin. Käytin projektissa jo valmiiksi luotuja t001 master- ja t002 minion-koneita. Olen jo raportoinut uusien virtuaalikoneiden luomisesta aikaisemmissa tehtävissä, joten en tee sitä nyt tässä raportissa. 

Aloitin tehtävän master koneella. Siirryin kansioon /srv/salt ja loin uuden kansion:

    $ sudo mkdir cleanup-projekti   #Loin uuden kansion cleanup-projekti

Kansion sisälle loin tiedoston:

    $ sudo nano init.sls   #Loin tiedoston nimeltä init.sls

Tiedoston sisälle määrittelin:

    etsi_vanhat_tiedostot:
      cmd.run:
        - name: find /home/vagrant/shared -maxdepth 1 -type f -mtime +7 -print

<img src="Näyttökuva 2025-05-02 153107.png" width="80%">

Tiedostossa määrittelin, että halusin etsiä kaikki yli 7 päivää vanhat tiedostot shared- kansiosta. Tiedostoja etsitään vain kyseistä kansiosta, mutta ei sen alikansioiden sisältä. Halusin lähteä rakentamaan projektia yksi osa kerralla, jotta voin varmistua, että kaikki sen osat toimii. Tämän vuoksi en vielä määritellyt tiedostojen poistoa init.sls-tiedostoon, vaan teen sen vasta kun muut osat projektista on rakennettu. 

Ajoin tilan komennolla:

    $ salt ’*’ state.apply cleanup-projekti   #Ajoin tilan kaikilla minioneilla

Vastaukseksi sain, että tilan ajaminen onnistui. Tila ajettiin ilman virheitä, mikä vahvisti, että etsintäkomento suoritettiin oikein. Varsinainen tulosten tarkistus tapahtuu minionilta erikseen, mutta tässä vaiheessa raportteja tai muuta ei oltu vielä luotu, joten tarkistettavaa ei vielä ollut. 

Palasin takaisin tiedostoon lisäämään:

    etsi_vanhat_tiedostot:
      cmd.run:
        - name: find /home/vagrant/shared -maxdepth 1 -type f -mtime +7 -print

    luo_loki:
      file.managed:
        - name: /home/vagrant/cleanup.log
        - contents: ‘Poistettujen tiedostojen lukumäärä: ‘

Tällä kertaa määrittelin, että poistetuista tiedostoista luodaan raportti, joka tallennetaan minionin kotihakemistoon tiedostoon cleanup.log. Tiedoston sisälle tuli teksti ”Poistettujen tiedostojen lukumäärä:”, minkä perään tulee myöhemmin näkyviin lukumäärä, montako tiedostoa on poistettu. Poistettujen tiedostojen lukumäärää en vielä lisännyt init.sls-tiedostoon. 

Ajoin taas tilan ja sain taas tilan ajon onnistuneen. Tässä ajossa etsittiin yli 7 päivää vanhat tiedostot shared-kansiosta ja kirjoitettiin niiden tiedot cleanup.log-tiedostoon sekä luotiin uusi cleanup_loki.log-tiedosto onnistuneesti. Molemmat tilat suoritettiin ilman virheitä.

<img src="Näyttökuva 2025-05-02 155618.png" width="60%">

Palasin init.sls-tiedoston pariin:

    etsi_tiedostot:
      cmd.run:
        - name:  
            count=$(find /home/vagrant/shared -maxdepth 1 -type f -mtime +7 -print | wc -l)
            echo “Poistettujen tiedostojen lukumäärä: $count”  >> /home/vagrant/cleanup.log
            find /home/vagrant/shared -maxdepth 1 -type f -mtime +7 -print >> /home/vagrant/cleanup.log

    luo_loki:
      file.managed:
        - name: /home/vagrant/cleanup.log
        - contents: ‘Poistetut tiedostot:’

<img src="Näyttökuva 2025-05-02 165423.png" width="60%">

Seuraavaksi lisäsin tiedostoon toiminnon, joka laskee löydettyjen tiedostojen määrän ja listaa ne cleanup.log-tiedostoon. 

Ajoin tilan ja sain taas onnistuneen vastauksen. Löytyi yksi yli 7 päivää vanha tiedosto. Siirryin minionin puolelle tarkistamaan cleanup.login sisällön. Tiedostossa kuitenkin näkyi vain teksti ”Poistetut tiedostot”, vaikka siellä piti olla lukumäärä, sekä tiedoston nimi. Palasin takaisin masterille muokkaamaan init.sls-tiedostoa.

    etsi_tiedostot:
      cmd.run:
        - name:  
            count=$(find /home/vagrant/shared -maxdepth 1 -type f -mtime +7 | wc -l)
            echo “Poistettujen tiedostojen lukumäärä: $count”  > /home/vagrant/cleanup.log
            find /home/vagrant/shared -maxdepth 1 -type f -mtime +7 -print >> /home/vagrant/cleanup.log

    luo_loki:
      file.managed:
        - name: /home/vagrant/cleanup.log
        - contents: ‘Poistetut tiedostot:’
        - unless: test -f /home/vagrant/cleanup.log

Lisäsin tiedostoon file.managed-tilaan tiedon siitä, että cleanup.log-tiedosto luodaan vain, jos sitä ei ole vielä olemassa (unless-ehto). Huomasin, että file.managed-tila ylikirjoittaa cmd.runin tuottaman sisällön, jolloin tiedostossa näkyi vain otsikko, joten muokkasin myös sen. Tallensin ja ajoin tilan. Siirryin taas minionille katsomaan cleanup.logia. Nyt toimi! Nyt siellä näkyi tieto siitä, mitä oli poistettu ja montako kappaletta (edelleenkään ei siis poistettu oikeasti mitään). 

<img src="Näyttökuva 2025-05-02 165807.png" width="60%">

<img src="Näyttökuva 2025-05-02 165829.png" width="60%">

Siirryin taas minionille katsomaan cleanup.logia. Nyt toimi! Nyt siellä näkyi tieto siitä, mitä oli poistettu ja montako kappaletta (edelleenkään ei siis poistettu oikeasti mitään). 

<img src="Näyttökuva 2025-05-02 171549.png" width="60%">

Halusin myös automatisoida kansion siivouksen joka päivälle kello 3.00. Lisäsin tiedostoon cron.present-tilan:

    etsi_tiedostot:
      cmd.run:
        - name:  
            count=$(find /home/vagrant/shared -maxdepth 1 -type f -mtime +7 | wc -l)
            echo “Poistettujen tiedostojen lukumäärä: $count”  > /home/vagrant/cleanup.log
            find /home/vagrant/shared -maxdepth 1 -type f -mtime +7 -print >> /home/vagrant/cleanup.log

    luo_loki:
      file.managed:
        - name: /home/vagrant/cleanup.log
        - contents: ‘Poistetut tiedostot:’
        - unless: test -f /home/vagrant/cleanup.log

    ajastus:
      cron.present:
        - name: ‘salt “*” state.apply cleanup-projekti’
        - user: root
        - hour: 3
        - minute: 0
        
<img src="Näyttökuva 2025-05-03 133352.png" width="60%">

Siirryin minionille tarkistamaan crontabin sisällön ja varmistin, että ajastus oli lisätty onnistuneesti. Annoin komennon:

    $ sudo crontab -l    #Näyttää ajastetut tehtävät crontabista

Crontabissa näkyi merkintä, joka ajaa cleanup-projektin tilan joka päivä kello 3.00.

<img src="Näyttökuva 2025-05-03 134932.png" width="60%">

Siirryin takaisin masterille. Tässä vaiheessa file.managed-tila alkoi tuntua turhalta, koska jo cmd.run-sisällä määrittelin raporttitiedoston luomisen. Halusin luoda selkeämmän rakenteen raporteille, joten loin yhden kansion "cleanup", minkä sisälle tulee aina erikseen raportti poistetuista tiedostoista ja raportin nimeksi "raportti" + päivämäärä. Aikaisemmassa määrittelyssä loin aina uuden raportin, joka ylikirjoitti vanhan, eli vanhoja raportteja ei voinut tarkastella lainkaan. Vaihdoin siis file.managed-tilan file.directory-tilaan:

    etsi_tiedostot:
      cmd.run:
        - name:  
            count=$(find /home/vagrant/shared -maxdepth 1 -type f -mtime +7 | wc -l)
            raporttipolku="/home/vagrant/cleanup/raportti-$(date +%Y-%m-%d).txt"
            echo “Poistettujen tiedostojen lukumäärä: $count”  > $raporttipolku
            find /home/vagrant/shared -maxdepth 1 -type f -mtime +7 -print >> $raporttipolku

    raporttikansio:
      file.directory:
        - name: /home/vagrant/cleanup
        - user: root

    cron_service:
      service.running:
        - name: cron

    ajastus:
      cron.present:
        - name: ‘salt “*” state.apply cleanup-projekti’
        - user: root
        - hour: 3
        - minute: 0

Lisäsin myös cronille service.running-tilan, joka varmistaa, että cron on käynnissä. 

Ajoin tilan vielä kerran varmistuakseni, että kaikki toimi.

<img src="Näyttökuva 2025-05-03 144956.png" width="60%">

<img src="Näyttökuva 2025-05-03 145011.png" width="60%">

Tarkastin taas minionin puolelta, että raportti oli varmasti luotu. Se oli onnistunut.

<img src="Näyttökuva 2025-05-03 145103.png" width="60%">

Tämän jälkeen palasin vielä kerran masterille init.sls-tiedostoon ja lisäsin delete-ominaisuuden, joka oikeasti poistaa löydetyt tiedostot:

    etsi_tiedostot:
      cmd.run:
        - name:  
            count=$(find /home/vagrant/shared -maxdepth 1 -type f -mtime +7 | wc -l)
            raporttipolku="/home/vagrant/cleanup/raportti-$(date +%Y-%m-%d).txt"
            echo “Poistettujen tiedostojen lukumäärä: $count”  > $raporttipolku
            find /home/vagrant/shared -maxdepth 1 -type f -mtime +7 -print -delete >> $raporttipolku

    raporttikansio:
      file.directory:
        - name: /home/vagrant/cleanup
        - user: root

    cron_service:
      service.running:
        - name: cron

    ajastus:
      cron.present:
        - name: ‘salt “*” state.apply cleanup-projekti’
        - user: root
        - hour: 3
        - minute: 0

<img src="Näyttökuva 2025-05-03 145348.png" width="60%">

Tallensin tiedoston ja ajoin sen. Sain onnistuneen vastauksen. 

<img src="Screenshot 2025-05-06 at 13.10.29.png" width="60%">

<img src="Screenshot 2025-05-06 at 13.10.16.png" width="60%">

Siirryin minionin puolelle tarkistamaan raporttikansion. Sielä oli raportti poistetusta tiedostosta, kuten pitikin. Tarkastin vielä shared-kansion, että raportissa mainittu tiedosto 'Näyttökuva 2025-04-04 125800.png' oli oikeasti poistettu. Sitä ei enää näkynyt kansiossa, joten tehtävä oli onnistunut ja löydetty vanha tiedosto oli poistettu kuten pitkin. 

<img src="Näyttökuva 2025-05-03 145755.png" width="60%">


## Lähteet:

Karvinen, T. https://terokarvinen.com/2018/salt-states-i-want-my-computers-like-this/ Luettu: 2.5.2025

https://docs.saltproject.io/en/3006/ref/states/all/salt.states.service.html Luettu: 2.5.2025

https://docs.saltproject.io/en/3006/ref/states/all/salt.states.file.html Luettu: 2.5.2025

https://stackoverflow.com/questions/31389483/find-and-delete-file-or-folder-older-than-x-days Luettu: 2.5.2025

https://unix.stackexchange.com/questions/447360/delete-only-files-older-than-7-days-mtime-and-find Luettu: 2.5.2025

https://docs.saltproject.io/en/3006/ref/states/all/salt.states.cron.html Luettu: 2.5.2025

https://www.geeksforgeeks.org/crontab-in-linux-with-examples/ Luettu: 2.5.2025

https://phoenixnap.com/kb/linux-date-command Luettu: 2.5.2025

