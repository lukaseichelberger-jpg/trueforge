---
name: bewohnereintritt
description: Instruktionen zur Bearbeitung von Neueintritten von Bewohnenden im Altersheim St. Otmar in St. Gallen. Nutze diese Skill wenn ein Neueintritt bearbeitet werden soll / eine neue Person in das Heim eintritt.
---

# Bewohnereintritt

Diese Skill bildet den kompletten, unternehmensspezifischen Ablauf für den Eintritt einer neuen Bewohnerin/eines neuen Bewohners ab. Sie liefert Instruktionen anhand welcher der Agent eine strukturierte Checkliste erstellen und möglichst eigenständig abarbeiten kann. Fehlende Informationen erfragt er gemäss Instruktionen der Skill bei den jeweilig zuständigen Personen.

## Referenzdokumente

- `assets/Anmeldeformular.pdf` – Formular mit den Informationen, welche für einen Eintritt ins Pflegeheim vorhanden sein müssen.
- `assets/mailvorlagen.md` – Vorformulierte Standard-Nachrichten und Kalendereinladungen für alle unten genannten Kommunikationsschritte. **Bei jedem dieser Schritte diese Vorlage laden und mit den bekannten Angaben ausgefüllt als versandfertigen Text ausgeben** – nicht neu formulieren.

## Dokumentvorlagen

Im Ordner `templates/` liegen die Vorlagen für die Dokumente dieses Ablaufs. Der Chat-Server stellt sie dir bereit, du liest sie **nicht** selbst ein:

| Vorlage | Zweck | Erstellen | Senden an |
|---|---|---|---|
| `bewohnerstammblatt` | Stammdaten, Kontakte und Vereinbarungen | nach Schritt 1 | Schritt 3: Apotheke; nach Schritt 4: aktualisiert an Apotheke und Administrator |
| `heimvertrag` | Vertrag zwischen Heim und Bewohner/in | Schritt 2 | Administrator (stellt aus und lässt unterzeichnen) |

So arbeitest du mit den Dokumenten:

1. **Fall eröffnen mit `process="bewohnereintritt"`** – nur so findet der Server die Vorlagen dieser Skill.
2. **Felder kennen:** Zu Beginn `list_templates(case_id)` aufrufen. Die dort genannten Feldnamen (z.B. `vorname`, `geburtsdatum`, `rechnungsempfaenger`) sind die verbindlichen Namen für alle Angaben.
3. **Angaben speichern:** Jede erhaltene Angabe sofort mit `set_case_data` unter genau diesem Feldnamen ablegen. Ja/Nein-Angaben als `"ja"` bzw. `"nein"` speichern (z.B. `telefon`, `internet`, `tv_radio`, `hausarzt_bleibt`).
4. **Erstellen:** `create_document(case_id, template)` übernimmt die gespeicherten Falldaten automatisch. Die Antwort nennt in `missing_fields`, was noch fehlt.
5. **Nachführen:** Kommen neue oder geänderte Angaben dazu, diese in `set_case_data` **und** mit `update_document(doc_id, values={...}, note="<Grund>")` in **jedem bereits erstellten Dokument** nachtragen, in dem das Feld vorkommt. Die IDs der Dokumente eines Falls zeigt `get_case`.
6. **Versenden:** Nur mit `send_document(to, doc_id, text)`. Den Inhalt eines Dokuments nie als Nachrichtentext abschreiben. Eine geänderte Version erneut senden und im Begleittext kurz sagen, was sich geändert hat.

Regeln für Dokumente:
- Den Wortlaut der Vorlagen **nie umformulieren**. Felder ausschliesslich über `values` füllen.
- Textänderungen mit `find`/`replace` nur für **Besondere Vereinbarungen** im Heimvertrag (§ 4) und nur auf ausdrückliche Anweisung des Administrators; im `note` festhalten, wer es angewiesen hat.
- Ein Dokument mit fehlenden Feldern (`[feld fehlt]`) nur an den Administrator senden, mit dem Hinweis, welche Angaben fehlen. An alle anderen Empfänger erst senden, wenn `missing_fields` leer ist – oder der Administrator den Versand trotzdem freigibt.

## Ablauf

Ein Administrator weist den Agenten an einen Eintritt zu bearbeiten. Anschliessend müssen folgende Schritte **in dieser Reihenfolge** abgearbeitet werden. (Markdown, `- [ ]`). Fehlende Informationen müssen bei den relevanten Personen einholen.

