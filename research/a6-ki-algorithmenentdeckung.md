# A6 — KI für Algorithmenentdeckung: Was hat KI wirklich zu P vs. NP beigetragen?

**Agent:** A6 (Pflichtrolle) · **Stand:** 2026-09-12
**Methode:** ausschließlich WebSearch (WebFetch gesperrt), ca. 34 Anfragen, Triangulation
pro nichttrivialem Claim. **Alle inhaltlichen Aussagen sind `[NUR-SNIPPET]`**, sofern nicht
anders markiert — kein Volltextzugang, keine nachgerechneten Beweisdetails.

---

## 0. Kernbefund in einem Satz

**Nein — es gibt kein KI-erzeugtes Resultat mit direkter Relevanz für P vs. NP.**
Was KI stattdessen geleistet hat, ist präzise abgrenzbar: (a) das Auffinden **endlicher
kombinatorischer Zeugen** innerhalb von Menschen gebauter Beweisrahmen (AlphaEvolve,
FunSearch), (b) das **empirische** Verbessern von Solvern und Heuristiken für NP-schwere
Probleme (SATLUTION, AutoModSAT, NeuroBack). Beides bewegt die Grenze zwischen P und NP
nicht — (a) setzt P ≠ NP als Hypothese voraus, (b) ist eine Konstantenverbesserung im
exponentiellen Regime.
**Konfidenz: hoch.** Kein einziger Gegenbefund gefunden.

---

## 1. Kernfall: arXiv:2509.18057 — AlphaEvolve in der Komplexitätstheorie

### 1.1 Bibliographisches

- **Autoren:** Ansh Nagda (UC Berkeley + Google DeepMind), Prabhakar Raghavan (Google),
  Abhradeep Thakurta (Google DeepMind).
- **Titelvarianz bestätigt:** v1–v4 „Reinforced Generation of Combinatorial Structures:
  Applications to Complexity Theory"; die aktuellen Versionen (u. a. v6, v7) tragen
  „… : Hardness of Approximation". **v7 datiert 09.03.2026.** [NUR-SNIPPET, Konfidenz hoch]
- **Status: PREPRINT.** Drei unterschiedlich formulierte Suchen nach einer
  Konferenz-/Journalversion (STOC/FOCS/ITCS/SODA) blieben ergebnislos; ein
  Zitationsdienst führt die Arbeit als „Preprint (2025), arXiv:2509.18057".
  **`[PREPRINT]` — nicht peer-reviewt.** Konfidenz mittel-hoch (Negativbefund aus Suche).
- **Begleitend:** Google-Research-Blogpost „AI as a research partner: Advancing theoretical
  computer science with AlphaEvolve" (research.google/blog, Sept. 2025).
- **Nachfolgearbeit:** arXiv:2603.09172 „… : Ramsey Numbers" (eingereicht 10.03.2026,
  v5 21.04.2026), gleiches Autorenteam: verbesserte **untere** Schranken für neun
  klassische Ramsey-Zahlen (u. a. R(3,13) 60→61, R(4,16) 170→174, R(4,18) 205→209).
  Bemerkenswerte Selbstaussage der Arbeit: praktisch *alle* bekannten
  Ramsey-Unterschranken werden ohnehin rechnerisch gewonnen — AlphaEvolve ersetzt eine
  Familie maßgeschneiderter Suchprogramme durch **einen** Meta-Algorithmus.

### 1.2 Welche Schranken wurden verbessert? (alle Leads bestätigt)

| Problem | Neu (AlphaEvolve) | Vorher | Bestätigt durch |
|---|---|---|---|
| MAX-4-CUT, NP-Härte der Approximation | **0,987** | 0,9883 | 3 Suchen, konsistent |
| MAX-3-CUT, NP-Härte der Approximation | **0,9649** | 0,9853 (bester *gadget-basierter* Wert) | 3 Suchen, konsistent |
| Metrisches TSP (über MWST-Gleichungsgadget) | **111/110** | 117/116 | 2 Suchen, konsistent |
| MAX-CUT / MAX-IS-Certification auf zufälligen 3-/4-regulären Graphen | nahezu optimale obere + bedingte untere Schranken | Kunisky–Yu (arXiv:2404.17012) | 2 Suchen |

Zusatzbefunde, bestätigt:
- Das MAX-4-CUT-Gadget hat **19 Variablen** und eine Gewichtung mit Faktoren bis **1429×**.
- Die Certification-Unterschranken beruhen auf der Konstruktion **nahezu extremaler
  Ramanujan-Graphen mit bis zu 163 Knoten**.
- Kleinere Zahl = **stärkere** Härteaussage. 0,987 < 0,9883 und 0,9649 < 0,9853 sind also
  echte Verschärfungen, keine Verschlechterungen. (Dieser Punkt wird in populärer
  Berichterstattung regelmäßig verdreht.)

### 1.3 **Die logische Form — der entscheidende Punkt**

Die Erwartung des Auftrags wird **bestätigt**. Präzise Formulierung:

