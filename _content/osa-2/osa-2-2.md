---
layout: chapter
title: Yhteenvetokyselyt
sub-section: true
---
# Yhteenvetokyselyt

Yhteenvetokysely laskee jonkin yksittäisen
arvon taulun riveistä.
Esimerkiksi voimme laskea taulun rivien määrän
tai sarakkeen kaikkien arvojen summan.
Voimme myös ryhmitellä rivejä sarakkeiden mukaan
ja suorittaa jokaiselle ryhmälle yhteenvetokyselyn.

## Koostefunktiot

Yhteenvetokyselyn perustana on koostefunktio,
joka laskee yhteenvetoarvon taulun riveistä.
Tavallisimmat koostefunktiot ovat seuraavat:

funktio | toiminta
--- | ---
`COUNT()` | laskee rivien määrän
`SUM()` | laskee summan arvoista
`MIN()` | hakee pienimmän arvoista
`MAX()` | hakee suurimman arvoista

## Esimerkkejä

Tarkastellaan taas edellisessä aliluvussa luotua taulua `Tuotteet`:

```x
id          nimi        hinta     
----------  ----------  ----------
1           retiisi     7         
2           porkkana    5         
3           nauris      4         
4           lanttu      8         
5           selleri     4         
```

Seuraava kysely hakee taulun rivien määrän:

```sql
SELECT COUNT(*) FROM Tuotteet;
```

```x
COUNT(*)
----------
5
```

Seuraava kysely hakee niiden rivien määrän, joissa hinta on 4:

```sql
SELECT COUNT(*) FROM Tuotteet WHERE hinta=4;
```

```x
COUNT(*)
----------
2
```

Seuraava kysely puolestaan laskee summan tuotteiden hinnoista:

```sql
SELECT SUM(hinta) FROM Tuotteet;
```

```x
SUM(hinta)
----------
28
```

## Rivien valinta

Jos yhteenvetokyselyssä on funktion sisällä tähti `*`,
kysely valitsee kaikki rivit.
Jos taas funktion sisällä on sarakkeen nimi,
kysely valitsee rivit, joissa sarakkeessa ei ole `NULL`.

Tarkastellaan esimerkkinä seuraavaa taulua,
jossa rivin 3 hintana on `NULL`:

```x
id          nimi        hinta     
----------  ----------  ----------
1           retiisi     7         
2           nauris      4         
3           lanttu               
4           selleri     4         
```

Seuraava kysely hakee rivien yhteismäärän:

```sql
SELECT COUNT(*) FROM Tuotteet;
```

```x
COUNT(*)  
----------
4
```

Seuraava kysely taas hakee niiden rivien määrän, joissa hinta ei ole `NULL`:

```sql
SELECT COUNT(hinta) FROM Tuotteet;
```

```x
COUNT(hinta)
------------
3
```

Voimme myös käyttää sanaa `DISTINCT` yhteenvetokyselyssä.
Esimerkiksi seuraava kysely ilmoittaa, montako eri
(ei `NULL`-arvoista) hintaa taulussa on:

```sql
SELECT COUNT(DISTINCT hinta) FROM Tuotteet;
```

```x
COUNT(DISTINCT hinta)
---------------------
2
```

## Ryhmittely

Ryhmittelyn avulla voimme yhdistää rivikohtaista ja koostefunktion
antamaa tietoa.
Ideana on, että rivit jaetaan ryhmiin `GROUP BY` -osassa
annettujen sarakkeiden mukaan ja tämän jälkeen
koostefunktion arvo lasketaan jokaiselle ryhmälle erikseen.

Tarkastellaan esimerkkinä seuraavaa taulua `Myynnit`,
joissa on tietoa tuotteiden myyntimääristä eri vuosina:

```x
id          tuote       vuosi       maara
----------  ----------  ----------  ----------
1           retiisi     2017        120
2           retiisi     2018        85
3           retiisi     2019        150
4           nauris      2017        30
5           nauris      2018        35
6           nauris      2019        10
7           lanttu      2017        75
8           lanttu      2018        100
9           lanttu      2019        80
```

Seuraava kysely hakee vuosikohtaisen yhteismyynnin ryhmittelyn avulla:

```sql
SELECT vuosi, SUM(maara) FROM Myynnit GROUP BY vuosi;
```

Kyselyn tulos on seuraava:

```x
vuosi       SUM(maara)
----------  ----------
2017        225       
2018        220       
2019        240       
```

Esimerkiksi vuoden 2017 yhteismyynti on 120 + 30 + 75 = 225.

Toisaalta voimme hakea tuotekohtaisen yhteismyynnin näin:

```sql
SELECT tuote, SUM(maara) FROM Myynnit GROUP BY tuote;
```

Kyselyn tulos on seuraava:

```x
tuote       SUM(maara)
----------  ----------
lanttu      255       
nauris      75        
retiisi     355      
```

Esimerkiksi lantun yhteismyynti on 75 + 100 + 80 = 255.

## Tulossarakkeen nimentä

Oletuksena tulostaulun sarake saa nimen suoraan
kyselyn perusteella,
mutta voimme halutessamme antaa myös oman nimen
`AS`-sanan avulla.
Tämän ansiosta voimme esimerkiksi selventää,
mistä yhteenvetokyselyssä on kyse.

Esimerkiksi seuraavassa kyselyssä toisen sarakkeen
nimeksi tulee `yhteensa`:

```sql
SELECT tuote, SUM(maara) AS yhteensa FROM Myynnit GROUP BY tuote;
```

Kyselyn tulos on seuraava:

```x
tuote       yhteensa
----------  --------
lanttu      255       
nauris      75        
retiisi     355      
```

Itse asiassa sana `AS` ei ole pakollinen,
eli voisimme kirjoittaa kyselyn myös näin:

```sql
SELECT tuote, SUM(maara) yhteensa FROM Myynnit GROUP BY tuote;
```

## Rajaus ryhmittelyn jälkeen

Voimme lisätä kyselyyn myös
`HAVING`-osan, joka rajaa tuloksia ryhmittelyn jälkeen.
Esimerkiksi seuraava kysely hakee tuotteet,
joiden myynti on ainakin 200:

```sql
SELECT tuote, SUM(maara) AS yhteensa
FROM Myynnit
GROUP BY tuote
HAVING yhteensa >= 200;
```

Kyselyn tulos on seuraava:

```x
tuote       yhteensa
----------  --------
lanttu      255       
retiisi     355      
```

## Kyselyn yleiskuva

Voimme käyttää kyselyissä monilla tavoilla
tähän mennessä oppimiamme osia,
kunhan ne esiintyvät toisiinsa nähden seuraavassa järjestyksessä:

`SELECT` – `FROM` – `WHERE` – `GROUP BY` – `HAVING` – `ORDER BY`

Tässä on esimerkki kyselystä, jossa on kaikki nämä osat:

```sql
SELECT tuote, SUM(maara) AS yhteensa
FROM Myynnit
WHERE vuosi < 2019
GROUP BY tuote
HAVING yhteensa >= 100
ORDER BY tuote;
```

Kysely hakee myynnit tuotteista ennen vuotta 2019,
näyttää vain tuotteet, joiden yhteismyynti näinä vuosina on vähintään 100,
ja järjestää tulokset tuotteen nimen mukaan.
Kyselyn tulos on seuraava:

```x
tuote       yhteensa
----------  --------
lanttu      175       
retiisi     205      
```

Huomaa osien `WHERE` ja `HAVING` ero:
`WHERE` rajoittaa rivejä ennen ryhmittelyä,
kun taas `HAVING` ryhmittelyn jälkeen.
