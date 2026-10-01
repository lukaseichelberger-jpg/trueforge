---
type: form
title: Eintritt – Detailfragen 1/2
description: Schritt 4, an die Angehörigen. Mit only_missing=true senden. Der Text enthält die Hinweise zu Wäsche, Versicherung und (bei Daueraufenthalt) Adressänderung.
---
Guten Tag {{ empfaenger }}

Für den Eintritt von {{ vorname }} {{ name }} am {{ eintrittsdatum }} klären wir noch einige Details. Vorab zur Information:
- Die Wäsche wird im Haus gewaschen (Unkostenbeitrag CHF 120 pro Monat).
- Hausrat- und Haftpflichtversicherung sind im Heim bereits inbegriffen.
{% if aufenthaltsart == "Daueraufenthalt" %}- Bitte melden Sie die Adressänderung beim Einwohneramt.
{% endif %}
Bitte senden Sie uns zusätzlich eine Kopie der Krankenkassenkarte (Vorder- und Rückseite).

| name | label | type | required | options | hint |
|---|---|---|---|---|---|
| rechnungsempfaenger | Rechnungsempfänger | text | ja | | Name und Adresse |
| primaerkontakt | Primärkontakt | text | ja | | |
| primaerkontakt_telefon | Telefon Primärkontakt | text | ja | | |
| hausarzt_bleibt | Kommt der bisherige Hausarzt weiterhin ins Heim? | radio | ja | ja; nein | bei "nein" übernimmt der Heimarzt |
| krankenkasse | Krankenkasse | text | ja | | |
| krankenkassen_nummer | Kartennummer Krankenkasse | text | ja | | |
| amtliche_post | Amtliche Post an die Bewohnerin/den Bewohner weiterleiten? | radio | ja | ja; nein | |
