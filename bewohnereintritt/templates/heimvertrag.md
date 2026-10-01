---
title: Heimvertrag
description: Vertrag zwischen Heim und eintretender Person. In Schritt 2 erstellen und an den Administrator senden. Zusatzleistungen tv_radio, telefon, internet mit "ja"/"nein" füllen. Ändert sich nach der Unterzeichnung etwas, neue Version erstellen und den Administrator informieren.
---
# Heimvertrag (Muster)

zwischen dem **Alters- und Pflegeheim St. Otmar, St. Gallen** (nachfolgend «Heim»)

und **{{ vorname }} {{ name }}**, geboren am {{ geburtsdatum }} (nachfolgend «Bewohner/in»),
{% if vertretung %}vertreten durch {{ vertretung }}.{% endif %}

## § 1 Gegenstand

Das Heim stellt der Bewohnerin/dem Bewohner ab dem **{{ eintrittsdatum }}** das Zimmer **{{ zimmer }}**
in der Wohngruppe {{ wohngruppe }} zur Verfügung. Der Aufenthalt erfolgt als **{{ aufenthaltsart }}**.

## § 2 Leistungen und Kosten

| Leistung | Kosten |
|---|---|
| Pension und Betreuung (Tagestaxe) | CHF {{ tagestaxe }} pro Tag |
| Wäsche (im Haus gewaschen) | CHF 120 pro Monat |
{% if tv_radio == "ja" %}| TV/Radio inkl. Strom | CHF 25 pro Monat |
{% endif %}{% if telefon == "ja" %}| Telefonanschluss | CHF 100 einmalig, CHF 25 pro Monat |
{% endif %}{% if internet == "ja" %}| Internetanschluss | CHF 100 einmalig, CHF 15 pro Monat |
{% endif %}
Die Pflegekosten werden nach der eingestuften Pflegestufe gemäss kantonalen Vorgaben verrechnet.
Hausrat- und Haftpflichtversicherung sind im Heim bereits inbegriffen.

## § 3 Rechnungsstellung

Die Rechnungen gehen monatlich an **{{ rechnungsempfaenger }}**.

## § 4 Besondere Vereinbarungen

Keine.

## § 5 Unterschriften

<table class="sign">
<tr><td class="line"></td><td class="gap"></td><td class="line"></td></tr>
<tr><td>Ort, Datum</td><td></td><td>Ort, Datum</td></tr>
<tr><td class="line"></td><td class="gap"></td><td class="line"></td></tr>
<tr><td>Für das Heim</td><td></td><td>{{ vorname }} {{ name }}{% if vertretung %} bzw. {{ vertretung }}{% endif %}</td></tr>
</table>