> Die Aussage „Es ist NP-schwer, MAX-4-CUT innerhalb des Faktors 0,987 zu approximieren"
> ist logisch äquivalent zu:
> **„Wenn es einen Polynomialzeit-Algorithmus gäbe, der MAX-4-CUT innerhalb 0,987
> approximiert, dann wäre P = NP."**

Daraus folgt unmittelbar:

1. **Die Resultate setzen P ≠ NP voraus, um Gehalt zu haben.** Wäre P = NP, wären sie
   sämtlich inhaltsleer (dann approximiert man MAX-4-CUT exakt in Polynomialzeit).
   Sie sind Aussagen *innerhalb* der Hypothese P ≠ NP, nicht Aussagen *über* sie.
   `[EIGENE EINSCHÄTZUNG]` auf Basis komplexitätstheoretischer Standarddefinition,
   Konfidenz hoch.
2. **Richtung der Bewegung:** Ein besseres Inapproximierbarkeitsresultat verschiebt die
   *obere* Grenze dessen, was Approximationsalgorithmen leisten könnten — es macht die
   Lücke zwischen bestem bekannten Algorithmus und Härteschranke *kleiner*, aber es sagt
   nichts darüber, ob P und NP zusammenfallen. Die Schranke zwischen P und NP wird nicht
   berührt.
