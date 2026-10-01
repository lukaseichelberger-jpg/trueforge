---
name: bewohnereintritt
description: Instruktionen zur Bearbeitung von Neueintritten von Bewohnenden im Altersheim St. Otmar in St. Gallen. Nutze diese Skill wenn ein Neueintritt bearbeitet werden soll / eine neue Person in das Heim eintritt.
---

# Bewohnereintritt

Diese Skill bildet den kompletten, unternehmensspezifischen Ablauf für den Eintritt einer neuen Bewohnerin/eines neuen Bewohners ab. Sie liefert Instruktionen anhand welcher der Agent eine strukturierte Checkliste erstellen und möglichst eigenständig abarbeiten kann. Fehlende Informationen erfragt er gemäss Instruktionen der Skill bei den jeweilig zuständigen Personen.

## Referenzdokumente

- `assets/Anmeldeformular.pdf` – Formular mit den Informationen, welche für einen Eintritt ins Pflegeheim vorhanden sein müssen.

## Nachrichtenvorlagen

Standard-Nachrichten und Kalendereinladungen liegen als Vorlagen (`type: message`) in `templates/`. Du verfasst diese Nachrichten **nicht selbst** und liest die Vorlagen nicht ein: Sende sie mit `send_template_message(to=[...], case_id, template, values)`. Der Server füllt Falldaten, Empfängername und Absender automatisch ein; du gibst nur in `values` an, was nicht schon in den Falldaten steht.

| Vorlage | Schritt | An | Zusätzlich |
|---|---|---|---|
| `nachricht_erstanfrage` | 1 | eintretende Person / rechtliche Vertretung | `aufzunehmen`, `rueckmeldedatum` |
| `nachricht_lobos` | 2 | Administrator | `doc_id` = Bewohnerstammblatt |
| `nachricht_stammblatt` | 3, Abschluss 4 | Apotheke, Administrator | `doc_id` = Bewohnerstammblatt |
| `nachricht_konfession` | 3 | zuständiger Seelsorger | |
| `kalender_eintritt` | 3 | Wohngruppenleitung, Wohngruppe, Pflegedienstleitung, Administration (ein Aufruf) | `eintrittszeit` |
| `kalender_angehoerigenbefragung` | 3 | wie Kalendereinladung Eintrittstag | `befragungsdatum` (Eintritt + 3 Monate) |

Meldet `send_template_message` `missing_fields`, wurde **nichts** gesendet: Angaben beschaffen und erneut aufrufen. Nur auf Anweisung des Administrators mit `allow_missing=true` trotz Lücken senden.

## Formulare

Die Abfragen dieses Ablaufs sind feste Formulare (`type: form`) in `templates/`. Sende sie mit `send_template_form(to, case_id, template)` – die Felder definierst du **nicht** selbst, und `send_form` verwendest du nur für Fragen, die kein festes Formular abdeckt.

| Formular | Schritt | An |
|---|---|---|
| `formular_anmeldung` (entspricht dem Anmeldeformular) | 1 | eintretende Person / rechtliche Vertretung |
| `formular_detailfragen`, `formular_zusatzleistungen` | 4 | Angehörige (mit `only_missing=true`) |

Die Antworten speichert der Server **automatisch** in den Falldaten (die Antwort trägt `saved_to_case`); dafür kein `set_case_data` aufrufen. Fehlende Angaben mit demselben Formular und `only_missing=true` nachfragen – es enthält dann nur noch die fehlenden Felder.

## Dokumentvorlagen

Im Ordner `templates/` liegen auch die Vorlagen für die Dokumente dieses Ablaufs (`type: document`). Der Chat-Server stellt sie dir bereit, du liest sie **nicht** selbst ein:

| Vorlage | Zweck | Erstellen | Senden an |
|---|---|---|---|
| `bewohnerstammblatt` | Stammdaten, Kontakte und Vereinbarungen | nach Schritt 1 | Schritt 3: Apotheke; nach Schritt 4: aktualisiert an Apotheke und Administrator |
| `heimvertrag` | Vertrag zwischen Heim und Bewohner/in | Schritt 2 | Administrator (stellt aus und lässt unterzeichnen) |

So arbeitest du mit den Dokumenten:

