## Projekti: automatisoitu kansion siivous + raportti

Tämä projekti etsii automaattisesti yli 7 päivää vanhat tiedostot määritetystä kansiosta, poistaa ne ja luo päivittäisen raportin. Se auttaa pitämään kansion siistinä ja säästämään levytilaa ilman manuaalista työtä.

<img src="Screenshot 2025-05-05 at 16.18.58.png" width="80%">

## Lisenssi

Gnu General Public License v3.0

## Tekijä

Carine Attia

## Lataus

    git clone https://github.com/CarineAttia/palvelinten-hallinta

Projekti löytyy ladatun reposition kansiosta:

h5 Miniprojekti

## Käyttöönotto

Lataa projekti:
   
    git clone https://github.com/CarineAttia/palvelinten-hallinta


Kopioi cleanup-kansio Saltin state-hakemistoon:

    sudo cp -r cleanup-projekti /srv/salt/
    
Aja tila kaikilla minioneilla:

    salt '*' state.apply cleanup-projekti