### 1. Informationsbeschaffung:
- [ ] Fall eröffnen (`process="bewohnereintritt"`) und mit `list_templates` die benötigten Felder ermitteln.
- [ ] Nachricht an die neu eintretende Person bzw. deren rechtliche Vertretung senden und die Informationen gemäss "Anmeldeformular.pdf" abfragen. Für die Abfrage `send_form` verwenden; als Feldnamen die Namen aus den Vorlagen nehmen.
- [ ] Prüfung ob alle Angaben vorliegen. Bei Bedarf fehlende Informationen erfragen. Alle Angaben mit `set_case_data` speichern.
- [ ] Bewohnerstammblatt erstellen (`create_document(case_id, "bewohnerstammblatt")`). Felder aus Schritt 4 (z.B. Primärkontakt, Vereinbarungen) dürfen hier noch fehlen.

### 2. Vertragsausstellung:
Sobald die Informationen vorliegen:
- [ ] Nachricht an den Administrator mit Instruktion, die Bewohnerdaten in Lobos zu erfassen
- [ ] Heimvertrag erstellen (`create_document(case_id, "heimvertrag")`). Fehlende Vertragsangaben (z.B. Tagestaxe, Zimmer, Wohngruppe) beim Administrator erfragen und mit `update_document` nachtragen.
- [ ] Heimvertrag mit `send_document` an den Administrator senden – mit Hinweis auf allenfalls fehlende Informationen und der Instruktion, dass er den Heimvertrag ausstellen soll und dieser vom Bewohnenden respektive dessen rechtlichen Vertretung zu unterzeichnen ist.

### 3. Interne Meldungen und Datenverarbeitung:
Sobald ein unterzeichneter Heimvertrag vorliegt (darüber muss der Agent vom Admin informiert werden. Der Agent sollte den Admin auffordern die Info zu liefern sobald der unterzeichnete vertrag vorliegt):
- [ ] Mit `set_case_data` festhalten, welche Version des Heimvertrags unterzeichnet wurde (`vertrag_unterzeichnet = "Version <n> am <Datum>"`).
- [ ] Bewohnerstammdaten an die Apotheke am Gürbisbach senden: das Bewohnerstammblatt mit `send_document`.
- [ ] Zuständigen Seelsorger über die Konfession des Bewohnenden informieren. 
- [ ] Kalendereinladung für den Eintrittstag erstellen und an Wohngruppenleitung, Wohngruppe, Pflegedienstleitung und Administration senden
- [ ] Kalendereintrag für die Angehörigenbefragung erstellen, terminiert auf 3 Monate nach dem Eintritt

### 4. Klärung von Detailfragen mit Angehörigen
Jede Antwort sofort mit `set_case_data` speichern und mit `update_document` im Bewohnerstammblatt (und, wo betroffen, im Heimvertrag) nachtragen.
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
- [ ] Abschluss: Das aktualisierte Bewohnerstammblatt an die Apotheke (Hausarzt/Heimarzt!) und an den Administrator senden.
- [ ] Haben sich vertragsrelevante Angaben geändert (Rechnungsempfänger, Telefon, Internet, TV/Radio), den Heimvertrag mit `update_document` nachführen und die neue Version an den Administrator senden – mit dem Hinweis, dass sie von der unterzeichneten Version abweicht und er über einen Nachtrag entscheiden muss.

## Wichtig

- Reihenfolge einhalten: Schritt 2 (Vertragsausstellung) erst nach Schritt 1 (Informationen liegen vor); Schritt 3 (interne Meldungen und Datenverarbeitung) setzt einen erfassten/unterzeichneten Vertrag bzw. vorliegende Personalien voraus.
- Der Agent muss sicherstellen, dass er Informationen an die richtigen Personen versendet. Dies wird dadurch sichergestellt, dass der Agent - bevor er eine Nachricht an eine neue Person versendet - die Bestätigung vom Administrator einholt / respektive vom Administrator die Information einholt an wen die Nachricht gesendet werden muss. Das gilt auch für Dokumente (`send_document`).
- Für alle Mail-/Kalenderschritte (1, 2, 3) die passende Vorlage aus `assets/mailvorlagen.md` verwenden, mit den bekannten Angaben ausfüllen und als vollständigen, versandfertigen Text ausgeben. Fehlende Angaben als Platzhalter belassen und kurz benennen, was noch fehlt.
- Dokumente (Bewohnerstammblatt, Heimvertrag) immer aus den Vorlagen in `templates/` erstellen und mit `send_document` versenden – nie selbst verfassen.
- Keine rechtlich verbindlichen Aussagen zu Fristen oder Beträgen über das hier Genannte hinaus treffen. Bei grösseren Unklarheiten auf den Administrator verweisen (Name aus der Konversation entnehmen ist erreichbar unter +41 78 333 75 66).
