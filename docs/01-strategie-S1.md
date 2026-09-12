# 01 — Forschungsstrategie

**Agent:** S1 „Forschungsstratege"
**Stand:** 11. September 2026
**Geltung:** Die Grundregeln aus `docs/00-briefing.md` (Belegpflicht, Konfidenzangaben,
Markierungen, keine Lösungsbehauptungen, Crank-Alarm) sind für dieses Dokument und für
alle daraus abgeleiteten Aufträge verbindlich.

---

## 0. Methodische Vorbemerkung zur Quellenlage (bitte zuerst lesen)

**Diese Recherche konnte keine einzige Quelle im Volltext lesen.**

Der Egress-Proxy dieser Umgebung blockiert WebFetch für praktisch alle relevanten
Domains — getestet und bestätigt für `arxiv.org`, `eccc.weizmann.ac.il`,
`acm-stoc.org`, `conference-publishing.com`, `blog.computationalcomplexity.org`,
`scottaaronson.blog`. Verfügbar war ausschließlich **WebSearch**, das Titel, URLs und
eine *modellgenerierte Zusammenfassung* der Trefferinhalte liefert.

Daraus folgt eine zusätzliche Markierung, die ich ab hier verwende und die ich für das
gesamte Projekt vorschlage:

> **`[NUR-SNIPPET]`** — Die Aussage stammt aus einer Suchergebnis-Zusammenfassung,
> nicht aus dem Volltext. Titel, Autoren, Venue und Jahr sind plausibel und
> mehrfach getroffen, aber inhaltliche Details (Schrankenkonstanten, genaue
> Voraussetzungen, Theoremnummern) sind **nicht am Original verifiziert**.

Suchergebnis-Zusammenfassungen sind selbst LLM-Ausgaben und können halluzinieren.
Ich habe das im Verlauf dieser Recherche konkret beobachtet: Zur Gasarch-Umfrage 2019
lieferte *ein und dieselbe* Antwort zwei unvereinbare Zahlen („66 %" und „ca. 80 %"
für P ≠ NP) — ein Lehrstück dafür, warum Triangulation nicht optional ist
(siehe §D.2 und Auftrag an S9).

**Konsequenz für das Projekt:** Ohne Volltextzugang ist Ebene 3 (echter inhaltlicher
Fortschritt, §B.3) faktisch ausgeschlossen und Ebene 2 nur eingeschränkt erreichbar.
Das ist kein Detail, sondern ein strategischer Befund. Er gehört offen ins
Konsenspapier. Falls Volltextzugang (arXiv/ECCC/DBLP) beschafft werden kann, sollte
das die **Priorität Nr. 1** vor jeder inhaltlichen Tiefenrecherche sein.

**Zusätzlicher Befund zur Quellenlage:** Die Suche wirft mehrfach Arbeiten mit
arXiv-IDs aus dem Zeitraum Ende 2025 bis Mitte 2026 aus, die inhaltlich stark nach
Crank-Material klingen — etwa „Toward P vs NP: An Observer-Theoretic Separation via
SPDP Rank and a ZFC-Equivalent Foundation within the N-Frame Model"
(arXiv 2512.11820). Solche Treffer stehen in denselben Ergebnislisten wie seriöse
Arbeiten. Ein Agent ohne Auditierungsdisziplin wird sie mit gleicher Autorität
zitieren. Das ist die zentrale operative Gefahr dieses Projekts.

---

# Teil A — Problemzerlegung

## A.0 Rahmung: Was heißt hier „bearbeiten"?

Die Frage „P = NP oder nicht?" ist für uns **nicht** die Aufgabe. Die Aufgabe zerfällt
in drei Schichten, und nur die unteren beiden sind für uns überhaupt adressierbar:

| Schicht | Frage | Für uns? |
|---|---|---|
| **L1 — Kartographie** | Was ist der belegbare Stand? Welche Ansätze existieren, welche sind tot, welche Barrieren gelten? | **Ja**, vollständig |
| **L2 — Meta-Analyse** | Warum ist das Problem hart? Welche Ansätze haben 2020–2026 tatsächlich Boden gewonnen? Wo sind KI-Methoden real angekommen und wo nur rhetorisch? | **Ja**, mit Sorgfalt |
| **L3 — Beitrag** | Neue Theoreme, neue Techniken, Barriereumgehung | **Nein**, außer in einem eng definierten Nischensinn (§B.2, §B.3) |

Der wertvollste Beitrag, den ein Team wie unseres 2026 leisten kann, liegt in **L2** —
und zwar an genau der Stelle, an der das Feld gerade am verwundbarsten ist: der
Beurteilung von KI-Behauptungen über mathematischen Fortschritt.

Der maßgebliche Rahmen bleibt: P vs. NP ist offen, und das Feld hat **keinen
tragfähigen Angriffsplan**. Lance Fortnow formuliert das im Juni 2026 explizit —
„we don't even have a viable approach to settling the P v NP problem" — und rät im
selben Atemzug von formalen Lean-Ansätzen ab. `[VERIFIZIERT als Blogaussage; NUR-SNIPPET]`
(Konfidenz: hoch, dass dies seine Position ist; hoch, dass sie die Mehrheitsposition
des Feldes abbildet.)
Quelle: https://blog.computationalcomplexity.org/2026/06/respect-p-v-np-problem.html

---

## A.1 Die Angriffsrichtungen im Einzelnen

### A.1.1 Untere Schranken / Schaltkreiskomplexität

**Was:** Zeige, dass ein NP-vollständiges Problem keine polynomiellen Schaltkreise hat.
Der direkteste Weg zu P ≠ NP (genauer: zu NP ⊄ P/poly, was stärker ist).

**Stand (Auswahl, belegt):**
- Beste bekannte untere Schranke für *allgemeine* Boolesche Schaltkreise einer expliziten
  Funktion: ca. **3,1n − o(n)**. Historie: Blum 3n − o(n) (1984) → Find–Golovnev–Hirsch–Kulikov
  (3 + 1/86)n (FOCS 2016) → 3,1n − o(n) (STOC 2022). `[VERIFIZIERT] [NUR-SNIPPET]`
  Konfidenz hoch.
  https://dl.acm.org/doi/abs/10.1145/3519935.3519976 · https://ieeexplore.ieee.org/document/7782921/
- **Das ist der Kern des Problems:** Um P ≠ NP zu zeigen, bräuchte man *superpolynomielle*
  Schranken. Wir stehen bei einem konstanten Faktor mal n. Der Abstand ist nicht
  quantitativ, sondern kategorisch. `[EIGENE EINSCHÄTZUNG]` Konfidenz: hoch.
- Fortschritt findet in **eingeschränkten Modellen** statt: monotone Schaltkreise
  (Rao 2026: exp(Ω̃(n^{1/2})) Gatter für Perfect Matching, ECCC TR26-129)
  `[PREPRINT] [NUR-SNIPPET]`, ACC⁰, Schwellwertschaltkreise, sowie
  „near-maximum" Schranken für hohe Klassen (Ren–Williams, ECCC TR26-118, Juli 2026)
  `[PREPRINT] [NUR-SNIPPET]`.
  https://eccc.weizmann.ac.il/report/2026/129/ · https://eccc.weizmann.ac.il/report/2026/118/

**Barrieren:** Alle drei bewiesenen Barrieren (Relativization, Natural Proofs,
Algebrization) treffen genau hier. Zusätzlich die **Locality Barrier** für Hardness
Magnification (Chen–Hirahara–Oliveira–Pich–Rajgopal–Santhanam, ITCS 2020 / JACM 2022)
`[VERIFIZIERT]`: https://dl.acm.org/doi/10.1145/3538391

**Bewertung:**
- *Theoretischer Aussichtsreichtum:* **hoch** — das ist der Königsweg, wenn er je gelingt.
- *Zugänglichkeit für uns:* **sehr niedrig** für eigenen Beitrag, **hoch** für Kartographie.
- *Teilerfolg konkret:* Eine präzise, aktuelle Landkarte „Welche Schranke gilt für welches
  Modell, seit wann, und welche Barriere blockiert die nächste Stufe" — inklusive einer
  ehrlichen Darstellung, warum 3,1n und superpolynomiell nicht auf derselben Skala liegen.

---

### A.1.2 Obere Schranken / Algorithmik

**Was:** Finde einen Polynomialzeit-Algorithmus für ein NP-vollständiges Problem
(⇒ P = NP), oder grenze ein, warum das nicht geht.

**Stand:** Kein ernstzunehmender Kandidat. Das Feld hat sich stattdessen in
**Fine-Grained Complexity** verlagert: bedingte untere Schranken unter SETH, OV-Hypothese,
3SUM. Williams' ETHR∘ETHR-Schranke unter der Orthogonal-Vectors-Vermutung (FOCS 2024)
ist ein Beispiel. `[NUR-SNIPPET]` Konfidenz: mittel.

**Wichtig für das Papier (Briefing §Kanonischer Kontext):** Praktische SAT-Solver lösen
Instanzen mit Millionen Variablen. Das ist **kein** Hinweis auf P = NP. Ebenso ist
Knapsacks pseudopolynomieller DP-Algorithmus O(nW) kein Polynomialzeit-Algorithmus.
Diese beiden Verwechslungen sind die häufigsten Laienfehler und müssen explizit
adressiert werden.

**Bewertung:**
- *Aussichtsreichtum:* **sehr niedrig** für P = NP; **mittel** für Fine-Grained-Erkenntnisse.
- *Zugänglichkeit:* **mittel** — die Literatur ist zugänglich, die Landschaft überschaubar.
- *Teilerfolg:* Saubere Darstellung der bedingten Schrankenlandschaft und der
  Impagliazzo'schen „Five Worlds" als Rahmen dafür, *welche* Welt die empirische Evidenz
  stützt. https://www.quantamagazine.org/the-researcher-who-explores-computation-by-conjuring-new-worlds-20240327/

---

### A.1.3 Geometric Complexity Theory (GCT)

**Was:** Mulmuley–Sohoni-Programm: Separiere algebraische Komplexitätsklassen
(VP vs. VNP, Determinante vs. Permanente) über Darstellungstheorie und algebraische
Geometrie — Multiplizitäten in Koordinatenringen von Gruppenvarietäten.

**Stand:** Der ursprüngliche Hauptplan ist **widerlegt**. Die Vermutung, dass
*occurrence obstructions* zur Separation ausreichen, wurde 2016 durch Ikenmeyer–Panova
(Advances in Mathematics) und Bürgisser–Ikenmeyer–Panova (JAMS) disproved.
`[VERIFIZIERT]` Konfidenz: hoch. https://arxiv.org/pdf/1604.06431
Das Programm lebt weiter (multiplicity obstructions sind stärker als occurrence
obstructions — SIAM J. Appl. Algebra Geom.), ist aber deutlich zurückgesetzt.
Aktuelle Aktivität: Ikenmeyer–Panova, Forum of Mathematics Pi 2024;
Bläser–Ikenmeyer, „Introduction to Geometric Complexity Theory", Theory of Computing
Graduate Surveys 2025. `[NUR-SNIPPET]` Konfidenz: mittel.
https://epubs.siam.org/doi/10.1137/19M1287638 · https://toc.cs.uchicago.edu/articles/gs010/bibliography.html

