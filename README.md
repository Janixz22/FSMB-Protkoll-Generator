# FSMB-Protokoll-Generator

Ein browserbasiertes Werkzeug der Fachschaftsvertretung Maschinenbau (RWTH Aachen)
zur Erstellung von Sitzungsprotokollen. Erzeugt aus konfigurierbaren Vorlagen eine
Markdown-Datei für [HedgeDoc](https://hedgedoc.fsmb.rwth-aachen.de), aus der später ein
formatiertes PDF-Protokoll generiert wird.

Läuft vollständig im Browser als einzelne HTML-Datei – keine Installation, kein
Server, kein Build-Schritt nötig. Für die PDF-Erzeugung (Schriftart- und ggf.
Bildabruf) wird eine Internetverbindung benötigt.

## Projektdateien

| Datei | Zweck |
|---|---|
| `FSMB-Protokoll-Generator.html` | Die Anwendung selbst – einfach im Browser öffnen |
| `logo.svg` | FSMB-Logo, wird bei der PDF-Erzeugung eingebettet (siehe [Logo](#logo)) |
| `FSMB-Konfiguration.example.json` | Anonymisierte Beispiel-Stammdaten als Vorlage |
| `FSMB-Konfiguration.json` | Eure echten Stammdaten (Namen etc.) – **bewusst nicht Teil des Repos**, siehe unten |

## Funktionen

- **Fünf Sitzungsmodi** mit jeweils eigenem Farbschema und passender
  Tagesordnung: FSV, FaS, AG-Sitzung, Gremien-Vorbesprechung, Ausschusssitzung
- **Markdown-Vorlage generieren**: Sitzungsmetadaten (Datum, Ort, Sitzungsleitung
  etc.) eintragen, gewünschte Tagesordnungspunkte auswählen, Vorlage
  direkt in einer neuen HedgeDoc-Notiz öffnen, herunterladen oder in die
  Zwischenablage kopieren
- **Tagesordnung vorab verschicken**: kompaktes PDF mit Sitzungsmetadaten und
  der aktuell gewählten Tagesordnung – zum Rundschicken vor der Sitzung, ganz
  ohne HedgeDoc-Umweg
- **PDF-Export**: fertig ausgefülltes Markdown-Protokoll hochladen – das PDF
  wird direkt im Browser erzeugt und heruntergeladen, ganz ohne Druckdialog.
  Im Protokoll eingebettete Bilder werden übernommen (siehe
  [Einschränkung bei Bildern](#bilder-im-protokoll)); Emojis werden aus dem
  PDF entfernt, da sie von der verwendeten PDF-Bibliothek nicht dargestellt
  werden können
- **Konfigurierbare Stammdaten** (Zahnrad-Tab): Mitgliederlisten für die
  Anwesenheitsliste, Berichts-TOPs je Modus, Raumvorschläge sowie Ausschüsse
  samt Mitgliedern – wird im Browser gespeichert (`localStorage`) und lässt
  sich als JSON exportieren/importieren (per Klick oder Drag & Drop)

## Workflow

1. **Generator**-Tab öffnen, Sitzungsmodus wählen und Metadaten ausfüllen
2. Benötigte Tagesordnungspunkte an-/abwählen
3. Optional: Tagesordnung als PDF herunterladen und vor der Sitzung verschicken
4. **In HedgeDoc öffnen** klicken: Die Vorlage wird in die Zwischenablage
   kopiert und eine neue Notiz auf [HedgeDoc](https://hedgedoc.fsmb.rwth-aachen.de)
   geöffnet – dort mit Cmd/Strg + V einfügen (alternativ Vorlage
   herunterladen bzw. kopieren). HedgeDoc ist außerdem jederzeit über den
   Link in der Kopfleiste erreichbar
5. Protokoll während der Sitzung in HedgeDoc ausfüllen
6. Im Tab **Export & PDF** den Notiztext einfügen (in HedgeDoc im
   Bearbeitungsmodus Cmd/Strg + A, Cmd/Strg + C) und übernehmen – oder wie
   bisher die als Markdown exportierte `.md`-Datei hochladen
7. Vorschau prüfen und auf „PDF herunterladen“ klicken – das fertige,
   formatierte Protokoll wird direkt als PDF-Datei heruntergeladen

## Konfiguration

Im Konfigurations-Tab lassen sich folgende Stammdaten pflegen:

- **Mitgliederlisten**: Stimmberechtigte und Ratsmitglieder für die
  Anwesenheitsliste
- **Berichte**: Titel der Berichts-TOPs, getrennt für FSV und FaS
- **Räume**: Vorschläge für das Ort-Feld im Generator
- **Ausschüsse**: Name und stimmberechtigte Mitglieder je Ausschuss –
  erscheinen als Dropdown im Modus „Ausschüsse“

Änderungen werden lokal im Browser gespeichert (`localStorage`). Beim ersten
Start (bzw. solange noch keine eigene Konfiguration gespeichert wurde) zeigt
der Konfigurations-Tab einen Hinweis an. `FSMB-Konfiguration.example.json`
zeigt den Aufbau anhand anonymisierter Beispieldaten und kann über die
Upload-Zone „Sichern & Laden“ importiert werden (Klick oder Drag & Drop) –
danach die Platzhalter durch eure echten Stammdaten ersetzen und über „JSON
exportieren“ als eigene `FSMB-Konfiguration.json` sichern. Zeitpunkt des
letzten Imports/Exports wird im Konfigurations-Tab angezeigt.

**Datenschutz-Hinweis:** `FSMB-Konfiguration.json` mit euren echten Namen ist
per `.gitignore` bewusst von der Versionierung ausgeschlossen, damit dieses
Repository öffentlich geteilt werden kann, ohne personenbezogene Daten
preiszugeben. Bewahrt eure echte Konfiguration lokal auf oder teilt sie nur
innerhalb der Fachschaft (z.B. per Sharepoint), nicht über dieses Repo.

## Logo

Das FSMB-Logo liegt als eigenständige Datei `logo.svg` im selben Ordner wie
die HTML-Datei und wird bei der PDF-Erzeugung automatisch nachgeladen. Das
funktioniert zuverlässig, wenn die Seite über einen Webserver bereitgestellt
wird (z.B. GitHub Pages) oder in Firefox.

Wird die Datei per Doppelklick lokal in **Chrome** geöffnet (`file://`),
blockiert der Browser aus Sicherheitsgründen das automatische Nachladen
benachbarter Dateien per JavaScript. Für diesen Fall gibt es im
Konfigurations-Tab eine Logo-Upload-Zone: `logo.svg` dort einmalig per Klick
oder Drag & Drop hochladen – danach ist das Logo dauerhaft im Browser
(`localStorage`) gespeichert und wird bei jeder weiteren PDF-Erzeugung ohne
erneutes Hochladen automatisch verwendet, auch bei rein lokaler Nutzung ohne
Server. Ohne diesen einmaligen Schritt wird das PDF ohne Grafik, mit einem
schlichten Schriftzug statt Logo, erzeugt – alle anderen Funktionen sind
davon nicht betroffen. Wer das Logo austauschen möchte, ersetzt `logo.svg`
durch eine eigene Datei gleichen Namens und lädt sie ggf. erneut über die
Upload-Zone hoch, um den lokalen Cache zu aktualisieren.

## Bilder im Protokoll

Im HedgeDoc-Protokoll eingebettete Bilder (`![Alt](URL)`, egal ob als
eigener Absatz oder innerhalb eines Aufzählungspunkts) werden beim
PDF-Export automatisch geladen und eingebettet. Das setzt voraus, dass der
Bild-Host per HTTP(S) einen `Access-Control-Allow-Origin`-Header sendet
(CORS) – ohne diesen Header blockiert der Browser das Auslesen der
Bilddaten aus Sicherheitsgründen, unabhängig vom verwendeten Tool. Ist das
nicht der Fall (z.B. bei der Standard-Konfiguration vieler HedgeDoc-Instanzen),
erscheint im PDF ein Platzhaltertext statt des Bildes; die restliche
PDF-Erzeugung bricht dadurch nicht ab. Abhilfe schafft nur ein entsprechend
gesetzter CORS-Header auf dem Bild-Host selbst.

## Tickboxen

Listenpunkte können als Tickbox geschrieben werden (HedgeDoc-/GFM-Syntax).
In Vorschau und PDF werden nur angekreuzte Einträge übernommen – als
normaler Aufzählungspunkt ohne Kästchen; nicht angekreuzte Einträge
(inkl. eingerückter Unterpunkte) entfallen komplett:

```markdown
Wer wird auf der Sitzung des Fakultätsrats anwesend sein?

- [x] Janick
- [ ] Emelie
- [x] Mayara
```

ergibt im Protokoll nur „Janick" und „Mayara".

## Technisches

- Einzelne, self-contained HTML-Datei (HTML/CSS/JavaScript, kein Framework)
- Externe Abhängigkeiten (via CDN): [marked.js](https://marked.js.org) zum
  Rendern von Markdown, [pdfmake](https://pdfmake.github.io/docs/) zur
  direkten PDF-Erzeugung im Browser
- PDF-Erzeugung erfolgt vollständig clientseitig über pdfmake (kein
  Druckdialog, kein serverseitiges Rendering); dadurch unabhängig vom
  verwendeten Browser. Für die Schriftart Arimo (Arial-kompatibel, Apache-
  Lizenz) werden beim ersten PDF-Export einmalig vier Schriftdateien von
  Google Fonts nachgeladen
- Konfiguration wird clientseitig per `localStorage` persistiert

## Nutzung

Datei `FSMB-Protokoll-Generator.html` direkt im Browser öffnen (Doppelklick
oder per „Datei öffnen“) – ein lokaler Server ist nicht erforderlich. Für die
PDF-Erzeugung ist eine Internetverbindung nötig (CDN-Skripte, Schriftabruf,
ggf. Logo/Bilder). Für die zuverlässigste Darstellung von Logo und Bildern
empfiehlt sich das Bereitstellen über einen Webserver (z.B. GitHub Pages)
oder die Nutzung von Firefox.
