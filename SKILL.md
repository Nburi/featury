---
name: feature-schmiede
description: "Baut aus einer Idee oder einem Problem ein fertiges, durchdachtes Feature oder eine neue App — inklusive Recherche, Produktplanung mit Signature Moment, Freigabe-Stop, autonomer Umsetzung und mehrstufigem Review mit Nutzer-Simulation. Verwende diesen Skill immer wenn der Nutzer ein neues Feature bauen, eine App von Grund auf erstellen, eine Idee umsetzen oder ein Problem in Software lösen will — auch bei Formulierungen wie «bau mir», «ich hätte gern eine Funktion die», «kannst du X einbauen», «ich brauche eine App für», «mir fehlt in der App noch», «setz das um», «implementier das». Auch triggern wenn eine Idee schon durchdacht ist und nur noch gebaut werden muss, oder wenn ein bestehendes Feature sich «noch nicht rund» anfühlt und überarbeitet werden soll. NICHT verwenden für reine Bugfixes ohne Feature-Anteil (dafür code-fixer) und nicht für reines Ideensammeln ohne Bauauftrag (dafür brainstorming)."
---
 
# Feature-Schmiede
 
Ziel ist nicht ein funktionierendes Feature. Ziel ist ein Feature, bei dem der Nutzer beim
ersten Benutzen kurz stehenbleibt.
 
Der Unterschied zwischen «funktioniert» und «grossartig» sind fast immer Kleinigkeiten, die
technisch trivial und trotzdem selten da sind: der leere Zustand, der Weg zurück, das Undo,
der eine Moment, der sich besser anfühlt als nötig. Dieser Skill zwingt dich, sie zu finden,
bevor der Nutzer sie findet.
 
**Gründlich schlägt schnell.** Wenn du zwischen «schneller fertig» und «besseres Resultat»
wählen musst, wählst du immer das bessere Resultat. Lange Laufzeit ist kein Problem.
 
In diesem Skill heisst **du** = Claude, **der Nutzer** = die Person, die den Auftrag gibt.
 
---
 
## Ablauf im Überblick
 
| Phase | Was passiert | Stop? |
|---|---|---|
| 1 | Informieren + Geschmacks-Inventar | nur bei echten Blockern |
| 2 | Produkt planen: Anforderungen + 3 Signature Moments | — |
| 3 | Umsetzung planen + Lückencheck | **JA — Freigabe abwarten** |
| 4 | Umsetzen | — |
| 5 | Review-Runden 0–4, autonom bis nichts mehr kommt | — |
| 6 | Übergabe: Was gefixt, was notiert | Ende |
 
Nach Phase 3 hältst du an. Danach läuft alles autonom durch bis zur Übergabe.
 
---
 
## Phase 1 — Informieren
 
Bevor du irgendwas planst, musst du zwei Dinge wissen: **was technisch geht** und
**was dem Nutzer gefällt**. Das Zweite wird meistens übersprungen — genau daran scheitern
die Features, die «korrekt, aber langweilig» sind.
 
### 1a. Technisches Inventar
 
- Framework, Version, Sprache, Build-Tooling
- Vorhandene Tests: Welche Art, wie werden sie ausgeführt?
- Verfügbare Tools: Kannst du die App starten? Gibt es einen Browser-Zugriff für visuelle
  Checks? Gibt es Subagents? Das entscheidet, wie Phase 5 aussieht.
- Bestehende Bibliotheken für UI, State, Animation — nichts Neues dazuholen, was es schon gibt.
### 1b. Geschmacks-Inventar (der wichtige Teil)
 
Lies die bestehende Codebase **wie eine Spezifikation, nicht wie Legacy-Code**. Sie ist der
beste verfügbare Beweis dafür, was der Nutzer mag. Schreib 5–7 Konventionen auf, die du
daraus ableitest:
 
> Beispiel aus einer bestehenden To-do-App:
> - Interaktion läuft über Gesten und Drag-and-Drop, nicht über Buttons und Modals
> - Datenerfassung passiert inline, nie in einem Dialog
> - Es gibt keine Bestätigungsdialoge — gelöschtes ist kurz zurückholbar
> - Dark Mode ist gleichwertig, nicht nachträglich drübergelegt
> - Tastatur-Shortcuts existieren für alles, was man mehr als einmal am Tag macht
 