3. **Unbedingt vs. bedingt — differenziert:**
   - MAX-3-CUT / MAX-4-CUT / TSP: Diese sind **unbedingte NP-Härte** (Gadget-Reduktionen
     auf PCP-/Håstad-Basis), *nicht* unter der Unique Games Conjecture. Das ist
     methodisch die stärkere Sorte. Numerisch sind sie allerdings deutlich schwächer als
     UGC-Resultate — unter UGC existieren scharfe Schwellen für MAX-3-CUT
     (vgl. arXiv:2608.00333 „Sharp Hardness for MAX-3-CUT and Quantum MAX-CUT").
     [NUR-SNIPPET, Konfidenz mittel]
   - Die **Certification-Unterschranken sind doppelt bedingt**: Sie hängen nicht an
     P ≠ NP, sondern an der **Kunisky–Yu-Vermutung** über die Ununterscheidbarkeit
     zufälliger d-regulärer Graphen von (verrauschten) zufälligen Lifts eines
     Ramanujan-Basisgraphen (arXiv:2404.17012). Das ist eine *average-case*-Annahme,
     die stärker und unbewiesener ist als P ≠ NP.
     **→ Diese Teilresultate sind also noch weiter von P vs. NP entfernt als die
     Inapproximierbarkeitsresultate.** Konfidenz mittel-hoch.

### 1.4 Arbeitsteilung Mensch / KI (Lead bestätigt)

Bestätigt, und in einer bemerkenswert klaren Formulierung aus der Sekundärdarstellung
des Google-Blogposts:

> „The approach uses AI to discover **a structure within the proof, not the proof itself**.
> The validity of the final theorem relies on two components: the correctness of the
> **lifting framework** [Mensch], and the verification of the discovered structure [Maschine].
> AlphaEvolve sidesteps hallucination issues by focusing on verifiable structures, not full
> proofs — **humans handle the lifting frameworks**." [NUR-SNIPPET, Konfidenz hoch]

Konkret:
- **Mensch:** stellt den Beweisrahmen (PCP-Theorem, Håstad-Reduktionen, das
  „Lifting"-Lemma, das aus einem endlichen Gadget ein universelles Theorem macht),
  definiert die Zielfunktion und das Erfolgskriterium.
- **KI:** durchsucht den Raum endlicher Gadgets/Graphen; findet Objekte, die Menschen
  nicht in Betracht gezogen hatten (19 Variablen, Gewichte 1429×).
- **Verifikation:** Der Lead ist bestätigt — die Kandidatenprüfung ist selbst exponentiell
  teuer, deshalb wurde **AlphaEvolve auch auf die Verifikationsprozedur angesetzt,
  Beschleunigung bis zu 10.000×**. Wichtiger Zusatzbefund: Die *finalen* Gadgets wurden
  anschließend mit einem **Brute-Force-Algorithmus, der explizit alle Möglichkeiten
  prüft**, unabhängig verifiziert. Die evolvierte Verifikation wird also nur zum
  Beschleunigen der Suche benutzt, nicht als letzte Beweisinstanz.
  [NUR-SNIPPET, Konfidenz mittel-hoch] — das ist methodisch sauber und entkräftet den
  naheliegenden Einwand „evolvierter Verifier ⇒ evolvierte Fehler".
- **Keine Hinweise auf Fehler, Rücknahmen oder publizierte Widerlegungen** gefunden
  (gezielte Suche). Negativbefund, Konfidenz mittel.

---

## 2. AlphaEvolve allgemein — und der Matrixmultiplikations-Verifikationsauftrag

### 2.1 arXiv:2506.13131 (AlphaEvolve) und arXiv:2511.02864 (Tao et al.)

- **2506.13131:** >50 offene mathematische Probleme; Wiederentdeckung des Standes der
  Technik in ca. **75 %** der Fälle, Verbesserung in ca. **20 %**. Kissing Number in
  Dimension 11: **593** (vorher 592). Verbesserung bei 14 Matrixmultiplikationsalgorithmen.
- **2511.02864 „Mathematical exploration and discovery at scale"** (Georgiev,
  Gómez-Serrano, **Tao**, Wagner; eingereicht 03.11.2025, v3 22.12.2025): **67 Probleme**
  aus Analysis, Kombinatorik, Geometrie, Zahlentheorie. Neue Konstruktionen u. a. für
  Nikodym-Mengen und das Kakeya-Problem über endlichen Körpern in Dimension 3, 4, 5;
  in einigen Fällen Generalisierung endlich vieler Instanzen zu einer allgemeinen Formel.
- **NP-Relevanz dieser beiden Arbeiten: praktisch null.** Keines der 67 bzw. >50 Probleme
  ist ein Komplexitätsproblem im Sinne von P vs. NP. Konfidenz hoch.
- **Taos eigene Einordnung** (Blogpost 05.11.2025, terrytao.wordpress.com): AlphaEvolve
  sei **kein autonomer Mathematiker**, benötige erhebliche menschliche Expertise zur
  Steuerung und Validierung und sei **anfällig für „clever workarounds"** (Reward Hacking);
  er warnt ausdrücklich davor, solche Werkzeuge ohne unabhängige Verifikationsmöglichkeit
  zu benutzen. [NUR-SNIPPET, Konfidenz mittel-hoch]

### 2.2 **Verifikationsauftrag 4×4-Matrixmultiplikation — Lead vollständig BESTÄTIGT**

Dies ist der beste verfügbare Test dafür, wie präzise die öffentliche Berichterstattung
über KI-Mathematik ist. Ergebnis: **die populäre Darstellung ist irreführend.**

Chronologie, aus mehreren unabhängig formulierten Suchen rekonstruiert:

| Jahr | Autor | Mult. | Voraussetzung |
|---|---|---|---|
| 1967 | Winograd | 48 | jeder **kommutative** Ring |
| 1969 | Strassen (2 Rekursionsstufen) | 49 | beliebiger (nichtkommutativer) Ring |
| **1970** | **Waksman** | **46** | jeder kommutative Ring **mit Division durch 2** |
| 2022/24 | Kauers–Moosbauer / AlphaTensor-Linie | 47 | **Charakteristik 2** |
| **2024** | **Kaporin** | **48** | explizites **komplexes** bilineares Schema, semi-analytische Lösung der Brent-Gleichungen |
| 2025 | **AlphaEvolve** | 48 | **komplexe** Koeffizienten (auf Halbzahlen gerundet), Charakteristik 0; **nichtkommutativ** |
| 2025 | Dumas–Pernet–Sedoglavic (arXiv:2506.13242) | 48 | **rationale** Koeffizienten, gültig über jedem Ring **außer Charakteristik 2** |
| 2026 | arXiv:2603.18699 | 48 | numerisch genauere rationale Variante |

**Antworten auf die gestellten Fragen:**

1. *Gilt das über beliebigen Ringen oder über einer engeren Klasse?* — **Engere Klasse.**
   AlphaEvolves Originalschema verlangt **komplexe Koeffizienten** (Charakteristik 0).
   Die Verallgemeinerung auf „jeden Ring außer Charakteristik 2" ist eine **nachträgliche
   menschliche Leistung** (Dumas, Pernet, Sedoglavic, Juni 2025), gewonnen durch eine
   Isotropie-Operation, die das Schema auf Q projiziert.
   **Konfidenz hoch** (zwei unabhängig formulierte Suchen, plus Primärabstract-Snippet).
2. *Stimmt es, dass Waksman (1970) bereits 46 erreichte?* — **Ja, unter anderen
   Voraussetzungen:** kommutativer Ring mit Division durch 2. Bestätigt über zwei
   unterschiedliche Suchformulierungen; die Kritik wurde öffentlich u. a. von Robin
   Houston und Fredrik Johansson (Mathstodon, Mai 2025) vorgetragen, mit der spitzen
   Bemerkung, DeepMind möge eine KI erfinden, die vor Neuheitsbehauptungen die Literatur
   nach Vorarbeiten durchsucht. **Konfidenz hoch.**
3. **Zusatzbefund, im Auftrag nicht enthalten:** Bereits **Kaporin (2024)** gab ein
   explizites komplexes Rang-48-Schema für 4×4 an — also **vor** AlphaEvolve.
   AlphaEvolves Beitrag wird in der Fachliteratur daher präziser als „unabhängige,
   exakte und öffentlich nachprüfbare Konstruktion" beschrieben.
   [NUR-SNIPPET, nur eine Quellenlinie (Katalog arXiv:2606.13408) — **Konfidenz mittel**,
   sollte bei Gelegenheit nachgeprüft werden.]

**Was bleibt:** Die **verteidigungsfähige** Formulierung lautet: *erste nichtkommutative
Rang-48-Zerlegung für 4×4 über Charakteristik 0, exakt und nachprüfbar, gefunden ohne
Vorwissen.* Die verbreitete Formulierung *„erste Verbesserung über Strassen hinaus seit
56 Jahren"* ist **falsch bzw. stark verkürzt** — sie vergleicht über verschiedene
Ringklassen hinweg und ignoriert Winograd 1967, Waksman 1970 und Kaporin 2024.
**Dieser Befund ist auch für das Abschlusspapier über den Einzelfall hinaus relevant:
Er zeigt, dass die Berichterstattung über KI-Mathematik systematisch Voraussetzungen
unterschlägt — genau der Fehlertyp, den die Muss-Kriterien M3 und M5 adressieren.**

---

## 3. FunSearch und Nachfolger — die Kernfrage

### 3.1 FunSearch (Nature 625, 18.01.2024, Romera-Paredes et al.) `[VERIFIZIERT]` (Venue)

- **Cap-Set:** Neue Cap-Menge der Größe **512** in Dimension n = 8 (vorher 496). Über
  „admissible sets" **neue untere Schranke für die Cap-Set-Kapazität: 2,2202** — die
  größte Verbesserung seit 20 Jahren.
- **Entscheidendes methodisches Detail:** Ellenberg erkannte im **lesbaren Code-Output**
  eine zuvor unbekannte Symmetrie-Annahme, nutzte sie zur Einschränkung des Suchraums
  und ließ FunSearch damit erneut laufen — erst so entstand die Schranke 2,2202. Die
  explizite Konstruktion der 512er-Cap-Menge wurde **von Hand** rekonstruiert.
  → Mensch-Maschine-Schleife, exakt wie bei AlphaEvolve.
- **Online Bin Packing:** neue Heuristiken.
- Kritische Einordnung: Ernest Davis (NYU), „Comment on (Romera-Paredes et al., 2023)".

### 3.2 Der Bin-Packing-Audit — wichtiger Negativbefund `[VERIFIZIERT]`

**arXiv:2510.27353, „An In-depth Study of LLM Contributions to the Bin Packing Problem",
angenommen bei *ACM Transactions on Evolutionary Learning and Optimization* (peer-reviewt).**
Die Autoren prüfen die Behauptung, LLM-getriebene genetische Suche liefere „neue
Einsichten" ins Online-Bin-Packing (uniforme und Weibull-Verteilung). Befund:
Die erzeugten Heuristiken sind zwar menschenlesbar, bleiben aber **selbst für
Fachleute weitgehend undurchsichtig**; die Autoren geben eine **einfachere
Zwei-Parameter-Heuristik** an, die effizienter, interpretierbarer und **auf eine
deutlich breitere Instanzklasse generalisierbar** ist.
→ Die „Einsicht"-Komponente der FunSearch-Bin-Packing-Behauptung hält der Prüfung
**nicht** stand. Konfidenz mittel-hoch.

### 3.3 Nachfolgesysteme

- **CodeEvolve** (arXiv:2510.14150, Open Source): Insel-basierte evolutionäre Suche,
  CVT-MAP-Elites-Archiv, LLM-Ensemble. Erreicht/übertrifft AlphaEvolve auf **5 von 9**
  Benchmark-Problemen; mit offenem Qwen3-Coder-30B über AlphaEvolve bei CirclePacking
  zu ~1/10 der Kosten.
- **AlphaResearch** (arXiv:2511.08522, eingereicht 11.11.2025, revidiert 01.04.2026):
  „duale Umgebung" aus ausführungsbasiertem verifizierbarem Reward **und** einem
  simulierten Peer-Review-Reward (Reward-Modell, trainiert auf >24.000 ICLR-Reviews).
  Bestes bekanntes Ergebnis beim **Circle-Packing**. Benchmark „AlphaResearchComp",
  8 offene algorithmische Probleme; besser als Vergleichssysteme auf 6 von 8.
- **Effective Harness Engineering** (arXiv:2605.15221; Ishibashi, Yano, Oyamada;
  13.05.2026): untersucht das **Harness** statt des Modells — viele flache vs. wenige
  tiefe Kandidaten bei festem Token-Budget; Umgang mit **„evaluation hacks"** (Programme,
  die die Scoring-Funktion ausbeuten statt das Problem zu lösen); sichere Parallelisierung.
