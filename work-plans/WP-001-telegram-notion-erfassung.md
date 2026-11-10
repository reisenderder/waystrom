# WP-001 — Telegram-Erfassungssystem für Netzanmeldungen

**Status:** Phase 1/2 (Analyse & Diskussion) — noch keine Umsetzung, kein Code,
kein Server-Zugriff. Dieses Dokument hält den Besprechungsstand fest, damit er
nach einem Kontext-Reset nicht verloren geht.

**Letzter Stand:** 2026-11-10

## 1. Ausgangsproblem

Aktuell entstehen vor Ort bei Netzanmeldungen (PV, Wallbox, Wärmepumpe, Zähler
usw.) Fotos, Videos, Sprach- und Textnachrichten (bisher u. a. über WhatsApp).
Diese Daten werden unstrukturiert gesammelt, später manuell durchsucht,
abgetippt und in Notion nachgetragen (Beispiel aus der Analyse: Fall
„Oschwald" — Seriennummern, Zählerstände, Optimierer-IDs wurden im Nachhinein
aus Fotos abgeschrieben, mit mehreren Korrekturrunden, knapp gerissenen
Fristen). Daten gehen dabei im Chaos verloren bzw. sind schwer wieder
auffindbar.

**Ziel:** Ein Telegram-basiertes Erfassungssystem, das Monteure vor Ort direkt
nutzen, das die eingehenden Infos automatisiert auswertet, strukturiert und in
Notion dokumentiert — so einfach bedienbar, dass Monteure unter Zeitdruck es
ohne Nachdenken benutzen können („idiotensicher").

## 2. Ergebnisse der Notion-Analyse (Phase 1)

Workspace „BASE" (Account rasul@waystrom.com) geprüft:

- **Wissensbasis**: `Netzanmeldung – Wissensbasis` (privat, Aufbauphase) mit
  Datenbanken `Netzbetreiber` und `Anmeldeverfahren`. Seite `PV-Anmeldung`
  definiert den nötigen Datensatz pro Anmeldung (Modul-/WR-/Speicherdaten,
  Lageplan, Fotos Zählerschrank/SLS-Schalter/Vorzählerbereich/Hauserdung,
  Betreiberdaten) und den groben Ablauf (Voranfrage → Einspeisegenehmigung →
  Inbetriebnahme/MaStR → ggf. Zählerwechsel).
- **`Netzanmeldung SOP`** existiert noch nicht wirklich — nur ein To-Do-Eintrag
  für den 17.11., geplant mit Claude-Unterstützung.
- **Operative Daten sind personengebunden**: parallele Datenbanken
  `Rasul-PV-Anlagen`, `Hamada-PV-Anlagen`, `Merdic Bau Netzanmeldungen`. Die
  Anleitung in „PV-Anmeldung" schreibt sogar vor, dass sich jeder eine eigene
  Arbeitsfläche unter seinem Namen anlegt — das begünstigt die Fragmentierung.
- Jede Kunden-Zeile hat Status-Felder (Anmeldung PV bei NB, Wallbox, MaStR,
  Fertigmeldung, Zählerwechsel, Installation & INB) und Datei-Felder
  (Lageplan, Vollmacht, Grundrissplan).
- Einzelne Kunden haben zusätzlich Checklisten-Unterseiten mit einem
  „Notizen / Verlauf"-Muster: datierte Einträge, in denen Ergebnisse aus
  Fotoauswertungen, offene Punkte und Korrekturen laufend nachgetragen werden
  (Beispiel: „Oschwald – Fertigmeldung & Unterlagen (Checkliste)"). Dieses
  Muster ist eine gute Vorlage für automatisch geführte Fall-Logs.
- `Wichtige Vorlagen und Links`: Vollmachten (PV/Wallbox/Wärmepumpe/Zähler),
  Schaltplanmuster, INB-Protokoll-Vorlage. VNB-Zugangsdaten liegen als
  Excel-Datei direkt in Notion.

## 3. Bereits entschiedene Punkte

| Thema | Entscheidung |
|---|---|
| Notion-Struktur | Bleibt unverändert. Das neue System ist eine eigenständige „Insel" (Code/Automatisierung außerhalb von Notion), die nur über die bestehende Struktur Daten in Notion ablegt. |
| Name/Ort | Lebt in diesem Repo (`waystrom-system`), Code-Teil unter `tools/` (genauer Pfad noch offen, z. B. `tools/erfassungsbot/`). |
| Umfang Phase 1 des Baus | Nur: Telegram-Erfassung → automatisierte Auswertung → Protokollierung → Notion-Ablage. Die Anmeldeautomatisierung bei den Netzbetreiber-Portalen kommt **später** als eigener Baustein dazu. |
| Geltungsbereich (fachlich) | Alles, was beim Netzbetreiber anmeldepflichtig ist: PV, Wallbox, Wärmepumpe, Zähler, Zusammenlegung, Kaskadierung, Messkonzepte usw. — nicht nur PV. |
| Bestandsfälle | Bleiben vorerst unangetastet, werden separat/nachträglich abgearbeitet. Nur neu eingehende Fälle laufen über das neue System. |
| VNB-Zugangsdaten | Unverändert lassen, nicht anfassen. |
| Telegram-Konto | Ein neues, normales Nutzerkonto wurde angelegt, muss aber nicht selbst gesteuert werden. Zusätzlich wird ein echter **Bot** über `@BotFather` erstellt (API-Token) — das ist die technische Basis für die Automatisierung. |
| Server | Vultr-VPS ist bereits vorhanden. Node.js/Docker vermutlich möglich, aber ungeprüft. Python wurde dort bereits erfolgreich eingesetzt → bevorzugter Tech-Stack: **Python**-Bot (z. B. `python-telegram-bot`/`aiogram`). |
| VPS-Zugang für Claude | Später klären, wenn es an die Umsetzung geht (nicht jetzt). |
| KI-/Analyse-Engine | Nicht „Notion KI" (keine offene Schnittstelle dafür) — die Analyse (Bild-/Spracherkennung, Textverständnis, Abgleich) läuft über die **Claude-API (Anthropic)**. Notion ist Wissens- und Datenquelle, nicht Analyse-Engine. |
| Freigabe-Schritt vor Notion-Ablage | **Automatisch, aber mit späterer Büro-Prüfung**: erkannte Daten landen sofort in Notion, werden aber als „ungeprüft" markiert, bis jemand im Büro (du/Hamada) sie bestätigt. |
| Bot-Architektur | **Option B** gewählt: Ein Bot als einzige sichtbare Anlaufstelle für die Monteure. Im Hintergrund laufen mehrere spezialisierte Komponenten/„Agenten" (Notion-Abgleich, Bild-/Sprachanalyse, Protokoll-Erstellung, Plausibilitätsprüfung), die sich gegenseitig prüfen können — aber unsichtbar für den Monteur. Grund: gleicher fachlicher Nutzen wie ein sichtbares Bot-Team, aber deutlich weniger Betriebskomplexität und für den Monteur maximal einfach (ein Ansprechpartner). Lässt sich später in echte separate Bots aufteilen, falls nötig. |
| Gruppenstruktur | **Eine gemeinsame Telegram-Gruppe** für alle Mitarbeiter (nicht 1:1-Chats, nicht ein Chat pro Kunde). Fall-Trennung erfolgt über **Telegram-Topics (Forum-Modus)**: pro Fall/Kunde ein eigenes Topic, das der Bot automatisch anlegt bzw. wiedererkennt (Abgleich über Notion). Nachrichten werden allein durch den Ort des Postens (welches Topic) dem richtigen Fall zugeordnet — kein zusätzliches Tippen/Auswählen nötig. Löst auch das Problem kleiner Abweichungen bei Name/Adresse, da der unscharfe Abgleich nur einmal beim Anlegen/Öffnen des Topics passiert, nicht bei jeder Nachricht. |
| Vor-Ort-Abgleich | Bot kennt vorab (aus Notion) laufende Projekte nach Adresse/Kunde. Monteur nennt vor Ort Adresse + Name, Bot gleicht unscharf ab, stellt vorhandene Eckdaten stichpunktartig zusammen und fragt zur Bestätigung („ist das der richtige Kunde/die richtige Adresse?"). |
| Fragen beantworten im Topic | Monteure können im jeweiligen Topic Fragen stellen (z. B. „wie ist der Ablauf bei naturenergie?"); Bot durchsucht relevante Notion-Inhalte (Wissensbasis, Fall-Verlauf), wertet sie mit Claude aus und antwortet direkt im Topic. Technisch machbar (entspricht dem, was in diesem Gespräch manuell mit den Notion-Werkzeugen gemacht wurde, nur automatisiert). |
| Journal/Logger — zwei Ebenen | (1) Pro Fall, in Notion selbst: Bot führt das bestehende „Notizen / Verlauf"-Muster (siehe Oschwald-Beispiel) automatisch weiter — jeder erkannte Wert, jede Nachfrage, jede Korrektur wird datiert nachgetragen. (2) Technischer Betriebs-Logger auf dem Server: unabhängiges Systemprotokoll für Fehlersuche/Nachvollziehbarkeit des Systems selbst. |

## 4. Noch offene Fragen

1. **Q&A-Baustein — Umfang v1 oder v2?** Soll das aktive Beantworten von
   Wissensfragen aus der Notion-Wissensbasis (siehe oben) von Anfang an Teil
   der ersten Version sein, oder zuerst nur Erfassung/Auswertung/
   Protokollierung/Notion-Ablage, und das Frage-Antwort-Verhalten kommt als
   zweiter Ausbauschritt? **→ Noch nicht beantwortet, letzte offene Frage vor
   dem Reset.**
2. **Datenbank-Zuordnung neuer Fälle:** Wie wird einem neuen Fall automatisch
   die richtige bestehende Datenbank (Rasul-/Hamada-/Merdic-PV-Anlagen)
   zugeordnet — z. B. fest hinterlegt pro Monteur? Erste Annahme reicht für
   einen ersten Entwurf, muss aber noch bestätigt werden.
3. **VPS-Zugang:** Art des Zugriffs (SSH-Schlüssel vs. du führst Schritte
   selbst aus) ist auf „später klären" gestellt — muss vor der technischen
   Umsetzung (Phase 3, Bot-Code) entschieden werden.
4. **Notion-Integration (Server-seitig):** Für den Server wird eine eigene
   Notion-Integration (API-Token mit Zugriff auf die betroffenen
   Datenbanken) benötigt — getrennt vom MCP-Zugriff in diesem Chat. Noch
   nicht eingerichtet, noch nicht besprochen, wie/wo der Token sicher
   abgelegt wird (nicht im Git-Repo).
5. **Telegram-Bot-Token:** Bot muss noch über `@BotFather` erstellt werden
   (reine Ausführung, aber noch nicht geschehen).

## 5. Wie nach einem Reset hier weitermachen

1. Dieses Dokument lesen (`work-plans/WP-001-telegram-notion-erfassung.md`).
2. Offene Frage 1 (Q&A-Baustein v1/v2) mit dem Nutzer klären.
3. Danach die übrigen offenen Punkte (Abschnitt 4) der Reihe nach klären.
4. Erst wenn der Nutzer explizit Phase 3 freigibt: `specs/functional-spec.md`
   und `specs/technical-spec.md` entsprechend ergänzen, weiterer
   Work-Plan-Fortschritt hier eintragen, dann erst Code/Server-Zugriff.
5. Bis dahin gilt weiter: keine Dateiänderungen ohne Analyse- und
   Diskussionsphase, kein Pull Request/Merge durch den Agenten (siehe
   `CLAUDE.md`).
