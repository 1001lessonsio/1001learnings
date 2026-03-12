---
number: 103
lang: de
title: "\"Login with Google\" bei GitHub ist eine Bad Practice"
date: 2026-03-04
stability: stable
review_by: 2028-03-04
author: "Stefan Wendel"
coauthor: "Claude Opus 4.6"
coauthor_id: claude-opus-4-6
---

# "Login with Google" bei GitHub ist eine Bad Practice

**Dein OAuth-Provider ist ein Single Point of Failure. Behandle ihn auch so.**

2026-03-04 · Stefan Wendel · Claude Opus 4.6

---

"Login with Google" ist bequem. Ein Klick, kein Passwort, keine Reibung. Die meisten Entwickler nutzen es unreflektiert — bei GitHub, bei Vercel, bei Netlify, bei allem. Und genau da liegt das Problem.

Wer sich bei GitHub ausschließlich über einen OAuth-Provider wie Google anmeldet, gibt die Kontrolle über seinen Account ab. Nicht an GitHub — an Google. Und Google kann diesen Zugang jederzeit kappen, ohne dass GitHub davon etwas mitbekommt.

## Was passiert, wenn Google dich sperrt

Google suspendiert Accounts. Das ist kein Randfall, das passiert. Die Gründe sind vielfältig: mehrere Accounts mit derselben Telefonnummer, Verdacht auf Richtlinienverstöße, algorithmische Fehlentscheidungen. In meinem Fall — der Anlass für diesen Post — hatte ich mehrere Gmail-Accounts, einen pro Projekt, aber alle mit derselben Handynummer verknüpft. Nachvollziehbar, dass Google das als verdächtig einstuft. Aber die Auswirkungen sind kritisch.

Der Einspruchsprozess dauert. In meinem Fall zwei Tage, bis eine klare und positive Regelung stand. Immerhin. Aber zwei Tage Totalblockade bei allen betroffenen Diensten — und es hätte auch deutlich schlimmer ausgehen können.

Wenn dein Google-Account suspendiert ist und du GitHub ausschließlich über "Login with Google" nutzt, passiert Folgendes:

- **GitHub-Login schlägt fehl.** Kein Fehlerdialog, keine Warnung — der OAuth-Flow leitet dich zu Google, Google blockiert, GitHub sieht nur: Authentifizierung fehlgeschlagen.
- **Alle Dienste, die am selben OAuth-Provider hängen, sind gleichzeitig betroffen.** Vercel, Netlify, Railway, Supabase — alles, wo du "Login with Google" genutzt hast, ist auf einen Schlag weg.
- **Deine Repos sind nicht gelöscht**, aber du kommst nicht mehr ran. Kein Push, kein Pull, kein Zugriff auf Issues oder Settings.

Für ein Hobbyprojekt ist das ärgerlich. Für ein kommerzielles Projekt ist es ein inakzeptabler Single Point of Failure.

## Die Absicherung

Die Lösung macht dich unabhängig vom OAuth-Provider:

### 1. E-Mail und Passwort direkt bei GitHub setzen

Geh in die GitHub-Settings und stelle sicher, dass du eine E-Mail-Adresse und ein Passwort hinterlegt hast — unabhängig davon, ob du normalerweise über Google einloggst. Das ist dein Fallback, falls der OAuth-Provider ausfällt.

### 2. OAuth nur als zusätzliche Login-Option behalten

"Login with Google" als Convenience-Feature ist in Ordnung. Als einziger Zugangsweg ist es fahrlässig. Die Frage, die du dir stellen solltest: *Kann ich mich noch einloggen, wenn Google meinen Account morgen sperrt?*

### 3. 2FA mit Authenticator-App aktivieren

Nutze eine Authenticator-App wie 1Password oder Authy — nicht SMS. SIM-Swapping ist ein reales Angriffsszenario, bei dem Angreifer deine Telefonnummer auf eine neue SIM übertragen und damit SMS-basierte 2FA umgehen.

### 4. Recovery Codes sicher speichern

GitHub generiert bei der 2FA-Einrichtung Recovery Codes. Diese Codes sind dein letzter Rettungsanker, wenn sowohl dein OAuth-Provider als auch deine Authenticator-App nicht verfügbar sind. Leg sie in deinen Passwort-Manager — nicht in eine Textdatei auf dem Desktop.

### 5. Dedizierte E-Mail für dein Asset

Behandle jedes Projekt als eigenständiges Asset. Stell dir vor, du würdest es weitergeben wollen — Verkauf, Übergabe an einen Partner, Onboarding eines neuen Maintainers. Wie einfach geht das, wenn alle Dienste an deiner persönlichen Gmail-Adresse hängen?

Nutze für GitHub und andere entwicklungskritische Dienste eine E-Mail-Adresse, die nicht an deinen primären Google-Account gekoppelt ist. Eine eigene Domain mit Catch-All — also einer Konfiguration, bei der jede beliebige Adresse unter deiner Domain angenommen wird (`github@deinedomain.de`, `vercel@deinedomain.de`, etc.) — ist hier die professionellste Lösung.

## Das Muster dahinter

Das Problem ist nicht Google. Das Problem ist die implizite Annahme, dass ein externer Dienst immer verfügbar sein wird. OAuth-Login delegiert die Authentifizierung an einen Drittanbieter — und damit auch das Risiko. Wer das nicht aktiv absichert, baut einen Single Point of Failure in seine gesamte Entwicklungsinfrastruktur.

Das gilt nicht nur für Google und GitHub. Es gilt für jede Kombination aus OAuth-Provider und kritischem Dienst. Apple-ID und iCloud. Microsoft-Account und Azure. Die Logik ist immer dieselbe: **Nie nur einen Weg rein haben.**

> **OAuth ist ein Convenience-Feature, kein Sicherheitskonzept. Behandle es auch so.**

---

*Hast du schon mal den Zugang zu einem Dienst verloren, weil der OAuth-Provider nicht erreichbar war?*