- **Kritischer Gegenpol, hoch relevant:** **arXiv:2602.16805, „Simple Baselines are
  Competitive with Code Evolution"** (Gideoni, Risi, Gal; 18.02.2026). Über drei Domänen
  (mathematische Schranken, agentische Scaffolds, ML-Wettbewerbe) erreichen **einfache
  Baselines** die aufwändigen Code-Evolution-Pipelines oder übertreffen sie. Zentrale
  Aussage für unsere Fragestellung: **Der Suchraum und das im Prompt kodierte
  Domänenwissen bestimmen die Leistungsobergrenze; die Evolutionspipeline ist sekundär.**
  Konfidenz mittel-hoch (Preprint, aber methodisch einschlägig).

### 3.4 **Kernfrage: Hat KI je einen asymptotisch besseren Algorithmus für ein NP-schweres Problem gefunden?**

**Nein.** Drei unterschiedlich formulierte Suchen, plus die Durchsicht aller oben
genannten Arbeiten. Der Befund ist einheitlich: KI-Suche liefert **Heuristiken und
Approximationsverfahren**, keine bewiesenen asymptotischen Verbesserungen für
NP-schwere Probleme. Eine Sekundärquelle formuliert es direkt: *„LLMs alone remain
insufficient to push the state-of-the-art forward when addressing NP-hard problems."*
**Konfidenz: hoch** für den Negativbefund; er ist über alle geprüften Systeme konsistent.

