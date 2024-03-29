---
layout: chapter
title: Peruskomennot
sub-section: true
---
# Peruskomennot
Tutustumme seuraavaksi tavallisimpiin SQL-komentoihin,
joiden avulla voimme lisätä, hakea, muuttaa ja poistaa
tietokannan sisältöä.
Nämä komennot muodostavat perustan relaatiotietokannan käyttämiselle.

## Taulun luonti

Komento `CREATE TABLE` luo taulun,
jossa on halutut sarakkeet.
Esimerkiksi seuraava komento luo taulun
`Tuotteet`, jossa on kolme saraketta:

```sql
CREATE TABLE Tuotteet (id INTEGER PRIMARY KEY, nimi TEXT, hinta INTEGER);
```

Voimme nimetä taulun ja sarakkeet haluamallamme tavalla.
Tällä kurssilla käytäntönä on, että kirjoitamme taulun nimen suurella
alkukirjaimella ja monikkomuotoisena.
Sarakkeiden nimet puolestaan kirjoitamme pienellä alkukirjaimella.

Jokaisesta sarakkeesta ilmoitetaan nimen lisäksi tyyppi.
Tässä taulussa sarakkeet `id` ja `hinta` ovat kokonaislukuja (`INTEGER`)
ja sarake `nimi` on merkkijono (`TEXT`).
Sarake `id` on lisäksi taulun _pääavain_ (`PRIMARY KEY`),
mikä tarkoittaa, että se yksilöi jokaisen taulun rivin
ja voimme viitata sen avulla kätevästi mihin tahansa riviin.

<text-box variant='hint' name='Pääavain'>

Tietokannan taulun pääavain voi olla mikä tahansa sarake tai
sarakkeiden yhdistelmä, joka yksilöi taulun jokaisen rivin.
Käytännössä hyvin tavallinen valinta pääavaimeksi on
kokonaislukumuotoinen id-numero.

Usein haluamme lisäksi, että id-numerolla on _juokseva numerointi_.
Tämä tarkoittaa, että kun tauluun lisätään rivejä,
ensimmäinen rivi saa automaattisesti
id-numeron 1, toinen rivi saa id-numeron 2, jne.

Juoksevan numeroinnin toteuttaminen riippuu tietokantajärjestelmästä.
Esimerkiksi
SQLite-tietokannassa `INTEGER PRIMARY KEY` -tyyppinen sarake
saa automaattisesti juoksevan numeroinnin.

</text-box>

## Tiedon lisääminen

Komento `INSERT` lisää uuden rivin tauluun.
Esimerkiksi seuraava komento lisää rivin
äsken luomaamme tauluun `Tuotteet`:

```sql
INSERT INTO Tuotteet (nimi,hinta) VALUES ('retiisi',7);
```

Tässä annamme arvot lisättävän rivin sarakkeille
`nimi` ja `hinta`.
Kun oletamme, että sarakkeessa `id` on juokseva numerointi,
se saa automaattisesti arvon 1, kun kyseessä on taulun ensimmäinen rivi.
Niinpä tauluun ilmestyy seuraava rivi:

```x
id          nimi        hinta     
----------  ----------  ----------
1           retiisi     7         
```

Jos emme anna arvoa jollekin sarakkeelle,
se saa oletusarvon.
Tavallisessa sarakkeessa oletusarvo on `NULL`,
mikä tarkoittaa tiedon puuttumista.
Esimerkiksi seuraavassa komennossa emme
anna arvoa sarakkeelle `hinta`:

```sql
INSERT INTO Tuotteet (nimi) VALUES ('retiisi');
```

Tällöin tauluun ilmestyy rivi, jossa hinta on `NULL` (eli tyhjä):

```x
id          nimi        hinta     
----------  ----------  ----------
1           retiisi     
```

## Esimerkkitaulu

Oletamme tämän aliluvun tulevissa esimerkeissä,
että olemme lisänneet tauluun `Tuotteet` seuraavat viisi riviä:

```sql
INSERT INTO Tuotteet (nimi,hinta) VALUES ('retiisi',7);
INSERT INTO Tuotteet (nimi,hinta) VALUES ('porkkana',5);
INSERT INTO Tuotteet (nimi,hinta) VALUES ('nauris',4);
INSERT INTO Tuotteet (nimi,hinta) VALUES ('lanttu',8);
INSERT INTO Tuotteet (nimi,hinta) VALUES ('selleri',4);
```

