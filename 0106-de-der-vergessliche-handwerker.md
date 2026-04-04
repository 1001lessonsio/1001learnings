---
number: 106
lang: de
title: "Der vergessliche Handwerker — Wie man AI-Agenten beibringt, sich an ihre eigenen Werkzeuge zu erinnern"
date: 2026-04-04
stability: volatile
review_by: 2027-04-04
updated: 2026-04-04
author: "Stefan Wendel"
coauthor: "Claude Opus 4.6"
coauthor_id: claude-opus-4-6
---

# Der vergessliche Handwerker — Wie man AI-Agenten beibringt, sich an ihre eigenen Werkzeuge zu erinnern

**Und warum Organisationen im Vergleich zu Einzelentwicklern hier ein stringentes Vorgehen brauchen.**

2026-04-04 · Stefan Wendel · Claude Opus 4.6

---

Denke an den besten Handwerker, den Du kennst. Er kann alles bauen — Möbel, Installationen, Elektrik. Aber jeden Morgen, wenn er bei Dir im Haus steht, hat er vergessen, wo seine Werkzeuge liegen. Er weiß nicht mal mehr, dass er gestern einen Akkuschrauber benutzt hat. Also improvisiert er. Schraubt mit bloßen Händen. Oder läuft los und kauft einen neuen.

Das ist kein hypothetisches Beispiel. Das ist Claude-Code/Codex/opencode/der KI-Agent der Wahl in Eurer Organisation nach einem Neustart. Dann, wenn ihr ihn am dringendsten braucht. Wenn aus der Spülmaschine das Wasser läuft, der Server down ist, der Kunde am Telefon wartet. 

## Ein Negativbeispiel: Das PDF Disaster

In einer Session am Vormittag brauchte ich ein PDF aus einem Markdown-Dokument. Claude fand einen eleganten Weg: ein kleines Python-Skript, `fpdf2` und `markdown` als Dependencies, fertig. Sauber, transparent, hat auf Anhieb funktioniert. Ich war beeindruckt.

Am Nachmittag — neue Session nach einem Context-Clear — brauchte ich wieder ein PDF. Gleicher Rechner, gleiches Repo, gleiches Markdown. Was folgte, war eine wilde Orgie aus Installationsversuchen: LaTeX herunterladen, pandoc suchen, irgendwelche npm-Packages ausprobieren. Das Python-Skript vom Vormittag? Existierte noch. Lag sogar im gleichen Verzeichnis. Aber der Agent hatte keine Ahnung davon.

Das war der Moment, in dem mir klar wurde: **Das Problem ist nicht der Agent. Das Problem bin ich.** Ich hatte das funktionierende Rezept nicht aufgeschrieben — jedenfalls nicht so, dass der Agent es in der nächsten Session wiederfinden konnte.

## Gute Handwerker haben gute Gewohnheiten

Der Handwerker ist nicht dumm, er braucht kein besseres Gehirn oder mehr Training (davon hat er mehr als genug). Er braucht ein System: Werkzeuge an einem festen Platz, Anleitungen an der Wand, eine Checkliste am Morgen. Kurz: gute Gewohnheiten.

Für AI-Agenten gibt es solche Gewohnheiten. Vier Stück — und jede löst ein eigenes Problem.

## Gewohnheit 1: CLAUDE.md — der Plan an der Wand

`CLAUDE.md` ist eine Datei, die der Agent beim Start jeder Session liest. Sie ist sein Gedächtnis — oder genauer: sein Ersatz dafür.

Es gibt zwei Ebenen:
- **Global** (`~/.claude/CLAUDE.md`): Gilt für alle Projekte auf der Maschine. Hier gehört rein, was immer gelten soll — zum Beispiel: *"PDFs werden ausschließlich mit `~/.claude/md_to_pdf.py` generiert. Kein LaTeX, kein pandoc, keine Nachinstallation."*. Das ist der Plan für das ganze Bauprojekt. Wie spielen die Räume zusammen? Was in welcher Reihenfolge? Welche Aufgabe hat wer?
- **Pro Repo** (`<repo>/CLAUDE.md`): Projektspezifisches Verhalten. Git-Konventionen, Deployment-Prozesse, Test-Strategien. Hier hängt der Plan an der Wand des Raumes, in dem gerade gearbeitet wird.

Das PDF-Problem war in 30 Sekunden gelöst, nachdem ich den Satz in die globale `CLAUDE.md` geschrieben hatte (bzw. schreiben ließ). Seitdem ist klar, wie PDFs generiert werden sollen.

**Die Lektion:** Wenn der Agent etwas heute richtig gemacht hat, lass es aufschreiben, bevor du die Session schließt. Nicht als Dokumentation für Menschen — als Instruktion für den Agenten.
Und sei dabei explizit: Benenne, wo es aufgeschrieben werden soll, also konkret: *"Bitte merke Dir in der CLAUDE.md des lokalen Repos, dass ..."*. Ein bloßes *"bitte merke Dir"* führt dazu, dass Claude die Information in internen Memory-Dateien ablegt, die verloren gehen können.