**Der stärkste Kandidat — und warum er die Frage nicht kippt:**

> **SATLUTION** (arXiv:2509.07367, „Autonomous Code Evolution Meets NP-Completeness",
> 09.09.2025). LLM-Agenten evolvieren **ganze Solver-Repositories** (hunderte Dateien,
> Zehntausende Zeilen C/C++), inklusive Selbst-Evolution der eigenen Evolutionsregeln.
> Ausgangspunkt: Codebasen und Benchmark der SAT Competition **2024**. Ergebnis auf dem
> **ungesehenen** Benchmark der SAT Competition **2025**: die evolvierten Solver lösen
> **347 / 345 / 344** Instanzen gegenüber **334** (Gold) und **331** (Silber) der
> menschlichen Gewinner; bester PAR-2-Wert. Aufwand: ~90.000 CPU-Stunden, < 20.000 USD.
> [NUR-SNIPPET, Konfidenz mittel-hoch]

Warum das P vs. NP nicht berührt:
1. Es ist eine **empirische Laufzeitverbesserung auf einer festen Instanzverteilung**,
   keine Aussage über worst-case-Komplexität.
2. Der evolvierte Solver ist weiterhin ein CDCL-Solver mit exponentiellem worst case;
   es gibt keinen Anspruch auf eine verbesserte obere Schranke.
3. Zum Vergleich der Größenordnungen: Die beste bekannte worst-case-Schranke für
   allgemeines 3-SAT liegt bei O*(1,307…^n) — Fortschritt findet dort in der
   **Konstante des Exponenten** statt. SATLUTION trägt dazu nichts bei.
4. **Datenkonflikt, nicht geglättet:** Eine Suche lieferte für den SAT-Competition-2025-
   Haupt-Sequenztrack „CaDiCaL-SC2025, PAR-2 2327,00, **161** gelöste Instanzen; Kissat-VSA
   160; AE-Kissat-bump 159", eine andere „AE_KISSAT **327** Instanzen", SATLUTION nennt
   Gold **334** / Silber **331**. Diese Zahlen sind untereinander inkonsistent
   (vermutlich verschiedene Tracks/Benchmarksets), **konnten aber nicht aufgelöst werden.**
   Die relative Aussage „SATLUTION > menschliche Gewinner" stammt aus der SATLUTION-Arbeit
   selbst und ist **nicht unabhängig bestätigt**.

---

## 4. Gelernte Heuristiken für SAT — Stand 2026

### 4.1 Die NeuroSAT-Linie: Leads bestätigt

- **NeuroSAT** (Selsam et al., arXiv:1802.03685, ICLR 2019): Message-Passing-GNN,
  trainiert nur auf dem Ein-Bit-Signal „erfüllbar/unerfüllbar". Generalisiert auf
  Graphenfärbung, Clique, Dominating Set, Vertex Cover — **aber nur auf kleinen
  Zufallsgraphen**. Die Arbeit sagt selbst: **„not competitive with state-of-the-art
  SAT solvers"**; die Hauptbeschränkung ist die **Skalierung auf große Instanzen**.
  **Der Lead zur Skalierungsgrenze ist damit direkt aus der Primärquelle bestätigt.
  Konfidenz hoch.**
- **NeuroBack** (arXiv:2110.14053, **ICLR 2024 — peer-reviewt** `[VERIFIZIERT]` (Venue)):
  Der pragmatische Gegenentwurf. Ein GNN sagt **einmalig vor Solverstart** Variablenphasen
  voraus; Inferenz offline, **kein GPU zur Laufzeit**. Resultat: Kissat löst **bis zu
  5,2 %** (SATCOMP-2022) bzw. **7,4 %** (SATCOMP-2023) mehr Instanzen.
- **CaDiCaL-PASAT:** +2 / +4 / +6 Instanzen auf SATCOMP 2023 / 2024 / 2025. [NUR-SNIPPET]

### 4.2 Die Gesamtlage — und warum sie stabil ist

- **Die SAT Competition 2025 wurde von klassischen CDCL-Solvern gewonnen**
  (CaDiCaL-/Kissat-Linie sequentiell; MallobSat im Shared-Memory-Parallel-Track,
  vor Painless-PRS-Kissat mit ~3 % PAR-2-Abstand). Kein neuronaler End-to-End-Solver
  spielt eine Rolle. Konfidenz hoch.