Taulun sisältö on siis seuraavanlainen:

```x
id          nimi        hinta     
----------  ----------  ----------
1           retiisi     7         
2           porkkana    5         
3           nauris      4         
4           lanttu      8         
5           selleri     4         
```

## Tiedon hakeminen

Komento `SELECT` suorittaa _kyselyn_ eli
hakee tietoa taulusta.
Yksinkertaisin tapa tehdä kysely on hakea kaikki tiedot taulusta:

```sql
SELECT * FROM Tuotteet;
```

Tässä tapauksessa kyselyn tulos on seuraava:

```x
id          nimi        hinta     
----------  ----------  ----------
1           retiisi     7         
2           porkkana    5         
3           nauris      4         
4           lanttu      8         
5           selleri     4         
```

Kyselyssä tähti `*` ilmaisee,
että haluamme hakea kaikki sarakkeet.
Kuitenkin voimme myös hakea vain osan sarakkeista.
Esimerkiksi seuraava kysely hakee vain tuotteiden nimet:

```sql
SELECT nimi FROM Tuotteet;
```

Kyselyn tulos on seuraava:

```x
nimi      
----------
retiisi    
porkkana             
nauris               
lanttu               
selleri              
```

Tämä kysely puolestaan hakee nimet ja hinnat:

```sql
SELECT nimi, hinta FROM Tuotteet;
```

Nyt kyselyn tulos muuttuu näin:

```x
nimi        hinta     
----------  ----------
retiisi     7         
porkkana    5         
nauris      4         
lanttu      8         
selleri     4         
```

Kyselyn tuloksena olevat rivit muodostavat taulun,
jota kutsutaan nimellä _tulostaulu_.
Sen sarakkeet ja rivit riippuvat kyselyn sisällöstä.
Esimerkiksi äskeinen kysely loi tulostaulun,
jossa on kaksi saraketta ja viisi riviä.

Tietokannan käsittelyssä esiintyy siis kahdenlaisia tauluja:
tietokannassa kiinteästi olevia tauluja,
joihin on tallennettu tietokannan sisältö,
sekä kyselyjen muodostamia väliaikaisia tulostauluja,
joiden tiedot on koostettu kiinteistä tauluista.

### Hakuehto

Liittämällä `SELECT`-kyselyyn `WHERE`-osan voimme
valita vain osan riveistä halutun ehdon perusteella.
Esimerkiksi seuraava kysely hakee tiedot lantusta:

```sql
SELECT * FROM Tuotteet WHERE nimi='lanttu';
```

Kyselyn tulos on seuraava:

```x
id          nimi        hinta     
----------  ----------  ----------
4           lanttu      8        
```

Ehdoissa voi käyttää sanoja `AND` ja `OR`
samaan tapaan kuin ohjelmoinnissa.
Esimerkiksi seuraava kysely etsii tuotteet,
joiden hinta on välillä 4...6:

```sql
SELECT * FROM Tuotteet WHERE hinta>=4 AND hinta<=6;
```

Kyselyn tulos on seuraava:

```x
id          nimi        hinta     
----------  ----------  ----------
2           porkkana    5         
3           nauris      4         
5           selleri     4         
```

### Järjestäminen

Oletuksena kyselyn tuloksena olevien rivien järjestys
voi olla mikä tahansa.
Voimme kuitenkin määrittää halutun
järjestyksen `ORDER BY` -osan avulla.
Esimerkiksi seuraava kysely hakee tuotteet
järjestyksessä nimen mukaan:

```sql
SELECT * FROM Tuotteet ORDER BY nimi;
```

Kyselyn tulos on seuraava:

```x
id          nimi        hinta     
----------  ----------  ----------
4           lanttu      8         
3           nauris      4         
2           porkkana    5         
1           retiisi     7         
5           selleri     4  
```

Järjestys on oletuksena pienimmästä suurimpaan.
Kuitenkin jos haluamme järjestyksen suurimmasta pienimpään,
voimme lisätä sanan `DESC` sarakkeen nimen jälkeen:

```sql
SELECT * FROM Tuotteet ORDER BY nimi DESC;
```

Tämän seurauksena kyselyn tulos on seuraava:

```x
id          nimi        hinta     
----------  ----------  ----------
5           selleri     4  
1           retiisi     7         
2           porkkana    5         
3           nauris      4         
4           lanttu      8         
```

