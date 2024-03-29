
## Mikä on tietokanta?

_Tietokanta_ (_database_) on tietokoneella oleva kokoelma tietoa,
johon voidaan suorittaa hakuja ja jonka sisältöä voidaan muuttaa.
Tietokantoja ovat esimerkiksi:

* nettisivuston käyttäjien tunnukset ja salasanat
* verkkokaupan tuotteet ja varastotilanne
* pankin tiedot asiakkaista ja tilitapahtumista
* päivittäin mitatut säätiedot eri paikoissa
* lentoyhtiön lentoaikataulut ja varaustilanne

Tietokantoja onkin nykyään valtavasti, ja useimmat ihmiset ovat
tavallisen päivän aikana yhteydessä lukuisiin tietokantoihin.


### Tietokantojen haasteet

Tietokantojen tekniseen toteutukseen liittyy monia haasteita,
eikä hyvin toimivan tietokannan toteuttaminen ole helppo tehtävä.
Keskeisiä haasteita ovat:

#### Tiedon määrä

Monessa tietokannassa on suuri määrä tietoa,
johon kohdistuu jatkuvasti hakuja ja muutoksia.
Miten toteuttaa tietokanta niin, että tietoon pääsee
käsiksi tehokkaasti?

#### Samanaikaisuus

Tietokannalla on yleensä useita käyttäjiä,
jotka voivat hakea ja muuttaa tietoa samaan aikaan.
Mitä tietokannan toteutuksessa tulee ottaa huomioon
tähän liittyen?

#### Yllätykset

Tietokannan sisällön tulisi säilyä järkevänä
myös yllättävissä tilanteissa.
Esimerkiksi mitä tapahtuu, jos sähköt katkeavat
juuri silloin, kun tietoa ollaan muuttamassa?

### Tietokantojen kehitys

Tietokantojen kehitys lähti vauhtiin 1970-luvulla
ja alalla oli monia yrittäjiä,
mutta yksi niistä syrjäytti pian muut:
relaatiomalli ja siihen liittyvä kyselykieli SQL.
Relaatiotietokantojen voittokulku on jatkunut
vuosikymmenten ajan tähän päivään asti,
ja useimmat käytössä olevat tietokannat
perustuvat edelleen relaatiomalliin.

Tällä kurssilla tutustumme relaatiotietokantoihin
sekä tietokannan käyttäjän että teknisen toteutuksen näkökulmasta.
Yksi kurssin punainen lanka onkin:
_miksi_ relaatiomalli on niin hyvä
tapa tietokannan toteuttamiseen?

Vaikka relaatiotietokannat ovat edelleen valta-asemassa,
niiden rinnalle on noussut viime aikoina vakavasti otettavia haastajia.
Yksi syy tilanteen muutokseen on ollut tarve uudenlaisille tietokannoille,
jotka sopivat paremmin hajautettuun käyttöön
kuten suurten nettisivustojen taustalle.

Usein esiintyvä termi NoSQL viittaa tietokantaan,
joka on jotain muuta kuin SQL-kieleen perustuva relaatiotietokanta.
Erityisesti dokumenttitietokannat ovat nostaneet päätään viime aikoina.
Vaikka tällä kurssilla keskitymmekin relaatiomalliin,
on siis hyvä pitää mielessä, että muitakin vaihtoehtoja on olemassa.