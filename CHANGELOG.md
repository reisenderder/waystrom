# Changelog

Dieses Dokument ist der **Logger** des Systems. Jede Änderung an Struktur,
Regeln, Specs oder Arbeitsweise wird hier datiert und nachvollziehbar
festgehalten – unabhängig von Git-Commits. Neue Einträge oben anfügen.

## Format

```
## Revision N – YYYY-MM-DD
- Was geändert wurde
- Warum (Grund/Anlass)
```

---

## Revision 2 – 2026-11-05
- Verbindlichen Branch- und GitHub-Ablauf in `CLAUDE.md` (Abschnitt `## Git`)
  festgeschrieben: jede Aufgabe läuft in einem eigenen Branch
  (`work/<thema>`) vom Hauptbranch, der Agent committet und pusht den
  Branch nach GitHub und stoppt danach. Pull Request und Merge führt
  ausschließlich der Nutzer selbst durch; der Agent darf beides nicht
  eigenständig ausführen.
- Grund: Der Nutzer legt fest, dass die Zusammenführung in den Hauptbranch
  (Pull Request + Merge) vollständig in seiner Hand bleibt, nach dem in
  anderen Projekten (u. a. „Musafirs Plan") bereits etablierten Muster.

## Revision 1 – 2026-11-04
- Projekt `waystrom-system` angelegt: Grundstruktur (specs, skills,
  user-stories, work-plans, journal, tools), README, CLAUDE.md, Changelog.
- Grund: Aufbau eines Fundaments in VS Code, mit dem die operative Arbeit in
  Notion für die Waystrom GmbH strukturiert und langfristig geregelt wird
  (Gebote/Verbote, Spezifikationen, Journal-Pflicht, Änderungs-Logging).
- Lokales Git-Repository initialisiert; GitHub-Remote
  `https://github.com/reisenderder/waystrom` vorgesehen, Verbindung folgt.
