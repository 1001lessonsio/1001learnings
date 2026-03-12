---
number: 101
lang: de
title: "Dein Agent weiß nichts. Shift Into — oder er lernt es nie."
date: 2026-02-28
stability: evergreen
review_by: 2028-02-28
author: "Stefan Wendel"
coauthor: "Claude Sonnet 4.6"
coauthor_id: claude-sonnet-4-6
---

# Dein Agent weiß nichts. Shift Into — oder er lernt es nie.

**Shift documentation into your build — or ship hallucinations.**

2026-02-28 · Stefan Wendel · Claude Sonnet 4.6

---

Wir stehen mitten in einem Paradigmenwechsel. AI Agents schreiben Code, refactoren Module, generieren Tests, erstellen Pull Requests. Tools wie Cursor, Claude Code, Copilot Workspace oder Devin versprechen eine Zukunft, in der wir Software nicht mehr Zeile für Zeile tippen, sondern auf höherer Ebene steuern. Klingt großartig. Ist es auch — mit einem gewaltigen Haken.

**Der Agent, den du gerade startest, wird just in diesem Moment geboren.**

Er hat kein Gedächtnis. Kein implizites Wissen. Keine Erinnerung an das Meeting von letzter Woche, in dem ihr euch gegen GraphQL und für REST entschieden habt. Kein Verständnis dafür, warum der Payment-Service so gebaut ist, wie er gebaut ist. Jeder Prompt, jeder neue Chat ist ein Reset auf null. Der Agent muss die Welt deines Projekts in Sekunden neu erlernen — und das einzige Material, das ihm zur Verfügung steht, ist das, was du ihm gibst.

Und genau hier wird Dokumentation plötzlich von "nice to have" zu "mission-critical".

## Das Garbage-In-Garbage-Out-Prinzip, reloaded

In der klassischen Softwareentwicklung war schlechte Dokumentation ärgerlich. Der neue Kollege brauchte halt zwei Wochen länger zum Onboarding. Jemand hat im Flur gefragt, und irgendwer wusste die Antwort.

In der agentengestützten Entwicklung gibt es keinen Flur. Es gibt keinen Kollegen, der "das mal eben erklärt". Es gibt nur den Kontext, den du dem Agent mitgibst. Und der bestimmt die Qualität des Outputs — deterministisch und gnadenlos.

**Dokumentation, die veraltet ist**, führt dazu, dass der Agent gegen eine API-Version entwickelt, die es nicht mehr gibt. Oder Patterns verwendet, die ihr längst aufgegeben habt.

**Dokumentation, die fehlt**, erzeugt Lücken. Und Lücken füllt ein LLM bekanntlich kreativ auf — mit Halluzinationen. Der Agent erfindet dann Endpunkte, die nicht existieren, oder trifft Architekturentscheidungen, die eurer Strategie widersprechen.

**Dokumentation, die nicht eindeutig ist**, führt zu falschen Abzweigungen. Der Agent nimmt den plausiblesten Pfad — und der ist nicht immer der richtige.

## Täglich grüßt das Murmeltier: Der Agent ist euer neuer Mitarbeiter. Jeden Tag. Zum ersten Mal.

Stellt euch vor, ihr müsstet jeden Morgen einem brillanten, aber komplett amnesischen Entwickler euer gesamtes Projekt erklären. Alles. Von der Architektur über Business-Regeln bis hin zu den Konventionen eures Teams. Jeden. Einzelnen. Tag.

Genau das passiert, wenn ihr mit AI Agents arbeitet. Und die Qualität dieser "Erklärung" — also eurer Dokumentation — entscheidet darüber, ob der Agent produktiv ist oder Chaos anrichtet.

Das bedeutet: Wir müssen Dokumentation als lebendiges, gepflegtes System begreifen. Nicht als Pflichtübung, die einmal geschrieben und dann vergessen wird.

## Was konkret zu tun ist

**Alles in Git, versioniert, nah am Code.** Jede Architekturentscheidung, jedes ADR (Architecture Decision Record), jede API-Spezifikation, jede Konvention — alles gehört ins Repository. Nicht in ein Confluence-Wiki, das seit 2022 nicht angefasst wurde. Nicht in den Kopf des Tech Leads. In Git. Beim Code. Versioniert. Reviewt. Aktuell.

**Dokumentation muss aktuell gehalten werden.** Das klingt banal, ist aber der schwierigste Teil. Ein Dokument, das den Stand von vor sechs Monaten beschreibt, ist schlimmer als kein Dokument — weil es aktiv in die Irre führt. Jede Änderung am System muss eine Frage auslösen: Stimmt die Doku noch?

**Lücken müssen systematisch erkannt werden.** Was ist nicht dokumentiert? Welches implizite Wissen steckt nur in den Köpfen des Teams? Wo trifft ein Agent zwangsläufig auf ein Informationsvakuum? Diese Lücken zu identifizieren ist keine einmalige Aufgabe, sondern ein fortlaufender Prozess.

**Fehlendes Wissen muss aktiv aufgefüllt werden.** Wenn ihr merkt, dass ein Agent an einer Stelle regelmäßig falsch abbiegt — dokumentiert genau diese Stelle. Jeder Fehler eines Agents ist ein Signal für fehlende oder unklare Dokumentation.

## Docfooding: Agents pflegen die Doku, die sie selbst brauchen

Dieselben Agents, die gute Dokumentation brauchen, können beim Erstellen und Pflegen dieser Dokumentation unterstützen:

Ein Agent kann euren Code analysieren und feststellen, wo Dokumentation fehlt oder veraltet ist. Er kann aus Code-Changes automatisch Zusammenfassungen generieren. Er kann ADRs im richtigen Format vorschlagen, wenn Architekturentscheidungen getroffen werden. Er kann bestehende Docs gegen den aktuellen Code-Stand prüfen und Inkonsistenzen flaggen.

Das ist kein Widerspruch — das ist ein positiver Feedback-Loop. Bessere Dokumentation führt zu besseren Agent-Outputs, und Agents helfen, die Dokumentation besser zu machen.

## Das Fazit

In einer Welt, in der AI Agents zunehmend zu echten Teammitgliedern werden, ist Dokumentation keine bürokratische Pflichtübung mehr. Sie ist die Schnittstelle zwischen menschlicher Intention und maschineller Umsetzung. Sie ist der Kontext, der entscheidet, ob euer Agent ein wertvoller Kollege oder eine unkontrollierbare Halluzinationsmaschine ist.

> **Shift Into: Dokumentation ist jetzt Teil des Build-Prozesses.**
>
> **Nicht Shift Left, nicht Shift Right — Shift Into. Doku ist kein Prozessschritt, den man irgendwohin verschieben kann. Doku ist jetzt ein integraler Bestandteil des Builds selbst. Wie der Compile-Step. Wie der Verify-Step. Shift documentation into your build — or ship hallucinations.**

Die Teams, die das früh verstehen und ihre Documentation-as-Code-Praxis ernst nehmen, werden mit AI Agents produktiv sein. Die anderen werden sich wundern, warum "AI-gestützte Entwicklung" bei ihnen nicht funktioniert.

Die Antwort steckt meistens nicht im Tool. Sie steckt in eurer Doku.

---

*Wie haltet ihr eure Dokumentation aktuell? Habt ihr schon Erfahrungen mit Agents gemacht, die an schlechter Doku gescheitert sind? Ich freue mich über Austausch.*
