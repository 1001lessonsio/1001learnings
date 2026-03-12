---
number: 102
lang: de
title: "How to: Mehrere GitHub Accounts sauber verwalten"
date: 2026-02-28
stability: stable
review_by: 2027-02-28
author: "Stefan Wendel"
coauthor: "Claude Sonnet 4.6"
coauthor_id: claude-sonnet-4-6
---

# How to: Mehrere GitHub Accounts sauber verwalten

**Ein SSH-Key pro Account. Ein Alias pro Host. Kein manuelles Umschalten.**

2026-02-28 · Stefan Wendel · Claude Sonnet 4.6

---

Wer als Entwickler mehrere GitHub-Accounts nutzt — etwa einen privaten, einen für die Firma und einen für ein Nebenprojekt — kennt das Problem: Git fragt nicht, welchen Account man gerade meint. Es nimmt einfach den ersten passenden Key. Das führt zu Pushes unter dem falschen Account, abgelehnten Authentifizierungen oder dem klassischen Fehler, einen privaten Commit unter dem Firmen-Account zu landen.

Die Lösung ist einfach und hält dauerhaft: **ein SSH-Key pro Account, konfiguriert über Host-Aliases in `~/.ssh/config`.**

## Das Setup

### 1. SSH-Keys generieren

Für jeden Account einen eigenen Ed25519-Key erstellen:

```bash
ssh-keygen -t ed25519 -C "du@example.com" -f ~/.ssh/id_ed25519_personal
ssh-keygen -t ed25519 -C "du@firma.com"   -f ~/.ssh/id_ed25519_work
ssh-keygen -t ed25519 -C "du@projekt.com" -f ~/.ssh/id_ed25519_project
```

Die `-f`-Option gibt den Dateinamen vor — so landen alle Keys sauber getrennt in `~/.ssh/`.

### 2. Public Keys bei GitHub hinterlegen

Für jeden Account:
1. Bei GitHub einloggen
2. **Settings → SSH and GPG keys → New SSH key**
3. Inhalt der jeweiligen `.pub`-Datei einfügen

```bash
cat ~/.ssh/id_ed25519_personal.pub   # für Account 1
cat ~/.ssh/id_ed25519_work.pub       # für Account 2
cat ~/.ssh/id_ed25519_project.pub    # für Account 3
```

### 3. ~/.ssh/config anlegen

```
Host github-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_personal

Host github-work
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_work

Host github-project
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_project
```

Jeder `Host`-Eintrag ist ein Alias für `github.com` — mit einem fest zugeordneten Key.

### 4. Verbindung testen

```bash
ssh -T git@github-personal
# Hi username-personal! You've successfully authenticated...

ssh -T git@github-work
# Hi username-work! You've successfully authenticated...
```

## Repos richtig verbinden

Statt `github.com` in der Remote-URL einfach den Alias verwenden:

```bash
# Neues Repo verbinden:
git remote add origin git@github-work:firma/repo.git

# Bestehende Remote umstellen:
git remote set-url origin git@github-personal:username/repo.git
```

SSH schaut beim Verbindungsaufbau in die Config, findet den passenden Alias und greift automatisch zum richtigen Key — ohne weiteres Zutun.

## Warum das funktioniert

Die `~/.ssh/config` ist eine Routing-Tabelle für SSH-Verbindungen. Wenn Git eine Verbindung zu `git@github-work:...` aufbaut, matcht SSH den Host `github-work`, liest den zugeordneten `IdentityFile` und authentifiziert sich mit genau diesem Key — unabhängig davon, welche anderen Keys im System vorhanden sind.

Das Ergebnis: **Jedes Repo pushes automatisch mit dem richtigen GitHub-Account**, solange die Remote-URL den passenden Alias verwendet. Kein `ssh-add`, kein Environment-Switching, keine Gefahr, unter dem falschen Account zu committen.

## Das Fazit

Mehrere GitHub-Accounts sind kein Sonderproblem — sie sind Normalzustand für jeden, der professionell entwickelt. Der Schlüssel ist, die Trennung einmalig sauber in `~/.ssh/config` zu kodieren. Danach läuft alles automatisch.

> **Einmal konfigurieren. Nie wieder drüber nachdenken.**

---

*Nutzt du mehrere GitHub-Accounts? Welche Fallstricke sind dir dabei begegnet?*
