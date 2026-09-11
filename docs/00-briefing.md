# Gemeinsames Briefing für alle Agenten

**Projekt:** Systematische, wissenschaftlich redliche Bestandsaufnahme zur Frage P vs. NP,
mit besonderem Fokus auf die Frage, ob KI-Methoden (LLMs, evolutionäre Code-Suche,
automatisches Beweisen, gelernte Heuristiken) hier substanziellen Fortschritt gebracht haben.

**Stand:** September 2026.

## Grundregeln für alle Agenten (verbindlich)

1. **Keine Lösung behaupten.** P vs. NP ist offen. Wer etwas anderes behauptet, muss
   einen nachprüfbaren, peer-reviewten Beleg liefern. Andernfalls: als Claim markieren,
   nicht als Fakt.
2. **Belegpflicht.** Jede nichttriviale Aussage bekommt eine Quelle (URL + Jahr + Venue).
   Unterscheide sauber:
   - `[VERIFIZIERT]` — peer-reviewt oder von der Community breit akzeptiert
   - `[PREPRINT]` — arXiv/ECCC, nicht begutachtet
   - `[CLAIM]` — Behauptung ohne Verifikation, ggf. widerlegt
   - `[EIGENE EINSCHÄTZUNG]` — deine Analyse, kein Literaturbefund
3. **Konfidenz angeben** (hoch/mittel/niedrig) bei allen Bewertungen.
4. **Negative Ergebnisse sind wertvoll.** "Es gibt hier nichts" ist ein legitimes,
   wichtiges Resultat. Nicht schönfärben, nicht auffüllen.
5. **Crank-Alarm.** Das Feld zieht massenhaft fehlerhafte Beweisversuche an
   (>120 dokumentierte). Bei jedem Claim: wer hat ihn geprüft? Gibt es eine
   veröffentlichte Widerlegung?
6. **Sprache:** Bericht auf Deutsch, Fachbegriffe auf Englisch belassen
   (z.B. "natural proofs barrier", nicht "Barriere natürlicher Beweise").

## Kanonischer Kontext (bekannt, nicht neu recherchieren)

- P vs. NP, gestellt 1971 (Cook) / 1973 (Levin), Clay Millennium Problem seit 2000.
- Cook-Levin: SAT ist NP-vollständig. Karp 1972: 21 Probleme. Ein polynomieller
  Algorithmus für *irgendein* NP-vollständiges Problem (3-SAT, Clique, Knapsack*,
  Vertex Cover, ...) ⇒ P = NP.
  *Achtung: Knapsack hat einen pseudopolynomiellen DP-Algorithmus (O(nW)); das ist
  KEIN Polynomialzeit-Algorithmus, weil W exponentiell in der Eingabelänge sein kann.
  Diese Verwechslung ist ein klassischer Fehler und muss im Papier explizit adressiert werden.
- Drei bewiesene Barrieren gegen bekannte Beweistechniken:
  1. Relativization (Baker–Gill–Solovay 1975)
  2. Natural Proofs (Razborov–Rudich 1994/97)
  3. Algebrization (Aaronson–Wigderson 2008)
- Umfragen (Gasarch 2002/2012/2019): große Mehrheit der Fachleute erwartet P ≠ NP.

## Ablagestruktur

Jeder Agent schreibt seinen Bericht nach `research/<agent-id>-<thema>.md`
und gibt zusätzlich eine kompakte Zusammenfassung (400–600 Wörter) zurück.