<text-box variant='hint' name='Mitä tarkoittaa DESC?'>

Tietokantakielessä järjestys on joko _nouseva_ (_ascending_)
eli pienimmästä suurimpaan
tai _laskeva_ (_descending_)
eli suurimmasta pienimpään.
Oletuksena järjestys on nouseva, ja
avainsana `DESC` tarkoittaa siis laskevaa järjestystä.

Itse asiassa SQL-kielessä on myös avainsana `ASC`,
joka tarkoittaa nousevaa järjestystä.
Seuraavat kyselyt toimivat siis samalla tavalla:

```sql
SELECT * FROM Tuotteet ORDER BY nimi;
```

```sql
SELECT * FROM Tuotteet ORDER BY nimi ASC;
```

Käytännössä sanaa `ASC` käytetään kuitenkin äärimmäisen harvoin.

</text-box>


Voimme myös järjestää rivejä usealla eri perusteella.
Esimerkiksi seuraava kysely järjestää rivit ensisijaisesti
kalleimmasta halvimpaan hinnan mukaan ja
toissijaisesti aakkosjärjestykseen nimen mukaan:

```sql
SELECT * FROM Tuotteet ORDER BY hinta DESC, nimi;
```

Kyselyn tulos on seuraava:

```x
id          nimi        hinta     
----------  ----------  ----------
4           lanttu      8         
1           retiisi     7         
2           porkkana    5         
3           nauris      4         
5           selleri     4  
```

Tässä tapauksessa nauris ja selleri järjestetään nimen mukaan,
koska ne ovat yhtä kalliita.

### Erilliset tulosrivit

Joskus tulostaulussa voi olla useita samanlaisia rivejä.
Näin käy esimerkiksi seuraavassa kyselyssä:

```sql
SELECT hinta FROM Tuotteet;
```

Koska kahden tuotteen hinta on 4,
kahden tulosrivin sisältönä on 4:

```x
hinta     
----------
7         
5         
4         
8         
4         
```

Jos kuitenkin haluamme vain erilaiset tulosrivit,
voimme lisätä kyselyyn sanan `DISTINCT`:

```sql
SELECT DISTINCT hinta FROM Tuotteet;
```

Tämän seurauksena kyselyn tulos muuttuu näin:

```x
hinta     
----------
7         
5         
4         
8         
```

## Tiedon muuttaminen

Komento `UPDATE` muuttaa taulun rivejä,
jotka täsmäävät haluttuun ehtoon.
Esimerkiksi seuraava komento muuttaa nauriin
hinnaksi 6:

```sql
UPDATE Tuotteet SET hinta=6 WHERE nimi='nauris';
```

Useita kenttiä voi muuttaa yhdistämällä muutokset pilkuilla.
Esimerkiksi seuraava komento muuttaa nauriin
nimeksi ananas ja hinnaksi 9:

```sql
UPDATE Tuotteet SET nimi='ananas', hinta=9 WHERE nimi='nauris';
```

Muutos voidaan myös laskea aiemman arvon perusteella.
Esimerkiksi seuraava komento kasvattaa
nauriin hintaa yhdellä:

```sql
UPDATE Tuotteet SET hinta=hinta+1 WHERE nimi='nauris';
```

Jos komennossa ei ole ehtoa, se vaikuttaa _kaikkiin_ riveihin.
Esimerkiksi seuraava komento muuttaa jokaisen tuotteen hinnaksi 3:

```sql
UPDATE Tuotteet SET hinta=3;
```

## Tiedon poistaminen

Komento `DELETE` poistaa taulusta rivit, jotka täsmäävät
annettuun ehtoon.
Esimerkiksi seuraava komento poistaa porkkanan tuotteista:

```sql
DELETE FROM Tuotteet WHERE nimi='porkkana';
```

Kuten muuttamisessa, jos ehtoa ei ole,
niin komento vaikuttaa kaikkiin riveihin.
Seuraava komento siis poistaa _kaikki_
tuotteet taulusta:

```sql
DELETE FROM Tuotteet;
```

Komento `DROP TABLE` poistaa tietokannan taulun
(ja kaiken sen sisällön).
Esimerkiksi seuraava komento poistaa taulun `Tuotteet`:

```sql
DROP TABLE Tuotteet;
```
