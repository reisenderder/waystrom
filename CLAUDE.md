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
