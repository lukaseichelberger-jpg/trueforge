---
title: Bewohnerstammblatt
description: Stammdaten der eintretenden Person. Nach Schritt 1 erstellen, mit jeder neuen Angabe nachführen. Geht in Schritt 3 an die Apotheke am Gürbisbach und nach Schritt 4 in aktualisierter Version an Apotheke und Administrator.
---
# Bewohnerstammblatt

## Personalien

| | |
|---|---|
| Name, Vorname | {{ name }}, {{ vorname }} |
| Geburtsdatum | {{ geburtsdatum }} |
| AHV-Nummer | {{ ahv_nummer }} |
| Bisherige Adresse | {{ strasse }}, {{ plz_wohnort }} |
| Zivilstand | {{ zivilstand }} |
| Konfession | {{ konfession }} |
| Krankenkasse / Kartennummer | {{ krankenkasse }} / {{ krankenkassen_nummer }} |

## Aufenthalt

| | |
|---|---|
| Eintrittsdatum | {{ eintrittsdatum }} |
| Wohngruppe / Zimmer | {{ wohngruppe }} / {{ zimmer }} |
| Aufenthaltsart | {{ aufenthaltsart }} |

## Kontakte

| | |
|---|---|
| Rechtliche Vertretung | {{ vertretung }} |
| Primärkontakt | {{ primaerkontakt }}, {{ primaerkontakt_telefon }} |
| Rechnungsempfänger | {{ rechnungsempfaenger }} |
| Ärztliche Betreuung | {% if hausarzt_bleibt == "ja" %}Bisheriger Hausarzt: {{ hausarzt }}{% elif hausarzt_bleibt == "nein" %}Heimarzt{% else %}[hausarzt_bleibt fehlt]{% endif %} |

## Vereinbarungen

| | |
|---|---|
| Amtliche Post weiterleiten | {{ amtliche_post }} |
| TV/Radio (CHF 25/Monat) | {{ tv_radio }} |
| Telefonanschluss (CHF 100 einmalig, CHF 25/Monat) | {{ telefon }} |
| Internetanschluss (CHF 100 einmalig, CHF 15/Monat) | {{ internet }} |
| Fotos/Videos erlaubt | {{ fotos }} |
| Taschengeld | {{ taschengeld }} |