- **Wo ML in SAT tatsächlich funktioniert:** in der **Maple-Familie** — sehr kleine,
  *nicht* tiefe Modelle, **online während des Solverlaufs** trainiert, bei jeder
  Verzweigungsentscheidung abgefragt, gespeist aus lokaler Konfliktanalyse. Inferenz
  ist dort praktisch kostenlos.
- **Wo es nicht funktioniert:** Deep-Learning-/GNN-Ansätze haben **deutlich teurere
  Inferenz** und skalieren voraussichtlich nicht auf die großen realen Instanzen der
  SAT Competitions. Häufiger Befund: ML-Modelle **reduzieren die Anzahl der Suchschritte,
  verschlechtern aber die Laufzeit**, weil brauchbare Modelle groß und langsam sind.
  Übliche Gegenmaßnahme: ML nur in der Anfangsphase, dann Übergabe an klassische Heuristik
  (so auch NeuroCore, NeuroBack).
  **→ Genau die im Auftrag genannte Skalierungsgrenze, aus mehreren Quellen bestätigt.
  Konfidenz hoch.**
- **AutoModSAT** (arXiv:2507.22876, v2 06.06.2026): LLM optimiert einen bewusst in sieben
  Heuristikfunktionen modularisierten CDCL-Solver; berichtet **+40 % gegenüber Baseline,
  +30 % gegenüber SOTA** — allerdings **domänenspezifisch** (per-domain heuristics) und
  selbstberichtet. [NUR-SNIPPET, Konfidenz niedrig-mittel]

### 4.3 „NP-Engine" (arXiv:2510.16476) — Kategorienklärung

NP-Engine ist **kein** Beitrag zur Algorithmik NP-schwerer Probleme, sondern ein
**Trainings- und Evaluationsframework für LLMs**: 10 Aufgaben in fünf Domänen, je mit
(i) steuerbarem Instanzgenerator, (ii) regelbasiertem Verifier, (iii) heuristischem Solver
als Ground-Truth-Lieferant. Damit RLVR-Training unter Schwierigkeitsgraden.
Ergebnis: Qwen2.5-7B-NP schlägt GPT-4o auf „NP-Bench" und generalisiert
out-of-domain auf Logik-/Puzzle-/Mathe-Aufgaben.
**Relevanz für P vs. NP: null.** Gemessen wird, wie gut ein Sprachmodell auf kleinen
Instanzen abschneidet, nicht wie schnell ein Algorithmus ist.
Der Name ist irreführend und sollte im Abschlusspapier nicht als Komplexitätsresultat
zitiert werden. Konfidenz hoch.

---

## 5. Synthese: Welche Aufgabenklasse löst KI-Suche — und warum fällt P vs. NP nicht hinein?

### 5.1 Antwort auf die Leitachse (§2 Team-Briefing): **BESTÄTIGT, mit Präzisierung**

Die Leitachse „suchbare endliche Zeugen vs. unendliche Quantifizierung über alle
Algorithmen" hat sich in jedem geprüften Fall gehalten. Sie ist aber **zu grob** und
sollte für das Abschlusspapier geschärft werden. Aus dem Material ergibt sich ein
**Drei-Bedingungen-Profil**; KI-Suche funktioniert genau dann, wenn alle drei erfüllt sind:

> **(B1) Endlichkeit & Repräsentierbarkeit.** Das gesuchte Objekt ist endlich und
> explizit hinschreibbar: ein Gadget mit 19 Variablen, ein Ramanujan-Graph mit 163 Knoten,
> eine Cap-Menge in Dimension 8, ein Rang-48-Tensor, ein Ramsey-Graph, ein Stück Solvercode.
>
> **(B2) Billiges (oder wenigstens erzwingbares) Verifikationsorakel.** Es gibt eine
> berechenbare Score-/Prüffunktion. Wo sie teuer ist, wird sie selbst optimiert
> (AlphaEvolve: bis 10.000× schneller) — aber das Endergebnis wird per Brute Force
> abgesichert. Ohne dieses Orakel kollabiert das Verfahren in Halluzination.
>
> **(B3) Ein von Menschen gestellter Lifting-Rahmen.** Ein *bewiesenes* Theorem, das aus
> dem endlichen Objekt eine allgemeine Aussage macht: das PCP-/Gadget-Lifting bei
> MAX-4-CUT, die Produktkonstruktion bei Cap-Sets, die Rekursion bei Strassen-artigen
> Zerlegungen. **Diesen Teil liefert ausnahmslos der Mensch.**

**Wichtige Präzisierung gegenüber der Briefing-Formulierung:** Die Achse ist *nicht*
„endlich vs. unendlich" im Ergebnis. FunSearchs Cap-Set-Kapazität 2,2202 **ist** eine
asymptotische, unendliche Aussage; die Inapproximierbarkeitsschranken sind universelle
Theoreme über alle Instanzen. Der Schnitt verläuft **eine Ebene tiefer**: endlich ist das
**Objekt, das die KI sucht** — die Unendlichkeit kommt ausschließlich über B3 hinein.
Die Achse sollte daher lauten: **endlicher Suchkern in einem menschlichen Lifting-Rahmen
vs. Probleme ohne endlichen Suchkern.**