**Bewertung:**
- *Aussichtsreichtum:* **niedrig bis mittel**, Zeithorizont Jahrzehnte. Mulmuley selbst
  hat das stets so dargestellt.
- *Zugänglichkeit für uns:* **sehr niedrig**. GCT verlangt Darstellungstheorie
  algebraischer Gruppen, invariante Theorie, Schubert-Kalkül. Ein KI-Team kann hier
  referieren, nicht beurteilen. **Das ist eine ehrliche Grenze, keine Bescheidenheitsfloskel.**
- *Teilerfolg:* Eine nüchterne Darstellung „Was hat GCT versprochen, was ist davon
  widerlegt, was ist geblieben" — inklusive der Korrektur des populären Missverständnisses,
  GCT sei „der Weg zu P vs. NP".

---

### A.1.4 Proof Complexity

**Was:** Untere Schranken für Beweissysteme (Resolution, Frege, Extended Frege, IPS).
Verbindung: Superpolynomielle Extended-Frege-Schranken für *alle* Tautologiefamilien
würden NP ≠ coNP implizieren ⇒ P ≠ NP.

**Stand:** Lebendiges, produktives Feld — aber weit vom Ziel.
- Für **AC⁰[p]-Frege** sind untere Schranken ein langjähriges offenes Problem. `[VERIFIZIERT]`
- 2025/26 gibt es Fortschritt auf der *Meta*-Ebene: „AC⁰[p]-Frege Cannot Efficiently Prove
  that Constant-Depth Algebraic Circuit Lower Bounds are Hard" (ITCS 2026, arXiv 2509.16824)
  `[PREPRINT, ITCS-2026-akzeptiert] [NUR-SNIPPET]`.
  https://arxiv.org/abs/2509.16824 · https://drops.dagstuhl.de/entities/document/10.4230/LIPIcs.ITCS.2026.99
- Schranken gegen IPS-Fragmente über konstant-großen endlichen Körpern (löst ein offenes
  Problem von Forbes–Shpilka–Tzameret–Wigderson) `[NUR-SNIPPET]`.
- STOC 2026: „Lower Bounds for Near-Quadratic-Depth Resolution over Parities"
  (Bhattacharya, Byramji, Chattopadhyay, Impagliazzo) `[NUR-SNIPPET]`. Konfidenz: mittel.
- Pich, „Towards P ≠ NP from Extended Frege lower bounds" (arXiv 2312.08163) — die
  explizite Brücke. `[PREPRINT]` https://arxiv.org/pdf/2312.08163

**Bewertung:**
- *Aussichtsreichtum:* **mittel** — strukturell eine der ehrlicheren Brücken zu P ≠ NP.
- *Zugänglichkeit:* **mittel**. Die Definitionen sind kombinatorisch-logisch und für
  ein diszipliniertes Team nachvollziehbar. Besser zugänglich als GCT.
- *Teilerfolg:* Eine präzise Implikationskette „Welche Beweissystem-Schranke impliziert
  welche Komplexitätsaussage" — als nachprüfbares Diagramm mit Quellen pro Kante.
  Das existiert verstreut, aber selten kompakt und aktuell.

---

### A.1.5 Meta-Komplexität / MCSP

**Was:** Die Komplexität der Frage „wie komplex ist dieses Objekt?" — MCSP (Minimum
Circuit Size Problem), Kolmogorov-Komplexitätsvarianten (MKTP, GapMINKT). Seit ca. 2018
das **dynamischste Teilfeld** der Komplexitätstheorie.

**Stand:**
- Hirahara, „NP-Hardness of Approximating Meta-Complexity" (STOC 2023) — NP-Härte für
  Approximation mit nahezu optimalen Lücken, via kryptographische Konstruktionen in
  Reduktionen. `[VERIFIZIERT]` https://dl.acm.org/doi/abs/10.1145/3564246.3585154
- Ob **MCSP selbst NP-vollständig** ist: weiterhin offen. Das ist eines der schärfsten
  offenen Teilprobleme des Feldes. `[VERIFIZIERT]` Konfidenz: hoch.
- 2026: bedingte NP-Härte für ImpMCSP und improper PAC-Learning unter iO-Annahmen
  (ECCC TR26-091) `[PREPRINT] [NUR-SNIPPET]`. https://eccc.weizmann.ac.il/report/2026/091/
- Meta-Komplexität berührt die Natural-Proofs-Barriere direkt: Razborov–Rudich lässt sich
  als Aussage *über* Meta-Komplexität lesen. Ren–Santhanam, „A Relativization Perspective
  on Meta-Complexity" (STACS 2022). `[VERIFIZIERT]`
  https://drops.dagstuhl.de/entities/document/10.4230/LIPIcs.STACS.2022.54

**Bewertung:**
- *Aussichtsreichtum:* **hoch (relativ)** — hier bewegt sich 2024–2026 real am meisten,
  und es ist der Ansatz mit der plausibelsten Barriereumgehung.
- *Zugänglichkeit:* **mittel bis gut**. Die Fragestellungen sind formulierbar, die
  Literatur ist konzentriert (Hirahara, Santhanam, Oliveira, Ren, Pich).
- *Teilerfolg:* **Dies ist die beste Zielrichtung für unser Team.** Konkret: eine
  aktuelle Karte der Meta-Komplexitäts-Implikationen inkl. der Frage, welche Ergebnisse
  unbedingt und welche nur unter kryptographischen Annahmen gelten. Letzteres wird in
  Sekundärquellen systematisch verwischt.

---

### A.1.6 Algebraische Methoden (arithmetische Schaltkreise, VP vs. VNP)

**Was:** Das algebraische Analogon von P vs. NP. Gilt als „einfacher" — und ist trotzdem offen.

**Stand:** Der bedeutendste Durchbruch der letzten Jahre:
Limaye–Srinivasan–Tavenas, erste superpolynomielle untere Schranken gegen *allgemeine*
arithmetische Schaltkreise **jeder konstanten Tiefe** über Körpern der Charakteristik 0
(FOCS 2021, JACM 72(4) 2025, CACM Research Highlight). `[VERIFIZIERT]` Konfidenz: hoch.
https://dl.acm.org/doi/10.1145/3734215 · https://cacm.acm.org/research-highlights/superpolynomial-lower-bounds-against-low-depth-algebraic-circuits/
Erweiterung auf beliebige Körper: Forbes, CCC 2024. `[VERIFIZIERT] [NUR-SNIPPET]`
Folgearbeit auf Meta-Ebene: „Meta-Mathematics of Algebraic Complexity" (LICS 2026),
Formalisierung der Rangmethode in der bounded-arithmetic-Theorie VNC². `[NUR-SNIPPET]`

**Bewertung:**
- *Aussichtsreichtum:* **mittel-hoch** als Vorstufe. Aber: „konstante Tiefe" ist weit von
  „allgemein" entfernt, und selbst VP ≠ VNP impliziert P ≠ NP **nicht** direkt.
- *Zugänglichkeit:* **mittel**.
- *Teilerfolg:* Klare Darstellung, was LST tatsächlich zeigt und was nicht — dieser Punkt
  wird in populärer Berichterstattung regelmäßig überdehnt.

---

### A.1.7 Unabhängigkeit von ZFC

**Was:** Ist P vs. NP vielleicht unentscheidbar in ZFC?

**Stand:** Aaronson, „Is P Versus NP Formally Independent?" (2003) — der kanonische
Referenztext; Tenor: unwahrscheinlich, aber nicht ausgeschlossen, und die vorhandenen
Unabhängigkeitsargumente sind schwächer als ihr populärer Ruf.
`[VERIFIZIERT]` https://www.scottaaronson.com/papers/indep.pdf
Ernstzunehmend ist die *schwächere* Variante: Unbeweisbarkeit in **bounded arithmetic**.
Pich–Santhanam (STOC 2021): starke average-case-Schranken gegen co-nichtdeterministische
Schaltkreise sind in T_PV nicht beweisbar; weitere Ergebnisse für PV₁, APC₁, S¹₂.
`[VERIFIZIERT] [NUR-SNIPPET]` https://arxiv.org/pdf/2305.15235
Gasarch-Umfrage 2019: Der Anteil derer, die „wird nie gelöst" sagen, lag im niedrigen
einstelligen bis niedrigen zweistelligen Prozentbereich — **die Snippets widersprechen
sich hier, siehe §0**. `[NUR-SNIPPET, ungeklärt]`

**Bewertung:**
- *Aussichtsreichtum:* **niedrig** für ZFC-Unabhängigkeit; **mittel** für
  bounded-arithmetic-Unbeweisbarkeit (dort passiert real etwas).
- *Zugänglichkeit:* **niedrig** (Beweistheorie), aber die *Ergebnislage* ist referierbar.
- *Teilerfolg:* Trennung der beiden Fragen. Sie werden in Laiendiskussionen ständig
  vermischt, und zwar in beide Richtungen.

---

### A.1.8 Empirische / experimentelle Ansätze — und der KI-Komplex