Diese Konventionen sind ab jetzt bindend. Wenn dein Plan gegen eine verstösst, musst du das
in Phase 3 explizit begründen.
 
### 1c. Wenn es noch keine Codebase gibt (neue App)
 
Dann fehlt die Quelle für den Geschmack — und dein Default wird der Durchschnitt aller
Trainingsdaten: Buttons, Modals, Bestätigungsdialoge, Bootstrap-Ästhetik. Das ist der Zustand,
in dem generische Apps entstehen. Ersatz:
 
1. Frag den Nutzer nach **zwei bis drei Apps, die sich für ihn richtig anfühlen** — und was
   genau daran. Eine Frage, zehn Sekunden Aufwand, trägt weiter als jede abstrakte Designfrage.
2. Leite daraus eine **Interaktions-Verfassung** ab: 5–7 Sätze im selben Format wie oben.
3. Schreib sie als `CONVENTIONS.md` ins Projekt. Beim nächsten Feature ist genau diese Datei
   das Geschmacks-Inventar aus 1b.
**Zuschnitt bei neuen Apps:** Version eins ist **ein durchgehender Weg**, von leer bis
Erfolgserlebnis. Nicht zwölf halbe Screens. Alles andere kommt als «später» in den Plan.
 
### 1d. Fragen
 
Fragen darfst du nur zu Dingen, die du nicht selbst herausfinden kannst und die den Plan
grundlegend ändern würden. Alles andere entscheidest du und legst es beim Freigabe-Stop offen.
Nie mehr als zwei Fragen auf einmal, und immer mit deiner eigenen Vermutung dazu.
 
---
 
## Phase 2 — Produkt planen
 
### 2a. Anforderungen mit Herkunft
 
Nicht mehr als 5–8 Punkte. Kriterium: **Was würde das Feature schlecht machen, wenn es fehlt?**
Nicht: was wäre noch nett.
 
Jede Anforderung bekommt eine Herkunft:
 
| Herkunft | Bedeutung | Beim Freigabe-Stop |
|---|---|---|
| `PROMPT` | steht so im Auftrag | nur auflisten |
| `CODEBASE` | aus Konventionen abgeleitet | nur auflisten |
| `GERATEN` | deine Annahme über den Nutzer | **explizit zur Bestätigung vorlegen** |
 
Die `GERATEN`-Zeilen sind die einzigen, bei denen der Nutzer wirklich hinschauen muss. Genau
dort sitzt das, was du falsch verstanden haben könntest.
 
Format pro Anforderung: **Anforderung → warum → Lösung**
 
> `CODEBASE` Eine Aufgabe muss in einem Griff verschiebbar sein
> → weil die App auf Gesten aufgebaut ist und ein Datepicker-Dialog den Fluss bricht
> → Long-Press öffnet einen radialen Schnellwahl-Ring: heute / morgen / Wochenende
 
### 2b. Signature Moment
 
Anforderungen sorgen dafür, dass nichts fehlt. Sie sorgen nie dafür, dass etwas begeistert.
Dafür ist dieser Slot da.
 
Schlag **drei** Signature Moments vor, der Nutzer wählt einen. Regeln:
 
- **Genau einer pro Feature.** Es ist ein Budget, keine Liste. Der Rest darf solide-langweilig
  sein — muss es sogar, sonst fällt der Moment nicht auf.
- Er wird als **konkreter Satz** formuliert, nicht als Kategorie.
  Nicht «schöne Animationen». Sondern: «wenn du eine Aufgabe abhakst, rutscht die nächste
  von selbst nach oben — mit einem Delay, das sich anfühlt wie Aufatmen.»
- Er darf bewusst überengineert sein und darf gegen die Konsistenz verstossen. Das ist der Punkt.
- Er braucht einen Ort: den Moment, den der Nutzer am häufigsten erlebt, oder den ersten.
**Der Signature Moment ist ab hier tabu für Aufräumarbeiten.** Er wird in Phase 5 nie
«vereinheitlicht», «konsistent gemacht» oder wegoptimiert.
 
---
 
## Phase 3 — Umsetzung planen, dann STOP
 