## Gewohnheit 2: Skills — jedes Werkzeug hat seinen Platz

Skills sind benutzerdefinierte Slash-Commands für Claude Code. Eine Markdown-Datei in `.claude/commands/`, die einen Prompt enthält. Wenn ich `/check-deploy` tippe, führt der Agent den Prompt aus — immer gleich, ohne Improvisation.

Statt bei jedem PDF-Problem den Lösungsweg neu zu erklären, definiere ich ihn einmal als Skill. Das Learning: Instruiere Claude so, dass alle verfügbaren Skills mittels `/help`-Command aufgelistet und erklärt werden.

**Vorschlag: `/org-help`** — ein Skill, der alle Org-eigenen Slash-Commands mit Kurzbeschreibung auflistet. Wer nicht weiß, was es gibt, kann nichts nutzen. Discovery ist kein Nice-to-have — Discovery ist die Voraussetzung für Nutzung.

Skills leben in `~/.claude/commands/` (global) oder `.claude/commands/` (pro Repo). Für ein einzelnes Projekt reicht das. Für ein Team oder eine Organisation wird Verteilung zum Thema — dazu gleich mehr.

## Gewohnheit 3: MCP-Server — rufe den Kollegen vom anderen Gewerk

Meine bisherige Erfahrung:

**Für einen Einzelentwickler bringt MCP kaum Mehrwert.** Ein Bash-Skript, das in `CLAUDE.md` referenziert wird, tut funktional dasselbe wie ein MCP-Tool. Ich kann den Agent bitten, ein Skript auszuführen — und er tut es. Der Protokoll-Overhead von MCP lohnt sich für eine Person nicht.

Der eigentliche MCP-Vorteil zeigt sich für Organisationen und löst drei Probleme:

**Erstens: Klar definierte Interfaces statt lokale Improvisationen.** `CLAUDE.md` gibt dem Agenten eine Instruktion: *"Benutze dieses Skript."* Er kann sie befolgen — oder in einer neuen Session improvisieren. Ein MCP-Tool gibt dem Agenten ein Interface: `generate_pdf` existiert als Werkzeug mit definierten Inputs. Er ruft es auf oder nicht — aber er kann nicht "drumherum" arbeiten. Der Unterschied ist: vom *"der Agent sollte X tun"* zum *"der Agent kann nur X tun"*. Das ist eine andere Qualität von Konsistenz.

**Zweitens: Zentrales Deployment statt individueller Konfiguration.** Ein MCP-Server ist ein einziges Deployment. Neue Version, Bug-Fix, neue Policy — einmal ändern, alle Agents haben es sofort. Kein Onboarding-Skript, kein Update auf 200 Maschinen.

**Drittens: Dynamische Daten statt unklarer Kontexte.** MCP kann live Kontext liefern — aktueller Ticket-Status, Infrastruktur-Zustand, DB-Abfragen. `CLAUDE.md` ist statisch: sie beschreibt, was zum Zeitpunkt des Schreibens wahr war.

**Fazit zu MCP:** Als Einzelentwickler: skip it, `CLAUDE.md` und Skripte reichen. Für Organisationen ab einer gewissen Größe: MCP ist der richtige Langzeit-Ansatz — nicht wegen Konsistenz der Vorgehensweise, sondern wegen erzwungener Konsistenz und zentraler Wartung.

## Gewohnheit 4: Die Checkliste am Morgen

Das ist der Baustein, den es noch nicht gibt — und der am dringendsten fehlt.

Stell dir vor, der Handwerker hat morgens eine Checkliste: *Akkuschrauber da? Bohrmaschine aufgeladen? Wasserwaage im Koffer?* Erst wenn alles abgehakt ist, fängt er an.

**Vorschlag: `/org-health`** — ein Skill, der prüft, ob das Agent-Setup vollständig ist:
- Sind alle erwarteten Skripte vorhanden?
- Sind alle MCP-Server erreichbar?
- Ist die globale `CLAUDE.md` aktuell?

Wenn etwas fehlt, repariert der Agent es selbst — oder meldet klar, was manuell zu tun ist. Das Konzept ist nicht neu: CI/CD-Pipelines prüfen den Zustand des Codes vor jedem Deployment. Der Agent braucht dasselbe für sein eigenes Setup.

## Was das für Organisationen bedeutet — heute schon

Alles, was ich bisher beschrieben habe, funktioniert für einen Einzelentwickler mit ein bisschen Disziplin. Für eine Organisation mit 50, 200 oder 1.000 Entwicklern ist Disziplin keine Strategie.