Hier liegt der eigentliche Auftrag dieses Projekts (Briefing: „besonderer Fokus").
Ich teile in drei klar getrennte Stränge, weil sie unterschiedlich reif sind.

#### (a) KI für Algorithmenentdeckung

**AlphaEvolve** (DeepMind, Mai 2025): evolutionärer Coding-Agent auf Gemini-Basis. Das
prominenteste Resultat: 4×4-Matrixmultiplikation mit **48 skalaren Multiplikationen**,
erstmals unter Strassens 49 seit 1969. `[PREPRINT/Industrieblog]`
https://deepmind.google/blog/alphaevolve-a-gemini-powered-coding-agent-for-designing-advanced-algorithms/

**Die Einordnung ist entscheidend und wird fast überall unterschlagen:**
Das Verfahren arbeitet über Ringe mit Division durch 2 (komplexwertig), **nicht über
allgemeine Ringe**. Waksmans Algorithmus von 1970 schafft 4×4 mit **46** Multiplikationen
über kommutativen Ringen mit Division durch 2; Winograd 1967 schafft 48 über beliebigen
kommutativen Ringen. AlphaEvolves Beitrag ist neu **für nichtkommutative Ringe mit
Division durch 2** — eine echte, aber wesentlich engere Aussage als „56-Jahre-Rekord
gebrochen". `[NUR-SNIPPET]` Konfidenz: mittel-hoch (mehrfach getroffen, u. a. Mastodon-Analyse
von Robin Houston und eine Folgearbeit arXiv 2506.13242 „A non-commutative algorithm for
multiplying 4x4 matrices using 48 non-complex multiplications").
https://mathstodon.xyz/@robinhouston/114507937280899656 · https://arxiv.org/pdf/2506.13242

**Relevanz für P vs. NP: praktisch null.** Konstantenverbesserungen bei
Matrixmultiplikation verschieben keine Klassengrenzen. `[EIGENE EINSCHÄTZUNG]` Konfidenz: hoch.

**Gelernte SAT-Heuristiken:** GNN-gestütztes CDCL (NeuroBack, ICLR 2024: +5,2 % gelöste
Instanzen auf SATCOMP-2022), Backbone-/UNSAT-Core-Prädiktion, Polaritätssteuerung.
`[VERIFIZIERT] [NUR-SNIPPET]` https://openreview.net/forum?id=samyfu6G93
**Relevanz für P vs. NP: null.** Bessere Heuristiken auf Benchmark-Verteilungen sagen
nichts über Worst-Case-Komplexität. Dieser Punkt muss im Papier hart gemacht werden,
weil er die häufigste populäre Fehlschlussquelle ist.

#### (b) KI für automatisches Beweisen

**AlphaProof** (DeepMind): RL-Agent im Lean-Umfeld, AlphaZero-inspiriert, Curriculum aus
Millionen autoformalisierter Probleme. IMO 2024: AlphaProof + AlphaGeometry erreichten
28 Punkte (Silbermedaillenschwelle). Publiziert in **Nature 2025**. `[VERIFIZIERT]`
Konfidenz: hoch. https://www.nature.com/articles/s41586-025-09833-y

**AlphaProof Nexus** (DeepMind, Mai 2026): LLM + AlphaProof + evolutionäre Verfahren.
Berichtet: **9 von 353** angegangenen offenen Erdős-Problemen gelöst, **44 von 492**
OEIS-Vermutungen bewiesen, alle Lean-geprüft; Inferenzkosten je Problem im niedrigen
dreistelligen Dollarbereich. `[PREPRINT] [NUR-SNIPPET]` Konfidenz: mittel — mehrfach
getroffen, aber ausschließlich über Tech-Presse; arXiv-Preprint (angegeben als 2605.22763)
nicht verifizierbar.
https://the-decoder.com/google-deepminds-alphaproof-nexus-solves-decades-old-math-problems-for-a-few-hundred-dollars/

**Der Navier–Stokes-Vorfall (8. September 2026) — drei Tage vor diesem Bericht:**
OpenAI meldete, ein internes KI-System habe einen Beweis für Finite-Time-Blowup der
Navier–Stokes-Gleichungen erzeugt, samt Lean-Formalisierung; 166-seitige Arbeit.
`[CLAIM]` Konfidenz in die Meldung: hoch. Konfidenz in die mathematische Gültigkeit
**als Lösung des Millennium-Problems: niedrig.** Gründe, alle aus der Berichterstattung
trianguliert:
1. Es betrifft die **geforcte Variante**, die das Clay-Kriterium nach Fefferman
   **nicht** abdeckt; OpenAI hat erklärt, den Preis nicht zu beanspruchen.
2. Das Clay Institute betrachtet das Problem nicht als gelöst.
3. Prioritätsstreit: Buckmaster (NYU) und Alpöge werfen OpenAI vor, nach Kenntnis ihres
   nahezu fertigen Resultats „front-running" betrieben zu haben; Vorwurf des
   wissenschaftlichen Fehlverhaltens.
4. **Der methodisch wichtigste Punkt:** Lean verifiziert die *Ableitung*, nicht die
   *Aussage*. Passt die formalisierte Aussage nicht exakt zu Feffermans Spezifikation
   (Abklingbedingungen, Glattheit der Forcierung, Funktionenraum), kompiliert der
   Beweis trotzdem — und löst das Problem trotzdem nicht.
5. Tao (7.9.2026) lobt das zugrundeliegende menschliche Resultat und beschreibt die
   Lage als „very strange and unprecedented decoupling … between getting answers and
   getting understanding".
`[NUR-SNIPPET]` https://www.nature.com/articles/d41586-026-02842-5 ·
https://www.implicator.ai/clay-institute-navier-stokes-openai-proof-claim/ ·
https://xenospectrum.com/en/openai-navier-stokes-singularity-clay-dispute/

**Dieser Vorfall ist für unser Projekt wichtiger als jedes einzelne
Komplexitätsresultat.** Er liefert die Vorlage dafür, wie ein KI-Durchbruchsclaim
aussieht, wie schnell er zerfällt und an welchen Stellen — und er ist der Grund, warum
Punkt 4 in unsere Falsifikationskriterien (§B.3) gehört.

#### (c) Direkte KI-Angriffe auf P vs. NP

- Dong et al., „Large Language Model for Science: A Study on P vs. NP" (arXiv 2309.05689,
  Microsoft Research): GPT-4 „schließt" nach 97 Dialogschritten auf P ≠ NP.
  `[CLAIM — wertlos als mathematischer Beitrag]` Standardkritik: Die Schwierigkeit von
  P vs. NP liegt in der Quantifikation über *alle* Algorithmen, auch noch nicht gedachte;
  die Arbeit macht wie zahllose Crank-Versuche implizite Strukturannahmen.
  Konfidenz: hoch. https://arxiv.org/abs/2309.05689
- Khanukov, „I Spent a Year Asking AI to Solve Math's Hardest Problem"
  (CACM BLOG@CACM, Mai 2026): einjähriger orchestrierter LLM-Versuch an P vs. NP.
  Ergebnis: Erkenntnisse über *Orchestrierung*, kein mathematisches Resultat. Fortnow
  reagiert im Juni 2026 direkt darauf mit „Don't waste your time trying a formal
  approach via Lean". `[CLAIM / VERIFIZIERT als Vorgang]` Konfidenz: hoch.
  https://cacm.acm.org/blogcacm/i-spent-a-year-asking-ai-to-solve-maths-hardest-problem-heres-what-happened/
- **Gegenbeispiel in die andere Richtung:** Aaronson berichtet (Sept. 2025), dass
  KI-Reasoning-Modelle ihm beim Beweis von **Orakelseparationen zwischen
  Quantenkomplexitätsklassen** nützlich waren — deutlich besser als ein Jahr zuvor.
  `[NUR-SNIPPET]` Konfidenz: mittel. Das ist die realistische aktuelle Obergrenze:
  KI als produktiver Assistent bei *abgegrenzten, technischen* Teilschritten der
  Komplexitätstheorie — nicht als Urheber von Programmen.

**Bewertung des KI-Komplexes gesamt:**
- *Aussichtsreichtum für P vs. NP:* **sehr niedrig** direkt; **mittel** indirekt
  (Formalisierung, Literaturarbeit, technische Teilschritte).
- *Zugänglichkeit für uns:* **hoch** — das ist die einzige Richtung, in der unser Team
  strukturellen Vorteil hat: Wir können die Claims schneller und systematischer prüfen,
  als die Fachcommunity es tut.
- *Teilerfolg:* Ein belastbares, dokumentiertes **Register der KI-Mathematik-Claims
  2023–2026** mit Status, Prüfinstanz, Widerlegung/Bestätigung und Zeitachse. Ich halte
  das für das **wertvollste realistische Lieferobjekt dieses Projekts.**

---

## A.2 Bewertungsmatrix

Skala: 1 (sehr niedrig) bis 5 (sehr hoch). Alle Werte `[EIGENE EINSCHÄTZUNG]`,
Konfidenz mittel-hoch, abgeleitet aus §A.1.

| Richtung | Theoret. Aussichtsreichtum | Zugänglichkeit für uns | Erwarteter Projektwert |
|---|---|---|---|
| Meta-Komplexität / MCSP | 4 | 3–4 | **hoch** |
| KI-Claim-Auditierung | 1 (für P vs NP) | 5 | **sehr hoch** |
| Proof Complexity | 3 | 3 | hoch |
| Untere Schranken / Schaltkreise | 5 | 1–2 | mittel (nur Kartographie) |
| Algebraische Komplexität / VP–VNP | 3–4 | 3 | mittel |
| Obere Schranken / Fine-Grained | 2 | 3 | mittel |
| Unabhängigkeit (bounded arithmetic) | 2–3 | 2 | mittel |
| GCT | 2 | 1 | niedrig |
| Unabhängigkeit von ZFC | 1 | 2 | niedrig |
| KI für Algorithmenentdeckung | 1 | 4 | niedrig-mittel |

**Lesart:** Spalten 1 und 2 sind fast antikorreliert. Genau das ist die strategische
Kernaussage: *Wo wir etwas beitragen können, wird P vs. NP nicht entschieden — und
umgekehrt.* Die Aufgabe besteht darin, in Spalte 3 zu optimieren, nicht in Spalte 1.

---

## A.3 Was für dieses Team aussichtslos ist — explizit

Ehrlichkeit schlägt Vollständigkeit (Briefing §4). Folgendes sollten wir **gar nicht
erst versuchen**:

1. **Eine neue untere Schranke beweisen.** Weder superpolynomiell noch für ein
   eingeschränktes Modell. Die Techniken erfordern jahrelang eingeübtes Handwerk
   (Approximationsmethode, Switching Lemmas, Kommunikationskomplexität, Rangmethoden),
   und jeder naive Versuch läuft in Razborov–Rudich. Konfidenz: sehr hoch.
2. **Einen Polynomialzeit-Algorithmus für ein NP-vollständiges Problem suchen.**
   Erwarteter Wert: null. Erwarteter Schaden (Zeit, Glaubwürdigkeit): erheblich.
3. **In GCT inhaltlich mitreden.** Wir können referieren. Wir können nicht beurteilen,
   ob eine Multiplizitätsschranke korrekt ist. Das offen zu sagen, ist Teil der Qualität
   des Berichts.
4. **P vs. NP in Lean angehen.** Fortnows Warnung (Juni 2026) ist präzise: Es gibt keine
   Beweisidee, die man formalisieren könnte. Formalisierung ohne Beweisidee ist
   Beschäftigungstherapie. Konfidenz: sehr hoch.
5. **Ein LLM „den Beweis finden lassen".** Dong et al. 2023 und Khanukov 2026 haben das
   in zwei Varianten durchgespielt. Ergebnis beide Male: kein Mathematik-Beitrag.
   Ein drittes Mal wäre kein Experiment, sondern eine Wiederholung mit bekanntem Ausgang.
6. **Physikalisch/analogisch motivierte „Separationen"** (Observer-Theorie, N-Frames,
   Thermodynamik, Quantengravitation). Diese Literatur existiert auf arXiv und ist
   durchgängig ohne Rezeption im Feld. Als Auditierungsobjekt relevant, als Quelle nicht.
7. **Die Gesamtliteratur „vollständig" erfassen.** cs.CC allein produziert mehrere
   hundert Preprints pro Jahr. Wir brauchen Tiefenschnitte, keine Bibliographie.

**Ein siebenter Punkt gilt bedingt:** Solange kein Volltextzugang besteht (§0), ist auch
jede Aussage über *technische Details* einzelner Arbeiten aussichtslos. Wir können dann
nur über Existenz, Zuordnung und Rezeption von Arbeiten reden, nicht über deren Inhalt.

---

# Teil B — Was wäre überhaupt ein Ergebnis?

## B.1 Ebene 1 — Sicher erreichbar

