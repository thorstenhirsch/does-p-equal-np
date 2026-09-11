# Sondierungsnotizen der Projektleitung (vor der Tiefenphase)

Stand: 2026-09-11. Methode: ausschließlich WebSearch (siehe Einschränkung unten).

## Methodische Einschränkung dieser Umgebung (WICHTIG, muss ins Abschlusspapier)

WebFetch ist durch den Egress-Proxy für praktisch alle externen Domains gesperrt.
Getestet und blockiert: `arxiv.org`, `ar5iv.labs.arxiv.org`, `eccc.weizmann.ac.il`,
`link.springer.com`, `blog.computationalcomplexity.org`, `en.wikipedia.org`.
Nutzbar ist nur **WebSearch** (Titel, URLs, synthetisierte Trefferzusammenfassung).

**Folge:** Wir können keine Volltexte lesen und keine Beweisdetails nachrechnen.
Alle Aussagen beruhen auf Suchergebnis-Synthesen und müssen trianguliert werden.
Markierung `[NUR-SNIPPET]` für alles, was nicht am Volltext verifiziert wurde.

## Vorbefunde

### 1. Kein Durchbruch bei P vs. NP
- Keine Quelle deutet auf eine Lösung hin. Lance Fortnows Blog (Post "Respect the
  P v NP Problem", 10.06.2026) adressiert explizit die wiederkehrende Behauptung,
  ein KI-Beweis stehe kurz bevor, und verneint sie. [NUR-SNIPPET]
- Gasarch-Umfragen: 2002 / 2012 / 2019 (dritte Umfrage, 124 Teilnehmer).
  ACHTUNG: Die Suchsynthese liefert widersprüchliche Zahlen (80 % vs. 66 % für P≠NP).
  Muss vom Meta-Agenten verifiziert werden — gutes Beispiel für Triangulationsbedarf.

### 2. Obere Schranken: weiterhin exponentiell
- Bestes bekanntes worst-case randomisiertes Zeitlimit für allgemeines 3-SAT:
  O*(1.307031578^n), über verbesserte PPSZ-Analyse (Scheder 2024; weitere
  Verbesserung der Analyse Juli 2026, arXiv:2607.10697). [NUR-SNIPPET]
- Das ist eine Verbesserung in der *Konstante des Exponenten*, nicht ansatzweise
  ein Schritt Richtung Polynomialzeit. Wichtiges Argument gegen die Intuition,
  "stetiger Fortschritt" führe irgendwann zu P = NP.

### 3. KI-Ansätze: reale Erfolge — aber nicht bei P vs. NP
- **AlphaEvolve** (DeepMind, arXiv:2506.13131): evolutionäre LLM-gestützte Code-Suche.
  Verifizierte Ergebnisse: 4x4-Matrixmultiplikation über C mit 48 statt 49
  skalaren Multiplikationen (erste Verbesserung über Strassen hinaus seit 56 Jahren);
  neue untere Schranke beim Kissing-Number-Problem in Dimension 11 (593).
  "Mathematical exploration and discovery at scale" (arXiv:2511.02864, mit Terence Tao):
  67 Probleme, in ca. 20 % der Fälle Verbesserung der besten bekannten Lösung.
- Es existiert ein Google-Research-Beitrag "AI as a research partner: Advancing
  theoretical computer science with AlphaEvolve" — direkt einschlägig, muss vertieft werden.
- **Automatisches Beweisen 2026**: AxiomProver (4 zuvor ungelöste Probleme, Anfang 2026);
  AlphaProof Nexus + 9 Erdős-Probleme (Mai 2026); OpenAI, 10 forschungsnahe Probleme
  (August 2026); Anthropic, Resultate zur Riemannschen Zetafunktion mit Lean-Formalisierung
  (August 2026) und Lean-Formalisierung von Fermats letztem Satz (September 2026). [NUR-SNIPPET]
- **Neuronale SAT-Solver** (NeuroSAT-Linie): skalieren nicht; nicht konkurrenzfähig
  mit CDCL-Solvern auf realen Instanzen; Forschung verlagert sich auf ML-Komponenten
  *innerhalb* klassischer Solver (Heuristiken), nicht auf End-to-End-Lösung.
- **CUSP-Benchmark** (arXiv:2605.22681): Frontier-Modelle haben substanzielle
  retrospektive wissenschaftliche Kompetenz, aber schwache prospektive Prognosefähigkeit.

### 4. Claims, die auditiert werden müssen
- **"SAT requires exhaustive search"** (Ke Xu, Guangyan Zhou), Frontiers of Computer
  Science 19(12), Dezember 2025. Behauptet P ≠ NP. Kritik: publizierter "Comment"
  (DOI 10.1007/s11704-025-53000-5); arXiv:2312.02071 "Evaluating the Claims of ...";
  arXiv:2401.01193 "Further Explanations on ...". Zentraler Punkt: Das Resultat gilt
  offenbar **nicht** für k-SAT mit konstanter Klausellänge (also nicht für 3-SAT),
  sondern nur für Instanzen mit langen Klauseln; Kritiker bemängeln eine unzulässige
  Annahme über *alle* möglichen SAT-Algorithmen. MUSS präzise geklärt werden.
- **arXiv:2309.05689** "Large Language Model for Science: A Study on P vs. NP"
  (Microsoft Research): GPT-4 "Socratic reasoning", 97 Dialogrunden, Schluss "P ≠ NP".
  Status als Beweis: zu prüfen (Erwartung: Methodendemonstration, kein Beweis).
- Zenodo-Sammlung "AI Principle Series and P≠NP Structural Proof" (2025) — Beispiel
  für die KI-verstärkte Crank-Literatur, die der Auditor einordnen soll.

### 5. Aktive, seriöse Forschungsfronten (2024–2026)
- **Meta-Komplexität / MCSP**: "SAT Reduces to the Minimum Circuit Size Problem"
  (ECCC 2023/165, Revision März 2025) — stärkster Hinweis bisher auf NP-Vollständigkeit
  von MCSP; Stichwort "hardness magnification".
- **Ryan Williams (Februar 2025)**: Jede Mehrband-Turingmaschine mit Zeit t ist in
  Raum O(√(t log t)) simulierbar — erste substanzielle Verbesserung seit ~50 Jahren.
  Kein P-vs-NP-Resultat, aber Beleg, dass in angrenzenden Fragen echte Bewegung ist.
- **Fine-grained complexity of NP-complete problems** (arXiv:2601.05044, Übersicht 2026).
- **Geometric Complexity Theory**: Mulmuley selbst veranschlagt ~100 Jahre; die
  Positivitätshypothesen gelten als "formidable".