1. **Fall eröffnen mit `process="bewohnereintritt"`** – nur so findet der Server die Vorlagen dieser Skill.
2. **Felder kennen:** Zu Beginn `list_templates(case_id)` aufrufen. Die dort genannten Feldnamen (z.B. `vorname`, `geburtsdatum`, `rechnungsempfaenger`) sind die verbindlichen Namen für alle Angaben.
3. **Angaben speichern:** Formularantworten speichert der Server selbst. Jede andere erhaltene Angabe sofort mit `set_case_data` unter genau diesem Feldnamen ablegen. Ja/Nein-Angaben als `"ja"` bzw. `"nein"` speichern (z.B. `telefon`, `internet`, `tv_radio`, `hausarzt_bleibt`).
4. **Erstellen:** `create_document(case_id, template)` übernimmt die gespeicherten Falldaten automatisch. Die Antwort nennt in `missing_fields`, was noch fehlt.
5. **Nachführen:** Kommen neue oder geänderte Angaben dazu, diese in `set_case_data` **und** mit `update_document(doc_id, values={...}, note="<Grund>")` in **jedem bereits erstellten Dokument** nachtragen, in dem das Feld vorkommt. Die IDs der Dokumente eines Falls zeigt `get_case`.
6. **Versenden:** Nur mit `send_document(to, doc_id, text)` oder als `doc_id` einer Nachrichtenvorlage (`send_template_message`). Den Inhalt eines Dokuments nie als Nachrichtentext abschreiben. Eine geänderte Version erneut senden und im Begleittext kurz sagen, was sich geändert hat.

Regeln für Dokumente:
- Den Wortlaut der Vorlagen **nie umformulieren**. Felder ausschliesslich über `values` füllen.
- Textänderungen mit `find`/`replace` nur für **Besondere Vereinbarungen** im Heimvertrag (§ 4) und nur auf ausdrückliche Anweisung des Administrators; im `note` festhalten, wer es angewiesen hat.
- Ein Dokument mit fehlenden Feldern (`[feld fehlt]`) nur an den Administrator senden, mit dem Hinweis, welche Angaben fehlen. An alle anderen Empfänger erst senden, wenn `missing_fields` leer ist – oder der Administrator den Versand trotzdem freigibt.

## Ablauf

Ein Administrator weist den Agenten an einen Eintritt zu bearbeiten. Anschliessend müssen folgende Schritte **in dieser Reihenfolge** abgearbeitet werden. (Markdown, `- [ ]`). Fehlende Informationen müssen bei den relevanten Personen einholen.

### 1. Informationsbeschaffung:
- [ ] Fall eröffnen (`process="bewohnereintritt"`) und mit `list_templates` die benötigten Felder ermitteln.
- [ ] Die neu eintretende Person bzw. deren rechtliche Vertretung mit `nachricht_erstanfrage` anschreiben und die Informationen gemäss "Anmeldeformular.pdf" mit `formular_anmeldung` abfragen.
- [ ] Prüfung ob alle Angaben vorliegen. Fehlende Informationen mit `formular_anmeldung` und `only_missing=true` erfragen.
- [ ] Aus den Bezugspersonen (`bezug1_…` bis `bezug4_…`, Funktion in `bezugN_rollen`) mit `set_case_data` ableiten: `rechnungsempfaenger` (Name und Adresse der Person mit Funktion Rechnungsempfänger) und `vertretung` (Person mit Funktion gesetzlicher Vertreter, Bevollmächtigte oder Beistand; sonst leer lassen).
- [ ] Eintrittsdatum, Aufenthaltsart, Wohngruppe und Zimmer stehen nicht im Anmeldeformular: beim Administrator erfragen und mit `set_case_data` speichern (`eintrittsdatum`, `aufenthaltsart` = "Daueraufenthalt" oder "Kurzaufenthalt", `wohngruppe`, `zimmer`).
- [ ] Bewohnerstammblatt erstellen (`create_document(case_id, "bewohnerstammblatt")`). Felder aus Schritt 4 (z.B. Primärkontakt, Vereinbarungen) dürfen hier noch fehlen.

### 2. Vertragsausstellung:
Sobald die Informationen vorliegen:
- [ ] Administrator mit `nachricht_lobos` (mit Bewohnerstammblatt als `doc_id`) instruieren, die Bewohnerdaten in Lobos zu erfassen
- [ ] Heimvertrag erstellen (`create_document(case_id, "heimvertrag")`). Fehlende Vertragsangaben (z.B. Tagestaxe, Zimmer, Wohngruppe) beim Administrator erfragen und mit `update_document` nachtragen.
- [ ] Heimvertrag mit `send_document` an den Administrator senden – mit Hinweis auf allenfalls fehlende Informationen und der Instruktion, dass er den Heimvertrag ausstellen soll und dieser vom Bewohnenden respektive dessen rechtlichen Vertretung zu unterzeichnen ist.

### 3. Interne Meldungen und Datenverarbeitung:
Sobald ein unterzeichneter Heimvertrag vorliegt (darüber muss der Agent vom Admin informiert werden. Der Agent sollte den Admin auffordern die Info zu liefern sobald der unterzeichnete vertrag vorliegt):
- [ ] Mit `set_case_data` festhalten, welche Version des Heimvertrags unterzeichnet wurde (`vertrag_unterzeichnet = "Version <n> am <Datum>"`).
- [ ] Bewohnerstammdaten an die Apotheke am Gürbisbach senden: `nachricht_stammblatt` mit dem Bewohnerstammblatt als `doc_id`.
- [ ] Zuständigen Seelsorger mit `nachricht_konfession` über die Konfession des Bewohnenden informieren.
- [ ] Kalendereinladung für den Eintrittstag (`kalender_eintritt`) an Wohngruppenleitung, Wohngruppe, Pflegedienstleitung und Administration senden
- [ ] Kalendereintrag für die Angehörigenbefragung (`kalender_angehoerigenbefragung`) senden, terminiert auf 3 Monate nach dem Eintritt

