
## Yksinkertainen tietokanta

Ennen kuin alamme tutustua olemassa oleviin
tietokantajärjestelmiin,
on kuitenkin hyvä kokeilla toteuttaa ensin
tietokannan käsittely _itse_ jollain
yksinkertaisella tavalla ja katsoa,
millaisiin ongelmiin mahdollisesti törmäämme.

Oletetaan, että haluamme toteuttaa tietokannan pankille.
Tietokannassa on tarkoitus säilyttää tietoa asiakkaiden tileistä
ja niiden tapahtumista.

### Tietokannan rakenne

Tallennamme tietokannan tekstitiedostoon `pankki.txt`,
jonka rivit kuvaavat tietokannan tapahtumia.
Erilaisia tapahtumia on kaksi: pankkiin luodaan uusi tili
(aluksi saldona on 0) tai jonkin tilin saldo muuttuu.
Tiedoston sisältö voi näyttää vaikkapa tältä:

```x
LUO TILI
NUMERO: 131778223
OMISTAJA: Uolevi
LUO TILI
NUMERO: 175299717
OMISTAJA: Maija
TEE SIIRTO
TILI: 131778223
SUMMA: 500
TEE SIIRTO
TILI: 131778223
SUMMA: -100
TEE SIIRTO
TILI: 175299717
SUMMA: 100
```

Tässä tapauksessa pankkiin on luotu ensin tilit
Uoleville ja Maijalle.
Tämän jälkeen Uolevin tilille siirretään 500 euroa.
Sitten Uolevin tililtä siirretään Maijan tilille 100 euroa
(eli Uolevin saldo vähenee 100:lla ja Maijan saldo kasvaa 100:lla).
Lopputuloksena Uolevin tilin saldo on 400 ja Maijan tilin saldo on 100.

Ideana on, että lisäämme jokaisen tapahtuman yhteydessä tiedoston loppuun
kolme riviä, jotka kuvaavat tapahtuman.
Sitten kun luemme tiedoston rivit läpi,
saamme tietoomme kaikki pankissa olevat tilit ja myös laskettua
niiden nykyisen saldon.

### Tein itse ja säästin?

Tällainen tietokanta toimii mainiosti,
jos voimme olettaa, että pankissa on vain vähän asiakkaita ja tapahtumia,
vain yksi asiakas käyttää kerrallaan tietokantaa ja tietokone toimii
täysin luotettavasti.
Ikävä kyllä nämä oletukset eivät useinkaan päde.

#### Tiedon määrä

Mitä tapahtuu, jos pankissa on miljoona asiakasta ja jokaisen asiakkaan
tiliin liittyy keskimäärin viisi tapahtumaa päivässä?
Tällöin tiedostoon lisätään noin 15 miljoonaa riviä päivässä
eli vuoden kuluttua tiedostossa on yli viisi miljardia riviä.

Tietokantamme suunnittelussa on vakava tehokkuuteen liittyvä heikkous:
kun haluamme selvittää asiakkaan saldon,
meidän täytyy käydä läpi tiedoston kaikki rivit.
Kun tietokannassa on paljon tietoa, tästä tulee sietämättömän hidasta.

Käytännössä tehokkuuteen liittyvät ongelmat näkyvät usein asiakkaille
siinä, että tietokantaa käyttävä palvelu toimii hitaasti.
Esimerkiksi tässä tapauksessa Maija voisi joutua odottamaan minuutin,
ennen kuin näkee tilinsä saldon verkkopankissa.

#### Samanaikaisuus

Moni järjestelmä toimii hyvin niin kauan, kuin sillä on vain yksi käyttäjä,
mutta samanaikaiset käyttäjät ovat omiaan aiheuttamaan ongelmia.
Mitä esimerkiksi tapahtuu, jos kaksi käyttäjää koettaa luoda tilin samaan aikaan?

Oletetaan esimerkiksi, että tarkoitus olisi luoda tilit Liisalle ja Aapelille
ja kirjoittaa tiedostoon seuraavat rivit:

```x
LUO TILI
NUMERO: 185421761
OMISTAJA: Liisa
LUO TILI
NUMERO: 111562714
OMISTAJA: Aapeli
```

Kuitenkin kun kaksi tietokannan käyttäjää kirjoittaa samaan aikaan
rivejä tiedostoon, rivit voivat _sekoittua_
ja tulos voi olla vaikkapa seuraava tiedosto:

```x
LUO TILI
NUMERO: 185421761
LUO TILI
NUMERO: 111562714
OMISTAJA: Aapeli
OMISTAJA: Liisa
```

Tässä Liisalta tuli tiedostoon
ensin kaksi riviä, sitten Aapelilta kaikki kolme riviä
ja lopuksi Liisalta vielä yksi rivi.
Tämän jälkeen on mahdotonta tietää, mikä tili kuuluu kenellekin,
eli tietokannan sisältö on tärveltynyt.

#### Yllätykset

Vielä yksi ongelma liittyy siihen,
että todellisuudessa tietokone voi sammua _milloin vain_
(esimerkiksi tulee sähkökatko tai joku vain vetää johdon seinästä).
Mitä tapahtuu, jos juuri sillä hetkellä on meneillään
tietokannan päivitys?

Esimerkiksi oletetaan, että Maija siirtää Uoleville
50 euroa ja tarkoitus olisi kirjoittaa tiedostoon seuraavat rivit:

```x
SIIRTO
TILI: 175299717
SUMMA: -50
SIIRTO
TILI: 131778223
SUMMA: 50
```

Kuitenkin sähköt katkeavat kolmen ensimmäisen rivin jälkeen,
minkä seurauksena tiedostoon päätyvät vain seuraavat rivit:

```x
SIIRTO
TILI: 175299717
SUMMA: -50
```

Nyt Maijan tililtä lähti 50 euroa mutta Uolevi ei saanut mitään,
eli 50 euroa _katosi_ eikä tätä edes voi havaita tiedoston sisällöstä,
kun sähköt palaavat.

### Mikä neuvoksi?

Tietokannan käsittelyn toteuttaminen on siis vaikea tehtävä,
emmekä tällä kurssilla koeta toteuttaa kaikkea itse,
vaan luotamme olemassa oleviin tietokantajärjestelmiin.
Alan tutkijat ja järjestelmien kehittäjät
ovat nähneet valtavasti vaivaa tietokannan
toteutukseen liittyvien haasteiden ratkaisemiseksi.
