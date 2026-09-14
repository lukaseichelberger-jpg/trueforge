---
name: bewohnereintritt
description: Instruktionen zum unternehmensspezifischen Ablauf des Bewohnereintritts (Aufnahmeprozess) im Alters-/Pflegeheim – Prozess wird immer von der Heimadministration ausgelöst. Das System beschreibt was im Zusammenhang mit einem Neueintritt alles geamcht werden muss: interne Nachrichten (Administration, Lobos-Erfassung, Bewohnerstammblatt, Konfession), Kalendereinladungen bis zu den mit Angehörigen zu klärenden Punkten (Rechnung, Arzt, Krankenkasse, Post, Wäsche, TV/Radio, Telefon/Internet, Fotos/Videos, Taschengeld, Versicherung). Nutze diese Skill wenn ein Neueintritt bearbeitet werden soll.
---

# Bewohnereintritt

Diese Skill bildet den kompletten, unternehmensspezifischen Ablauf für den Eintritt einer neuen Bewohnerin/eines neuen Bewohners ab. Sie liefert Instruktionen anhand welcher der Agent eine strukturierte Checkliste erstellen und möglichst eigenständig abarbeiten kann. Fehlende Informationen erfragt er gemäss Instruktionen der Skill bei den jeweilig zuständigen Personen.

## Referenzdokumente

- `assets/Anmeldeformular.pdf` – Anmeldeformular mit allen bei der eintretenden Person/deren Vertretung abzufragenden Feldern (Personalien, Angehörige/Bezugspersonen, Hausarzt, Krankenkasse, Finanzielles, Patientenverfügung/Vorsorgeauftrag/Vollmacht, Aufenthalt vor Eintritt, Zimmerwunsch, Wünsche).
- `assets/mailvorlagen.md` – Vorformulierte Standard-Nachrichten und Kalendereinladungen für alle unten genannten Kommunikationsschritte (Erstanfrage, Administration/Lobos, Bewohnerstammblatt, Konfession, Kalendereinladung Eintrittstag, Kalendereintrag Angehörigenbefragung). **Bei jedem dieser Schritte diese Vorlage laden und mit den bekannten Angaben ausgefüllt als versandfertigen Text ausgeben** – nicht neu formulieren.

## Ablauf

Wenn der Administrator den Auftrag gibt einen Eintritt zu bearbeiten, die folgenden Schritte **in dieser Reihenfolge** abarbeiten (Markdown, `- [ ]`). Fehlende Informationen bei den relevanten Personen einholen.

### 1. Erstkontakt & Informationsbeschaffung
- [ ] Nachricht an die neu eintretende Person bzw. deren rechtliche Vertretung via Slack senden und die Angaben gemäss Anmeldeformular abfragen (Personalien, Angehörige/Bezugspersonen, Hausarzt, Krankenkasse, Finanzielles, Patientenverfügung/Vorsorgeauftrag/Vollmacht, gewünschter Eintritt, Zimmerwunsch)

### 2. Vertrag
- [ ] Sobald die Angaben vorliegen: Nachricht an die Heimadministration (Slack ID: U0C2KN8RR96) dass der Heimvertrag ausgestellt werden kann und dieser von der rechtlichen Vertretung zu unterzeichnen ist.

### 3. Interne Erfassung & Meldungen
- [ ] Nachricht an Administration (Slack ID: U0C2KN8RR96) mit der Instruktion, die Bewohnerdaten in Lobos zu erfassen und auszudrucken
- [ ] Bewohnerstammblatt senden an **Slack ID: U0C1A03RNLX**
- [ ] Konfession separat senden an **Slack ID: U0C1A03RNLX**

### 4. Kalendereinladungen
- [ ] Kalendereinladung für den Eintrittstag erstellen und an Wohngruppenleitung, Wohngruppe, Pflegedienstleitung und Administration senden
- [ ] Kalendereintrag für die Angehörigenbefragung erstellen, terminiert auf 3 Monate nach dem Eintritt

### 5. Mit Angehörigen zu klären
- [ ] Rechnungsempfänger und Primärkontakt festlegen
- [ ] Klären, ob der bisherige Hausarzt weiterhin ins Heim kommt oder der Heimarzt die Betreuung übernimmt
- [ ] Kopie der Krankenkassenkarte (beidseitig) sowie die Kartennummer einreichen lassen
- [ ] Klären, ob amtliche Post an den Bewohner/die Bewohnerin weitergeleitet werden soll
- [ ] Bei Daueraufenthalt: Angehörige auf die Pflicht zur Adressänderung beim Einwohneramt hinweisen
- [ ] Angehörige informieren: Wäsche wird im Haus gewaschen, Unkostenbeitrag CHF 120/Monat
- [ ] Klären, ob ein TV-Gerät oder Radio mitgebracht wird (Kosten inkl. Strom: CHF 25/Monat)
- [ ] Einverständnis klären, ob Fotos/Videos vom Bewohner/von der Bewohnerin gemacht und veröffentlicht werden dürfen
- [ ] Klären, ob und in welcher Höhe Taschengeld abgegeben werden darf
- [ ] Klären, ob ein Telefonanschluss gewünscht ist (CHF 100 einmalig, CHF 25/Monat) und ob ein Internetanschluss gewünscht ist (CHF 100 einmalig, CHF 15/Monat)
- [ ] Angehörige darauf hinweisen, dass Hausrat- und Haftpflichtversicherung im Heim bereits inbegriffen sind

## Wichtig

- Reihenfolge einhalten: Schritt 2 (Vertrag) erst nach Schritt 1 (Informationen liegen vor); Schritt 3 (interne Meldungen) setzt einen erfassten/unterzeichneten Vertrag bzw. vorliegende Personalien voraus.
- Kontaktangaben exakt wie oben verwenden, nicht selbst herleiten oder korrigieren, auch wenn Domain/Empfänger uneinheitlich wirken.
- Für alle Mail-/Kalenderschritte (1, 3, 4) die passende Vorlage aus `assets/mailvorlagen.md` verwenden, mit den bekannten Angaben ausfüllen und als vollständigen, versandfertigen Text ausgeben. Fehlende Angaben als Platzhalter belassen und kurz benennen, was noch fehlt.
- Keine rechtlich verbindlichen Aussagen zu Fristen oder Beträgen über das hier Genannte hinaus treffen.