### 4. Klärung von Detailfragen mit Angehörigen
Die Punkte unten mit `formular_detailfragen` und `formular_zusatzleistungen` (je `only_missing=true`) an die Angehörigen klären; der Text des ersten Formulars enthält bereits die Hinweise zu Wäsche, Versicherung und Adressänderung. Die Antworten landen automatisch in den Falldaten; sie mit `update_document` im Bewohnerstammblatt (und, wo betroffen, im Heimvertrag) nachtragen. Die Kopie der Krankenkassenkarte kommt nicht über das Formular – ihren Eingang separat bestätigen lassen.
- [ ] Rechnungsempfänger definieren (`rechnungsempfaenger`)
- [ ] Primärkontakt festlegen (`primaerkontakt`, `primaerkontakt_telefon`)
- [ ] Klären, ob der bisherige Hausarzt weiterhin ins Heim kommt oder der Heimarzt die Betreuung übernehmen soll (`hausarzt_bleibt`, `hausarzt`)
- [ ] Kopie der Krankenkassenkarte (beidseitig) sowie die Kartennummer einreichen lassen (`krankenkassen_nummer`)
- [ ] Klären, ob amtliche Post an den Bewohner/die Bewohnerin weitergeleitet werden soll (`amtliche_post`)
- [ ] Bei Daueraufenthalt: Angehörige auf die Pflicht zur Adressänderung beim Einwohneramt hinweisen
- [ ] Angehörige informieren: Wäsche wird im Haus gewaschen, Unkostenbeitrag CHF 120/Monat
- [ ] Klären, ob ein TV-Gerät oder Radio mitgebracht wird (Kosten inkl. Strom: CHF 25/Monat) (`tv_radio`)
- [ ] Klären, ob ein Telefonanschluss gewünscht ist (CHF 100 einmalig, CHF 25/Monat) und ob ein Internetanschluss gewünscht ist (CHF 100 einmalig, CHF 15/Monat) (`telefon`, `internet`)
- [ ] Einverständnis klären, ob Fotos/Videos vom Bewohner/von der Bewohnerin gemacht und veröffentlicht werden dürfen (`fotos`)
- [ ] Klären, ob und in welcher Höhe Taschengeld abgegeben werden darf/soll (`taschengeld`)
- [ ] Angehörige darauf hinweisen, dass Hausrat- und Haftpflichtversicherung im Heim bereits inbegriffen sind
- [ ] Abschluss: Das aktualisierte Bewohnerstammblatt mit `nachricht_stammblatt` (als `doc_id`) an die Apotheke (Hausarzt/Heimarzt!) und an den Administrator senden.
- [ ] Haben sich vertragsrelevante Angaben geändert (Rechnungsempfänger, Telefon, Internet, TV/Radio), den Heimvertrag mit `update_document` nachführen und die neue Version an den Administrator senden – mit dem Hinweis, dass sie von der unterzeichneten Version abweicht und er über einen Nachtrag entscheiden muss.

## Wichtig

- Reihenfolge einhalten: Schritt 2 (Vertragsausstellung) erst nach Schritt 1 (Informationen liegen vor); Schritt 3 (interne Meldungen und Datenverarbeitung) setzt einen erfassten/unterzeichneten Vertrag bzw. vorliegende Personalien voraus.
- Der Agent muss sicherstellen, dass er Informationen an die richtigen Personen versendet. Dies wird dadurch sichergestellt, dass der Agent - bevor er eine Nachricht an eine neue Person versendet - die Bestätigung vom Administrator einholt / respektive vom Administrator die Information einholt an wen die Nachricht gesendet werden muss. Das gilt auch für Dokumente (`send_document`).
- Für alle Nachrichten- und Kalenderschritte (1, 2, 3) die passende Nachrichtenvorlage mit `send_template_message` senden – nie selbst ausformulieren oder abschreiben.
- Dokumente (Bewohnerstammblatt, Heimvertrag) immer aus den Vorlagen in `templates/` erstellen und mit `send_document` versenden – nie selbst verfassen.
- Keine rechtlich verbindlichen Aussagen zu Fristen oder Beträgen über das hier Genannte hinaus treffen. Bei grösseren Unklarheiten auf den Administrator verweisen (Name aus der Konversation entnehmen ist erreichbar unter +41 78 333 75 66).
