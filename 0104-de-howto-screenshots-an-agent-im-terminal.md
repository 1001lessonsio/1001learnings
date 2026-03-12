---
number: 104
lang: de
title: "Howto: Mein Workflow um Screenshots an einen Agent im Terminal zu übergeben"
date: 2026-03-06
updated: 2026-03-09
stability: volatile
review_by: 2027-03-06
author: "Stefan Wendel"
coauthor: "Claude Opus 4.6"
coauthor_id: claude-opus-4-6
---

# Howto: Mein Workflow um Screenshots an einen Agent im Terminal zu übergeben

**Ein simpler Trick, der meinen Terminal-Workflow mit Claude Code komplett gemacht hat.**

2026-03-06 · Stefan Wendel · Claude Opus 4.6

---

> **Update 2026-03-09:** Claude Code unterstützt inzwischen das direkte Einfügen von Bildern per `Ctrl+V` in der Kommandozeile — kein Dateipfad mehr nötig. Der unten beschriebene Workflow bleibt trotzdem sinnvoll: für strukturierte Sessions, mehrere Geräte und automatisches Aufgreifen durch den Agent. Beide Wege ergänzen sich.

> Dieses Howto beschreibt meinen Workflow auf **macOS**. Die Grundidee funktioniert überall, aber
> die konkreten Schritte beziehen sich auf macOS-Bordmittel.

Ich bin seit Anfang 2026 mit meinen AI-Agents fast nur noch im Terminal unterwegs — mit Claude Code
in Ghostty. Vorher war ich überzeugt, meine gewohnte IDE nie zu verlassen. Inzwischen ist das
Terminal mein Hauptwerkzeug. Eine Sache fehlte recht schnell: **Wie zeige ich dem Agent, was ich auf
dem Bildschirm sehe?**

In einem Terminal gibt es kein Drag-and-Drop. Inzwischen unterstützt Claude Code `Ctrl+V` zum direkten Einfügen von Bildern aus der Zwischenablage — praktisch für schnelle Einzelfälle. Für einen strukturierten, wiederholbaren Workflow lohnt sich aber ein fester Ablageort: Gib Deinem AI-Agent den Pfad zu den Screenshots.

## Der Workflow in drei Schritten

**1. Screenshot-Ordner einrichten**

Einen festen Ordner für Screenshots anlegen und macOS so konfigurieren, dass Screenshots dort
landen:

```bash
mkdir -p ~/Documents/_screenshots
defaults write com.apple.screencapture location ~/Documents/_screenshots
killall SystemUIServer
```

Tipp 1: Nicht den Desktop nehmen. Ihr werdet in Euren Sessions mit Euren AI-Agents viele Screenshots
machen. Wer jetzt schon ohne Agents den Desktop mit Screenshots gefüllt hat — bitte umstellen.

Tipp 2: Den Screenshot-Ordner auf iCloud Drive legen (z.B. `~/Documents/_screenshots`). Wenn ihr mit mehreren Macs arbeitet, sind die Screenshots automatisch auf allen Geräten verfügbar.

**2. Screenshot machen**

`Cmd + Shift + 4` — Bereich auswählen, fertig. Die Datei landet automatisch im festgelegten Ordner.

**3. Agent darauf hinweisen**

Im Terminal einfach sagen:

```
Bitte schau dir den letzten Screenshot an.
```

Der Agent liest den neuesten Screenshot aus dem Ordner und kann darauf reagieren — UI-Feedback
geben, Fehler erkennen, Layout-Fragen beantworten.

## Warum das funktioniert

Claude Code kann Bilder lesen, wenn es einen Dateipfad bekommt. Der feste Ablageort macht den
Workflow vorhersagbar: kein Suchen, kein Kopieren, kein Erklären. In meiner `CLAUDE.md` habe ich
den Screenshot-Ordner und die Regeln dafür hinterlegt: Der Agent nimmt immer den neuesten
Screenshot, fragt bei Unklarheiten nach und fordert bei visuellen Aufgaben proaktiv einen
Screenshot an. Das spart mir jedes Mal die Erklärung.

Das ist kein Feature, das ist ein Workaround. Aber er funktioniert jeden Tag zuverlässig.

---

## Changelog

**2026-03-09** — Claude Code unterstützt nun `Ctrl+V` zum direkten Einfügen von Bildern in der Kommandozeile. Disclaimer und Einleitung entsprechend aktualisiert.
