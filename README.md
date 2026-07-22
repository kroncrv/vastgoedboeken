# Vastgoedboeken: roofhandel van Joods vastgoed in Nederland tijdens WOII

Databestanden en gemeentelijke onderzoeksrapporten bij het onderzoek van Pointer (KRO-NCRV) naar de verkoop van Joods vastgoed door de Duitse bezetter, en hoe Nederlandse gemeenten daarmee zijn omgegaan.

**[LINK: hier de gepubliceerde Pointer-artikelen over dit onderzoek toevoegen]**

## Inhoud

- [🏚️ Over dit onderzoek](#🏚️-over-dit-onderzoek)
- [📂 De bestanden](#📂-de-bestanden)
  - [📜 verkaufsbucher.csv](#📜-verkaufsbuchercsv)
  - [📍 locaties.csv](#📍-locatiescsv)
  - [🏛️ gegevens_gemeenten.csv](#🏛️-gegevens_gemeentencsv)
  - [📄 rapporten/](#📄-rapporten)
- [🔎 Zelf onderzoek doen naar je familiegeschiedenis](#🔎-zelf-onderzoek-doen-naar-je-familiegeschiedenis)
- [⚖️ Licentie en bronvermelding](#⚖️-licentie-en-bronvermelding)
- [✉️ Contact en meewerken](#✉️-contact-en-meewerken)

## 🏚️ Over dit onderzoek

Tijdens de Tweede Wereldoorlog verkochten de Duitse bezetters panden van eigenaren die vaak Joods waren. Het ging in totaal om circa 7.500 transacties. De administratie daarvan hield de bezetter bij in de zogeheten Verkaufsbücher: achttien boeken, waarvan het eerste (met laufnummer 1 tot 449) verloren is gegaan.

Na de oorlog kwamen de Verkaufsbücher in het archief van het Nederlandse Beheersinstituut (NBI) terecht. Het Nationaal Archief heeft de inhoud gedigitaliseerd en als open data gepubliceerd. Pointer bracht die data samen met eigen onderzoek naar hoe gemeenten met dit verleden zijn omgegaan: welke gemeenten onderzoek lieten doen, wat daaruit kwam, en hoe grondig dat onderzoek was.

Deze repository bevat de brondata, de resultaten van dat gemeenteonderzoek, en de onderliggende rapporten. Zo kan iedereen nagaan waar de bevindingen op zijn gebaseerd, en zelf verder zoeken naar een pand of familie.

### Over de originele Verkaufsbücher-data

Onderstaande uitleg komt rechtstreeks van het Nationaal Archief, en hoort bij `data/verkaufsbucher.csv`.

> Voor zover de wet dit toestaat geeft het Nationaal Archief betreffende dit bestand (verkaufsbucher20170509.csv) de auteursrechten en naburige rechten op, samen met alle aanverwante claims. Dit werk is gepubliceerd vanuit Nederland.

De volledige, actuele bronpublicatie staat op [nationaalarchief.nl/onderzoeken/open-data/open-data-indexen](https://www.nationaalarchief.nl/onderzoeken/open-data/open-data-indexen).

De kolommen in het originele bestand zijn ingedeeld in vijf blokken:

- **Algemeen**: toegangsnummer (2.09.16, het NBI-archief), inventarisnummer, laufnummer (volgnummer) en beheernummer (Verwaltungsnummer)
- **Te verkopen panden**: plaats, adres(sen), naam/adres/woonplaats van de eigenaar(s)
- **Kopers**: naam/bedrijf van de koper(s), hun adres, woonplaats en land van herkomst
- **Koopdata**: voorlopige en definitieve koopdatum
- **Beheerder, notaris en financiën**: beheerder, notaris, verkoopprijs, aanbetaling, kosten, omzetbelasting, nettobedrag, en overschrijvingsgegevens

In de originele boeken staat deze informatie in 17 kolommen. Voor de data-invoer zijn sommige daarvan verder opgesplitst (bijvoorbeeld doordat een transactie meerdere panden of kopers had). Sommige aantekeningen uit de originele boeken zijn niet overgenomen. De originele boeken kun je raadplegen in de studiezaal van het Nationaal Archief.

## 📂 De bestanden

### 📜 verkaufsbucher.csv

Het `Data`-tabblad uit het Nationaal Archief-bestand, ongewijzigd op één kolom na (zie [licentie](#⚖️-licentie-en-bronvermelding)). Elke rij is één transactie uit de originele Verkaufsbücher.

Kolommen die je nodig hebt om de rest te kunnen duiden:

- `Toegang`, `Inv.nr.`, `Algemeen laufnr`, `Beheernummer` — archiefverwijzing naar het originele boek en de plek daarin
- `Gegevens te verkopen pand(en) Plaats`, `Adres`, `Adres 2` t/m `Adres 8` — adres(sen) van het verkochte pand (meerdere kolommen omdat één transactie soms meerdere panden omvatte)
- `Naam eigenaar 1 persoon` t/m `5`, `Adres 1 eigenaar`, `Woonplaats  eigenaar` — de oorspronkelijke eigenaar(s)
- `Naam koper 1` t/m `9`, `Bedrijf 1/2 koper`, `Adres 1/2 koper`, `land van afkomst koper(s)` — de koper(s)
- `Voorlopige koopdatum`, `Definitieve koopdatum 1/2` — wanneer de verkoop plaatsvond
- `Notaris(sen) Notaris 1`, `Beheerder(s) Beheerder 1` — betrokken notaris en beheerder
- `Financiėle gegegevens Verkoopprijs`, `Aanbetaling`, `nettobedrag` — de financiële afhandeling

Overige kolommen volgen dezelfde indeling als hierboven beschreven bij [Over de originele Verkaufsbücher-data](#over-de-originele-verkaufsbücher-data).

### 📍 locaties.csv

Alle transacties uit `verkaufsbucher.csv`, gegeocodeerd tot punten op de kaart. Eén transactie kan meerdere rijen hebben als er meerdere panden bij hoorden.

| Kolom | Betekenis |
|---|---|
| `gemeente_2026` | Huidige gemeente-indeling |
| `transactie_id` | Koppelt terug naar de transactie (zelfde `Algemeen laufnr`) |
| `straatnaam`, `huisnummer`, `plaats` | Ontleed adres |
| `lat`, `lon` | Coördinaten (WGS84) |
| `geocode_precisie` | Hoe zeker het punt is: `adres` (exact), `straat` (alleen straatniveau), `plaats` (alleen plaatsniveau), `kadastraal_onbekend` (kadastrale aanduiding, niet te geocoderen) of `failed` (niet gevonden) |
| `object` | Objecttype (bijvoorbeeld "Weiland") als er geen straatadres was |
| `eigenaar`, `koper` | Naam van eerste eigenaar en koper |
| `verkoopprijs`, `koopdatum` | Uit de brondata |
| `rapport` | Link(s) naar het gemeentelijke onderzoeksrapport in `rapporten/`, als daar via dit transactie is gekoppeld |
| `filter_rapport` | Ruwe waarde uit de brondata: een bestandsnaam, `Y` (rapport bestaat, maar niet aan een specifiek bestand gekoppeld) of leeg (geen rapport) |
| `rapport_pagina` | Paginaverwijzing binnen dat rapport |
| `kop_artikel`, `link_artikel` | Titel en link naar een Pointer-artikel dat dit adres uitlicht |
| `filter_verhaal` | `Y` als dit adres in een gepubliceerd Pointer-verhaal voorkomt (op basis van `kop_artikel`/`link_artikel` of een match met de publicatielijst) |
| `filter_pandjesbaas` | `Y` als de koper is aangemerkt als bevestigde pandjesbaas (iemand die structureel Joods vastgoed opkocht), inclusief transacties waarbij dezelfde koper elders in de dataset al bevestigd is |
| `filter_gemeente`, `filter_bedrijf` | Overige redactionele markeringen, gebruikt voor de filters op de kaart |
| `needs_review` | `True` als dit punt extra archiefonderzoek verdient, `False` als het adres met voldoende zekerheid vaststaat |

### 🏛️ gegevens_gemeenten.csv

Onderzoeksstatus en kwaliteitsbeoordeling per gemeente, samengesteld uit twee brontabellen: welk onderzoek een gemeente heeft laten doen, en hoe Pointer dat onderzoek beoordeelde.

| Kolom | Betekenis |
|---|---|
| `gemeente` | Gemeentenaam |
| `transacties` | Aantal Verkaufsbücher-transacties in deze gemeente |
| `transacties_gemeente` | Aantal transacties waarbij de gemeente zelf koper was |
| `onderzoek_nodig` | Of onderzoek naar deze gemeente relevant is |
| `onderzoek` | Of de gemeente onderzoek heeft laten doen |
| `afgerond` | Of dat onderzoek is afgerond |
| `resultaat` | Uitkomst in het kort (bijvoorbeeld "Erkenning", "Publicatie") |
| `onafhankelijk` | Of het onderzoek onafhankelijk is uitgevoerd |
| `uitgevoerd_door` | Onderzoeksbureau of instelling |
| `compensatie` | Bekend bedrag aan onderzoekskosten of compensatie |
| `rapport` | Link(s) naar het onderzoeksrapport in `rapporten/` |
| `onafhankelijk_uitgevoerd`, `alle_transacties_onderzocht`, `gemeentelijke_transacties_onderzocht`, `scope_voorbij_verkaufsbucher`, `actieve_rol_gemeente_bezetting`, `rechtsherstel_onderzocht`, `naoorlogse_behandeling_onderzocht`, `naheffingen_onderzocht`, `gemeenschap_betrokken` | Pointer's beoordeling per criterium (Ja/Nee), alleen gevuld voor gemeenten waarvan een rapport is doorgelicht |
| `score_inhoud`, `score_impact` | Score van 0 tot 9 op inhoud en impact van het onderzoek |
| `matrix` | Kwalificatie op basis van beide scores (bijvoorbeeld "Goud", "Verdwenen rapport") |

Niet elke gemeente is doorgelicht. Rijen zonder score-kolommen betekenen dat Pointer dat rapport (nog) niet heeft beoordeeld, niet dat het rapport ontbreekt.

### 📄 rapporten/

De 92 gemeentelijke onderzoeksrapporten als PDF. Sommige bestandsnamen bevatten meerdere gemeenten (bijvoorbeeld "Aalsmeer, Amstelveen, Beverwijk...pdf"), omdat die gemeenten samen één onderzoek lieten uitvoeren.

## 🔎 Zelf onderzoek doen naar je familiegeschiedenis

Denk je dat een familielid in `verkaufsbucher.csv` of `locaties.csv` voorkomt? Zo kun je verder zoeken.

Doorzoek eerst de CSV's op achternaam of adres. Excel, Google Sheets of R/Python werken daarvoor prima; `locaties.csv` is ook op een kaart te tonen als je de `lat`/`lon`-kolommen gebruikt.

Vind je een naam, dan zijn dit goede vervolgstappen:

- **Nationaal Archief, archief van het Nederlands Beheersinstituut (NBI)**: hier liggen de originele Verkaufsbücher en aanverwante dossiers over rechtsherstel na de oorlog. Doorzoekbaar via [nationaalarchief.nl](https://www.nationaalarchief.nl).
- **WieWasWie.nl**: voor genealogisch onderzoek. Geboorte-, huwelijks- en overlijdensakten helpen om familierelaties te reconstrueren.
- **Joods Monument** ([joodsmonument.nl](https://www.joodsmonument.nl)): biografische gegevens van Joodse Nederlanders die de Holocaust niet overleefden, vaak inclusief foto's en familieverhalen.
- **Gemeentelijke en regionale archieven**: veel details over een specifiek pand of gezin staan niet landelijk ontsloten, maar wel in het archief van de gemeente of streekarchief waar het pand stond. Zoek op de naam van het archief plus "beeldbank" of "archiefzoeker".

Loop je vast, of vind je iets dat aanvulling of correctie verdient? Zie [Contact en meewerken](#✉️-contact-en-meewerken).

## ⚖️ Licentie en bronvermelding

Deze repository combineert twee soorten materiaal met elk hun eigen herkomst:

- **`data/verkaufsbucher.csv`** is Nationaal Archief open data. Het Nationaal Archief heeft zelf afstand gedaan van auteursrecht op dit bestand (zie [hierboven](#over-de-originele-verkaufsbücher-data)). Voor dit bestand geldt dus geen Pointer-licentie.
- **Alle overige bestanden** (`locaties.csv`, `gegevens_gemeenten.csv`, de rapporten in `rapporten/`, en deze README) zijn eigen werk van Pointer (KRO-NCRV), gepubliceerd onder [Creative Commons Attribution 4.0](https://creativecommons.org/licenses/by/4.0/deed.nl) (CC BY 4.0). Je mag dit materiaal gebruiken en aanpassen, ook commercieel, mits je Pointer (KRO-NCRV) als bron vermeldt. Volledige tekst in [LICENSE](LICENSE).

## ✉️ Contact en meewerken

Klopt er iets niet, mis je een gemeente, of heb je aanvullende informatie? Meld het via een issue op deze repository, of neem contact op met Jerry Vermanen, onderzoeksjournalist bij Pointer: jerry.vermanen@kro-ncrv.nl.
