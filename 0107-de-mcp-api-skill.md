---
number: 107
lang: de
title: "MCP, API oder Skill? — Warum das die falsche Frage ist"
date: 2026-07-07
stability: stable
review_by: 2027-07-07
author: "Stefan Wendel"
coauthor: "Claude Opus 4.8"
coauthor_id: claude-opus-4-8
---

# MCP, API oder Skill?

**MCP, API und Skill sind keine Alternativen — sie ergänzen sich**

2026-07-07 · Stefan Wendel · Claude Opus 4.8

---

Wer AI-Agenten in die eigene Systemlandschaft holt, stößt früher oder später auf die Frage: *Bauen wir dafür einen MCP-Server? Eine API? Einen Skill?* Sie wird als "wähle zwischen" gestellt - was aber nicht notwendig ist. Jede der drei Ansätze hat eine eigene Aufgabe. Sobald man diese Aufgaben sauber trennt, löst sich die Frage auf.

Die Kurzantwort vorweg: Die **API** ist die Quelle der Wahrheit. Der **MCP-Server** macht sie für den Agenten greifbar. Der **Skill** bettet das Ganze in eine wiederholt ausführbare Form.

## Die drei Bausteine — von unten nach oben gedacht

Am klarsten wird es, wenn man von der Datenquelle aus nach oben denkt.

**API / Datenquelle — die Wahrheit.**
Ganz unten liegt das System, das die Realität kennt: Git-Repo, Wiki, MES, die Datenbank. Die API sollte die *einzige* Quelle der Wahrheit sein — aktuell, versioniert, verbindlich. Nur: Der Agent sollte keine rohen Endpunkte verstehen müssen. Eine API ist als Datenquelle stark und als Agenten-Schnittstelle schwach.

**MCP — die agentenfreundliche Schnittstelle.**
Ein MCP-Server macht die rohe Datenquelle für KI-Agenten aufrufbar: `get_product_info`, `search_engineering_context`. Statt „verstehe diese 40 Endpunkte" heißt es jetzt: „Wann haben wir zuletzt ein Container-Image in die Produktion deployed". Für Just-In-Time Fragen ist das die stärkste Schnittstelle.

**Skill — die Orchestrierung.**
Ein Skill beschreibt einen **Arbeitsprozess über mehrere Schritte**: „Suche erst im Engineering-Wiki, dann im PLM, zuletzt im letzten Release-Manifest." Ein Skill speichert keine Information und kennt keine Daten. Er ist die Regie, nicht die Bühne. 

## Skill ist nicht gleich Prompt

Der Unterschied zwischen einem Skill und der direkten Nutzung eines Prompts ist nicht sofort offensichtlich — beides ist ja nur Text für den Agenten. Er liegt in der **Orchestrierung**.

Ein Prompt ist eine einzelne Aufforderung. Ein Skill bildet einen **mehrstufigen Ablauf** ab — eine Abfolge von Schritten mit Entscheidungen dazwischen: erst hier suchen, dann dort, im Zweifel diese Quelle bevorzugen, das Ergebnis so aufbereiten. Der Skill kapselt Vorgehenswissen, nicht Fachwissen.

Wichtig dabei: Der Skill sollte die eigentliche Information **nicht selbst enthalten** — sonst veraltet sie. Er beschreibt den Weg zur Wahrheit, nicht die Wahrheit.

## Die Gegenüberstellung

| Baustein | Rolle | Enthält die Wahrheit? | Stärke | Schwäche |
|---|---|---|---|---|
| **API / Datenquelle** | Wahrheitsquelle | Ja | Immer aktuell, verbindlich | Roh, nicht agentenfreundlich |
| **MCP** | Agenten-Schnittstelle | Nein (verweist) | Agent fragt gezielt | Braucht Quelle dahinter |
| **Skill** | Orchestrierung | Nein | Bildet mehrstufigen Ablauf ab | Veraltet, wenn er Infos speichert |

## Der richtige Fluss

Für momentane, produktbezogene Informationen sieht der Weg so aus:

```
User-Frage
   ↓
AI Agent
   ↓
Skill: "Produktinformation beantworten"   ← entscheidet WO und WIE gesucht wird
   ↓
MCP-Tool: get_product_info / search_engineering_context   ← agentenfreundliche Schnittstelle
   ↓
API / Datenquelle: Wiki / PLM / MES / Git   ← liefert die Wahrheit
```

Zwei konkrete Beispiele:

- **„Welches JDK setzen wir für die Entwicklung ein?"** → Das MCP-Tool fragt Git-Repo, Build-Dateien, Devcontainer, CI-Konfiguration oder Engineering-Wiki ab.
- **„Wie lange muss Fertigungsteil XY abkühlen?"** → Das MCP-Tool fragt MES, PLM, Arbeitsplan oder Prozessdatenbank ab — mit Versionsstand, Produktvariante und Gültigkeitsdatum.

## Was das konkret heißt

- Trenne die drei Rollen bewusst: eine Datenquelle als einzige Wahrheit, ein MCP-Tool davor, ein Skill als Regie.
- Lege die Wahrheit in die API/Datenquelle — niemals in den Skill.
- Baue MCP-Tools grob-granular und agentenfreundlich (`get_product_info`), nicht als 1:1-Abbild roher Endpunkte.
- Formuliere Skills als Ablauf mit Entscheidungspunkten: erst X, dann Y, im Zweifel Z.

## Das Fazit

Für aktuelle, produktbezogene Informationen sind **MCP + API/Datenquelle** die stärkste Kombination. Der **Skill orchestriert** — er ist Ablauf, nicht Speicher.

Merksatz: **Der Skill entscheidet, wo gesucht wird. MCP liefert die Schnittstelle. Die API liefert die Wahrheit.**

Wer diese drei Rollen sauber trennt, muss sich nie mehr fragen, ob es „ein MCP-Server oder eine API" sein soll. Die Antwort ist fast immer: beides — plus ein Skill, der weiß, wie sie zusammenspielen.

→ Siehe [1001lessons.io/105 — Claude Skills am Beispiel Netlify Deploy-Check](https://1001lessons.io/105): wiederkehrende Agent-Workflows gehören in einen Skill.
