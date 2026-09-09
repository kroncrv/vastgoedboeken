# Vastgoedboeken: roofhandel van Joods vastgoed in Nederland tijdens WOII

Databestanden en alle gemeentelijke rapporten van De Vastgoedboeken: het onderzoek van Pointer (KRO-NCRV) naar de roofhandel van Joods vastgoed tijdens de Tweede Wereldoorlog, en of Nederlandse gemeenten hun eigen rol daarin hebben onderzocht.

Alle publicaties over dit onderzoek: [pointer.nl/vastgoedboeken](https://pointer.nl/vastgoedboeken).

In dit artikel lees je welke keuzes we hebben gemaakt: [placeholder]()

[Klik hier om data en rapporten te downloaden](https://github.com/kroncrv/vastgoedboeken/archive/refs/heads/main.zip)

## Inhoud

- [🏚 Over dit onderzoek](#-over-dit-onderzoek)
- [📂 De databestanden](#-de-databestanden)
  - [📜 verkaufsbucher.csv](#-verkaufsbuchercsv)
  - [📍 locaties.csv](#-locatiescsv)
  - [🏛 gegevens_gemeenten.csv](#-gegevens_gemeentencsv)
  - [📄 rapporten/](#-rapporten)
- [🔎 Zelf onderzoek doen naar je familiegeschiedenis](#-zelf-onderzoek-doen-naar-je-familiegeschiedenis)
  - [🗂️ Bestanden openen](#-bestanden-openen)
  - [🗄️ Archiefonderzoek](#_archiefonderzoek)
- [⚖ Licentie en bronvermelding](#-licentie-en-bronvermelding)
- [✉ Contact en meewerken](#-contact-en-meewerken)

## 🏚 Over dit onderzoek

Tijdens de Tweede Wereldoorlog zijn Joodse woningen, bedrijfspanden, begraafplaatsen, synagogen en stukken grond onteigend en doorverkocht door de Duitse bezetter. Het ging om ongeveer 7.500 transacties van zo'n 9.000 panden.

De administratie van deze roofhandel werd bijgehouden in de zogeheten Verkaufsbücher: achttien boeken met handgeschreven transacties, waarvan het eerste boek verloren is gegaan.

Na de oorlog kwamen de Verkaufsbücher in bezit van het Nationaal Archief. Zij hebben de inhoud gedigitaliseerd en [als open data gepubliceerd](https://www.nationaalarchief.nl/onderzoeken/open-data/open-data-indexen). Pointer doet sinds 2020 jaarlijks een rondvraag onder de gemeenten die in de Verkaufsbücher worden genoemd: is het onteigende vastgoed na de bevrijding weer teruggegeven aan de terugkeerders of nabestaanden. En wat was de rol van de gemeenten in deze roofhandel?

Deze repository bevat de brondata, de resultaten van dat gemeenteonderzoek, en de onderliggende rapporten. Zo kan iedereen nagaan waar de bevindingen op zijn gebaseerd en zelf verder zoeken naar een pand of familie.

### Over de originele Verkaufsbücher-data

Onderstaande uitleg komt rechtstreeks van het Nationaal Archief, en hoort bij `data/verkaufsbucher.csv`.

> Voor zover de wet dit toestaat geeft het Nationaal Archief betreffende dit bestand (verkaufsbucher20170509.csv) de auteursrechten en naburige rechten op, samen met alle aanverwante claims. Dit werk is gepubliceerd vanuit Nederland.

De oorspronkelijke bronpublicatie staat op [nationaalarchief.nl/onderzoeken/open-data/open-data-indexen](https://www.nationaalarchief.nl/onderzoeken/open-data/open-data-indexen).

In dit bestand worden transacties op de volgende manier beschreven:

- **Algemeen**: toegangsnummer (2.09.16, het NBI-archief), inventarisnummer, laufnummer (volgnummer) en beheernummer (Verwaltungsnummer)
- **Te verkopen panden**: plaats, adres(sen), naam/adres/woonplaats van de eigenaar(s)
- **Kopers**: naam/bedrijf van de koper(s), hun adres, woonplaats en land van herkomst
- **Koopdata**: voorlopige en definitieve koopdatum
- **Beheerder, notaris en financiën**: beheerder, notaris, verkoopprijs, aanbetaling, kosten, omzetbelasting, nettobedrag, en overschrijvingsgegevens

In de originele administratie staat deze informatie in 17 kolommen. Voor de data-invoer zijn sommige daarvan verder opgesplitst (bijvoorbeeld doordat een transactie meerdere panden of kopers had). Sommige aantekeningen uit de originele boeken zijn niet overgenomen. De originele boeken kun je raadplegen in de studiezaal van het Nationaal Archief.

## 📂 De databestanden

### 📜 verkaufsbucher.csv

Het `Data`-tabblad uit het Nationaal Archief-bestand. Elke rij is één transactie uit de originele Verkaufsbücher.

Kolommen die je nodig hebt om een transactie te beschrijven:

- `Toegang`, `Inv.nr.`, `Algemeen laufnr`, `Beheernummer` — archiefverwijzing naar het originele boek en de plek daarin
- `Gegevens te verkopen pand(en) Plaats`, `Adres`, `Adres 2` t/m `Adres 8` — adres(sen) van het verkochte pand (meerdere kolommen omdat één transactie soms meerdere panden omvatte)
- `Naam eigenaar 1 persoon` t/m `5`, `Adres 1 eigenaar`, `Woonplaats  eigenaar` — de oorspronkelijke eigenaar(s)
- `Naam koper 1` t/m `9`, `Bedrijf 1/2 koper`, `Adres 1/2 koper`, `land van afkomst koper(s)` — de koper(s)
- `Voorlopige koopdatum`, `Definitieve koopdatum 1/2` — wanneer de verkoop plaatsvond
- `Notaris(sen) Notaris 1`, `Beheerder(s) Beheerder 1` — betrokken notaris en beheerder
- `Financiėle gegegevens Verkoopprijs`, `Aanbetaling`, `nettobedrag` — de financiële afhandeling

De overige kolommen volgen dezelfde indeling als hierboven beschreven bij [Over de originele Verkaufsbücher-data](#over-de-originele-verkaufsbücher-data).

### 📍 locaties.csv

Alle adressen uit `verkaufsbucher.csv`, aangevuld met coördinaten. Eén transactie kan meerdere adressen bevatten.

| Kolom | Betekenis |
|---|---|
| `gemeente_2026` | Indeling naar gemeenten in 2026 |
| `transactie_id` | Koppelt terug naar de transactie (zelfde `Algemeen laufnr`) |
| `straatnaam`, `huisnummer`, `plaats` | Ontleed adres. `huisnummer` kan meerdere nummers bevatten (bijvoorbeeld `123/125/127`) als de brontekst een adresveld met meerdere huisnummers gebruikte — die blijven bij elkaar als één adres, met één coördinaat |
| `lat`, `lon` | Coördinaten (WGS84) |
| `geocode_precisie` | Hoe zeker het punt is: `adres` (exact), `handmatig` (coördinaat handmatig gecontroleerd/aangeleverd, bijvoorbeeld via een lezerstip — niet door PDOK gegokt), `straat` (alleen straatniveau), `plaats` (alleen plaatsniveau), `kadastraal_onbekend` (kadastrale aanduiding, niet te geocoderen), `geen_adres` (bron vermeldt geen straatadres, bijvoorbeeld bij "Bauland" of "Weiland") of `failed` (niet gevonden) |
| `object` | Objecttype (bijvoorbeeld "Weiland") als er geen straatadres was |
| `eigenaar`, `koper` | Naam van eerste eigenaar en koper. Bij sommige transacties zijn meerdere eigenaren of kopers betrokken geweest. Kijk hiervoor in de originele Verkaufsbücher-data. |
| `verkoopprijs`, `koopdatum` | Uit de brondata |
| `rapport` | Link(s) naar het gemeentelijke onderzoeksrapport in `rapporten/` |
| `filter_rapport` | Filter om alle transacties (waarde is `Y`) te zien die in een rapport worden beschreven |
| `rapport_pagina` | Paginaverwijzing binnen dat rapport |
| `kop_artikel`, `link_artikel` | Titel en link naar een Pointer-artikel dat over die transactie gaat |
| `filter_verhaal` | Filter om alle transacties (waarde is `Y`) te zien die in een gepubliceerd Pointer-verhaal voorkomt |
| `filter_pandjesbaas` | Filter om alle transacties (waarde is `Y`) te zien waarvan een van de kopers is aangemerkt als pandjesbaas (iemand die minimaal 5 panden heeft gekocht) |
| `filter_gemeente` | Filter om alle transacties (waarde is `Y`) te zien waarbij een gemeente de koper was|
| `filter_bedrijf` | Filter om alle transacties (waarde is `Y`) te zien waarbij een bedrijf de koper was |
| `needs_review` | `True` als dit coördinaat niet nauwkeurig genoeg is om op de kaart weer te geven, `False` als het adres met voldoende zekerheid is bepaald |

### 🏛 gegevens_gemeenten.csv

De onderzoeksstatus en beoordeling per gemeente. Welk onderzoek een gemeente heeft laten doen, en hoe volledig dat onderzoek is.

| Kolom | Betekenis |
|---|---|
| `gemeente` | Gemeentenaam |
| `gemeentecode` | CBS-gemeentecode (bijvoorbeeld `GM0363` voor Amsterdam), volgens de gemeentelijke indeling op 1 januari 2026 |
| `provincie` | Provincie waarin de gemeente ligt, volgens diezelfde CBS-indeling |
| `transacties` | Aantal Verkaufsbücher-transacties in deze gemeente |
| `transacties_gemeente` | Aantal transacties waarbij de gemeente zelf koper was |
| `onderzoek_nodig` | Gemeenten met minstens 10 transacties of waar de gemeente zelf de koper was |
| `onderzoek` | Of de gemeente onderzoek heeft laten doen |
| `afgerond` | Of dat onderzoek is afgerond |
| `resultaat` | De uitkomst van het onderzoek: `Beperkt vooronderzoek` (geen officieel onderzoeksrapport gepubliceerd), `Publicatie` (onderzoeksrapport gepubliceerd, maar geen reactie uit lokale politiek), `Erkenning` (onderzoeksrapport gepubliceerd, met erkenning of excuses van lokale politiek) of `Moreel rechtsherstel` (onderzoeksrapport gepubliceerd, met + financiële compensatie of andere permanente maatregel) |
| `onafhankelijk` | Of het onderzoek onafhankelijk is uitgevoerd |
| `uitgevoerd_door` | Onderzoeksbureau of instelling |
| `compensatie` | Bekend bedrag aan onderzoekskosten of compensatie |
| `rapport` | Link(s) naar het onderzoeksrapport in `rapporten/` |
| `onafhankelijk_uitgevoerd`, `alle_transacties_onderzocht`, `gemeentelijke_transacties_onderzocht`, `scope_voorbij_verkaufsbucher`, `actieve_rol_gemeente_bezetting`, `rechtsherstel_onderzocht`, `naoorlogse_behandeling_onderzocht`, `naheffingen_onderzocht`, `gemeenschap_betrokken` | Pointer's beoordeling per criterium (Ja/Nee), alleen gevuld voor gemeenten waarvan een rapport is doorgelicht |

De gemeenten Amsterdam, Rotterdam, Utrecht en Den Haag hebben een onderzoek gepubliceerd voordat Pointer aan de rondvraag begon. Hoewel die rapporten wel beschikbaar zijn, hebben we de onderzoeksopzet daarvan niet beoordeeld.

Niet elke gemeente heeft een onderzoeksrapport gepubliceerd. Sommige gemeenten hebben enkel een beperkt vooronderzoek gedaan.

### 📄 rapporten/

De gemeentelijke onderzoeksrapporten als PDF. Sommige bestandsnamen bevatten meerdere gemeenten (bijvoorbeeld "Aalsmeer, Amstelveen, Beverwijk...pdf"), omdat die gemeenten een gezamenlijk onderzoek hebben laten uitvoeren.

## 🔎 Zelf onderzoek doen naar je familiegeschiedenis

Ben je nieuwsgierig geworden naar een familielid of adres in `verkaufsbucher.csv` of `locaties.csv`? Hieronder vind je een aantal tips om verder te zoeken.

### 🗂️ Databestanden openen

**Voor beginners**
Als je een CSV-bestand opent door erop te dubbelklikken, dan is het mogelijk dat rijen en kolommen verschuiven. Heb je nog niet eerder een CSV-bestand geopend, dan vind je hieronder hoe je dat kunt doen via Excel of Google Spreadsheets.

In Excel ga je naar Archief > Importeren > CSV-bestand, en doorloop je alle stappen.

In Google Spreadsheets ga je naar Bestand > Importeren > Uploaden, en doorloop je alle stappen.

**Voor gevorderden**
Je kunt de CSV's het beste doorzoeken op achternaam of adres. Daarnaast is `locaties.csv` ook op een kaart te tonen als je de `lat`/`lon`-kolommen gebruikt.

### 🗄️ Archiefonderzoek

Vind je een interessante naam of adres, dan zijn dit goede vervolgstappen:

- **[Nederlands Beheersinstituut](https://www.nationaalarchief.nl/onderzoeken/zoekhulpen/nederlandse-beheersinstituut-nbi-1945-1968) (NBI)**: hier liggen de dossiers van overlevenden en nabestaanden die hun woning weer wilden terugkrijgen. In dit archief zoek je op de achternaam en woonplaats van een persoon. Vervolgens kun je dat dossier ter inzage aanvragen bij het Nationaal Archief in Den Haag.
- **[Centraal Archief Bijzondere Rechtspleging](https://www.nationaalarchief.nl/onderzoeken/zoekhulpen/centraal-archief-bijzondere-rechtspleging-cabr) (CABR)**: in dit archief vind je de dossiers van personen die na de Tweede Wereldoorlog zijn onderzocht op collaboratie. Het gaat om ruim 400.000 personen. In deze dossiers vind je getuigenverklaringen, rechterlijke oordelen en andere correspondentie. Op de website [Oorlog Voor De Rechter](https://oorlogvoorderechter.nl/) kun je zoeken op personen die in het CABR zitten: handig om mogelijke kopers van Joodse woningen te onderzoeken.
- **[NIOD](https://www.niod.nl/home/)**: het NIOD heeft een uitgebreid archief van documenten, foto's en video's die over de Tweede Wereldoorlog gaan. Een groot deel van dat archief is gedigitaliseerd en online beschikbaar.
- **[Oorlogsbronnen](https://www.oorlogsbronnen.nl/)**: over de Tweede Wereldoorlog zijn talloze digitale bronnen te vinden. Een van de beste startpunten voor je onderzoek is Oorlogsbronnen: deze website doorzoekt meerdere bronnen tegelijk en geeft context voor overkoepelende onderwerpen.
- **[Arolsen Archives](https://arolsen-archives.org/en/)**: het grootste archief over slachtoffers en overlevenden van de Holocaust.
- **[WieWasWie](https://wiewaswie.nl/)**: voor genealogisch onderzoek. Geboorte-, huwelijks- en overlijdensakten kunnen je helpen om familierelaties te reconstrueren.
- **[Joods Monument](https://www.joodsmonument.nl)**: biografische gegevens van Joodse Nederlanders die de Holocaust niet overleefden, vaak inclusief foto's en familieverhalen.
- **[Delpher](https://www.delpher.nl/)**: in dit krantenarchief kun je artikelen vinden van 1618 tot 1995. Over sommige panden of adressen zijn artikelen geschreven, en op sommige namen kun je (overlijdens)advertenties vinden.
- **Gemeentelijke en regionale archieven**: veel details over een specifiek pand of gezin zijn niet in het Nationaal Archief te vinden, maar wel in het archief van de gemeente of streekarchief waar het pand stond. Daarnaast hebben veel regionale archieven grote beeldbanken met foto's. Mogelijk staat daar het pand of de persoon op die jij zoekt.

Loop je vast, of vind je iets dat aanvulling of correctie verdient? Zie [Contact en meewerken](#-contact-en-meewerken).

## ⚖ Licentie en bronvermelding

Deze repository combineert twee soorten materiaal met elk hun eigen herkomst:

- **`data/verkaufsbucher.csv`** is Nationaal Archief open data. Het Nationaal Archief heeft zelf afstand gedaan van auteursrecht op dit bestand (zie [hierboven](#over-de-originele-verkaufsbücher-data)). Voor dit bestand geldt dus geen Pointer-licentie.
- **Alle overige bestanden** (`locaties.csv`, `gegevens_gemeenten.csv`, de rapporten in `rapporten/`, en deze README) zijn eigen werk van Pointer (KRO-NCRV), gepubliceerd onder [Creative Commons Attribution 4.0](https://creativecommons.org/licenses/by/4.0/deed.nl) (CC BY 4.0). Je mag dit materiaal gebruiken en aanpassen, ook commercieel, mits je Pointer (KRO-NCRV) als bron vermeldt. Volledige tekst in [LICENSE](LICENSE).

## ✉ Contact en meewerken

Klopt er iets niet, mis je een gemeente, of heb je aanvullende informatie? Meld het via een issue op deze repository, of neem contact op met de redactie van Pointer: pointer@kro-ncrv.nl.