Was bei mir einmal passiert, passiert in einer 200-Personen-Org 200 Mal. Gleichzeitig, an verschiedenen Tagen, mit verschiedenen Workarounds. Der Support wird geflutet. Wissen divergiert. Teams erfinden Räder, die andere längst gebaut haben.

Die Frage ist: **Was ist "zentral" in einer Organisation?**

**CLAUDE.md:** Eine kanonische globale Vorlage, versioniert in einem Org-internen Repo. Änderungen via PR. Verteilung per Onboarding-Skript oder Dotfiles-Management.

**Skills:** Ein zentrales Repo verwaltet die Org-Skills. Ein Onboarding-Skript symlinkt sie in `~/.claude/commands/`. Jeder Entwickler hat die gleichen Slash-Commands — und `/org-help` zeigt, was es gibt.

**MCP:** Statt Skripte auf 200 Maschinen aktuell zu halten, deployt man einen MCP-Server. Alle Agents verbinden sich automatisch. Updates sind sofort wirksam.

**Health-Check:** `/org-health` wird zum First-Level-Support. Bevor ein Entwickler ein Ticket öffnet, lässt er den Agent selbst prüfen, ob sein Setup stimmt. Die meisten "der Agent verhält sich komisch"-Probleme lösen sich durch einen fehlenden MCP-Server oder eine veraltete `CLAUDE.md`.

**Konkret: ein `claude-setup`-Onboarding-Skript**

```bash
# Einmalig auf neuer Maschine:
curl -s https://internal.org/claude-setup.sh | bash
```

Das Skript kopiert die globale `CLAUDE.md`, installiert Org-Skills, schreibt die MCP-Server-Konfiguration und führt `/org-health` als Abschluss-Check aus. Ein neuer Entwickler ist in unter einer Minute einsatzbereit — mit dem gleichen Setup wie alle anderen.

## Eine wichtige Idee für Organisationen: das MCP-Gateway

Die vier Gewohnheiten lösen das Problem für heute. Aber eine Frage bleibt: Eine Organisation hat nicht *einen* MCP-Server — sie hat viele. Deployment, Ticketsystem, Monitoring, Datenbank-Zugriff, interne Dokumentation. Jeder einzelne muss in der Konfiguration jedes Entwicklers stehen. Das ist exakt das gleiche Verteilungsproblem, das man mit Skripten und `CLAUDE.md` hatte — nur eine Abstraktionsebene höher.

Die Lösung kennen wir aus der API-Welt: ein Gateway. Ein einziger Endpunkt, hinter dem alle Backend-Services stehen.

**Ein MCP-Gateway würde bedeuten:**
- **Ein Endpunkt statt 15** — der Agent verbindet sich mit dem Gateway, nicht mit jedem MCP-Server einzeln. In der lokalen Konfiguration steht genau eine URL.
- **Discovery** — der Agent fragt: *"Welche Tools habe ich?"* Das Gateway aggregiert alle Backend-MCP-Server und liefert eine konsolidierte Tool-Liste. Das ist `/org-help` auf Infrastruktur-Ebene.
- **Zentrale Governance** — wer darf welche Tools nutzen? Audit-Log: wer hat wann was aufgerufen? Rate Limiting: kein Agent schießt den Ticket-Server ab.
- **Zero-Config-Änderungen** — neuer MCP-Server im Backend? Das Gateway routet automatisch. Tool deaktiviert wegen Security-Issue? Am Gateway abschalten, sofort für alle weg.

Jede Organisation, die Microservices betreibt, hat irgendwann ein API-Gateway gebaut. Jede Organisation, die MCP-Server betreibt, wird dasselbe für MCP brauchen.

## Was bleibt

AI-Agenten sind keine schlechten Handwerker. Sie sind vergessliche Handwerker. Und die Lösung ist nicht, auf besseres Gedächtnis zu warten — die Lösung ist, ihre Werkzeuge so zu organisieren, dass sie sie jeden Morgen wiederfinden.

Für einen Einzelentwickler reicht eine `CLAUDE.md` und die Disziplin, funktionierende Rezepte aufzuschreiben. Für eine Organisation reicht das nicht. Da braucht es ein stringentes Vorgehen: versionierte Konfiguration, verteilte Skills, zentrale MCP-Server und einen Agent, der seinen eigenen Zustand prüfen kann.

---

*Wie handhabt ihr das? Habt ihr schon erlebt, dass euer Agent in der nächsten Session etwas "vergessen" hat? Ich freue mich über Austausch.*

## Changelog

**2026-04-04** — Statusleisten-Beispiel entfernt, Fokus auf PDF-Beispiel als durchgängigen roten Faden. "Schichten" durch "Gewohnheiten" ersetzt. Überschriften geschärft. Kürzungen und Straffung. Neuer Abschnitt: MCP-Gateway als Idee für Organisationen.
