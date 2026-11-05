# Arbeitsregeln für dieses Projekt (Waystrom GmbH)

Dieses Repository ist das Fundament, mit dem die Arbeit in Notion bei der
Waystrom GmbH strukturiert wird. Diese Regeln gelten zusätzlich zu den
globalen Anweisungen des Nutzers und haben für dieses Projekt Vorrang bei
Detailfragen der Arbeitsweise.

## Grundsatz

- Die operative Arbeit läuft in Notion. Dieses Repo definiert **wie** sie
  abläuft (Regeln, Specs, Skills, Pläne) und enthält – nur wo nötig –
  tatsächlichen Code (`tools/`), z. B. für Notion-Automatisierung.
- Jeder Baustein (`specs/`, `skills/`, `user-stories/`, `work-plans/`,
  `journal/`, `tools/`) ist eigenständig definiert, baut aber langfristig auf
  den anderen auf. Keine isolierten, nirgends verankerten Änderungen.

## Pflicht: Changelog vor Wirksamkeit

- Jede Änderung an Regeln, Specs, Skills oder Arbeitsweise wird **zuerst** in
  `CHANGELOG.md` als neue datierte Revision festgehalten (was geändert wurde,
  warum), bevor sie als gültig gilt.
- Reine Journal-Einträge (was in Notion/operativ gemacht wurde) gehören nach
  `journal/`, nicht in den Changelog. Der Changelog ist für Änderungen am
  System selbst reserviert.

## Pflicht: Journal

- Abgeschlossene Arbeitseinheiten werden in `journal/` dokumentiert: was
  gemacht wurde, wie es gemacht wurde, mit Datum.

## Specs

- `specs/global-spec.md` – übergeordnete Prinzipien, die für alle anderen
  Specs gelten.
- `specs/functional-spec.md` – was das System/die Arbeitsweise leisten muss.
- `specs/future-spec.md` – geplante, noch nicht umgesetzte Richtung.
- `specs/technical-spec.md` – technische Umsetzung (Tools, Automatisierung,
  Notion-Integration).
- `specs/visual-rules.md` – Vorgaben zu Darstellung/Struktur in Notion.

Specs müssen nicht vollständig sein, aber jede Ergänzung muss in die
passende Datei, nicht lose irgendwo verstreut.

## Sprache

Alle Dokumente in diesem Projekt werden auf Deutsch geführt.

## Git

- Jede inhaltliche Änderung erhält einen eigenen, aussagekräftigen Commit.
- Vor destruktiven Git-Operationen (reset --hard, force-push etc.) immer erst
  `git status` prüfen und Rückfrage an den Nutzer stellen.

### Pflicht: Branch- und GitHub-Ablauf

**Start einer neuen Aufgabe**

1. Eine neue Aufgabe beginnt nur von einem aktuellen, sauberen Hauptbranch
   (`master`) aus.
2. Vor Beginn wird der lokale Hauptbranch mit dem Remote (`origin`)
   synchronisiert.
3. Für jede Aufgabe bzw. jedes Thema wird ein eigener Arbeitsbranch vom
   Hauptbranch angelegt, benannt nach dem Muster `work/<thema>`.
4. Es wird nie direkt auf dem Hauptbranch gearbeitet und nie der Branch einer
   vorangegangenen, bereits abgeschlossenen Aufgabe weiterverwendet.

**Abschluss einer Aufgabe**

1. Durchgeführte Arbeit und Repo-Status prüfen.
2. Änderungen im Arbeitsbranch committen.
3. Den Arbeitsbranch per `push` nach GitHub übertragen.
4. Nach dem Push **stoppen** und dem Nutzer melden, dass der Branch bereit
   zur Prüfung ist.

**Pull Request und Merge**

1. Den Pull Request erstellt ausschließlich der Nutzer selbst.
2. Den Merge in den Hauptbranch führt ausschließlich der Nutzer selbst durch.
3. Es ist untersagt, einen Pull Request zu erstellen oder einen Merge
   durchzuführen – auch nicht über `gh`, es sei denn, der Nutzer fordert dies
   im Einzelfall ausdrücklich an.
4. Bis der Nutzer einen erfolgten Merge bestätigt hat, wird die abgeschlossene
   Arbeit nicht mit dem Hauptbranch synchronisiert und keine neue Aufgabe
   begonnen.

**Nach bestätigtem Merge**

Nachdem der Nutzer den erfolgten Merge gemeldet hat:

1. Zum Hauptbranch wechseln.
2. Den aktuellen Stand vom Remote holen.
3. Den lokalen Hauptbranch per Fast-Forward aktualisieren.
4. Prüfen, dass lokaler und entfernter Hauptbranch übereinstimmen und das
   Arbeitsverzeichnis sauber ist.
5. Den abgeschlossenen lokalen Arbeitsbranch löschen.
6. Auf dem sauberen, aktuellen Hauptbranch verbleiben.

Das Löschen des entfernten Arbeitsbranches auf GitHub obliegt dem Nutzer,
falls er dies für nötig hält.

Die nächste Aufgabe beginnt erst nach bestätigtem Merge, Synchronisation des
Hauptbranchs und Anlage eines neuen Arbeitsbranches vom aktuellen
Hauptbranch aus.
