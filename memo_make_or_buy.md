# Make-or-Buy-Memo — Annotation der restlichen Anzeigen

**An:** Projektleitung FIDP-Projekt
**Von:** Lara Marx
**Datum:** 2026-06-04
**Betreff:** Empfehlung zur Annotation der restlichen ~60 Stellenanzeigen — Mensch, Frontier-LLM oder Hybrid

---

## 1. Empfehlung

Ich empfehle einen **Hybrid-Ansatz** statt „alles Mensch" oder „alles Frontier". Konkret: `vertragsart` und `erfahrungslevel` lasse ich vom Frontier-LLM (GPT 5.5) annotieren, `erfahrungslevel` zusätzlich mit Stichprobenkontrolle. `homeoffice` und `skills_top3` werden **nicht** ungeprüft übernommen, und die Gehaltsfelder prüfe ich bei jeder konkreten Gehaltsnennung manuell nach. So nutze ich die Stärke des Frontiers auf den robusten Feldern, ohne die schwachen Felder blind zu vertrauen.

## 2. Begründung anhand κ

Datengrundlage: Mensch ↔ Frontier auf n = 12 gemeinsamen Anzeigen. Für die kategorialen Felder nutze ich Cohen's κ aus `validate.py`; bei `gehalt_*` und `skills_top3` ist κ wenig aussagekräftig, daher dort nur die Übereinstimmung als Ergänzung.

| Feld | κ (validate.py) | Übereinstimmung | Bewertung |
|---|---|---|---|
| vertragsart | 1.000 | 100 % | fast perfekt → übernehmbar |
| erfahrungslevel | 0.864 | 92 % | fast perfekt → übernehmbar, mit Kontrolle |
| homeoffice | 0.464 | 58 % | moderat → nicht blind übernehmen |
| gehalt_min_eur | — (Accuracy 0.917) | 11/12 | nur 1 positiver Fall → κ nicht aussagekräftig |
| gehalt_zeitraum | — (Accuracy 0.917) | 11/12 | wie oben |
| skills_top3 | — (Übereinst. 2/12) | 17 % | sehr niedrig → nur geprüft übernehmen |

**`vertragsart`** ist mit κ = 1.000 das verlässlichste Feld: In allen 12 Anzeigen stimmen Frontier und ich überein. Hier vertraue ich dem Frontier.

**`erfahrungslevel`** liegt mit κ = 0.864 ebenfalls im „fast perfekten" Bereich. Die einzige Abweichung war `12826` (Fachinformatiker Systemintegration): ich `junior`, Frontier `mid`. Das war kein Modellfehler, sondern ein echter Grenzfall — die Anzeige bietet einen Ausbildungs-Einstieg an und verlangt zugleich „Support-Erfahrung". Solche Fälle gibt es selten, deshalb übernehme ich das Feld, aber mit Stichprobe.

**`homeoffice`** ist mit κ = 0.464 (moderat) das eigentliche Problemfeld. Alle fünf Abweichungen sind dasselbe Muster: ich `ja`, Frontier `teilweise`. An `15939-BB-633097` sieht man warum — „Mobiles Arbeiten" steht nur als Benefit-Stichpunkt, und das Schema ordnet „mobiles Arbeiten" eigentlich `teilweise` zu. Das ist eine wiederkehrende Auslegungsfrage zwischen `ja` und `teilweise`, kein Zufallsfehler. Dieses Feld übernehme ich nicht blind.

**Gehaltsfelder:** κ = 0.000 ist hier irreführend, weil mein Gold 11× `null` ist und es nur einen positiven Fall gibt (`12862`, „Jahresgehalt von 45.000 bis 50.000 Euro"). Ausgerechnet dort lag das Frontier richtig und ich falsch — ich hatte das Gehalt übersehen. κ misst das nicht; deshalb bewerte ich diese Felder über die konkreten Gehaltsnennungen, nicht über κ.

**`skills_top3`** hat mit nur 2/12 exakten Treffern die niedrigste Übereinstimmung. Oft liegen Frontier und ich inhaltlich nah beieinander, wählen aber andere oder anders geschriebene Begriffe. Ohne festes Skill-Vokabular ist das Feld für eine ungeprüfte Übernahme zu instabil.

## 3. Schwellwert-Logik

Meine Grenze ziehe ich am κ der kategorialen Felder:

- **κ ≥ 0.8** (fast perfekt) → direkt nutzbar: `vertragsart`, `erfahrungslevel`.
- **κ 0.6 – 0.8** (substanziell) → nur mit Stichprobenkontrolle übernehmen. (Aktuell fällt kein Feld genau hierher; die Schwelle gilt für künftige Läufe.)
- **κ < 0.6** → nicht übernehmen, sondern manuell annotieren oder Schema/Prompt überarbeiten: `homeoffice`.
- Felder, bei denen κ wegen weniger positiver Fälle nicht trägt (`gehalt_*`), werden **fallweise** bewertet: bei jeder konkreten Gehaltsnennung manuell prüfen.

Bei `erfahrungslevel` setze ich die Schwelle bewusst nicht starr: κ = 0.864 ist gut, aber bei n = 12 entspricht **eine** abweichende Anzeige schon ~8 Prozentpunkten. Anders würde ich entscheiden, wenn die Stichprobe deutlich größer wäre oder das Schema die `junior`/`mid`-Grenze schärfer definieren würde. Dann könnte ich `erfahrungslevel` auch ohne laufende Kontrolle übernehmen.

## 4. Risiko-Sicherung

Wenn ich falsch liege, verzerren systematisch falsche Felder (z. B. durchgehend `ja` statt `teilweise` bei homeoffice, oder ein übersehenes Gehalt) spätere Auswertungen, ohne dass es jemandem auffällt. Dagegen baue ich eine Stichprobenkontrolle ein:

- Von den restlichen 60+ Frontier-annotierten Anzeigen prüfe ich **10 zufällig gezogene** manuell nach.
- Finde ich in einem Feld **mehr als 2 Fehler**, annotiere ich dieses Feld für den ganzen Rest neu (bzw. überarbeite Prompt/Schema und lasse das Feld erneut laufen).
- `homeoffice` und `skills_top3` gehen ohnehin in die manuelle Nachkontrolle; bei den Gehaltsfeldern prüfe ich jede Anzeige mit konkreter Geldangabe.

## Einordnung der Zahlen

- **3 der 12 Anzeigen waren Few-Shot-Beispiele** im Frontier-Prompt (`15939-BB-632493-7878-9058-S`, `13509-00002110865001-S`, `13635-7fbe73ac_JB5131141-S`). Auf diesen ist die Übereinstimmung leicht überschätzt, weil das Frontier dort meine Sollwerte gesehen hat. Die echten κ-Werte liegen daher tendenziell etwas niedriger als gemessen.
- **n = 12 ist klein.** Eine einzelne abweichende Anzeige entspricht ~8 Prozentpunkten, und κ reagiert bei schiefen Verteilungen (wie den fast nur `null`-Gehältern) empfindlich. Die Empfehlung ist deshalb als begründete Tendenz zu lesen, nicht als statistisch harter Beleg. Für eine belastbarere Entscheidung würde ich vor dem Vollausrollen eine größere Vergleichsstichprobe annotieren.
