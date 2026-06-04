# Make-or-Buy-Memo — Annotation der restlichen Anzeigen

> Gerüst für Phase 5, Block 5.3. Platzhalter `_…_` nach dem echten Frontier-Lauf
> mit eigenen κ-Werten aus `notebooks/04_frontier_compare.ipynb` füllen.
> Eine Seite, substantielle Sätze statt Aufzählungen. Diesen Hinweis-Block am Ende löschen.

**An:** Projektleitung FIDP-Projekt
**Von:** _Name_
**Datum:** _YYYY-MM-DD_
**Betreff:** Empfehlung zur Annotation der restlichen ~60 Stellenanzeigen — Mensch, Frontier-LLM oder Hybrid

---

## 1. Empfehlung

_In ein bis zwei Sätzen: Frontier macht alle / Mensch macht alle / Hybrid mit konkreter Aufteilung._
_Beispiel-Form: „Ich empfehle einen Hybrid: Felder X und Y vom Frontier-LLM, Felder Z weiterhin manuell bzw. mit Stichprobenkontrolle."_

## 2. Begründung an meinem κ

Datengrundlage: Mensch ↔ Frontier auf n = 12 Anzeigen (`claude-…` / `gpt-…`, Datum _…_).

| Feld | κ | Übereinstimmung | Vertraue ich dem Frontier? |
|---|---|---|---|
| homeoffice | _…_ | _…/12_ | _ja/nein_ |
| vertragsart | _…_ | _…/12_ | _ja/nein_ |
| erfahrungslevel | _…_ | _…/12_ | _ja/nein_ |
| gehalt_min_eur | _…_ | _…/12_ | _ja/nein_ |
| gehalt_zeitraum | _…_ | _…/12_ | _ja/nein_ |
| skills_top3 | _…_ | _…/12_ | _ja/nein_ |

_Auf welchen Feldern ist die Übereinstimmung hoch genug, um die Annotation abzugeben? Auf welchen nicht — und warum (mit Bezug auf einen konkreten Disagreement-Fall aus dem Notebook)?_

## 3. Schwellwert-Logik

_Wo liegt meine Grenze? Beispiel: „Ab κ ≥ 0,6 (substanziell) auf einem Feld lasse ich es vom Frontier annotieren, darunter manuell."_
_Bei n = 12 entspricht eine einzelne abweichende Anzeige ca. 8 Prozentpunkten — wie gehe ich mit dieser Unsicherheit um? Was müsste anders sein (größere Stichprobe, besserer Prompt, geschärftes Schema), damit ich anders entscheide?_
_Hinweis dokumentieren: 3 der 12 Anzeigen waren Few-Shot-Beispiele im Prompt — auf diesen ist die Übereinstimmung leicht überschätzt._

## 4. Risiko-Sicherung

_Wenn ich falsch liege: was geht schief (z. B. systematisch falsches erfahrungslevel verzerrt spätere Auswertungen)?_
_Welche Stichprobenkontrolle baue ich ein? Beispiel: „Von den 60 Frontier-annotierten Anzeigen prüfe ich 10 zufällige manuell nach; ab > 2 Fehlern annotiere ich das betroffene Feld komplett neu."_