- Wie wird es gebaut? Welche Dateien, welche Reihenfolge, welche Abhängigkeiten?
- Wo verstösst der Plan gegen eine Konvention aus 1b — und warum ist das hier richtig?
- Lückencheck: Geh die Anforderungsliste einmal rückwärts durch. Deckt der Plan jede ab?
  Kommt beim Durchdenken eine Anforderung dazu, die vorher fehlte?
- Was gehört **nicht** dazu? Explizite «später»-Liste. Sie ist deine Schutzmauer gegen
  Scope-Wachstum in Phase 5.
Dann legst du dem Nutzer vor:
 
1. Anforderungen, `GERATEN`-Zeilen zuoberst und markiert
2. Die drei Signature Moments zur Auswahl
3. Den Umsetzungsplan in Kurzform
4. Die «später»-Liste
**Und wartest.** Nicht weiterbauen, nicht «schon mal anfangen».
 
---
 
## Phase 4 — Umsetzen
 
Halte dich an den Plan. Wenn beim Bauen klar wird, dass ein Teil des Plans nicht trägt:
umsetzen, was funktioniert, und die Abweichung notieren — nicht heimlich den Plan umschreiben
und nicht zurückfragen. Die Abweichung kommt in die Übergabe.
 
Bau den Signature Moment nicht zuletzt. Was zuletzt kommt, wird lieblos.
 
---
 
## Phase 5 — Review
 
Hier entscheidet sich alles. Die Regel, die diese Phase überhaupt erst sinnvoll macht:
 
> **Die Anforderungsliste ist die erste Linse, nie die einzige.**
> Du hast sie geschrieben — gegen sie allein geprüft, hakst du sie ab.
 
Jede Runde stellt deshalb eine **andere Frage**. Wenn Subagents verfügbar sind, führt sie
ein Subagent durch, der den Plan nicht gesehen hat. Sonst machst du sie selbst — aber
strikt in der Rolle der jeweiligen Runde.
 
### Runde 0 — Anforderungs-Abgleich
 
Die einzige Runde, die **fehlende** Dinge findet. Alle anderen finden nur, was da ist und stört.
Deswegen kommt sie zuerst.
 
Geh die Anforderungsliste aus Phase 2 Punkt für Punkt durch. Pro Anforderung:
**Wo genau im fertigen Produkt wird sie eingelöst?** Nicht «der Code ist da», sondern: welcher
Screen, welche Interaktion, welcher Zustand.
 
Drei mögliche Ausgänge:
 
| Status | Bedeutung | Was passiert |
|---|---|---|
| `ERFÜLLT` | konkret benennbarer Ort im Produkt | nichts |
| `HALB` | technisch vorhanden, aber im Ablauf nicht erreichbar oder unauffällig | **fixen** |
| `FEHLT` | gar nicht gebaut | **fixen** |
 
`HALB` ist die interessante Kategorie und der Grund, warum diese Runde nicht «nur abhaken» ist.
Beispiel: Kategorien existieren, aber man kann sie nur beim Anlegen setzen und nie mehr ändern —
technisch erfüllt, im Ablauf tot.
 
Wenn Subagents verfügbar sind: Der Agent bekommt **nur die Anforderungsliste und die laufende
App**, nicht den Plan und nicht den Code-Diff. Sonst prüft er gegen die Absicht statt gegen das
Ergebnis.
 
### Runde 1 — Ablauf
 
Geh das Feature dreimal durch und notier jede Stelle, an der du zögerst oder etwas erklären
müsstest:
 
1. **Leerer Zustand** — nichts angelegt, erste Benutzung. Bei einer neuen App ist das *der*
   Zustand, nicht einer von dreien, und der am liebslosesten gebaute Screen überhaupt.
2. **Voller Zustand** — 200 Einträge, lange Texte, Umlaute, ein Eintrag mit 400 Zeichen.
3. **Falsch bedient** — leeres Formular, doppelter Klick, Abbruch mitten drin, zurück-Taste.
### Runde 2 — Fixes verifizieren
 
Nur die Funde aus Runde 1. Ist es wirklich weg? Ist dabei was anderes kaputtgegangen?
Laufen die Tests?
 
### Runde 3 — Wo lügt das UI
 