### 5.2 Warum P vs. NP außerhalb liegt

- **B1 fehlt:** Es ist kein endliches Objekt bekannt, dessen Auffinden die Frage
  entscheidet. Ein P=NP-Beweis wäre ein Algorithmus **plus** ein Korrektheits- und
  Laufzeitbeweis; ein P≠NP-Beweis quantifiziert über **alle** Algorithmen. Weder das eine
  noch das andere ist ein Objekt in einem durchsuchbaren endlichen Raum.
- **B2 fehlt:** Es gibt keine billige Prüffunktion für „das ist ein gültiger
  P-vs-NP-Beweis". (Eine Lean-Formalisierung prüft die *Ableitung*, nicht die
  *Angemessenheit der Voraussetzungen* — Muss-Kriterium M4; siehe A7.)
- **B3 fehlt am gravierendsten:** Es existiert kein Lifting-Rahmen. Die drei Barrieren
  (Relativization, Natural Proofs, Algebrization) sind genau die Feststellung, dass die
  bekannten Rahmen nicht tragen. Fortnow (Juni 2026, sinngemäß): „we don't even have a
  viable approach". **Wenn der Rahmen fehlt, hat die KI nichts, worin sie suchen könnte.**
- **Zusatzargument aus Gideoni et al. (arXiv:2602.16805):** Selbst *innerhalb* von
  B1–B3 ist nicht die Suchmaschinerie der Engpass, sondern **der von Menschen entworfene
  Suchraum**. Das verschärft die Diagnose: Bei P vs. NP kennt niemand den Suchraum.

### 5.3 Eine ehrliche Gegenprobe — was am nächsten an ein Gegenbeispiel heranreicht

1. **SATLUTION** — KI verbessert einen Algorithmus für das kanonische NP-vollständige
   Problem und schlägt menschliche Weltmeister. Aber: empirisch, auf fester
   Instanzverteilung, ohne jede asymptotische Aussage. **Kein Gegenbeispiel.**
2. **AlphaEvolve/Ramsey (arXiv:2603.09172)** — ein **einziger** Meta-Algorithmus ersetzt
   neun maßgeschneiderte Suchprogramme. Das ist Generalisierung über die
   *Suchverfahren*, nicht über die *Aussagen*. **Kein Gegenbeispiel**, aber der
   interessanteste Hinweis darauf, dass die KI-Beitragsebene vom Einzelobjekt zur
   Methode aufsteigen kann.
3. **FunSearch/Cap-Set 2,2202** — echte asymptotische Verbesserung, erzeugt aus einem
   endlichen Fund. Bestätigt B1+B3 exemplarisch. **Kein Gegenbeispiel.**

**Kein Gegenbefund zur Leitachse gefunden. Konfidenz hoch.**

### 5.4 Was ich *nicht* verifizieren konnte

- Keine Volltextprüfung. Die logische Form der Sätze in arXiv:2509.18057 ist aus
  Abstract-Snippets und der komplexitätstheoretischen Standarddefinition von
  „NP-hard to approximate within c" erschlossen, nicht am Satzwortlaut abgelesen.
- Ob arXiv:2509.18057 inzwischen bei einer Konferenz angenommen wurde (Negativbefund
  aus Suche, kein Beleg für Annahme *und* kein Beleg für Ablehnung).
- Die SAT-Competition-2025-Instanzzahlen (161 vs. 327 vs. 334/331) — **offener
  Widerspruch, bewusst nicht geglättet.**
- Die SATLUTION-Überlegenheitsbehauptung ist nicht unabhängig repliziert.
- Kaporin 2024 (komplexes Rang-48-Schema vor AlphaEvolve): nur eine Quellenlinie.
- Die AutoModSAT-Zahlen (+40 %/+30 %) sind selbstberichtet.
- Ob die MAX-3-CUT/MAX-4-CUT-Resultate im Detail **vollständig** unbedingt sind (kein
  versteckter UGC-Anteil) — plausibel, aber nicht am Volltext geprüft.

---

## 6. Quellen

**Kernfall AlphaEvolve / Komplexitätstheorie**
- https://arxiv.org/abs/2509.18057 — Nagda, Raghavan, Thakurta, „Reinforced Generation of
  Combinatorial Structures: Hardness of Approximation" (v1 2025-09, v7 2026-03-09). `[PREPRINT]`
- https://arxiv.org/abs/2509.18057v2 , https://arxiv.org/abs/2509.18057v4 (Titelvariante
  „Applications to Complexity Theory")
- https://arxiv.org/abs/2603.09172 — dies., „… : Ramsey Numbers" (2026-03-10). `[PREPRINT]`
- https://research.google/blog/ai-as-a-research-partner-advancing-theoretical-computer-science-with-alphaevolve/
  — Google Research Blog (2025). `[CLAIM/Industrie]`
- https://arxiv.org/abs/2404.17012 — Kunisky, Yu, „Computational hardness of detecting graph
  lifts and certifying lift-monotone properties of random regular graphs".
