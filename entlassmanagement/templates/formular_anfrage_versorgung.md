---
type: form
title: Anfrage Anschlussversorgung
description: Schritt 3, an einen Anbieter (Spitex, Sanitätshaus, Physiotherapie, Mahlzeitendienst, Apotheke, Kurzzeitpflege). Mit save_prefix = Schritt-ID senden. leistung = was genau dieser Anbieter übernehmen soll (Umfang, Häufigkeit, ab wann), antwort_bis = Frist.
submit_label: Rückmeldung senden
---
Guten Tag {{ empfaenger }}

Für den Austritt von {{ patient_name }} (geb. {{ geburtsdatum }}), {{ adresse }}, am {{ austrittsdatum }} aus dem Spital Rosenau fragen wir Sie für folgende Leistung an:

{{ leistung }}

Bitte geben Sie uns bis {{ antwort_bis }} Bescheid, ob Sie die Leistung übernehmen können.

Freundliche Grüsse
{{ absender }}, im Auftrag des Sozialdienstes Spital Rosenau

| name | label | type | required | options | hint |
|---|---|---|---|---|---|
| rueckmeldung | Können Sie die Leistung übernehmen? | radio | ja | Zusage; Absage; Rückfrage | |
| start | Ab wann? | date | | | bei Zusage |
| bemerkung | Bemerkung / Rückfrage | textarea | | | z.B. Grund der Absage, Einschränkungen, Ihre Frage |
