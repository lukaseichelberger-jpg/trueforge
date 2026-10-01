---
title: Bewohnerstammblatt
description: Personalien und Kontakte der eintretenden Person. Nach Schritt 1 erstellen; geht in Schritt 3 an die Apotheke am Gürbisbach.
---
# Bewohnerstammblatt

## Personalien

| | |
|---|---|
| Name, Vorname | {{ name }}, {{ vorname }} |
| Geburtsdatum | {{ geburtsdatum }} |
| AHV-Nummer | {{ ahv_nummer }} |
| Bisherige Adresse | {{ adresse }} |
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
| Ärztliche Betreuung | {% if hausarzt_bleibt == "ja" %}Bisheriger Hausarzt: {{ hausarzt }}{% else %}Heimarzt{% endif %} |