- https://arxiv.org/abs/1303.6437 — Karpinski, Lampis, Schmied, „New Inapproximability Bounds
  for TSP" (ISAAC 2013 / JCSS 2015). `[VERIFIZIERT]`
- https://arxiv.org/html/2608.00333 — „Sharp Hardness for MAX-3-CUT and Quantum MAX-CUT"
  (UGC-bedingte scharfe Schranken; zum Vergleich). `[PREPRINT]`

**AlphaEvolve allgemein / Mathematik**
- https://arxiv.org/abs/2506.13131 — „AlphaEvolve: A coding agent for scientific and
  algorithmic discovery". `[PREPRINT]`
- https://arxiv.org/abs/2511.02864 — Georgiev, Gómez-Serrano, Tao, Wagner, „Mathematical
  exploration and discovery at scale" (v3 2025-12-22). `[PREPRINT]`
- https://terrytao.wordpress.com/2025/11/05/mathematical-exploration-and-discovery-at-scale/
  — Taos eigene Einordnung.
- https://cs.nyu.edu/~davise/papers/AlphaEvolveNotes.pdf — Ernest Davis, „Some comments on
  AlphaEvolve".

**Matrixmultiplikation (Verifikationsauftrag)**
- https://arxiv.org/abs/2506.13242 — Dumas, Pernet, Sedoglavic, „A non-commutative algorithm
  for multiplying 4x4 matrices using 48 non-complex multiplications" (2025-06-16). `[PREPRINT]`
- https://arxiv.org/abs/2603.18699 — „A more accurate rational non-commutative algorithm …" (2026).
- https://arxiv.org/html/2606.13408 — „A catalog of fast matrix multiplication algorithms with
  exhaustive derivations" (Chronologie inkl. Kaporin 2024).
- https://mathstodon.xyz/@robinhouston/114507937280899656 — Robin Houston zur Neuheitsfrage.
- https://mathstodon.xyz/@fredrikj/114508287537669113 — Fredrik Johansson zu Waksman 1970.

**FunSearch und Nachfolger**
- https://www.nature.com/articles/s41586-023-06924-6 — Romera-Paredes et al., „Mathematical
  discoveries from program search with large language models", Nature 625 (2024-01-18). `[VERIFIZIERT]`
- https://cs.nyu.edu/~davise/papers/FunSearchComment.pdf — Ernest Davis, Comment.
- https://arxiv.org/abs/2510.27353 — „An In-depth Study of LLM Contributions to the Bin Packing
  Problem", angenommen ACM TELO, DOI 10.1145/3821574. `[VERIFIZIERT]` (Venue)
- https://arxiv.org/abs/2510.14150 — CodeEvolve. `[PREPRINT]`
- https://arxiv.org/abs/2511.08522 — AlphaResearch. `[PREPRINT]`
- https://arxiv.org/abs/2605.15221 — Ishibashi, Yano, Oyamada, „Effective Harness Engineering
  for Algorithm Discovery with Coding Agents" (2026-05-13). `[PREPRINT]`
- https://arxiv.org/abs/2602.16805 — Gideoni, Risi, Gal, „Simple Baselines are Competitive with
  Code Evolution" (2026-02-18). `[PREPRINT]`
- https://arxiv.org/html/2605.20086 — „What Do Evolutionary Coding Agents Evolve?" (nicht
  vertieft geprüft). `[PREPRINT]`

**SAT**
- https://arxiv.org/abs/2509.07367 — „Autonomous Code Evolution Meets NP-Completeness"
  (SATLUTION, 2025-09-09). `[PREPRINT]`
- https://arxiv.org/abs/1802.03685 — Selsam et al., „Learning a SAT Solver from Single-Bit
  Supervision" (NeuroSAT, ICLR 2019). `[VERIFIZIERT]`
- https://arxiv.org/abs/2110.14053 — Wang et al., „NeuroBack: Improving CDCL SAT Solving using
  Graph Neural Networks", ICLR 2024. `[VERIFIZIERT]`
- https://arxiv.org/abs/2507.22876 — „Discovering heuristics in a complex SAT solver with large
  language models" (AutoModSAT, v2 2026-06-06). `[PREPRINT]`
- https://arxiv.org/abs/2510.16476 — „NP-Engine: Empowering Optimization Reasoning in Large
  Language Models with Verifiable Synthetic NP Problems". `[PREPRINT]`
- https://satcompetition.github.io/2025/satcomp25slides.pdf — Ergebnisse SAT Competition 2025.
- https://cca.informatik.uni-freiburg.de/papers/BiereFallerFleuryFroleyksPollitt-SAT-Competition-2025-solvers.pdf
  — Biere et al., CaDiCaL/Kissat SC2025.
- https://satres.kikit.kit.edu/news/2025-08-15-satcomp/ — MallobSat, Parallel-Track 2025.

**Einordnung**
- https://blog.computationalcomplexity.org/2025/08/ai-and.html — Fortnow (08/2025), AlphaEvolve
  sei „more about optimizing a search space than coming up with a new proof approach".
