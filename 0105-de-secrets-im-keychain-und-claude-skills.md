---
number: 105
lang: de
title: "Claude Skills — am Beispiel Netlify Deploy-Check"
date: 2026-03-12
stability: volatile
review_by: 2027-03-12
author: "Stefan Wendel"
coauthor: "Claude Opus 4.6"
coauthor_id: claude-opus-4-6
---

# Claude Skills — am Beispiel Netlify Deploy-Check

**Das Rad muss nicht immer wieder erfunden werden — Wiederkehrende Agent-Workflows gehören in einen Skill.**

2026-03-12 · Stefan Wendel · Claude Opus 4.6

---

Ein [Skill](https://docs.anthropic.com/en/docs/claude-code/slash-commands) ist ein wiederverwendbares "Slash-Command" für Claude Code — eine Markdown-Datei mit Anweisungen, die der Agent auf Abruf ausführt. Statt denselben Prompt immer wieder zu tippen, definiert man ihn einmal als Skill und ruft ihn mit `/skill-name` auf.

In unserem Fall wollen wir folgenden Workflow in einem Skill abbilden: Nach einem PR-Merge auf `main` triggert Netlify automatisch einen Build. Die Frage ist immer dieselbe: Ist der Deploy schon durch? Bisher habe ich dafür jedes Mal das Netlify-Dashboard geöffnet oder eine curl-Zeile aus der Shell-History gefischt oder 5- bis 10-mal auf CMD-R gedrückt. Alles unterbricht den Flow. Und der Flow ist ja das neue Gold, also: unterbrich niemals den Flow ;-)

Die Lösung besteht aus zwei Teilen: Den API-Token für die Netlify-API-Abfrage, ob der Deploy schon durch ist, einmalig sicher auf seinem Rechner ablegen — und den ganzen Workflow als Claude Code Skill verpacken, damit er jederzeit mit einem Slash-Command abrufbar ist.
		
## Teil 1: Secrets im macOS Keychain

macOS hat einen eingebauten, verschlüsselten Secret Store: den Keychain. Kein Homebrew-Paket, kein externes Tool — das CLI `security` ist auf jedem Mac vorhanden.

**Token speichern** (einmalig):

```bash
security add-generic-password -s "netlify-api-token" -a "1001lessons" -w "DEIN_TOKEN"
```

**Token abrufen** (in Scripts oder Prompts):

```bash
security find-generic-password -s "netlify-api-token" -a "1001lessons" -w
```

Das war's. Der Token liegt verschlüsselt im Keychain, geschützt durch Login-Passwort oder Touch ID. Kein Risiko, ihn versehentlich zu committen. Kein `.env`-File, das in `.gitignore` stehen muss und trotzdem irgendwann im falschen Repo landet.

Dieses Muster funktioniert für jeden API-Token, SSH-Passphrase oder sonstigen Secret, den man in der Shell braucht.

## Teil 2: Claude Code Skills

Claude Code unterstützt eigene Slash-Commands — sogenannte Skills. Eine Skill-Datei ist nichts weiter als eine Markdown-Datei in `.claude/commands/`, die einen Prompt enthält. Wenn ich `/check-deploy` eintippe, führt Claude den Prompt aus.

**Datei anlegen:** `.claude/commands/check-deploy.md`

```markdown
Prüfe den aktuellen Netlify Deploy-Status für 1001lessons.io.

Nutze den Netlify API Token aus dem macOS Keychain:
security find-generic-password -s "netlify-api-token" -a "1001lessons" -w

Ruf damit die Netlify Deploy API ab und gib aus:
- State (building / ready / error)
- Zeitstempel
- Deploy-Dauer
```

**Aufrufen:** Einfach `/check-deploy` in Claude Code eintippen. Der Agent liest den Token aus dem Keychain, ruft die API ab und gibt den Status aus. Keine curl-Zeile kopieren, kein Dashboard öffnen.

## Warum das ein gutes Muster ist

Die Kombination aus Keychain und Skill löst zwei Probleme gleichzeitig:

**Secrets bleiben sicher.** Der Token steht weder im Prompt noch in einer Datei, die im Repo landen könnte. Der Agent liest ihn zur Laufzeit aus dem Keychain — genau wie ein Shell-Script es tun würde.

**Workflows werden wiederverwendbar.** Statt jedes Mal den gleichen API-Call zu erklären, definiere ich den Workflow einmal als Skill. Der Agent hat den Kontext — er weiß welches Projekt, welche Site, welche API. Ich tippe einen Slash-Command und bekomme das Ergebnis.

Das Prinzip lässt sich auf jeden wiederkehrenden Workflow übertragen: CI-Status prüfen, DNS-Einträge abfragen, Lighthouse-Scores abrufen. Wenn man denselben Prompt zum dritten Mal tippt, lohnt sich ein Skill.

## Schritt für Schritt

1. **Netlify Personal Access Token** anlegen: netlify.com → User Settings → Applications
2. **Token im Keychain speichern:** `security add-generic-password -s "netlify-api-token" -a "ACCOUNT" -w "TOKEN"`
3. **Netlify Site ID** raussuchen: Site Settings → Site details
4. **Skill anlegen:** `.claude/commands/check-deploy.md` mit dem Prompt-Template
5. **Testen:** `/check-deploy` in Claude Code eingeben

## Goodie on top: Automatische Deploy-Benachrichtigung

Es geht noch einen Schritt weiter. In meinem Setup muss ich `/check-deploy` gar nicht selbst aufrufen — Claude teilt mir den Deploy-Status automatisch mit, wenn es im Kontext passt.

Der Trick: In der `CLAUDE.md` steht als Anweisung, dass der Agent nach einem PR-Merge oder Push auf `main` prüfen soll, ob der Deploy durchgelaufen ist. Claude kennt den Skill, kennt den Kontext — und ruft den Check selbstständig auf. Kein Slash-Command nötig.

Technisch passiert dabei nichts Magisches. Claude Code liest beim Start der Session die `CLAUDE.md` und sieht die registrierten Skills. Wenn die Situation passt — gerade wurde ein PR gemergt, gerade wurden Posts ins Content-Repo gepusht — führt der Agent den Skill eigenständig aus und meldet: „Deploy ist ready, hat 45 Sekunden gedauert."

Das ist der eigentliche Mehrwert von Skills gegenüber Shell-Aliases oder Bookmarks: Der Agent kann sie nicht nur auf Befehl ausführen, sondern auch proaktiv — weil er den Kontext versteht, in dem sie sinnvoll sind.