- Sieht etwas klickbar aus und tut nichts?
- Gibt es einen Ladezustand für alles, was dauern kann?
- Sagt eine Fehlermeldung, was zu tun ist — oder nur, dass etwas schiefging?
- Verspricht ein Label mehr, als der Knopf einlöst?
- Verschwindet etwas ohne Rückmeldung?
### Runde 4 — Nutzer-Simulation
 
Die stärkste Runde, weil sie nicht nach Fehlern sucht, sondern nach **Reibung**. Ein Reviewer
mit dem Auftrag «finde Probleme» findet Probleme, auch wenn keine da sind. Ein Agent mit einem
Ziel hat einen echten Massstab: Alles, was ihn aufhält, ist real.
 
Setup:
 
- Der Agent bekommt **eine Rolle und ein Ziel**, keine Anleitung und **keine Feature-Liste**.
  Sonst hakt er Features ab, statt zu arbeiten — und du hast wieder eine Checkliste statt
  einen Nutzer. Maximal ein Satz zur App («eine To-do-App»).
- **Die Rolle darf nicht der Nutzer sein, für den geplant wurde.** Sonst schreibst du Plan und
  Testfall aus derselben Vorstellung, und beides passt perfekt zusammen. Nimm eine Rolle aus
  dem Katalog unten, mindestens jede zweite Runde eine schräge.
- Er protokolliert **während** der Arbeit, nicht danach: was er als Nächstes tun wollte, wo er
  gesucht hat, was er stattdessen gefunden hat. Ein Fazit hinterher ist immer geglättet.
- Er darf sich **beschweren, aber nichts vorschlagen**. Sobald er Lösungen liefert, wechselt er
  in den Entwickler-Modus und redet sich das Problem klein, weil er den Fix schon sieht.
Auswertung:
 
- **Reibung** (er wusste nicht, wie) → wird gefixt.
- **Fehlender Funktionsumfang** (die App kann es objektiv nicht) → kommt auf die «später»-Liste,
  wird **nicht** nachgebaut.
- **Signature-Moment-Check:** Erwähnt das Protokoll ihn? Nicht «gut gelöst», nicht «hat mich
  überrascht», gar nichts? Dann ist er nicht gelandet — egal wie sauber er implementiert ist.
  Das ist ein härteres Kriterium als jede Anforderung.
### Scope-Regeln für alle Runden
 
| Fund | Was passiert |
|---|---|
| Liegt **innerhalb** des Features | sofort fixen |
| Fasst **andere Dateien** an | nur notieren |
| Kippt eine **Architekturentscheidung** | nur notieren |
| Betrifft den **Signature Moment** | tabu — nicht anfassen |
| Steht auf der **«später»-Liste** | nur notieren |
 
### Abbruchkriterium
 
Zwei Bedingungen, beide müssen erfüllt sein:
 
> 1. **Jede Anforderung ist `ERFÜLLT`** — oder bewusst als «später» notiert, mit Grund.
> 2. **Eine ganze Runde findet nichts Neues mehr.**
 
Bedingung 1 allein reicht nicht: Sie ist prüfbar und deswegen abhakbar, und sie sagt nichts
darüber, ob sich das Ergebnis gut anfühlt. Bedingung 2 allein reicht auch nicht: Die Runden 1–4
finden nur, was stört — nicht, was fehlt.
 
Runden dürfen sich wiederholen, solange die Linse wechselt. Wenn dieselbe Linse zum zweiten Mal
nichts findet, ist Schluss.
 
---
 
## Phase 6 — Übergabe
 
Kurz, keine Zeremonie:
 
- Was gebaut wurde, in drei Sätzen
- Der gewählte Signature Moment und ob er in Runde 4 aufgetaucht ist
- Was in den Review-Runden gefunden **und gefixt** wurde
- Was **notiert, aber nicht gefixt** wurde — mit Grund (Scope / Architektur / später-Liste)
- Abweichungen vom Plan aus Phase 4
---
 
## Rollen-Katalog für Runde 4
 
Erweiterbar — neue Rollen einfach unten anhängen.
 
