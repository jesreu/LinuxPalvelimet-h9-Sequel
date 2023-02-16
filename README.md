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
    
Nyt voit ajaa komennon `psql`, joka käynnistää tietokannan. Näet että tietokanta on käynnissä muuttuneesta promptista `yourLinuxUserNameHere=>`.

![tietokantakayntiin](https://user-images.githubusercontent.com/112503770/219339148-ee8eea02-19d1-467d-8e79-fd5eb781362a.png)


Kokeillaan yksinkertaisella testillä toimiiko tietokanta

    SELECT 2+2;
    
Tämän SQL-lauseen tulisi tulostaa luku 4, katsotaan mitä saamme:

![sql_kysely](https://user-images.githubusercontent.com/112503770/219339239-425fef35-bccb-4247-8e91-20ec76f28732.png)

Huomaamme, että komento tulostaa kyselyn tulokset, joka näkyy ----- katkoviivan alapuolella. Kyselyn tulos vastaa haluttua, joten tietokanta toimii.
    
## b)
Luodaan hieman monimutkaisempi tietokanta, jossa kokeilemme CRUD toiminnallisuuden. CRUD = luonti, luku, päivitys ja poisto toiminnallisuudet.

    CREATE TABLE students(student_id SERIAL PRIMARY KEY, name VARCHAR(30), gender CHAR(1));
    CREATE TABLE courses(course_id SERIAL PRIMARY KEY, name VARCHAR(30));
    CREATE TABLE gradesi(course_id INT, student_id INT, grade CHAR(1), PRIMARY KEY(course_id , student_id), CONSTRAINT fk_student FOREIGN KEY(student_id) REFERENCES students(student_id), CONSTRAINT fk_course FOREIGN KEY(course_id) REFERENCES courses(course_id));
    
Grades taulu on kirjoitettu gradesi, koska pitkän komennon kirjoittamisessa helposti tulee virhe jos toinen, niin unohtuneen grade attribuutin takia jouduin tekemään uuden taulun hieman eri nimellä.
    
![monimutkaisempi_taulukko](https://user-images.githubusercontent.com/112503770/219339453-b4d982b2-199a-403b-980e-3af9b92a2937.png)

Tietokannassa on siis oppilaita, kursseja ja arvosanoja.  Arvosana tauluun on liitetty tieto oppilaasta ja kurssista.

### Create
Lisätään nyt hieman tietoa tietokantaan INSERT lauseilla. Tarkistettu SELECT lauseella, että lisäys onnistui.

Kahden oppilaan tiedot:
    INSERT INTO students(name,gender) VALUES('Matti','M');
    INSERT INTO students(name,gender) VALUES('Maija','N');
    SELECT * FROM students;
    
![Oppilasdate](https://user-images.githubusercontent.com/112503770/219339517-9e4d691e-9fe3-4cbc-980f-974b50ea2ade.png)


Kahden kurssin tiedot:
    INSERT INTO courses(name) VALUES('Linux Palvelimet');
    INSERT INTO courses(name) VALUES('Ohjelmointi 1');
    SELECT * FROM courses;
    
![kurssidate](https://user-images.githubusercontent.com/112503770/219339586-8d6cd6f2-49ac-416e-9c47-f090ded35be2.png)
  
Lisätään 2 arvosanaa Matille.
    INSERT INTO gradesi(course_id,student_id, grade) VALUES(1,1,'5');
    INSERT INTO gradesi(course_id,student_id, grade) VALUES(2,1,'4');
    
![Arvosanat](https://user-images.githubusercontent.com/112503770/219339775-25c8a24d-b326-4c56-b6be-198c601fa788.png)

### Read
Muotoillaan monimutkaisempi SELECT lause, jolla voidaan katsoa oppilaiden suorituksia. 
    
    SELECT students.name, students.gender, gradesi.grade, courses.name FROM students JOIN gradesi ON students.student_id = gradesi.student_id JOIN courses ON gradesi.course_id = courses.course_id;
    
 Kysyle ottaa oppilaan nimen, sukupuolen, kurssin arvosanan ja kurssin nimen. JOIN lauseen avulla voidaan napata dataa eri tauluilta samaan kyselyyn JOIN toimii viiteavaimien FOREIGN KEY avulla.
 
![Matin_suoritukset](https://user-images.githubusercontent.com/112503770/219339847-173c6532-d607-4c43-8561-943d09674146.png)
 
### Update
Päivitetään tietokannan tietoja UPDATE lauseella. Vaihdetaan kaikki mies oppilaat naisiksi.

    UPDATE students SET gender='N' WHERE gender='M';
    SELECT * FROM students;
    
![Paivitus](https://user-images.githubusercontent.com/112503770/219339911-7f62d46b-c6eb-4cc4-a99a-94f878fa6411.png)

### Delete
Poistetaan jotain tietoa DELETE lauseella. Tässä vaiheessa mainitsen, että ei pitäisi olla mahdollista poistaa tietoa, joka on viiteavaimena muualla. Kokeillaan poistaa Matti, jonka pitäisi epäonnistua, koska Matilla on arvosanoja. 

    DELETE FROM students WHERE name='Matti';

![eponnipoisto](https://user-images.githubusercontent.com/112503770/219339964-08a84148-64d4-41df-a687-c7cfd17c8734.png)


Ei onnistu, eli kaikki ok. Poistetaan nyt jotain onnistuneesti. Voimme poistaa Maijan, koska Maijalla ei ole arvosanoja.

    DELETE FROM students WHERE name='Maija';
    SELECT * FROM students;

![onnispoisto](https://user-images.githubusercontent.com/112503770/219340154-d65a6937-5c11-4ea5-9a11-324fd2996f80.png)

Voimme lopettaa tietokannan käytön painamalla `ctrl & d` joka lopettaa yhteyden tietokantaan.

## Lähteet:

    https://terokarvinen.com/2023/linux-palvelimet-2023-alkukevat/
    https://terokarvinen.com/2016/03/03/install-postgresql-on-ubuntu-new-user-and-database-in-3-commands/?fromSearch=postgres
    https://terokarvinen.com/2016/postgresql-install-and-one-table-database-sql-crud-tutorial-for-ubuntu/
    https://statesmaneffiomedem.wordpress.com/advantages-and-disadvantages-of-client-side-and-server-side-scripting/
    