Kriterium: Erreichbar mit reiner Recherche- und Syntheseleistung, unabhängig von Glück.

| # | Zielzustand | Überprüfbar durch |
|---|---|---|
| 1.1 | **Landkarte der Angriffsrichtungen** mit Stand 2026, je Richtung: Kernidee, bester bekannter Stand, blockierende Barriere, aktive Gruppen | Jede Zeile hat ≥ 1 Quelle mit URL, Jahr, Venue |
| 1.2 | **Barrierenkatalog**: Relativization, Natural Proofs, Algebrization, Locality — jeweils: was genau wird ausgeschlossen, was *nicht*, welche Ergebnisse umgehen sie | Für jede Barriere ≥ 1 dokumentiertes Beispiel eines Resultats, das sie umgeht (z. B. Williams' NEXP ⊄ ACC⁰) |
| 1.3 | **Claim-Register KI-Mathematik 2023–2026**: jeder öffentlich gewordene KI-Durchbruchsclaim mit Datum, Behauptung, Prüfinstanz, Ausgang | Tabelle; jede Zeile mit Primär- und Widerlegungsquelle |
| 1.4 | **Crank-Taxonomie**: die 5–8 wiederkehrenden Fehlermuster in P-vs-NP-Beweisversuchen, jeweils mit dokumentiertem Beispiel | Jedes Muster mit ≥ 1 publizierter Widerlegung belegt |
| 1.5 | **Fehlschluss-Korrekturen** für das Papier: Knapsack/pseudopolynomiell, SAT-Solver-Praxis ≠ P = NP, VP ≠ VNP ⇏ P ≠ NP, Lean-Verifikation ≠ Aussagenkorrektheit | Je Punkt eine formal saubere Gegendarstellung |
| 1.6 | **Dissens-Register**: alle Punkte, an denen unsere Agenten sich widersprechen oder Quellen sich widersprechen (Beispiel: Gasarch-Zahlen, §0) | Vollständigkeit prüfbar gegen Agentenberichte |

**Konfidenz, dass Ebene 1 erreichbar ist: hoch** — mit der Einschränkung, dass alle
technischen Details bis zur Beschaffung von Volltextzugang `[NUR-SNIPPET]` bleiben.

## B.2 Ebene 2 — Ambitioniert, aber möglich

| # | Zielzustand | Falsifizierbarkeitskriterium |
|---|---|---|
| 2.1 | **Implikationsgraph**: gerichteter Graph „Aussage X ⇒ Aussage Y" über Meta-Komplexität, Proof Complexity, Schaltkreisschranken; jede Kante mit Quelle | Ein Fachmensch kann jede Kante prüfen; falsche Kante = falsifiziert |
| 2.2 | **Barriere-Zuordnung der KI-Ansätze**: für jede KI-Methode (evolutionäre Suche, RL-Beweiser, LLM-Konjektur) die Aussage, *an welcher* der drei Barrieren sie scheitern würde | Ein Gegenbeispiel, das die Barriere nachweislich umgeht, falsifiziert die Zuordnung |
| 2.3 | **Reproduzierbares Negativexperiment**: dokumentierter, versionierter Versuch, ein modernes Modell eine *bekannte, nichttriviale* Komplexitätsaussage beweisen zu lassen (z. B. Baker–Gill–Solovay oder Ladners Theorem) — inkl. Fehleranalyse | Prompts, Modellversionen, Ausgaben im Repo; Dritte können nachfahren |
| 2.4 | **Präzise offene Teilfragen**: 5–10 Fragen, formuliert so, dass ein Fachmensch sie als wohlgestellt und offen anerkennt | Test: keine der Fragen ist in der Literatur bereits beantwortet |
| 2.5 | **Zeitreihe der Feldmeinung**: Gasarch 2002/2012/2019 sauber rekonstruiert, Widersprüche aufgelöst, ggf. mit belegter Aussage „es gibt seither keine neue Umfrage" | Zahlen aus Primärquelle (SIGACT News) belegt |
| 2.6 | **Synthese-These**: eine begründete, angreifbare Gesamtaussage zur Projektfrage — z. B. „KI-Methoden haben zwischen 2023 und 2026 *keinen* Beitrag zu unteren Schranken geleistet, wohl aber messbar zu Formalisierung und Teilschrittautomatisierung" | Ein einziges dokumentiertes Gegenbeispiel falsifiziert sie |

**2.3 verdient Hervorhebung.** Ein sauber dokumentiertes *Negativexperiment* ist unter
unseren Bedingungen wertvoller als jede weitere Literaturzusammenfassung — und es ist
das Einzige in diesem Projekt, das echte Primärdaten erzeugt. Briefing §4: Negative
Ergebnisse sind wertvoll.

## B.3 Ebene 3 — Sehr unwahrscheinlich

**Was müsste passieren?** Für echten Fortschritt bei P vs. NP wäre nötig:
1. Eine Beweistechnik, die **nicht relativiert, nicht naturalisiert und nicht
   algebrisiert** — und deren Nicht-Naturalisierung *nachgewiesen*, nicht behauptet ist.
2. Diese Technik müsste über ein Spielzeugmodell hinaus skalieren. (Williams' NEXP ⊄ ACC⁰
   erfüllt Kriterium 1, skaliert aber nicht. Das ist der historische Normalfall.)
3. Der Durchbruch käme fast sicher aus dem Umfeld Meta-Komplexität oder Proof Complexity,
   nicht aus einer neuen Richtung.

**Wahrscheinlichkeit, dass unser Team dazu beiträgt:** Unter 1 %. `[EIGENE EINSCHÄTZUNG]`,
Konfidenz hoch. Fortnow, August 2026: erwartet keine baldige Lösung, da ein vollständig
neuer Ansatz nötig wäre und KI „doesn't yet think outside the box". `[NUR-SNIPPET]`

### B.3.1 Falsifikationskriterien: Durchbruch vs. Irrtum

Diese Liste ist das operative Herzstück für den Claim-Auditor. Ein Claim wird
**abgelehnt**, wenn auch nur eines der Muss-Kriterien verletzt ist.

**MUSS-Kriterien (alle erfüllt = weiter prüfen; eines verletzt = ablehnen):**

- **M1 — Barrierenrechenschaft.** Der Text sagt *explizit und im Detail*, warum das
  Argument nicht relativiert, nicht naturalisiert und nicht algebrisiert. Ein Beweis,
  der die drei Barrieren nicht einmal erwähnt, ist mit sehr hoher Wahrscheinlichkeit falsch.
  **Dies ist der schärfste Einzelfilter.**
- **M2 — Kein Übergeneralisierungs-Test bestanden?** Das Argument darf nicht,
  mit minimaler Änderung angewandt, auch etwas **Falsches** beweisen — etwa eine
  Härteschranke für ein Problem, für das ein effizienter Algorithmus bekannt ist,
  oder eine Aussage, die einer bekannten Relativierung widerspricht.
  Der Xu–Zhou-Fall ist genau hier gescheitert: Die Behauptung kollidiert mit bekannten
  Ergebnissen, dass 3-SAT ohne erschöpfende Suche (wenn auch exponentiell) lösbar ist.
- **M3 — Keine unbegründete Strukturannahme über Algorithmen.** Fast jeder gescheiterte
  P ≠ NP-Versuch nimmt implizit an, ein Algorithmus habe eine bestimmte Form. Die Härte
  des Problems liegt in der Quantifikation über *alle* Algorithmen.
- **M4 — Die formalisierte Aussage stimmt mit der behaupteten überein.**
  Bei Lean-/Coq-verifizierten Claims: Ein unabhängiger Auditor prüft das *Statement*
  (Definitionen von P, NP, Kodierung, Maschinenmodell), nicht den Beweis. Der Beweis ist
  durch den Kernel gesichert, die Aussage nicht. **Lehre aus Navier–Stokes, Sept. 2026.**
- **M5 — Keine Axiomschmuggelware.** Keine versteckten Zusatzannahmen, keine
  nichtstandardmäßigen Definitionen von „polynomiell", „Algorithmus" oder „Eingabegröße".

**Positive Indikatoren (erhöhen die Glaubwürdigkeit):**
- Der Beweis liefert als Nebenprodukt bekannte Resultate neu — ein Zeichen, dass die
  Technik trägt.
- Die Autoren benennen die Grenzen ihrer Technik selbst und präzise.
- Anerkannte Fachleute aus dem Kernfeld (nicht: aus angrenzenden Feldern, nicht:
  Pressestellen) äußern sich substanziell positiv unter Nennung nachvollziehbarer Gründe.
- Ein *Preprint mit Diskussion*, nicht eine Pressemitteilung, geht voran.

**Rote Flaggen (jede einzelne senkt die Priorität drastisch):**
- Kurzer Beweis für ein 55 Jahre offenes Problem.
- Ankündigung über Presse/Social Media vor Preprint.
- Keine Reaktion der Autoren auf technische Einwände, oder Reaktion nur als
  Missverständnisvorwurf.
- Veröffentlichung in einem Venue ohne Komplexitätstheorie-Kompetenz — oder in einem
  Journal, in dem ein Autor Herausgeberfunktion hat. **Der Fall Xu–Zhou:** „SAT requires
  exhaustive search" erschien in *Frontiers of Computer Science* 19(12), Dez. 2025; ein
  Autor ist Deputy Editor-in-Chief des Journals; Allender, Williams u. a. forderten
  Rücknahme, der Chefredakteur lehnte ab; ein „Comment on 'SAT requires exhaustive search'"
  wurde stattdessen im selben Journal publiziert (DOI 10.1007/s11704-025-53000-5), und die
  Community wertet den Vorgang als Zusammenbruch des Begutachtungsprozesses.
  `[VERIFIZIERT für den Vorgang] [CLAIM — widerlegt für die P ≠ NP-Behauptung]`
  Konfidenz: hoch. https://link.springer.com/article/10.1007/s11704-025-53000-5 ·
  https://arxiv.org/pdf/2312.02071 ·
  https://blog.computationalcomplexity.org/2025/08/some-thoughts-on-journals-refereeing.html
- Prioritätsstreit oder Verdacht auf „front-running" (Navier–Stokes 2026).
- Behauptung, eine Barriere „trivial" oder „durch einen neuen Blickwinkel" zu umgehen.

**Operativer Test bei P = NP-Claims (nicht bei P ≠ NP):**
Ein behaupteter Polynomialzeit-Algorithmus für ein NP-vollständiges Problem ist
**empirisch sofort testbar.** Er muss offene SAT-Competition-Instanzen lösen bzw.
kryptographische Challenges brechen. Wer den Algorithmus nicht laufen lässt oder nicht
laufen lassen kann, hat keinen. Dieser Test kostet Stunden statt Monate und sollte
**immer zuerst** kommen.

**Zeitkriterium:** Ein Durchbruch gilt für uns frühestens als „wahrscheinlich echt",
wenn er (a) auf arXiv/ECCC steht, (b) mindestens drei unabhängige Fachleute aus der
Komplexitätstheorie ihn substanziell geprüft haben, (c) nach sechs Monaten keine
publizierte Widerlegung existiert, und (d) er auf STOC/FOCS/CCC angenommen wurde.
Vorher: `[CLAIM]`. Ohne Ausnahme.

---

# Teil C — Teamzusammensetzung für die Tiefenphase

**Begründung der Auswahl.** Die Aufteilung folgt der Bewertungsmatrix (§A.2), nicht der
Struktur des Fachgebiets. Drei Agenten (S2, S3, S5) decken den *Kanon* ab, weil ohne ihn
jede KI-Bewertung haltlos wäre. Drei Agenten (S7, S8, S9) decken den *Projektfokus* ab.
Zwei (S4, S6) decken die Richtungen mit dem besten Verhältnis aus Aussichtsreichtum und
Zugänglichkeit. Bewusst **kein** eigener GCT-Agent: Das Thema ist für uns nicht
beurteilbar (§A.3.3) und wird von S6 mit klarer Tiefenbegrenzung mitgenommen.
Redundanz ist an drei Stellen eingebaut: Barrieren (S2/S3), Meta-Komplexität (S3/S4),
KI-Claims (S7/S8/S9).

---

### S2 — Kanon & Landkarte

- **Forschungsfrage:** Was ist der belegbare Konsensstand zu P vs. NP im Jahr 2026, und
  welche Barrieren schließen welche Beweistechniken wie genau aus?
- **Suchbegriffe:** `P vs NP status report`, `Allender status report P versus NP`,
  `Aaronson P=?NP survey`, `Fortnow status of the P versus NP problem CACM`,
  `Fifty Years of P vs NP`, `relativization Baker Gill Solovay`,
  `natural proofs Razborov Rudich`, `algebrization Aaronson Wigderson`,
  `Impagliazzo five worlds`, `NEXP ACC0 Williams`, `locality barrier`
- **Autoren:** Eric Allender, Scott Aaronson, Lance Fortnow, Avi Wigderson,
  Russell Impagliazzo, Ryan Williams, Alexander Razborov
- **Venues:** SIGACT News Complexity Theory Column, CACM, Clay Mathematics Institute,
  Bulletin of the EATCS, Theory of Computing Graduate Surveys
- **Lieferobjekte:** (a) Landkarte aller Angriffsrichtungen (Ebene-1-Ziel 1.1);
  (b) Barrierenkatalog mit *Umgehungsbeispielen* (1.2); (c) präzise Formulierung dessen,
  was die Barrieren **nicht** ausschließen — das ist der von Cranks meistmissbrauchte Punkt.
- **Überschneidung:** Barrieren mit S3 (bewusst doppelt — kritischste Stelle);
  Historie mit S9.

---

### S3 — Untere Schranken & Schaltkreiskomplexität

- **Forschungsfrage:** Wie weit sind untere Schranken 2026 tatsächlich, Modell für Modell —
  und wo genau liegt die Grenze zwischen „bewiesen" und „bedingt"?
- **Suchbegriffe:** `circuit lower bounds explicit function 3.1n`,
  `affine disperser lower bound`, `monotone circuit lower bounds`,
  `ACC0 lower bounds`, `threshold circuit lower bounds`, `hardness magnification`,
  `constructive separations`, `Karchmer-Wigderson`, `communication complexity lower bounds`,
  `rigidity matrix`, `ECCC circuit lower bounds 2026`
- **Autoren:** Ryan Williams, Lijie Chen, Hanlin Ren, Igor Carboni Oliveira,
  Alexander Golovnev, Anup Rao, Emanuele Viola, Benjamin Rossman
- **Venues:** FOCS, STOC, CCC, ECCC (TR25-xxx, TR26-xxx), ITCS
- **Lieferobjekte:** (a) Tabelle „Modell × beste bekannte Schranke × Jahr × Quelle";
  (b) Darstellung der Größenordnungslücke zwischen 3,1n und superpolynomiell in einer
  Form, die Laien nicht in die Irre führt; (c) Status von Hardness Magnification
  inkl. Locality Barrier; (d) Bewertung, ob 2024–2026 dort *irgendetwas* Richtung
  P vs. NP bewegt wurde (erwartete Antwort: nein — dann bitte so sagen).
- **Überschneidung:** Barrieren mit S2; Williams' TIME/SPACE-Resultat mit S5;
  Meta-Komplexität mit S4.

---

### S4 — Meta-Komplexität, Proof Complexity & Unabhängigkeit

- **Forschungsfrage:** Welche Implikationsketten führen 2026 am plausibelsten in Richtung
  P ≠ NP — und welche davon gelten unbedingt, welche nur unter kryptographischen Annahmen?
- **Suchbegriffe:** `MCSP NP-hardness`, `meta-complexity`, `Kolmogorov complexity
  time-bounded MKTP`, `GapMINKT`, `one-way functions meta-complexity`,
  `Extended Frege lower bounds P NP`, `AC0[p]-Frege lower bounds`,
  `Ideal Proof System IPS lower bounds`, `bounded arithmetic unprovability circuit lower
  bounds`, `PV1 APC1 S12 unprovability`, `Is P vs NP formally independent`
- **Autoren:** Shuichi Hirahara, Rahul Santhanam, Ján Pich, Hanlin Ren, Iddo Tzameret,
  Igor Carboni Oliveira, Jan Krajíček, Toniann Pitassi, Sam Buss
- **Venues:** STOC, FOCS, CCC, ITCS, LICS, STACS, ECCC, Journal of Symbolic Logic
- **Lieferobjekte:** (a) **Implikationsgraph** (Ebene-2-Ziel 2.1) — das zentrale
  Lieferobjekt dieses Agenten; (b) Liste der *unbedingten* vs. *bedingten* Resultate;
  (c) saubere Trennung „Unabhängigkeit von ZFC" vs. „Unbeweisbarkeit in bounded
  arithmetic"; (d) Antwort auf: Ist MCSP NP-vollständig? (erwartet: offen — mit Beleg).
- **Überschneidung:** Natural Proofs mit S2 und S3; Formalisierung mit S8.
- **Hinweis:** Dies ist nach meiner Einschätzung der **inhaltlich ertragreichste**
  Agentenauftrag des Projekts.

---

### S5 — Obere Schranken, Algorithmik & SAT-Praxis

- **Forschungsfrage:** Was können wir algorithmisch wirklich, wo liegen die bedingten
  Schranken, und warum widerlegt die SAT-Solver-Praxis P ≠ NP nicht?
- **Suchbegriffe:** `fine-grained complexity SETH`, `orthogonal vectors conjecture`,
  `3SUM hardness`, `exponential time hypothesis`, `simulating time square-root space
  Williams`, `Cook Mertz tree evaluation`, `catalytic computing`,
  `SAT competition results 2025 2026`, `CDCL solver scaling`,
  `approximation algorithms PCP inapproximability`, `Knapsack pseudopolynomial`
- **Autoren:** Ryan Williams, Virginia Vassilevska Williams, James Cook, Ian Mertz,
  Russell Impagliazzo, Marijn Heule, Armin Biere
- **Venues:** STOC, FOCS, SODA, SAT Conference, CCC, JACM
- **Lieferobjekte:** (a) Darstellung von Williams' TIME[t] ⊆ SPACE[O(√(t log t))]
  (STOC 2025 Best Paper, JACM 2025) und was es **nicht** bedeutet — es ist ein
  Zeit-Raum-Resultat, kein Schritt zu P ≠ NP; (b) Cook–Mertz Tree Evaluation in
  O(log n · log log n) und der Katalyse-Komplex; (c) die Fehlschluss-Korrekturen
  (Ebene-1-Ziel 1.5); (d) empirische Einordnung der SAT-Solver-Leistungsfähigkeit.
- **Überschneidung:** Williams' Resultat mit S3; SAT-Heuristiken mit S7.

---

### S6 — Algebraische Wege: arithmetische Schaltkreise & GCT

- **Forschungsfrage:** Was hat die algebraische Route seit 2021 tatsächlich geliefert,
  und wie weit trägt sie realistisch?
- **Suchbegriffe:** `Limaye Srinivasan Tavenas constant depth algebraic circuits`,
  `VP versus VNP`, `permanent versus determinant`, `algebraic circuit lower bounds any
  field Forbes`, `polynomial identity testing derandomization`,
  `geometric complexity theory occurrence obstructions`, `Kronecker coefficients`,
  `multiplicity obstructions`
- **Autoren:** Nutan Limaye, Srikanth Srinivasan, Sébastien Tavenas, Michael Forbes,
  Amir Shpilka, Christian Ikenmeyer, Greta Panova, Peter Bürgisser, Ketan Mulmuley,
  Markus Bläser
- **Venues:** FOCS, STOC, CCC, JACM, JAMS, Advances in Mathematics, Forum of Mathematics Pi
- **Lieferobjekte:** (a) Was LST 2021/2025 exakt zeigt und wo die Grenze zu
  „allgemeine arithmetische Schaltkreise" verläuft; (b) GCT-Bilanz: Was wurde versprochen,
  was 2016 widerlegt (no occurrence obstructions), was bleibt;
  (c) **explizite Tiefenbegrenzung**: eine kurze Liste „Diese GCT-Fragen können wir nicht
  beurteilen" — Ehrlichkeit über die eigene Grenze ist hier Lieferobjekt, nicht Mangel.
- **Überschneidung:** Proof Complexity / IPS mit S4.

---

### S7 — KI für Algorithmenentdeckung *(Pflichtrolle laut Auftrag)*

- **Forschungsfrage:** Hat KI-gestützte Algorithmenentdeckung 2023–2026 irgendein
  Resultat hervorgebracht, das für P vs. NP relevant ist — und wenn nein, was hat sie
  stattdessen geliefert?
- **Suchbegriffe:** `AlphaEvolve algorithm discovery`, `AlphaTensor matrix multiplication`,
  `FunSearch cap set`, `LLM evolutionary program search`,
  `neural network SAT solver NeuroBack GNN`, `learned branching heuristics CDCL`,
  `reinforcement learning combinatorial optimization`, `NPHardEval LLM benchmark`,
  `AlphaEvolve 48 multiplications Waksman Winograd`
- **Autoren/Gruppen:** Google DeepMind (Alhussein Fawzi, Matej Balog, Bernardin Tao),
  Robin Houston (kritische Einordnung), Manuel Kauers & Jakob Moosbauer (Folgearbeiten
  zur Matrixmultiplikation)
- **Venues:** Nature, DeepMind Blog, NeurIPS, ICLR, ICML, arXiv cs.DS / cs.LG, SAT Conference
- **Lieferobjekte:** (a) Tabelle „KI-Resultat × behaupteter Beitrag × tatsächlicher
  Geltungsbereich × Relevanz für P vs. NP"; (b) **die AlphaEvolve-Fallstudie ausbuchstabiert**:
  48 Multiplikationen, aber über welchen Ringen, verglichen mit Waksman 1970 (46) und
  Winograd 1967 (48) — das ist der Musterfall für „Claim vs. Kleingedrucktes";
  (c) belastbare Aussage zu gelernten SAT-Heuristiken und warum Benchmark-Gewinne
  keine Worst-Case-Aussage sind.
- **Überschneidung:** SAT-Praxis mit S5; Claim-Prüfung mit S9 (bewusst redundant).

---

### S8 — KI für automatisches Beweisen & Formalisierung *(Pflichtrolle laut Auftrag)*

- **Forschungsfrage:** Wo steht automatisches/KI-gestütztes Beweisen im September 2026
  wirklich, und was folgt daraus für die Beweisbarkeit von Komplexitätsaussagen?
- **Suchbegriffe:** `AlphaProof Lean reinforcement learning Nature`,
  `AlphaProof Nexus Erdős problems`, `AlphaGeometry`, `Seed-Prover`, `Harmonic Aristotle`,
  `Lean 4 mathlib formalization complexity theory`, `autoformalization`,
  `OpenAI Navier-Stokes Lean formalization 2026`, `Clay Institute Navier-Stokes claim`,
  `LLM theorem proving hallucination formal verification`
- **Autoren/Gruppen:** Google DeepMind, OpenAI, Harmonic, Terence Tao, Kevin Buzzard,
  Jeremy Avigad, Thomas Bloom (erdosproblems.com), Tristan Buckmaster, Levent Alpöge
- **Venues:** Nature, arXiv cs.LO / cs.AI / math.LO, NeurIPS, ITP/CPP, Lean Zulip
- **Lieferobjekte:** (a) Zeitachse des KI-Beweisens 2024–2026 mit belegtem
  Verifikationsstatus je Resultat; (b) **Navier–Stokes-Fallstudie (Sept. 2026)** als
  methodisches Lehrstück: Lean prüft die Ableitung, nicht die Aussage — inkl.
  Prioritätsstreit und Haltung des Clay Institute; (c) Antwort auf: Gibt es *irgendeine*
  nichttriviale Komplexitätsaussage, die 2026 KI-erzeugt und formal verifiziert wurde?
  (d) Bewertung von Fortnows Rat, P vs. NP nicht über Lean anzugehen.
- **Überschneidung:** Erdős-/OEIS-Claims mit S9; Formalisierbarkeit mit S4.

---

### S9 — Claim-Auditor & Crank-Forensik *(Pflichtrolle laut Auftrag)*

- **Forschungsfrage:** Welche P-vs-NP- und KI-Mathematik-Claims wurden 2023–2026 erhoben,
  wer hat sie geprüft, und mit welchem Ausgang?
- **Suchbegriffe:** `P versus NP page Woeginger list claimed proofs`,
  `refutation P vs NP proof`, `SAT requires exhaustive search comment refutation`,
  `Frontiers of Computer Science retraction P NP`,
  `OpenAI GPT-5 Erdős problems retracted`, `Kevin Weil Erdős claim`,
  `Thomas Bloom erdosproblems misrepresentation`,
  `Gasarch P=?NP poll third SIGACT News`, `crank mathematics complexity`,
  `Large Language Model for Science P vs NP criticism`
- **Autoren:** Gerhard Woeginger (Liste, letzte Aktualisierung Sept. 2016, 116 Einträge;
  Woeginger † 2022), Eric Allender, Lance Fortnow, Bill Gasarch, Scott Aaronson,
  Thomas Bloom, Terence Tao
- **Venues:** arXiv (Widerlegungsarbeiten), ECCC, Blogs (Computational Complexity,
  Shtetl-Optimized, Gödel's Lost Letter), Fachpresse (Nature News, Quanta), TechCrunch u. a.
  für Zeitachsen von Rücknahmen
- **Lieferobjekte:** (a) **Claim-Register** (Ebene-1-Ziel 1.3) — die zentrale
  Projekttabelle; (b) **Crank-Taxonomie** (1.4); (c) Aufarbeitung der drei Leitfälle:
  Xu–Zhou/FCS (Peer-Review-Versagen), GPT-5/Erdős Okt. 2025 (Rücknahme nach 17 Stunden;
  die „Entdeckungen" waren aufgefundene Literatur; Hassabis nannte es „embarrassing"),
  OpenAI/Navier–Stokes Sept. 2026 (Verifikations- und Prioritätsstreit);
  (d) **Auflösung des Gasarch-Zahlenwiderspruchs** aus §0 anhand der Primärquelle
  (SIGACT News Column 100, pollpaper3.pdf) — und Feststellung, ob es nach 2019 eine
  weitere Umfrage gab; (e) Pflege des Dissens-Registers (1.6).
- **Überschneidung:** **Mit allen.** S9 hat ein Vetorecht gegen jeden Claim im
  Konsenspapier (§D.4) und prüft insbesondere die KI-Claims von S7 und S8 unabhängig nach.

---

**Nicht besetzte Rollen und warum:** Kein Quantenkomplexitäts-Agent (BQP berührt P vs. NP
nur peripher; bei Bedarf Zusatzauftrag an S3). Kein Kryptographie-Agent (die relevanten
Verbindungen kommen über Meta-Komplexität bei S4 an). Kein eigener GCT-Agent (§A.3.3).

---

# Teil D — Methodik & Qualitätssicherung

## D.1 Gegen Motivated Reasoning

Das strukturelle Risiko dieses Projekts: Ein Team, das ein halbes Jahr über P vs. NP
recherchiert, entwickelt den Wunsch, *etwas gefunden zu haben*. Gegenmaßnahmen:

1. **Erwartungs-Vorabfestlegung.** Jeder Agent notiert **vor** der Recherche in einem
   Satz, was er zu finden erwartet. Am Ende vergleicht er. Abweichungen werden
   berichtet — sie sind das Interessanteste am Bericht.
2. **Negativ-Quote als Pflicht.** Jeder Agent liefert mindestens **drei** explizite
   Negativbefunde („Hier ist zwischen X und 2026 nichts Relevantes passiert"),
   jeweils mit Beleg für die Suche. Ein Bericht ohne Negativbefunde gilt als unvollständig
   und geht zurück. (Briefing §4.)
3. **Rotierende Red-Team-Paarung.** Jeder Agentenbericht bekommt einen fachfremden
   Zweitleser mit dem einzigen Auftrag: *Finde den schwächsten Beleg und die stärkste
   unbelegte Behauptung.* Paarungen: S2↔S7, S3↔S8, S4↔S9, S5↔S6.
4. **Erzwungene Gegenhypothese.** Zu jeder Synthese-These (Ebene-2-Ziel 2.6) formuliert
   ein anderer Agent die stärkste Gegenposition, bevor die These ins Papier darf.
5. **Kein Auffüllen.** Wo nichts ist, steht „hier ist nichts" — nicht ein schwächerer
   Befund als Platzhalter. Länge ist kein Qualitätsmaß.
6. **Sponsor-Bias-Markierung.** Resultate aus Industrielaboren, die über Blogpost oder
   Presse statt über Preprint kommuniziert werden, tragen eine eigene Markierung.
   DeepMind und OpenAI sind Interessenträger, nicht neutrale Instanzen.

## D.2 Gegen Halluzination von Fachliteratur

Dies ist unter den Bedingungen aus §0 das **größte technische Risiko** des Projekts.

1. **Kein Zitat ohne Treffer.** Eine Quelle darf nur zitiert werden, wenn ihre URL in
   einem tatsächlichen Suchergebnis erschienen ist. Aus dem Gedächtnis rekonstruierte
   Zitate sind verboten — auch und gerade dann, wenn sie richtig aussehen.
2. **Vollständiges Quintupel.** Jede Quelle: Autor(en) · Titel · Venue · Jahr · URL.
   Fehlt ein Element, wird die Aussage `[NUR-SNIPPET]` degradiert oder gestrichen.
3. **Triangulationsregel.** Jeder projektrelevante Claim wird mit **mindestens zwei
   unterschiedlich formulierten** Suchanfragen gegengeprüft; bei Ebene-2-Aussagen mit
   drei. Trifft nur eine Anfrage, gilt: Konfidenz höchstens *niedrig*, Markierung
   `[NUR-SNIPPET]`.
4. **Snippet-Quarantäne.** Zahlen, Konstanten, Theoremnummern und exakte Schrankenwerte
   aus Suchzusammenfassungen sind **grundsätzlich** `[NUR-SNIPPET]`. Der beobachtete
   Gasarch-Widerspruch (§0) ist der Beleg dafür, dass das keine Formalie ist.
5. **Halluzinations-Köder (Kalibrierung).** Jeder Agent sucht einmal pro Sitzung gezielt
   nach einer plausiblen, aber frei erfundenen Arbeit (z. B. „Santhanam Ren superpolynomial
   Frege lower bounds 2026"). Liefert die Suchzusammenfassung dazu inhaltliche Aussagen
   statt Fehlanzeige, ist die gesamte Sitzung als unzuverlässig zu kennzeichnen.
   **Dies ist die einzige Kontrolle, die wir ohne Volltextzugang haben.**
6. **Konsistenzanker.** Bekannte Fixpunkte prüfen: Erscheint ECCC-Nummerierung plausibel
   (TR26-xxx im Herbst 2026 im Bereich 150–180)? Passen STOC/FOCS-Jahre zur Konferenzhistorie?
   Ist ein zitierter Autor im richtigen Feld aktiv?
7. **Trennung Primär-/Sekundärquelle.** Tech-Presse, Medium, Substack, LinkedIn und
   aggregierende Newsseiten sind **niemals** Beleg für einen mathematischen Inhalt.
   Sie sind zulässig als Beleg für *Vorgänge* (wer wann was behauptet hat, wer widersprach).
   Beim Navier–Stokes-Komplex ist genau diese Trennung entscheidend.
8. **Eskalation bei Volltextbedarf.** Ist eine Aussage ohne Volltext nicht entscheidbar,
   wird sie in die Liste `offene-verifikationen.md` eingetragen statt geraten.

## D.3 Umgang mit widersprüchlichen Befunden

**Grundsatz: Widersprüche werden nicht gemittelt, geglättet oder weggelassen.**

1. **Dissens-Register.** Jeder Widerspruch — zwischen Agenten *oder* zwischen Quellen —
   kommt in `research/dissens-register.md` mit: Gegenstand, Position A + Beleg,
   Position B + Beleg, Status.
2. **Eskalationsleiter:**
   - *Stufe 1 — Scheinwiderspruch:* Beide Seiten reden über verschiedene Dinge
     (unterschiedliche Modelle, Voraussetzungen, Klassen). Häufigster Fall.
     Auflösung: Präzisierung, beide Aussagen bleiben stehen.
   - *Stufe 2 — Quellenqualität:* Eine Seite hat die stärkere Quelle
     (peer-reviewt > Preprint > Blog > Presse). Auflösung: schwächere Position wird
     als `[CLAIM]` geführt, nicht gestrichen.
   - *Stufe 3 — Echter offener Dissens:* Beide Seiten gleich gut belegt.
     Auflösung: **keine.** Der Dissens kommt als solcher ins Papier. Das ist ein
     Ergebnis, kein Defekt.
3. **Verbot der Autoritätsauflösung.** Kein Widerspruch wird dadurch entschieden, dass
   ein Agent „sicherer klingt" oder mehr geschrieben hat.
4. **Widerspruch als Signal.** Häufen sich Widersprüche zu *einem* Thema, ist das
   typischerweise ein Hinweis auf eine halluzinierte Quelle oder auf einen real
   umstrittenen Punkt im Feld — beides berichtenswert.

## D.4 Prüfschritte für jeden Claim vor Aufnahme ins Konsenspapier

Jeder Claim durchläuft diese Kette. Abbruch an jeder Stelle möglich.

| # | Schritt | Abbruchkriterium |
|---|---|---|
| **1** | **Existenzprüfung** — gibt es die Quelle? URL aus echtem Suchtreffer, Quintupel vollständig | Kein Treffer ⇒ Claim gestrichen |
| **2** | **Triangulation** — ≥ 2 unabhängig formulierte Suchanfragen bestätigen | Nur 1 Treffer ⇒ Konfidenz *niedrig* + `[NUR-SNIPPET]` |
| **3** | **Statusklassifikation** — `[VERIFIZIERT]` / `[PREPRINT]` / `[CLAIM]` / `[EIGENE EINSCHÄTZUNG]` (+ `[NUR-SNIPPET]`) | Keine Einstufung möglich ⇒ zurück an Agent |
| **4** | **Geltungsbereichsprüfung** — sagt der Claim *genau* das, was die Quelle sagt? Keine stille Verallgemeinerung (Musterfall: AlphaEvolve „über allgemeinen Ringen"?) | Überdehnung ⇒ Claim wird eingeengt neu formuliert |
| **5** | **Widerlegungssuche** — aktive Suche nach publizierter Kritik, Comment, Retraction (Briefing §5: *Wer hat das geprüft?*) | Widerlegung existiert ⇒ `[CLAIM]` + Widerlegung **im selben Satz** |
| **6** | **Barriere-/Plausibilitätsprüfung** — bei Separations- oder Algorithmus-Claims: Muss-Kriterien M1–M5 aus §B.3.1 | Ein M verletzt ⇒ `[CLAIM]`, als sehr wahrscheinlich fehlerhaft markiert |
| **7** | **Red-Team-Gegenlesung** — Zweitleser laut §D.1.3 | Ungelöster Einwand ⇒ Dissens-Register |
| **8** | **Auditor-Freigabe (S9)** — Vetorecht | Veto ⇒ Claim bleibt draußen oder wandert in den Anhang „ungeklärt" |
| **9** | **Konfidenzangabe** — hoch / mittel / niedrig, mit Begründung | Fehlt ⇒ zurück |

**Zusatzregel für alles, was ein KI-System behauptet:** Schritt 6 wird um M4
(Statement-Audit) verschärft. Ein Lean-Zertifikat ist ein Beleg dafür, dass *ein* Theorem
bewiesen wurde — nicht dafür, dass es *das gemeinte* Theorem ist. Navier–Stokes 2026 ist
der Präzedenzfall, auf den sich das Papier an dieser Stelle beruft.

## D.5 Zitationsqualität unter der Snippet-Beschränkung (§0)

Ergänzend zu D.2, weil die Beschränkung das Projekt strukturell prägt:

- **Zwei-Klassen-Bibliographie.** Die Quellenliste wird geteilt in
  *(A) am Volltext verifiziert* — derzeit **leer** — und *(B) über Suchtreffer belegt*.
  Diese Teilung erscheint sichtbar im Konsenspapier. Sie zu verschweigen wäre
  wissenschaftlich unredlich.
- **Bibliographische Metadaten sind belastbarer als Inhalte.** Titel, Autoren, Venue und
  DOI/URL treten in Suchtreffern mehrfach und konsistent auf; inhaltliche
  Zusammenfassungen sind Modellausgaben. Konsequenz: *Wir zitieren, dass eine Arbeit
  existiert und wo sie erschien, deutlich zuversichtlicher als das, was in ihr steht.*
- **Formulierungsdisziplin.** Statt „Hirahara zeigt, dass …" schreiben wir „Nach
  Suchtreffern zu Hirahara (STOC 2023) betrifft die Arbeit … `[NUR-SNIPPET]`", solange
  kein Volltext vorliegt.
- **Verifikations-Rückstandsliste.** `docs/offene-verifikationen.md` sammelt alle
  Aussagen, die bei Volltextzugang nachgeprüft werden müssen — priorisiert nach
  Bedeutung für die Kernthesen. Das macht die Lücke abarbeitbar statt dauerhaft.
- **Beschaffung hat Vorrang.** Falls ein Weg zu arXiv/ECCC/DBLP-Volltexten geöffnet
  werden kann, sollte das vor der Tiefenphase geschehen. Der Qualitätsunterschied ist
  größer als jeder Zugewinn durch mehr Agenten.

---

## E. Arbeitsplan und Abbruchregeln

**Phasen:**
1. *Zugangsklärung* (vorgeschaltet): Volltextzugang beschaffen oder dessen Fehlen
   dokumentieren. Ergebnis ändert die Zielsetzung von Ebene 2 auf Ebene 1+.
2. *Tiefenphase:* S2–S9 parallel, Berichte nach `research/<agent-id>-<thema>.md`
   (Briefing §Ablagestruktur).
3. *Konfliktphase:* Dissens-Register, Red-Team-Paarungen, S9-Audit.
4. *Synthese:* Konsenspapier mit Zwei-Klassen-Bibliographie und offenem Dissens-Anhang.

**Abbruchregeln (wichtig, weil dieses Thema unbegrenzt Zeit absorbiert):**
- Ein Agent, der nach seiner Recherche nur Negativbefunde hat, ist **fertig** — nicht
  gescheitert.
- Kein zweiter Rechercheversuch zu einer Richtung, die in §A.3 als aussichtslos gelistet
  ist, ohne neuen Anlass.
- Sobald das Claim-Register (1.3) und die Landkarte (1.1) stehen, ist der Projektwert
  gesichert. Alles Weitere ist Bonus.

---

## F. Die drei wichtigsten Warnungen

**W1 — Die Quellenlage macht inhaltliche Präzision derzeit unmöglich.**
Ohne Volltextzugang können wir belegen, *dass* eine Arbeit existiert, aber nicht
zuverlässig, *was* in ihr steht. Jede Konstante, jede Voraussetzung, jede Theoremnummer
in diesem Projekt ist bis auf Weiteres `[NUR-SNIPPET]`. Ein Papier, das diese Grenze
nicht sichtbar macht, wäre unredlich — und angreifbar an genau der Stelle, an der wir
anderen Unredlichkeit vorwerfen.

**W2 — Die verlockendsten Ergebnisse werden die falschen sein.**
Das Umfeld 2025/2026 produziert laufend Meldungen der Form „KI löst Jahrhundertproblem":
GPT-5/Erdős (Okt. 2025, nach 17 Stunden zurückgezogen), AlphaEvolve
(Rekord — aber über einer engeren Ringklasse als berichtet), OpenAI/Navier–Stokes
(Sept. 2026, Clay Institute sieht das Problem nicht als gelöst, dazu Prioritätsstreit).
Jede dieser Meldungen wird uns als „Fortschritt" angeboten werden. Die Muss-Kriterien
M1–M5 und der Neun-Schritte-Filter existieren genau dafür. **Am gefährlichsten ist
dabei nicht der offensichtliche Crank, sondern der seriös aufgemachte Claim aus einem
großen Labor mit Lean-Zertifikat.** Lean verifiziert Ableitungen, nicht Aussagen.

**W3 — Der wahrscheinlichste Ausgang ist ein Negativbefund, und das ist in Ordnung.**
Meine Erwartung `[EIGENE EINSCHÄTZUNG]`, Konfidenz hoch: Wir werden feststellen, dass
KI-Methoden zwischen 2023 und 2026 **keinen** Beitrag zu unteren Schranken oder zur
P-vs-NP-Frage geleistet haben, wohl aber messbare Beiträge zu Formalisierung,
Literaturerschließung und abgegrenzten technischen Teilschritten (Aaronsons
Orakelseparationen als bestes dokumentiertes Beispiel). Das Feld selbst sagt dasselbe:
Fortnow im Juni 2026 — kein tragfähiger Ansatz in Sicht. Die Versuchung wird sein,
dieses Ergebnis interessanter zu machen, als es ist. Ihr nachzugeben wäre der einzige
Weg, wie dieses Projekt tatsächlich scheitern kann.

---

## Quellenliste

**Klasse A — am Volltext verifiziert:** *keine.* Siehe §0.

**Klasse B — über Suchtreffer belegt (Metadaten mehrfach getroffen, Inhalte `[NUR-SNIPPET]`):**

*Kanon, Surveys, Status*
1. Allender, E.: *A Status Report on the P versus NP Question.* Advances in Computers, 2009. https://people.cs.rutgers.edu/~allender/papers/advances.in.computing.pdf
2. Aaronson, S.: *P =? NP.* (Survey, 116/121 S.; in: Open Problems in Mathematics, 2016.) https://www.scottaaronson.com/papers/pnp.pdf · Blogeintrag: https://www.scottaaronson.com/blog/?p=3095
3. Aaronson, S.: *Is P Versus NP Formally Independent?* 2003. https://www.scottaaronson.com/papers/indep.pdf
4. Fortnow, L.: *The Status of the P versus NP Problem.* CACM 2009. https://cacm.acm.org/research/the-status-of-the-p-versus-np-problem/
5. Fortnow, L.: *Fifty Years of P vs. NP and the Possibility of the Impossible.* CACM. https://lance.fortnow.com/papers/files/pvnp50.pdf
6. Clay Mathematics Institute: *P vs NP and Complexity Lower Bounds* (Abstracts, April 2025). https://www.claymath.org/wp-content/uploads/2025/04/Abstracts-PvNP.pdf
7. Quanta Magazine: *Complexity Theory's 50-Year Journey to the Limits of Knowledge*, 2023. https://www.quantamagazine.org/complexity-theorys-50-year-journey-to-the-limits-of-knowledge-20230817/
8. Quanta Magazine: *The Researcher Who Explores Computation by Conjuring New Worlds* (Impagliazzo, Five Worlds), 2024. https://www.quantamagazine.org/the-researcher-who-explores-computation-by-conjuring-new-worlds-20240327/

*Untere Schranken, Schaltkreise*
9. Find, Golovnev, Hirsch, Kulikov: *A Better-Than-3n Lower Bound for the Circuit Complexity of an Explicit Function.* FOCS 2016. https://ieeexplore.ieee.org/document/7782921/
10. *3.1n − o(n) circuit lower bounds for explicit functions.* STOC 2022. https://dl.acm.org/doi/abs/10.1145/3519935.3519976
11. Chen, Hirahara, Oliveira, Pich, Rajgopal, Santhanam: *Beyond Natural Proofs: Hardness Magnification and Locality.* ITCS 2020 / JACM 2022. https://dl.acm.org/doi/10.1145/3538391 · https://arxiv.org/pdf/1911.08297
12. Rao, A.: *Monotone circuit lower bounds for perfect matching.* ECCC TR26-129, Juli 2026. https://eccc.weizmann.ac.il/report/2026/129/
13. Ren, H.; Williams, R.: *Near-maximum circuit lower bound for E^prMA/1.* ECCC TR26-118, Juli 2026. https://eccc.weizmann.ac.il/report/2026/118/
14. *Simple general magnification of circuit lower bounds.* arXiv 2503.24061. https://arxiv.org/pdf/2503.24061

*Zeit/Raum, Algorithmik*
15. Williams, R. R.: *Simulating Time With Square-Root Space.* STOC 2025 (Best Paper), JACM 2025. https://people.csail.mit.edu/rrw/time-vs-space.pdf · https://dl.acm.org/doi/10.1145/3798104 · https://eccc.weizmann.ac.il/report/2025/017/download/
16. Cook, J.; Mertz, I.: *Tree Evaluation Is in Space O(log n · log log n).* STOC 2024. https://dl.acm.org/doi/10.1145/3618260.3649664
17. Quanta Magazine: *Catalytic Computing Taps the Full Power of a Full Hard Drive*, Feb. 2025. https://www.quantamagazine.org/catalytic-computing-taps-the-full-power-of-a-full-hard-drive-20250218/

*Meta-Komplexität, Proof Complexity, Unabhängigkeit*
18. Hirahara, S.: *NP-Hardness of Approximating Meta-Complexity: A Cryptographic Approach.* STOC 2023. https://dl.acm.org/doi/abs/10.1145/3564246.3585154 · https://eccc.weizmann.ac.il/report/2023/046/
19. Ren, H.; Santhanam, R.: *A Relativization Perspective on Meta-Complexity.* STACS 2022. https://drops.dagstuhl.de/entities/document/10.4230/LIPIcs.STACS.2022.54
20. ECCC TR26-091 (bedingte NP-Härte ImpMCSP / PAC-Learning unter iO), 2026. https://eccc.weizmann.ac.il/report/2026/091/
21. Pich, J.; Santhanam, R.: *Unprovability of Strong Complexity Lower Bounds in Bounded Arithmetic.* arXiv 2305.15235. https://arxiv.org/pdf/2305.15235
22. Pich, J.: *Towards P ≠ NP from Extended Frege lower bounds.* arXiv 2312.08163. https://arxiv.org/pdf/2312.08163
23. *AC⁰[p]-Frege Cannot Efficiently Prove That Constant-Depth Algebraic Circuit Lower Bounds Are Hard.* ITCS 2026 / arXiv 2509.16824. https://arxiv.org/abs/2509.16824 · https://drops.dagstuhl.de/entities/document/10.4230/LIPIcs.ITCS.2026.99
24. *Meta-Mathematics of Algebraic Complexity.* LICS 2026. https://drops.dagstuhl.de/entities/document/10.4230/LIPIcs.LICS.2026.49
25. ECCC TR26-133 (Garlik, Gryaznov, Ren, Tzameret; weak rank principle, PCR_F2), Aug. 2026. https://eccc.weizmann.ac.il/report/2026/133/

*Algebraische Komplexität, GCT*
26. Limaye, N.; Srinivasan, S.; Tavenas, S.: *Superpolynomial Lower Bounds Against Low-Depth Algebraic Circuits.* FOCS 2021 / JACM 72(4), 2025 / CACM Research Highlight. https://dl.acm.org/doi/10.1145/3734215 · https://cacm.acm.org/research-highlights/superpolynomial-lower-bounds-against-low-depth-algebraic-circuits/
27. Forbes, M.: *Low-Depth Algebraic Circuit Lower Bounds over Any Field.* CCC 2024. https://drops.dagstuhl.de/storage/00lipics/lipics-vol300-ccc2024/LIPIcs.CCC.2024.31/LIPIcs.CCC.2024.31.pdf
28. Bürgisser, Ikenmeyer, Panova: *No occurrence obstructions in geometric complexity theory.* 2016 / JAMS. https://arxiv.org/pdf/1604.06431
29. *On Geometric Complexity Theory: Multiplicity Obstructions Are Stronger Than Occurrence Obstructions.* SIAM J. Appl. Algebra Geom. https://epubs.siam.org/doi/10.1137/19M1287638
30. Bläser, M.; Ikenmeyer, C.: *Introduction to Geometric Complexity Theory.* Theory of Computing Graduate Surveys, 2025. https://toc.cs.uchicago.edu/articles/gs010/bibliography.html
31. Mulmuley, K.; Sohoni, M.: *Geometric Complexity Theory I.* SIAM J. Comput. 31(2). https://dl.acm.org/doi/10.1137/S009753970038715X

*KI für Algorithmenentdeckung*
32. Google DeepMind: *AlphaEvolve: A Gemini-powered coding agent for designing advanced algorithms.* Mai 2025. https://deepmind.google/blog/alphaevolve-a-gemini-powered-coding-agent-for-designing-advanced-algorithms/
33. Houston, R.: kritische Einordnung des AlphaEvolve-Resultats (Waksman 1970: 46 Mult.; Winograd 1967: 48). Mathstodon. https://mathstodon.xyz/@robinhouston/114507937280899656
34. *A non-commutative algorithm for multiplying 4x4 matrices using 48 non-complex multiplications.* arXiv 2506.13242. https://arxiv.org/pdf/2506.13242
35. Yang et al.: *NeuroBack: Improving CDCL SAT Solving using Graph Neural Networks.* ICLR 2024. https://openreview.net/forum?id=samyfu6G93
36. *Neural Approaches to SAT Solving: Design Choices and Interpretability.* arXiv 2504.01173. https://arxiv.org/pdf/2504.01173

*KI für automatisches Beweisen*
37. *Olympiad-level formal mathematical reasoning with reinforcement learning* (AlphaProof). Nature, 2025. https://www.nature.com/articles/s41586-025-09833-y
38. AlphaProof Nexus (DeepMind, Mai 2026): 9/353 Erdős-Probleme, 44/492 OEIS-Vermutungen. Berichterstattung: https://the-decoder.com/google-deepminds-alphaproof-nexus-solves-decades-old-math-problems-for-a-few-hundred-dollars/ · https://cryptobriefing.com/deepmind-alphaproof-nexus-erdos-problems/
39. Nature News: *OpenAI claims huge maths breakthrough on a famed 'Millennium Problem'.* Sept. 2026. https://www.nature.com/articles/d41586-026-02842-5
40. *Clay Institute Won't Call Navier-Stokes Solved by OpenAI.* https://www.implicator.ai/clay-institute-navier-stokes-openai-proof-claim/
41. *OpenAI Claims Its AI Proved a Navier-Stokes Blowup — Mathematicians Question Priority and Verification.* https://xenospectrum.com/en/openai-navier-stokes-singularity-clay-dispute/
42. Smithsonian Magazine: *A.I. May Have Solved a Longstanding Math Problem … It Ignited a Controversy Over Who Gets Credit.* Sept. 2026. https://www.smithsonianmag.com/smart-news/ai-may-have-solved-a-longstanding-math-problem-with-a-million-dollar-prize-it-ignited-a-controversy-over-who-gets-credit-180989472/
43. Fortnow, L.: *Navier-Stokes and Lean.* Computational Complexity Blog, Sept. 2026. https://blog.computationalcomplexity.org/2026/09/navier-stokes-and-lean.html

*Claims, Widerlegungen, Feldmeinung*
44. Fortnow, L.: *Respect the P v NP Problem.* Computational Complexity Blog, 10. Juni 2026. https://blog.computationalcomplexity.org/2026/06/respect-p-v-np-problem.html
45. Fortnow, L.: *Centaur Math.* Computational Complexity Blog, Aug. 2026. https://blog.computationalcomplexity.org/2026/08/centaur-math.html
46. Allender, E. (Gastbeitrag) / Fortnow, L.: *Some thoughts on journals, refereeing, and the P vs NP problem.* Aug. 2025. https://blog.computationalcomplexity.org/2025/08/some-thoughts-on-journals-refereeing.html
47. Xu, K.; Zhou, G.: *SAT requires exhaustive search.* Frontiers of Computer Science 19(12), Dez. 2025. `[CLAIM — widerlegt]` https://journal.hep.com.cn/fcs/EN/10.1007/s11704-025-50231-4
48. *Comment on "SAT requires exhaustive search".* Frontiers of Computer Science, DOI 10.1007/s11704-025-53000-5. https://link.springer.com/article/10.1007/s11704-025-53000-5
49. *Evaluating the Claims of "SAT Requires Exhaustive Search".* arXiv 2312.02071. https://arxiv.org/pdf/2312.02071
50. Dong et al.: *Large Language Model for Science: A Study on P vs. NP.* arXiv 2309.05689. `[CLAIM]` https://arxiv.org/abs/2309.05689
51. Khanukov, D.: *I Spent a Year Asking AI to Solve Math's Hardest Problem. Here's What Happened.* BLOG@CACM, Mai 2026. https://cacm.acm.org/blogcacm/i-spent-a-year-asking-ai-to-solve-maths-hardest-problem-heres-what-happened/
52. TechCrunch: *OpenAI's embarrassing math* (GPT-5/Erdős-Rücknahme, Okt. 2025). https://techcrunch.com/2025/10/19/openais-embarrassing-math/
53. the-decoder: *Leading OpenAI researcher announced a GPT-5 math breakthrough that never happened.* https://the-decoder.com/leading-openai-researcher-announced-a-gpt-5-math-breakthrough-that-never-happened/
54. Woeginger, G. J.: *The P-versus-NP page* (116 Einträge, Stand Sept. 2016). https://www.win.tue.nl/~wscor/woeginger/P-versus-NP.htm
55. Gasarch, W.: *Guest Column: The Third P =? NP Poll.* SIGACT News 50(1), 2019. https://dl.acm.org/doi/10.1145/3319627.3319636 · https://www.cs.umd.edu/users/gasarch/BLOGPAPERS/pollpaper3.pdf · Blog: https://blog.computationalcomplexity.org/2019/03/third-poll-on-p-vs-np-and-related.html
56. Fortnow, L.: *P v NP Papers Galore.* April 2025. https://blog.computationalcomplexity.org/2025/04/p-v-np-papers-galore.html
57. Aaronson, S.: Shtetl-Optimized, Archiv Komplexität (u. a. Sept. 2025 zu KI-gestützten Orakelseparationen). https://scottaaronson.blog/?cat=5

**Nicht als Quelle verwendet, als Auditierungsobjekt vermerkt:**
- arXiv 2512.11820, *Toward P vs NP: An Observer-Theoretic Separation via SPDP Rank and a ZFC-Equivalent Foundation within the N-Frame Model* — keine erkennbare Rezeption im Feld. `[CLAIM]`

---

*Ende des Berichts S1. Alle Bewertungen in §A.2, §B und §F sind `[EIGENE EINSCHÄTZUNG]`.
Alle Literaturangaben sind `[NUR-SNIPPET]`, solange kein Volltextzugang besteht (§0).*
