---
layout: chapter
title: SQLite-tietokanta
sub-section: true
---
# SQLite-tietokanta

SQLite on yksinkertainen avoimesti saatavilla oleva tietokantajärjestelmä,
joka soveltuu hyvin SQL-kielen opetteluun.
Voit kokeilla helposti SQL-kieleen
liittyviä asioita SQLiten avulla,
ja käytämme sitä tämän kurssin harjoituksissa.

<text-box variant='hint' name='Tietokantajärjestelmät'>

SQLite on mainio valinta SQL-kielen harjoitteluun,
mutta siinä on tiettyjä rajoituksia,
jotka voivat aiheuttaa ongelmia todellisissa sovelluksissa.

Laajasti käytettyjä avoimia tietokantajärjestelmiä ovat
MySQL ja PostgreSQL.
Niissä on suuri määrä ominaisuuksia,
jotka puuttuvat SQLitestä,
mutta toisaalta niiden asentaminen ja käyttäminen
on vaikeampaa.

Eri tietokantajärjestelmien välillä siirtyminen on onneksi helppoa,
koska kaikissa on samantapainen SQL-kieli.
Tutustumme PostgreSQL-tietokannan käyttämiseen
myöhemmin Tietokantasovellus-kurssilla.

</text-box>


## SQLite-tulkki

SQLite-tulkki on ohjelma, jonka kautta voi käyttää
SQLite-tietokantaa.
Tulkki käynnistyy antamalla komentorivillä komento `sqlite3`.
Tämän jälkeen voit kirjoittaa joko suoritettavia SQL-komentoja tai
pisteellä alkavia SQLite-tulkin omia komentoja.

Jos käyttämälläsi koneella ei ole vielä SQLite-tulkkia,
voit asentaa sen tästä:

* https://www.sqlite.org/download.html

Valitse oman käyttöjärjestelmäsi mukainen paketti,
jonka vieressä on otsikko _command-line tools_
(eli komentorivityökalut).
Tarvittava tiedosto on se, jonka nimi alkaa `sqlite3`.

## Esimerkki

SQLite-tulkissa tietokanta on oletuksena muistissa
(_in-memory database_),
jolloin se on aluksi tyhjä ja katoaa,
kun tulkki suljetaan.
Tämä on hyvä tapa testailla SQL-kielen ominaisuuksia.
Keskustelu tulkin kanssa voi näyttää vaikkapa tältä:

```x
$ sqlite3
SQLite version 3.11.0 2016-02-15 17:29:24
Enter ".help" for usage hints.
Connected to a transient in-memory database.
Use ".open FILENAME" to reopen on a persistent database.
sqlite> CREATE TABLE Tuotteet (id INTEGER PRIMARY KEY, nimi TEXT, hinta INTEGER);
sqlite> .tables
Tuotteet
sqlite> INSERT INTO Tuotteet (nimi,hinta) VALUES ('retiisi',7);
sqlite> INSERT INTO Tuotteet (nimi,hinta) VALUES ('porkkana',5);
sqlite> INSERT INTO Tuotteet (nimi,hinta) VALUES ('nauris',4);
sqlite> INSERT INTO Tuotteet (nimi,hinta) VALUES ('lanttu',8);
sqlite> INSERT INTO Tuotteet (nimi,hinta) VALUES ('selleri',4);
sqlite> SELECT * FROM Tuotteet;
1|retiisi|7
2|porkkana|5
3|nauris|4
4|lanttu|8
5|selleri|4
sqlite> .mode column
sqlite> .headers on
sqlite> SELECT * FROM Tuotteet;
id          nimi        hinta     
----------  ----------  ----------
1           retiisi     7         
2           porkkana    5         
3           nauris      4         
4           lanttu      8         
5           selleri     4         
sqlite> .quit
```

Esimerkissä luomme aluksi taulun `Tuotteet`
ja tarkastamme sitten komennolla `.tables`,
mitä tauluja tietokannassa on.
Ainoa taulu on `Tuotteet`, mikä kuuluu asiaan.

Tämän jälkeen lisäämme tauluun rivejä
ja haemme sitten kaikki rivit taulusta.
SQLite-tulkin oletustapa näyttää tulosrivit pystyviivoin erotettuina
ei ole kovin tyylikäs,
minkä vuoksi parannamme tulostusta komennoilla
`.mode column` (jokaisella sarakkeella on kiinteä leveys) ja
`.headers on` (sarakkeiden nimet näytetään).

Lopuksi suoritamme komennon `.quit`, joka sulkee SQLite-tulkin.

## Tietokanta tiedostossa

Käynnistyksen yhteydessä SQLite-tulkille voi antaa parametrina tiedoston,
johon tietokanta tallennetaan.
Tällöin tietokannan sisältö säilyy tallessa tulkin sulkemisen jälkeen.

Seuraavassa esimerkissä tietokanta tallennetaan
tiedostoon `testi.db`.
Tämän ansiosta tietokannan sisältö on edelleen tallessa,
kun tulkki käynnistetään uudestaan.

```x
$ sqlite3 testi.db
SQLite version 3.11.0 2016-02-15 17:29:24
Enter ".help" for usage hints.
sqlite> CREATE TABLE Tuotteet (id INTEGER PRIMARY KEY, nimi TEXT, hinta INTEGER);
sqlite> .tables
Tuotteet
sqlite> .quit
$ sqlite3 testi.db
SQLite version 3.11.0 2016-02-15 17:29:24
Enter ".help" for usage hints.
sqlite> .tables
Tuotteet
sqlite> .quit
```

## Komennot tiedostosta

Voimme myös ohjata SQLite-tulkille tiedoston,
jossa olevat komennot suoritetaan peräkkäin.
Tämän avulla voimme automatisoida komentojen suorittamista.
Esimerkiksi voimme laatia seuraavan tiedoston `komennot.sql`:

```x
CREATE TABLE Tuotteet (id INTEGER PRIMARY KEY, nimi TEXT, hinta INTEGER);
INSERT INTO Tuotteet (nimi,hinta) VALUES ('retiisi',7);
INSERT INTO Tuotteet (nimi,hinta) VALUES ('porkkana',5);
INSERT INTO Tuotteet (nimi,hinta) VALUES ('nauris',4);
INSERT INTO Tuotteet (nimi,hinta) VALUES ('lanttu',8);
INSERT INTO Tuotteet (nimi,hinta) VALUES ('selleri',4);
.mode column
.headers on
SELECT * FROM Tuotteet;
```

Tämän jälkeen voimme ohjata komennot tiedostosta tulkille näin:

```x
$ sqlite3 < komennot.sql
id          nimi        hinta     
----------  ----------  ----------
1           retiisi     7         
2           porkkana    5         
3           nauris      4         
4           lanttu      8         
5           selleri     4         
```
