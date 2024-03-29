## Relaatiomalli

Relaatiomalli syrjäytti kilpailijansa 1970-luvulla ja
on hallinnut tietokantojen maailmaa siitä lähtien.
Mallin ytimessä on kaksi perusideaa:
kaikki tieto tallennetaan tauluihin riveinä,
jotka voivat viitata toisiinsa,
ja tietokannan käyttäjä käsittelee tietoa
SQL-kielellä,
joka kätkee käyttäjältä tietokannan sisäisen toiminnan yksityiskohdat.

### Tietokannan rakenne

Relaatiotietokanta muodostuu _tauluista_ (_table_),
joissa on kiinteät _sarakkeet_ (_column_).
Tauluihin tallennetaan tietoa _riveinä_ (_row_),
joilla on tietyt arvot sarakkeissa.
Jokaisessa taulussa on kokoelma tiettyyn asiaan
liittyvää tietoa.

Seuraavassa kuvassa on esimerkki tietokannasta,
jota voisi käyttää osana verkkokaupan toteutusta.
Tauluissa `Tuotteet`, `Asiakkaat` ja `Ostokset`
on tietoa tuotteista, asiakkaista ja heidän ostoskoriensa sisällöstä.

<img src="/taulut.png">

Tauluissa `Tuotteet` ja `Asiakkaat`
jokaisella rivillä on
yksilöllinen id-numero, jonka avulla niihin voi viitata.
Tämän ansiosta taulussa `Ostokset` voidaan esittää id-numeroiden
avulla, mitä tuotteita kukin asiakas on valinnut.
Tässä esimerkissä Uolevin korissa on porkkana ja selleri
ja Maijan korissa on retiisi, lanttu ja selleri.


### SQL-kieli

_SQL_ (_Structured Query Language_) on vakiintunut tapa
käsitellä relaatiotietokannan sisältöä.
Kielessä on komentoja, joiden avulla tietokannan käyttäjä
(esimerkiksi tietokantaa käyttävä ohjelmoija)
voi lisätä, hakea, muuttaa ja poistaa tietoa.

SQL-komennot muodostuvat avainsanoista
(kuten `SELECT` ja `WHERE`),
taulujen ja sarakkeiden nimistä sekä muista arvoista.
Esimerkiksi komento

```sql
SELECT hinta FROM Tuotteet WHERE nimi='retiisi';
```

hakee tietokannan tuotteista retiisin hinnan.
Komentojen lopussa on puolipiste `;` ja voimme
käyttää välilyöntejä ja rivinvaihtoja haluamallamme tavalla.
Esimerkiksi voisimme kirjoittaa äskeisen komennon myös näin
kolmelle riville:

```sql
SELECT hinta
FROM Tuotteet
WHERE nimi='retiisi';
```

Tutustumme SQL-kieleen tarkemmin materiaalin luvuissa 2–4.

<text-box variant='hint' name='SQL-kielen tausta'>

SQL-kieli syntyi 1970-luvulla, ja siinä on paljon muistumia
_vanhan ajan ohjelmoinnista_.
Tällaisia piirteitä ovat esimerkiksi:

- Avainsanat ovat kokonaisia englannin kielen sanoja,
  ja komennot muistuttavat englannin kielen lauseita.
- Avainsanoissa kirjainkoolla ei ole väliä. Esimerkiksi
  `SELECT`, `select` ja `Select` tarkoittavat samaa.
  Avainsanat on tapana kirjoittaa kokonaan suurilla kirjaimilla.
- Merkki `=` tarkoittaa sekä asetusta että yhtäsuuruusvertailua
  (nykyään ohjelmoinnissa yhtäsuuruusvertailu on yleensä `==`).

SQL-kielestä on olemassa _standardi_,
joka pyrkii antamaan yhteisen pohjan kielelle.
Käytännössä jokainen toteutus toimii kuitenkin
hieman omalla tavallaan.
Tällä kurssilla keskitymme SQL:n ominaisuuksiin,
jotka ovat yleisesti käytettävissä eri tietokannoissa.

</text-box>
 
### Tietokannan sisäinen toiminta

Tietokannan sisällön ja käyttäjän välissä on
_tietokantajärjestelmä_,
jonka tehtävänä on käsitellä käyttäjän
antamat SQL-komennot.
Esimerkiksi kun käyttäjä antaa komennon,
joka hakee tietoa tietokannasta,
tietokantajärjestelmän tulee löytää jokin hyvä tapa
käsitellä komento ja toimittaa tulokset takaisin
käyttäjälle mahdollisimman nopeasti.

SQL-kielen hienoutena on, että käyttäjän riittää
_kuvailla_, mitä tietoa hän haluaa,
minkä jälkeen tietokantajärjestelmä hoitaa likaisen työn
ja hankkii tiedot tietokannan uumenista.
Tämä on mukavaa käyttäjälle, koska hänen ei tarvitse
tietää mitään tietokannan sisäisestä toiminnasta
vaan voi luottaa tietokantajärjestelmään.

Tietokantajärjestelmän toteuttaminen on monimutkainen tehtävä,
ja vain asiaan vihkiytyneet ovat perillä tietokantojen
sisäisen toiminnan yksityiskohdista.
Monet asiat kuuluvat tämän kurssin laajuuden ulkopuolelle,
mutta luvuissa 6 ja 7 pääsemme silti osallisiksi joistakin
tietokantojen taustalla olevista salaisuuksista.

## uusi osa