| Rolle | Ziel |
|---|---|
| Lehrling, erste Woche im Betrieb | «Plan deine erste Woche: Schule, Betrieb, Abgaben» |
| Jemand, der die App vor 3 Wochen zuletzt geöffnet hat | «Finde raus, was du verpasst hast, und räum auf» |
| Vereinstrainer mit 14 Athleten | «Organisier das nächste Wochenende für die Gruppe» |
| Jemand am Handy, unterwegs, eine Hand frei | «Trag drei Sachen ein, die dir gerade eingefallen sind» |
| Jemand, der die App gerade zum ersten Mal öffnet | «Finde raus, wofür das gut ist, und probier's aus» |
| Jemand, der schon 400 Einträge drin hat | «Finde die eine Sache von letztem Monat wieder» |
| *(schräg)* Jemand, der die App zweckentfremdet | «Nutz das als Einkaufsliste» |
| *(schräg)* Jemand mit sehr schlechter Verbindung | «Trag was ein, während die Hälfte lädt» |
 
---
 
## Beispiele
 
Erweiterbar — neue Fälle einfach unten anhängen.
 
### Beispiel 1: Signature Moment, gut vs. schlecht
 
❌ **Zu vage, wird beim Bauen zu nichts:**
> «Die App soll sich flüssig und modern anfühlen.»
 
❌ **Kategorie statt Moment:**
> «Wir bauen schöne Micro-Animations für alle Interaktionen.»
 
✅ **Ein Moment, ein Ort, ein Gefühl:**
> «Wenn du die letzte Aufgabe des Tages abhakst, kollabiert die Liste nicht zu einem leeren
> Kasten, sondern zu einer einzigen Zeile: was morgen ansteht. Kein Konfetti, kein 'Gut
> gemacht' — nur der nächste Tag, schon bereit.»
 
### Beispiel 2: Anforderung mit Herkunft
 
❌ **Generisch, Konsens, hilft niemandem:**
> Die nächsten Aufgaben müssen klar erkennbar sein → hervorheben
 
✅ **Mit Herkunft und echter Lösung:**
> `GERATEN` Die nächsten drei Aufgaben müssen ohne Scrollen und ohne Filter sichtbar sein
> → weil die App laut Auftrag «beim Öffnen sofort sagen soll, was jetzt dran ist», und
> Hervorhebung in einer langen Liste dafür nicht reicht
> → fixe Kopfzone «Jetzt» mit max. drei Einträgen, Rest darunter, Zone verschwindet wenn leer
 
### Beispiel 3: Protokoll aus Runde 4
 
❌ **Fazit im Entwickler-Modus (unbrauchbar):**
> «Die App funktioniert gut. Die Aufgabenverwaltung ist intuitiv. Verbesserungsvorschlag:
> ein Datepicker wäre hilfreich.»
 
✅ **Protokoll während der Arbeit:**
> «Wollte 'Berufsschule Mittwoch' auf nächste Woche schieben. Hab dreimal auf das Datum
> getippt, nichts passierte. Dann das Menü rechts gesucht — nicht gefunden. Am Ende die
> Aufgabe gelöscht und neu erstellt.»
 
### Beispiel 4: Scope-Entscheid in Phase 5
 
> **Fund:** Beim Anlegen einer Aufgabe mit leerem Titel wird sie trotzdem gespeichert.
> → **innerhalb des Features → sofort fixen.**
 
> **Fund:** Die Sortierung der Liste ist global und lässt sich nicht pro Projekt setzen.
> → **kippt eine Architekturentscheidung → notieren, nicht bauen.**
 
> **Fund:** Der Abhak-Moment fühlt sich anders an als der Rest der App.
> → **Signature Moment → tabu, nicht anfassen.**
 
### Beispiel 5: Freigabe-Stop, richtig formuliert
 
✅
> Plan steht. Zwei Annahmen, bei denen ich raten musste — schau dir vor allem die an:
>
> `GERATEN` Wiederkehrende Aufgaben sind Teil davon (weil «Woche planen» sonst jede Woche
> Handarbeit ist)
> `GERATEN` Kategorien sind farbig, nicht textlich (weil die App sonst nirgends Labels nutzt)
>
> Drei Signature Moments zur Wahl: [A] … [B] … [C] …
>
> Nicht dabei, bewusst: Erinnerungen, Sharing, Kalender-Sync.
 
### Beispiel 6: *(hier weitere Fälle anhängen)