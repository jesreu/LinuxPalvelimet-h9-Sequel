# LinuxPalvelimet-h9-Sequel
    Kirjoittanut: Jesse Reunamo
    Kurssi:       Linux-palvelimet
    Linkki:       https://terokarvinen.com/2023/linux-palvelimet-2023-alkukevat/

## x)
Mitä etuja on palvelulla, jota käytetään wepissä selaimella, jonka koodi ajetaan palvelimella ja taustalla on tietokanta?
Eritellään palvelun ominaisuuksista hyviä ja huonoja puolia.
### Weppiselain:
Hyvää:
 > - Helposti saatavilla kaikille käyttäjille.
 > - Vuokraus halpaa & skaalaatuvaa pilvipalvelimien avulla.
 > - Laajat vaihtoehdot miten toteuttaa sivusto (Html/css tai React-app yms. kirjasto)
 
 Huonoa:
 > - Mahdolliset turvallisuus riskit wepissä. (vs. paikallisesti toimiva ohjelmisto)
 > - Vaikea taata yhteensopivuus kaikilla mahdollisilla selaimilla -> kehitys usein keskittyy suosituimpiin selaimiin.
### Koodipalvelimella:
Hyvää:
 > - Webbi käyttäjä ei tiedä, mitä palvelimella tapahtuu esim html virhekoodit ei kerro koko totuutta käyttäjälle.
 
 Huonoa:
 > - Suorituspaine palvelimella vs käyttäjäkohtaisesti suorittaminen selaimella
### Tietokanta:
Hyvää:
 > - Säästää ohjelmoijan työtä: valmiit säännöt tietojen käsittelylle.
 > - Mahdollistaa useamman käyttäjän samanaikaisesti.
 > - Virhekäsittely (Mitä tapahtuu, jos tiedon eheys on uhattuna)
 > - Standardi käsittely kieli 
 > - Ei sidottuna tiettyyn ohjelmointi kieleen
 
 Huonoa:
 > - Monimutkainen
 > - Saattaaa olla kallis ratkaisu.
 > - Ylläpito
 > - Ohjelmistoissa on jotain pieniä eroja esimerkiksi: Päivämäärät, vaikka SQL usein toimii ristiin eri tietokantojen välillä.
 
## a)
Näin asennat PostgresSQL ohjelmiston linux koneellesi. Aloita ajamalla komennot komentokehoitteessa:

    sudo apt-get update
    sudo apt-get -y install postgresql
    
Nyt voit ajaa komennot, joilla käynnistät ohjelman, luot sinun linuxkäyttäjälle tietokannan ja tietokantakäyttäjän.

    sudo systemctl start postgresql
    sudo -u postgres createdb yourLinuxUserNameHere
    sudo -u postgres createuser yourLinuxUserNameHere
    
Nyt voit ajaa komennon `psql` joka käynnistää tietokannan. Näet että tietokanta on käynnissä muuttuneesta promptista `yourLinuxUserNameHere=>`.

kuve

Kokeillaan yksinkertaisella testillä toimiiko tietokanta

    SELECT 2+2;
    
Tämän SQL-lauseen tulisi tulostaa luku 4, katsotaan mitä saamme:

kuve

Huomaamme, että komento tulostaa kyselyn tulokset, joka näkyy ----- katkoviivan alapuolella. Kyselyn tulos vastaa haluttua, joten tietokanta toimii.
    

## b)


Voimme lopettaa tietokannan käytön painamalla `ctrl & d`

## Lähteet:

    https://terokarvinen.com/2023/linux-palvelimet-2023-alkukevat/
    https://terokarvinen.com/2016/03/03/install-postgresql-on-ubuntu-new-user-and-database-in-3-commands/?fromSearch=postgres
    https://terokarvinen.com/2016/postgresql-install-and-one-table-database-sql-crud-tutorial-for-ubuntu/
    https://statesmaneffiomedem.wordpress.com/advantages-and-disadvantages-of-client-side-and-server-side-scripting/
    
