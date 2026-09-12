# A4 — Obere Schranken & SAT-Praxis

**Agent:** A4 "Obere Schranken & SAT-Praxis"
**Stand:** 12.09.2026 · **Suchbudget:** 30 WebSearch-Anfragen (Limit eingehalten)
**Methodenhinweis (verbindlich nach `docs/02-team-briefing.md` §1):** WebFetch ist gesperrt.
Alle Befunde beruhen auf WebSearch-Snippets und sind grundsätzlich `[NUR-SNIPPET]`, sofern
nicht anders markiert. Wir können belegen, *dass* eine Arbeit existiert — nicht zuverlässig,
*was* in ihr steht.

**Forschungsfrage:** Was können wir bei NP-vollständigen Problemen algorithmisch tatsächlich —
und warum widerlegt die Praxisleistung von SAT-Solvern die Vermutung P ≠ NP nicht?

**Kernthese:** Der gesamte algorithmische Fortschritt bei NP-vollständigen Problemen findet
*innerhalb* des exponentiellen Regimes statt und ist, quantitativ gemessen, seit ca. 2011
faktisch zum Stillstand gekommen. Die Praxisleistung von SAT-Solvern ist keine Evidenz für
P = NP, sondern ein Befund über die *Struktur realer Instanzen*; die Solver-Technologie
unterliegt sogar einer **unbedingten, nicht von P vs. NP abhängigen unteren Schranke**
(Haken 1985 über CDCL ≈ Resolution), die sie nachweislich nicht überwindet.

---

## 1. Exakte Algorithmen: die tatsächlichen Zahlen

### 1.1 3-SAT — der derzeitige Weltrekord

| Ergebnis | general 3-SAT | Unique-3-SAT | Status |
|---|---|---|---|
| Scheder (2024) | O*(1.307031594ⁿ) | O*(1.306972377ⁿ) | `[VERIFIZIERT]` (peer-reviewt) |
| Jiang & Cai, Juli 2026, arXiv:2607.10697 | **O*(1.307031578ⁿ)** | **O*(1.306969598ⁿ)** | `[PREPRINT]` `[NUR-SNIPPET]` |

- **Verifiziert existent:** arXiv:2607.10697, "A Better Analysis For PPSZ For 3-SAT",
  Tao Jiang und Shaowei Cai, eingereicht 12.07.2026. Zwei unabhängig formulierte
  Suchanfragen lieferten übereinstimmend dieselben acht bzw. neun signifikanten Stellen.
  Konfidenz, dass die Zahlen so im Abstract stehen: **hoch**. Konfidenz in die
  Beweiskorrektheit: **nicht bewertbar** (Preprint, nicht begutachtet).
- Methodisch laut Snippet: Scheders reguläre/irreguläre Schätzungen bleiben unverändert,
  ersetzt wird nur die Endrekombination durch ein explizites **LP-Dualzertifikat**.
  Bemerkenswert: Der Fortschritt ist rein **analytisch**, nicht algorithmisch — es ist
  derselbe PPSZ-Algorithmus von 1998, nur schärfer analysiert. `[NUR-SNIPPET]`
- **Zuordnungsvorbehalt (Konfidenz niedrig):** Scheder hat 2024 *zwei* einschlägige
  Arbeiten — "PPSZ is better than you think", *TheoretiCS* 3, Art. 5 (2024); und mit
  J. P. Steinberger "PPSZ for General k-SAT and CSP — Making Hertli's Analysis Simpler and
  3-SAT Faster", *computational complexity* 33, Art. 13 (2024), doi:10.1007/s00037-024-00259-y.
  Welche der beiden genau den Wert 1,307031594 liefert, ließ sich ohne Volltext **nicht**
  trennen. Eine Suchantwort schrieb die 1,307031578 fälschlich der
  *computational-complexity*-Arbeit zu — unvereinbar mit dem Jiang/Cai-Abstract und ein
  Musterfall für Snippet-Synthesefehler (Briefing §1).

**Größenordnung des Fortschritts 2024 → 2026:** Δ = 1,6 · 10⁻⁸, also die **achte
Nachkommastelle**. Für n = 1000 Variablen ist das ein Speedup von Faktor
exp(1000 · 1,6·10⁻⁸/1,307) ≈ 1,0000122 — **0,0012 %**. `[EIGENE EINSCHÄTZUNG]`

### 1.2 Allgemeines k-SAT

- Alle bekannten k-SAT-Algorithmen haben Laufzeit der Form 2^(n(1 − c/k + o(1/k))):
  **c = 1** für PPZ (1997), **c ≈ 1,44** für Schöning (1999), **c ≈ 1,64** für PPSZ.
  `[NUR-SNIPPET]`, Konfidenz mittel-hoch (in zwei Suchen konsistent).
- Hansen, Kaplan, Zamir, Zwick, "Faster k-SAT algorithms using biased-PPSZ", STOC 2019 —
  systematische Nutzung von "Bias" bei der Variablenvorhersage. `[VERIFIZIERT]` (STOC).
- **Entscheidend:** Die Form 2^(n(1−c/k)) heißt, dass der Speedup gegenüber Brute-Force 2ⁿ
  mit wachsendem k **gegen 1 geht**. Für k = 100 ist der Basisgewinn vernachlässigbar.
  Genau das ist die Aussage von SETH (§2).

### 1.3 TSP, Graphenfärbung, Maximum Independent Set

| Problem | Beste bekannte exakte Laufzeit | Jahr | Bemerkung |
|---|---|---|---|
| TSP (allgemein) | O(n² · 2ⁿ), Speicher O(n · 2ⁿ) | **1962** | Bellman; Held & Karp. Seit **64 Jahren unverbessert**. |
| TSP (Grad ≤ 3) | 2^(3n/4)·poly = O(1.682ⁿ) | ~2012 | nur Spezialfall |
| Chromatische Zahl | 2ⁿ · poly(n), exp. Speicher | 2006/09 | Björklund, Husfeldt, Koivisto (Inklusion–Exklusion) |
| Chromatische Zahl, Polyspace | schlechter als 2ⁿ | 2017 | "Faster Graph Coloring in Polynomial Space" |
| Maximum Independent Set | O(1.1996ⁿ), **Polyspace** | ~2013/17 | Xiao & Nagamochi, arXiv:1312.6260 |
| MIS, Vorgänger | O(1.2109ⁿ), exp. Speicher | 1986 | Robson |

- **Explizit offen in der Literatur:** Ob TSP in O(cⁿ) mit **c < 2** lösbar ist, ist
  unbekannt. `[NUR-SNIPPET]`, Konfidenz mittel-hoch. Ein 64 Jahre altes
  Dynamic-Programming-Verfahren von 1962 ist der Stand der Kunst.
- MIS: 1986 (1,2109) → 2013 (1,1996) = 0,0113 Basisverbesserung in **27 Jahren**,
  wobei der Hauptgewinn die Reduktion von exponentiellem auf polynomiellen Speicher war.
- Gleichförmiger Befund über alle Probleme: **kein einziges NP-vollständiges Problem hat
  einen bekannten subexponentiellen Algorithmus.** Ein Negativbefund von 60 Jahren
  konzentrierter Forschung.

### 1.4 Historische Entwicklung der 3-SAT-Basis — quantitative Extrapolation

Basis b in O*(bⁿ) und der handlichere Exponentenkoeffizient c = log₂(b), d. h. Laufzeit
2^(cn); **c = 1** wäre Brute-Force, **c = 0** wäre Polynomialzeit:

| Jahr | Autor(en) | Basis b | c = log₂ b | Status |
|---|---|---|---|---|
| 1985 | Monien & Speckenmeyer | 1,618 (φ) | 0,6943 | `[VERIFIZIERT]` |
| 1992/99 | Kullmann | 1,5045 | 0,5893 | `[NUR-SNIPPET]` |
| 1996 | Schiermeyer | 1,497 | 0,5822 | `[NUR-SNIPPET]` |
| 1998 | PPSZ (Paturi, Pudlák, Saks, Zane) | ~1,362 | ~0,4458 | `[NUR-SNIPPET]` |
| 1999 | Schöning (random walk) | 1,3334 (=4/3) | 0,4151 | `[VERIFIZIERT]` |
| ~2003–06 | Rolf | 1,32216 → 1,32113 | ~0,4023 | `[NUR-SNIPPET]` |
| 2011 | Hertli (PPSZ-Bound gilt auch general) | ~1,30704 | ~0,38621 | `[NUR-SNIPPET]`* |
| 2024 | Scheder | 1,307031594 | 0,3862055… | `[VERIFIZIERT]` |
| 2026 | Jiang & Cai | 1,307031578 | 0,3862055… | `[PREPRINT]` |

*\* Hertlis Arbeit ("3-SAT Faster and Simpler — Unique-SAT Bounds for PPSZ Hold in General",
FOCS 2011, S. 277–284, arXiv:1103.2165) ist **verifiziert existent**; der exakte Wert
1,30704 ließ sich in den Suchen nicht bestätigen und stammt aus dem Standardzitat.
Konfidenz: mittel.*

**Rate des Fortschritts, in drei Phasen** (eigene Rechnung aus obigen c-Werten,
`[EIGENE EINSCHÄTZUNG]`, Konfidenz mittel wegen der Unsicherheit beim Hertli-Wert):

| Phase | Dauer | Δc | Δc pro Jahr | relativ zur 1. Phase |
|---|---|---|---|---|
| 1985 → 1999 | 14 J. | 0,2792 | **2,0 · 10⁻²** | 1 |
| 1999 → 2011 | 12 J. | 0,0289 | **2,4 · 10⁻³** | 8× langsamer |
| 2011 → 2026 | 15 J. | ≈ 9,3 · 10⁻⁶ | **6,2 · 10⁻⁷** | **~32 000× langsamer** |

**Extrapolation, ehrlich gerechnet:**
- *Naiv-linear* über 41 Jahre (Δc = 0,3081 → 7,5·10⁻³/Jahr): c = 0 um **2077**.
  Diese Zahl ist **methodisch wertlos**, weil sie die Verlangsamung ignoriert. Sie wird
  hier nur genannt, weil sie in populären Darstellungen genau so entsteht.
- *Mit der Rate der letzten 15 Jahre* (6,2·10⁻⁷/Jahr): c = 0 in **≈ 620 000 Jahren**.
- *Realistischer als beide:* Die Reihe sieht nicht wie eine Annäherung an 0 aus, sondern
  wie **Konvergenz gegen einen positiven Grenzwert** bei c ≈ 0,386. Genau das behauptet
  die ETH (§2). Die Datenreihe ist mit der ETH **vollständig verträglich** und enthält
  keinen Hinweis auf einen Weg zu c = 0.

**Was heißt das absolut?** 1,307ⁿ für n = 1000 sind 2³⁸⁶ ≈ 1,5·10¹¹⁶ Operationen — mehr
als das Quadrat der Atomzahl im beobachtbaren Universum (~10⁸⁰). Schon n = 500 ergibt
2¹⁹³ ≈ 1,3·10⁵⁸. Reale Industrie-Benchmarks haben **Millionen** Variablen. Die exakten
Worst-Case-Algorithmen sind für Praxisgrößen **vollkommen irrelevant** — sie sind
Theorieinstrumente, keine Werkzeuge. `[EIGENE EINSCHÄTZUNG]`, Konfidenz hoch.

**Zusatzargument gegen die Extrapolationsintuition:** Selbst wenn die Basis weiter sänke,
führt kein stetiger Weg von b > 1 zu Polynomialzeit. b = 1,01 ist für n = 10⁶ immer noch
2^14355. Die Größe, die auf 0 müsste, ist c — und die gesamte bekannte Algorithmik
operiert im Korridor 0,38 < c < 1.

---

## 2. ETH und SETH

### 2.1 Was sie besagen

- **ETH** (Impagliazzo & Paturi 1999/2001): 3-SAT ist nicht in 2^(o(n)) lösbar; formal
  s₃ > 0, mit s_k = inf{δ : k-SAT in O*(2^(δn))}. `[VERIFIZIERT]`
- **SETH**: lim_(k→∞) s_k = 1. Für kein ε > 0 löst ein Verfahren *alle* k-SAT in
  O*((2−ε)ⁿ). `[VERIFIZIERT]`
- **Sparsification Lemma** (Impagliazzo–Paturi–Zane 2001): ETH bezüglich n ist äquivalent
  zu ETH bezüglich der Klauselzahl m. `[NUR-SNIPPET]`

### 2.2 Verhältnis zu P ≠ NP — die Implikationskette

```
SETH  ⟹  ETH  ⟹  P ≠ NP
```

Beide sind **strikt stärker** als P ≠ NP:
- Wäre P = NP, wären ETH und SETH falsch. **Umgekehrt nicht:** ETH könnte falsch sein
  (3-SAT in 2^(√n)) und P ≠ NP trotzdem gelten. Subexponentiell ist nicht polynomiell.
- Eine Widerlegung von SETH wäre **kein** Schritt Richtung P = NP; sie besagte nur, dass
  die Basis 2 für großes k unterboten wird.
- **Konsequenz für das Papier:** Wer ETH/SETH als "Beleg für P ≠ NP" zitiert, argumentiert
  zirkulär — es sind *stärkere unbewiesene Annahmen*, keine Evidenz. Ihr Wert liegt darin,
  **quantitative** (fine-grained) untere Schranken zu liefern, die P ≠ NP allein nicht hergibt.

### 2.3 Was daraus folgt (fine-grained complexity)

- Unter ETH: kein f(k)·n^(o(k))-Algorithmus für k-Clique (Chen, Huang, Kanj, Xia, STOC 2004).
- Unter SETH: Orthogonal-Vectors-Vermutung; daraus u. a. Edit Distance nicht in O(n^(2−ε))
  (Backurs–Indyk 2015) — untere Schranken **innerhalb von P**.
- Übersicht 2026: **arXiv:2601.05044**, Jesper Nederlof, "An Invitation to 'Fine-grained
  Complexity of NP-Complete Problems'", ca. 40 S., Januar 2026, **under review**.
  Verifiziert existent; laut Snippet ist die Leitfrage, **ob die naiven
  Brute-Force-Baselines bereits optimal sind**. `[PREPRINT]` `[NUR-SNIPPET]`

### 2.4 Evidenz und Gegenevidenz 2024–2026

**Pro:**
- Die gesamte Algorithmik liefert nur 2^(n(1−c/k)); die 1/k-Form ist genau, was SETH
  vorhersagt. Konfidenz **mittel** (Abwesenheit von Gegenbeispielen ist schwache Evidenz).
- **ECCC-Report 2025/188, "Strong ETH Holds for Bounded-Depth Resolution over Parities"**:
  SETH wird in einem **eingeschränkten Beweissystem** bewiesen. Echter, aber eng begrenzter
  Fortschritt — ein unbedingtes Resultat im restriktiven Modell, kein Beweis von SETH.
  `[PREPRINT]` `[NUR-SNIPPET]`; Existenz: Konfidenz hoch, genaue Aussage: Konfidenz mittel.

**Contra / Vorsicht:**
- **Super-Strong ETH ist FALSCH für zufälliges k-SAT** (Vyas & Williams, arXiv:1810.06081).
  Reales negatives Datum: eine naheliegende Verschärfung von SETH ist widerlegt.
- **NSETH** (Carmosino et al. 2016): SETH-basierte untere Schranken lassen sich für
  bestimmte Klassen *nicht* über deterministische Reduktionen beweisen — ein
  Barrieren-Analogon **innerhalb** der fine-grained complexity.
- Explizite Skepsis in der Literatur: SETH sei "nicht über die Härte *eines* Problems,
  sondern über eine *Folge* von Problemen, für die wir jeweils schon schnellere Algorithmen
  als Brute-Force kennen"; das mindere das Vertrauen in SETH und die darauf gestützten
  Schranken. Eine Suchantwort formulierte sogar, SETH könne "nicht als plausible
  Komplexitätsannahme bezeichnet werden". `[NUR-SNIPPET]`, Konfidenz mittel.
- `[CLAIM]` **arXiv:2102.02624, "The #ETH is False, #k-SAT is in Sub-Exponential Time"** —
  ungeprüft, keine Rezeption auffindbar. → §6.

**Gesamtbewertung:** ETH gilt als weithin plausibel, SETH als deutlich fragiler. Kein
Resultat 2024–2026 hat eine der beiden bewiesen oder widerlegt. Konfidenz: **hoch**.

---

## 3. Das SAT-Solver-Paradox (Kernabschnitt)

### 3.1 Die Beobachtung

Moderne CDCL-Solver (Conflict-Driven Clause Learning) lösen routinemäßig industrielle
Instanzen mit **Millionen von Variablen und Klauseln** — obwohl SAT NP-vollständig ist.
Die Literatur benennt das offen als Rätsel: Es "verblüfft Theoretiker und
Solver-Entwickler seit zwei Jahrzehnten"; die gängige Erklärung lautet, die Solver nutzten
die den Industrieinstanzen innewohnende **Struktur** aus. `[NUR-SNIPPET]`, breit bezeugt,
Konfidenz hoch.

### 3.2 Warum das *kein* Argument für P = NP ist — fünf unabhängige Gründe

**(a) Die unbedingte untere Schranke: CDCL *ist* Resolution.**
CDCL mit unbeschränkten Neustarts **p-simuliert die allgemeine Resolution** — unabhängig
bewiesen von Pipatsrisawat & Darwiche (2011) und Atserias, Fichte & Thurley. Der Overhead
ist O(n⁴) (Beyersdorff & Böhm später O(n³)): Hat eine Formel über n Variablen eine
Resolutionswiderlegung der Länge L, existiert ein CDCL-Beweis mit O(n⁴·L) Schritten.
`[VERIFIZIERT]` (mehrfach bezeugt, Standardresultat).

Damit ist **jede** untere Resolutionsschranke eine untere CDCL-Schranke:
- **Haken 1985**: Jeder Resolutionsbeweis des Schubfachprinzips PHP_n hat exponentielle
  Größe — die erste untere Schranke für unbeschränkte Resolution überhaupt.
  `[VERIFIZIERT]`, verschärft u. a. von Beame & Pitassi.
- **Tseitin-Formeln über Expandergraphen**: exponentielle Resolutionsschranken (Urquhart;
  später Ben-Sasson & Wigderson). `[VERIFIZIERT]`

**Das ist der entscheidende, meist übersehene Punkt:** Hier braucht man *keine Annahme über
P vs. NP*. Es ist **unbedingt bewiesen**, dass die in der Praxis eingesetzte
Solver-Technologie auf explizit angebbaren, winzigen Formelfamilien exponentiell scheitert.
Ein Solver, der 10 Millionen Variablen bewältigt, kann an PHP mit wenigen Dutzend Tauben
scheitern. Praxisleistung und Worst-Case-Härte **koexistieren nachweislich** —
das SAT-Solver-Argument ist nicht bloß unbewiesen, es ist *empirisch und theoretisch
bereits widerlegt*.

**(b) Die Wettbewerbszahlen selbst widerlegen die Erzählung "SAT ist gelöst".**

| Wettbewerb / Track | Sieger | Gelöst | Timeout |
|---|---|---|---|
| SAT Comp. **2025**, Main sequential | AE Kissat MAB | **327 / 400** | 5000 s |
| SAT Comp. 2025, Main sequential (2.) | kissat-public | 321 / 400 | 5000 s |
| SAT Comp. 2025, Parallel (64 Threads) | MallobSat | — (1. nach PAR-2, 3 % Vorsprung) | — |
| SAT Comp. **2026**, Main sequential | satsuma-iter+kissat | **276** | 5000 s |
| SAT Comp. 2026, Parallel (32 Kerne) | mallob-quick | **300** | 1000 s |
| SAT Comp. 2026, Cloud (800 Kerne) | mallob-quick | **301** | 200 s |

`[NUR-SNIPPET]`, Konfidenz mittel-hoch. *Die Gesamtinstanzzahl 400 und der 5000-s-Timeout
sind für 2025 verifiziert; für 2026 ist die Gesamtzahl **nicht** verifiziert — der
Vergleich 327 vs. 276 ist daher **kein** Beleg für Regress, die Benchmarksets wechseln
jährlich und werden gezielt an der Leistungsgrenze kuratiert.*

Zwei belastbare Lesarten:
1. **2025: 73 von 400 Instanzen blieben in 5000 s ungelöst (18,25 %)** — Jahr für Jahr,
   seit Jahrzehnten, bei einem Benchmark, der gerade *nicht* aus Zufallsinstanzen besteht.
2. **Der Ertrag von Rechenleistung kollabiert.** 2026, jeweils derselbe Benchmark:
   1 Kern × 5000 s → 276; 32 Kerne × 1000 s (= 32 000 Kern-Sekunden, 6,4-fache CPU-Zeit)
   → 300; 800 Kerne × 200 s (= 160 000 Kern-Sekunden, 32-fache CPU-Zeit) → **301**.
   Die letzte Verfünffachung der Kernzahl bringt **eine einzige zusätzliche Instanz**.
   `[EIGENE EINSCHÄTZUNG]`, Konfidenz mittel (Hardware/Timeouts sind nicht identisch,
   die Größenordnung des Effekts ist dennoch aussagekräftig). Genau so sieht
   Exponentialität in der Praxis aus: Mehr Hardware kauft additiv wenige Instanzen,
   nicht eine Klasse.

**(c) Der Phasenübergang: künstliche Instanzen sind sofort hart.**
Zufälliges 3-SAT hat bei Klauseldichte **m/n ≈ 4,267** einen Phasenübergang erfüllbar →
unerfüllbar; dort liegen im Mittel die **härtesten** Instanzen. Man kann also für wenige
hundert Variablen — lächerlich klein — Instanzen erzeugen, an denen Solver scheitern,
während Industrieinstanzen mit 10⁶ Variablen fallen.
**Redlichkeitsnuance:** Der Wert 4,267 stammt aus der **Kavitätsmethode** der
statistischen Physik und ist für k = 3 **nicht bewiesen**. Bewiesen sind Schranken
(u. a. ≥ 3,52, Hajiaghayi–Sorkin / Kaporis et al.). **Ding, Sly & Sun** haben die
Erfüllbarkeitsschwelle für alle hinreichend großen k bewiesen (*Annals of Mathematics*
196(1), 2022) — für **k = 3 bleibt sie offen**. `[VERIFIZIERT]` (Existenz/Venue),
`[NUR-SNIPPET]` (Details).

**(d) Die Struktur-Erklärung ist selbst noch offen — und wäre kein Polynomialitätsbeweis.
Hier gibt es einen expliziten Widerspruch in der Literatur, der nicht geglättet wird:**

- *Ältere Linie* (Ganesh et al., "On the Hierarchical Community Structure of Practical SAT
  Formulas", ~2021): **Treewidth** korreliert nicht gut mit der Lösezeit; **Backdoors**
  korrelieren empirisch nicht stark mit der CDCL-Laufzeit; die **hierarchische
  Community-Struktur (HCS)** sei der bislang prädiktivste Parameter, Modularität korreliere
  "moderat". `[NUR-SNIPPET]`
- *Neuere Linie* (Zhang, Xia, Li, Li, **Vardi**, **Ganesh**, "Understanding CDCL Solvers via
  Scalability Studies and Proofdoors", **arXiv:2605.15506**, 07.08.2026; vgl.
  arXiv:2603.26286 "Proofdoors and Efficiency of CDCL Solvers"): Eine Studie über
  **766 BMC-Familien mit über 76 600 Instanzen** findet **lineares, polynomielles und
  exponentielles** CDCL-Skalierungsverhalten *innerhalb desselben Benchmarks*, **ohne
  offensichtliche syntaktische Unterscheidung** zwischen den Regimen — und stellt fest,
  dass **Treewidth, Klausel-Variablen-Verhältnis *und Community-Struktur* linear von
  exponentiell skalierenden Familien nicht trennen**. Vorgeschlagen wird stattdessen der
  Parameter **"proofdoor"** (Folge von Interpolanten zwischen Formelchunks): auf linearen
  Familien berechnet CDCL kleine Proofdoors inkrementell, auf exponentiellen Familien
  sind sie exponentiell. `[PREPRINT]` `[NUR-SNIPPET]`

**Bewertung des Widerspruchs:** Ganesh ist Mitautor *beider* Arbeiten; die 2026er Arbeit
ist daher als Selbstkorrektur/Verfeinerung zu lesen, nicht als Fremdwiderspruch. Beide
Zahlen und Aussagen werden hier nebeneinander berichtet (Briefing §1). **Für das
Abschlusspapier ist die 2026er Arbeit die stärkere Munition**: Sie zeigt empirisch, dass
CDCL auf realen Industrieinstanzen **auch exponentiell skaliert** — die "SAT ist praktisch
leicht"-Erzählung ist also nicht einmal für Industrieinstanzen durchgehend wahr, und nach
20 Jahren Forschung gibt es **keinen anerkannten strukturellen Parameter**, der die
Leichtigkeit erklärt. Konfidenz **mittel-hoch**.

Selbst der beste dieser Parameter wäre eine *Korrelationsaussage über eine
Instanzverteilung*, kein Theorem über SAT. Er sagt: "diese Teilmenge ist leicht", nicht
"SAT ist leicht".

**(e) Selektionseffekt.** Industrieinstanzen entstehen aus Verifikations-, Planungs- und
Konfigurationsproblemen, die Menschen so **modellieren**, dass sie lösbar werden. Instanzen,
an denen der Solver scheitert, erscheinen nicht als Erfolgsmeldung, sondern als
abgebrochenes Projekt oder umformuliertes Modell. Die sichtbare Erfolgsstatistik ist
überlebensverzerrt. `[EIGENE EINSCHÄTZUNG]`, Konfidenz mittel-hoch.

### 3.3 Die logische Form des Fehlschlusses

P = NP ist eine Aussage über **alle** Instanzen und eine **Worst-Case**-Garantie.
Solver-Erfolg ist eine Aussage über eine **empirische Verteilung**. Von "∃ viele leichte
Instanzen" folgt "∀ Instanzen leicht" nicht.

**Der SAT-Solver-Fehlschluss und der Knapsack-Fehlschluss (`00-briefing.md`) sind
strukturgleich:** Beide ersetzen die Quantifizierung über alle Eingaben durch eine über
eine handlichere Teilmenge (kleine Zahlen W / strukturierte Instanzen). Das entspricht
exakt dem Muss-Kriterium **M3** aus `docs/02-team-briefing.md` §4 ("Wird stillschweigend
über eine eingeschränkte Klasse quantifiziert statt über alle?"). Empfehlung: Im Papier
sollten beide Fehlschlüsse **gemeinsam** und als *eine* Fehlerform präsentiert werden.

### 3.4 Der konkrete Preis der Praxiserfolge

Selbst dort, wo SAT-Technologie große kombinatorische Fragen *entscheidet*, zeigt der
Ressourcenverbrauch die Exponentialität:
- **Boolesches Pythagoreisches Tripel-Problem** (Heule, Kullmann, Marek 2016,
  arXiv:1605.00723, Cube-and-Conquer): DRAT-Beweis von **fast 200 Terabyte**; daraus ein
  komprimiertes Zertifikat von 68 GB. `[VERIFIZIERT]` (mehrfach bezeugt, publiziert; die
  Verifikation wurde später formal nachgezogen, *J. Autom. Reasoning*).
- **Schur Number Five** (Heule 2017, arXiv:1711.08076, AAAI 2018): Antwort n = 160;
  Beweis **zwei Petabyte**, zertifiziert mit einem formal verifizierten Checker.
  `[VERIFIZIERT]`

Das sind keine Polynomialzeit-Erfolge, sondern hochoptimierte, massiv parallele Suchen an
der Grenze des physisch Speicherbaren — für **jeweils eine einzige** kombinatorische Frage.

---

## 4. Approximation und Parametrisierung

### 4.1 PCP-Theorem und Inapproximierbarkeit

- **PCP-Theorem** (Arora–Safra; Arora, Lund, Motwani, Sudan, Szegedy 1992/98;
  kombinatorischer Beweis: Dinur 2007): NP = PCP(log n, O(1)). `[VERIFIZIERT]`
  Äquivalent zur **Gap-Version** von MAX-3SAT — und *das* ist der Grund, warum Approximation
  überhaupt hart sein kann: Die Reduktion erzeugt eine Lücke, die ein Approximations-
  algorithmus schließen müsste.
- **Håstad, "Some Optimal Inapproximability Results"**: Es ist NP-hart zu unterscheiden,
  ob eine 3-CNF erfüllbar ist oder keine Belegung mehr als (7/8 + ε) der Klauseln erfüllt.
  Also: MAX-3SAT nicht besser als 7/8 approximierbar (falls P ≠ NP) — **und 7/8 erreicht
  bereits eine zufällige Belegung**. Der Abstand zwischen trivialem Algorithmus und
  Unmöglichkeit ist **null**. Allgemein: MAX-Ek-SAT nicht besser als 1 − 2^(−k) + ε.
  `[VERIFIZIERT]`
- **Håstad, "Clique is hard to approximate within n^(1−ε)"** (*Acta Mathematica*):
  ursprünglich unter NP ⊄ ZPP; **Zuckerman (2006/07) derandomisierte es mittels Dispersern
  auf die Annahme P ≠ NP**. `[VERIFIZIERT]` (in zwei Suchen konsistent bestätigt).
  Für n = 10⁶ heißt n^(1−ε) ein Approximationsfaktor in der Größenordnung 10⁶ — d. h.
  **kein** nichttrivialer Algorithmus.

### 4.2 Warum Knapsack ein FPTAS hat und MAX-CLIQUE nicht

Didaktisch der beste Hebel gegen die Intuition "NP-vollständig = gleich schwer":

| | Knapsack | MAX-CLIQUE |
|---|---|---|
| Härtegrad | **schwach** NP-hart | stark NP-hart |
| Pseudopolynomielles DP | ja, O(nW) bzw. O(n·OPT) | nein |
| Approximation | **FPTAS** (Ibarra–Kim 1975, Lawler 1979) | nicht einmal Faktor n^(1−ε) |
| Grund | Zielfunktionswerte lassen sich **runden**, der Fehler bleibt multiplikativ klein | PCP erzeugt eine **Lücke**, die keine Rundung schließt |

Der verbindende Satz: **Ein stark NP-hartes Problem hat kein FPTAS, außer P = NP**
(Garey & Johnson). Knapsacks FPTAS existiert genau deshalb, weil Knapsack *nicht* stark
NP-hart ist — dieselbe Tatsache, die den pseudopolynomiellen DP ermöglicht.
`[VERIFIZIERT]`, Konfidenz hoch.
**Konsequenz:** "Knapsack ist NP-vollständig und wir lösen es täglich" ist derselbe
Fehlschluss wie das SAT-Solver-Argument — und die Theorie erklärt hier sogar *präzise, warum*
das eine geht und das andere nicht. Diese Erklärung ist die stärkste verfügbare Antwort
auf die Praxisintuition.

### 4.3 APX-Härte

MAX-3SAT ist **APX-vollständig**; ein PTAS für irgendein APX-hartes Problem implizierte
P = NP. Die Landschaft FPTAS ⊊ PTAS ⊊ APX ⊊ … ist also **kein Kontinuum von Leichtigkeit**,
sondern eine durch das PCP-Theorem erzwungene **Stufung mit bewiesenen Trennungen** (modulo
P ≠ NP). `[VERIFIZIERT]`, Konfidenz hoch.

### 4.4 Unique Games Conjecture nach dem 2-to-2-Games-Theorem

**Gilt UGC als wahrscheinlich wahr? — Die Evidenz ist ausdrücklich gegenläufig.
Beide Seiten werden berichtet:**

*Dafür:*
- **Khot, Minzer, Safra** haben die **2-to-2-Games-Vermutung** (mit unvollständiger
  Completeness) bewiesen — u. a. über die Charakterisierung nicht-expandierender Mengen im
  Shortcode-Graphen vom Grad 2. Das gilt als "**starke Evidenz** für die Wahrheit der UGC"
  und geht in einem technischen Sinn "**halfway**" zur UGC (Barak, "Unique Games
  Conjecture – halfway there?", 2018). Folgen: verbesserte NP-Härte für Vertex Cover u. a.
  `[VERIFIZIERT]` (STOC 2018 / breit rezipiert).

*Dagegen:*
- **Arora, Barak, Steurer**, "Subexponential Algorithms for Unique Games and Related
  Problems": exp(k·n^ε)-Zeit-Algorithmus für Unique Games. Die Autoren selbst formulieren,
  die Resultate "**stop short of refuting the UGC**", legten aber nahe, dass Unique Games
  "**significantly easier than NP-hard problems**" seien. Eine Suchantwort bezeichnete dies
  als "**strong evidence against the UGC**". `[VERIFIZIERT]` (Existenz/Autoren),
  `[NUR-SNIPPET]` (Bewertung).

**Eigene Einordnung (Konfidenz mittel):** Die beiden Befunde sind nicht formal
widersprüchlich — ein subexponentieller Algorithmus schließt NP-Härte nicht aus, macht sie
aber unter ETH über lineargroße Reduktionen unmöglich. Die korrekte Formulierung für das
Papier lautet: **UGC ist seit 2018 signifikant gestärkt, aber nicht entschieden; es gibt
substanzielle algorithmische Gegenevidenz; ein Konsens "wahrscheinlich wahr" besteht
nicht.** Wer nur eine der beiden Seiten zitiert, verzerrt.

### 4.5 W-Hierarchie und FPT

- FPT ⊆ W[1] ⊆ W[2] ⊆ … ⊆ XP. **k-Clique ist W[1]-vollständig** — eines der ersten
  W[1]-vollständigen Probleme und das Leitproblem parametrisierter Intraktabilität.
  `[VERIFIZIERT]`
- Unter ETH: kein f(k)·n^(o(k))-Algorithmus für Clique (Chen, Huang, Kanj, Xia, STOC 2004).
  `[NUR-SNIPPET]`, Konfidenz hoch.
- **Neuere Verschärfungen (2021–2024):** Konstantfaktor-Approximation von k-Clique ist
  **W[1]-hart** (Bingkai Lin, STOC 2021); das Inapproximationsverhältnis wurde auf
  **k^(o(1))** verbessert (Karthik C.S. & Khot; unabhängig Lin, Ren, Sun, Wang, unter ETH;
  vgl. arXiv:2304.02943, "Improved Hardness of Approximating k-Clique under ETH"). Unter
  **Gap-ETH** gibt es keine o(k)-FPT-Approximation. Ein alternatives Framework ohne
  algebraische Zwischenprobleme: Chen, Feng, Laekhanukit, Liu. `[NUR-SNIPPET]`,
  Konfidenz mittel-hoch.
- **Befund:** Der Parametrisierungs-Ausweg ist an genau denselben Problemen blockiert wie
  der Approximations-Ausweg. Vertex Cover ist FPT (O*(1,2738^k)), Clique ist es nicht — und
  seit 2021 ist selbst die *Approximation* von Clique nicht FPT. Die Härte verschwindet
  nicht, sie wandert in einen anderen Parameter. **Diese Konvergenz dreier unabhängiger
  Zugänge (exakt, approximativ, parametrisiert) auf dieselbe Barriere ist die stärkste
  strukturelle Evidenz für P ≠ NP, die dieser Bericht liefert.** `[EIGENE EINSCHÄTZUNG]`,
  Konfidenz mittel-hoch.

---

## 5. Quantencomputer

### 5.1 Warum NP-vollständige Probleme nicht effizient fallen

**(a) Grover ist nur quadratisch — und auf Brute-Force angewandt sogar *schlechter* als
klassische Algorithmik.** Grover (1996) durchsucht N Möglichkeiten in O(√N). Auf 2ⁿ
Belegungen angewandt: 2^(n/2) = **1,4142ⁿ**. Der beste bekannte **klassische** Algorithmus
liegt bei **1,30703ⁿ** (§1.1). Grover-auf-Brute-Force ist also *langsamer* als der Stand
klassischer Technik.
Erst die Kombination von Amplitudenverstärkung mit Schöning schlägt beides:
**O(1,153ⁿ · poly(n))** (Ambainis; in den Suchen als "square-root speedup" von Schönings
1,329…ⁿ bestätigt). Und bleibt **exponentiell**. `[NUR-SNIPPET]` für 1,153;
die Gegenüberstellung 1,414 > 1,307 ist `[EIGENE EINSCHÄTZUNG]`, Konfidenz hoch.
**Das ist die wirkungsvollste Einzelzahl dieses Abschnitts** und ein gutes Gegengift zur
populären Vorstellung, Quantencomputer "probierten alles gleichzeitig".

**(b) BBBV-Schranke.** Bennett, Bernstein, Brassard, Vazirani (1997): Relativ zu einem
zufälligen Orakel braucht jeder Quantenalgorithmus Ω(√N) Anfragen — **Grover ist optimal**
für Black-Box-Suche. Damit existiert Orakel-Evidenz für NP ⊄ BQP. Aaronsons Formulierung:
"Das BBBV-Theorem sagt uns, dass es keinen einfachen Beweis von NP ⊆ BQP zu haben gibt, der
das NP-Problem einfach als Black Box behandelt." `[VERIFIZIERT]`

**(c) Keine strukturelle Evidenz — in *beide* Richtungen.** Aaronson: Es gibt **keine
strukturelle Evidenz**, die die Vermutung NP ⊄ BQP mit vorquantischen Überzeugungen über
Komplexitätsklassen verbindet; niemand weiß, wie man zeigen könnte, dass aus NP ⊆ BQP auch
P = NP folgte. `[NUR-SNIPPET]`, Quellen: Aaronson, "NP-complete Problems and Physical
Reality"; Aaronson, Ingram, Kretschmer, "The Acrobatics of BQP", CCC 2022. Konfidenz hoch.
Die Konsensformulierung lautet: Man vermutet NP ⊄ BQP "aus ähnlichen Gründen, aus denen man
P ≠ NP vermutet — BBBV plus das Ausbleiben jedes vielversprechenden Ansatzes, die
Voraussetzungen dieses Theorems im Worst Case zu umgehen".

**(d) Die entscheidende Asymmetrie — ein sofort anwendbarer Crank-Filter.**
Da **P ⊆ BQP**, gilt: **NP ⊄ BQP ⟹ P ≠ NP.** Jede Arbeit, die behauptet zu beweisen,
NP-harte Probleme lägen nicht in BQP, behauptet damit implizit einen Beweis von P ≠ NP und
unterliegt allen drei Barrieren (M1). `[EIGENE EINSCHÄTZUNG]`, Konfidenz hoch.
→ Konkreter Fall: `[CLAIM]` **arXiv:2311.05624, "NP-hard problems are not in BQP"**
(v3, Okt. 2024). Nach obiger Logik ein impliziter P-≠-NP-Beweis; keine Rezeption
auffindbar. → §6.

### 5.2 Stand 2025/2026 und seriöse Gegenstimmen

- **Negativbefund:** Die Suche ergab **keine** ernsthafte, peer-reviewte Gegenposition
  2025/2026, die NP ⊆ BQP für plausibel hielte. Die Konsensformulierung bleibt: "heute
  scheint es **extrem unwahrscheinlich**, dass Quantencomputer NP-vollständige Probleme in
  Polynomialzeit lösen." `[NUR-SNIPPET]`, Konfidenz hoch.
- Aktuelle Arbeiten (2024–2026) bewegen sich in der erwarteten Bahn: Grover-QAOA für 3-SAT
  mit **quadratischem** Speedup (arXiv:2402.02585); "Assessing fault-tolerant quantum
  advantage for k-SAT with structure" (arXiv:2412.13274); k-SAT mit verallgemeinerter
  Quantenmessung (*npj Quantum Information* 2025). Alle liefern **polynomielle Speedups
  über exponentiellen Laufzeiten**, keine Klassenverschiebung. `[NUR-SNIPPET]`
- **Kategorienfehler, der im Papier adressiert werden sollte:** Quantum-Advantage-
  Demonstrationen betreffen **Sampling**-Probleme, nicht NP-vollständige
  Entscheidungsprobleme. Die Berichterstattung vermengt das regelmäßig.

---

## 6. Crank-Alarm: zu prüfende Claims aus dieser Recherche

| Claim | Quelle | Einschätzung |
|---|---|---|
| "#ETH is False, #k-SAT is in Sub-Exponential Time" | arXiv:2102.02624 | `[CLAIM]`, ungeprüft. Hätte enorme Konsequenzen; keine Rezeption auffindbar. Hohe Crank-Wahrscheinlichkeit. |
| "NP-hard problems are not in BQP" | arXiv:2311.05624 (v3, 2024) | `[CLAIM]`. Impliziert P ≠ NP (da P ⊆ BQP). Müsste M1–M5 bestehen; keine Rezeption auffindbar. |

Beide sind **nicht** als Belege zu verwenden. Sie werden dokumentiert, damit spätere Agenten
nicht unbemerkt über dieselben Treffer stolpern.

---

## 7. Verifikationslücken (ehrlich)

1. **Zuordnung der Zahl 1,307031594** zu einer der beiden Scheder-2024-Arbeiten: ungeklärt.
   Eine Suchantwort war nachweislich falsch.
2. **Hertlis exakter Wert 1,30704** (2011): Arbeit verifiziert existent, Wert nicht
   bestätigt. Er geht in die Phasenrechnung §1.4 ein — bei einem Wert von z. B. 1,30716
   änderte sich die Aussage qualitativ **nicht** (die Verlangsamung bliebe bei Faktor >1000).
3. **Zwischenschritte 1992–2006** (Kullmann 1,5045; Schiermeyer 1,497; Rolf
   1,32216 → 1,32113): jeweils nur über ein Snippet belegt, teils mit unklarer Jahreszahl.
4. **SAT Competition 2026**: Gesamtzahl der Main-Track-Instanzen **nicht** verifiziert;
   der Vergleich 327 (2025) vs. 276 (2026) ist deshalb **nicht** als Trend verwertbar.
5. **ECCC 2025/188**: Titel belegt, Aussage nur aus dem Titel erschlossen.
6. **Held–Karp 1962 / "seither unverbessert"**: mehrfach bestätigt, nie am Volltext.
7. **Ambainis' 1,153ⁿ**: Zahl in zwei Snippets, Originalarbeit nicht identifiziert.
8. Zur **arXiv:2605.15506** (Proofdoors) liegt nur der Abstract-Inhalt vor; die zentrale
   Aussage ("Community-Struktur trennt nicht") ist deshalb `[PREPRINT]` `[NUR-SNIPPET]`.

---

## 8. Antwort auf die Leitachse (`docs/02-team-briefing.md` §2)

**Der Befund bestätigt die Achse und ergänzt sie um eine vierte Kategorie.**

Die exakten Algorithmen (§1) sind der Reinfall-Testfall für B1/B2/B3:
- **B1 (endliches Objekt):** verletzt. Gesucht ist kein Gadget, sondern ein **Algorithmus
  mit Korrektheitsbeweis für alle Eingaben**.
- **B2 (billiges Verifikationsorakel):** verletzt. Ob eine Analyse die Basis 1,307031578
  liefert, ist eine *Beweisfrage*, keine Auswertungsfrage.
- **B3 (menschlicher Lifting-Rahmen):** verletzt.

**Jiang & Cai 2026 ist dafür ein perfektes Kontrastbeispiel.** Ihr Fortschritt kommt aus
einem **LP-Dualzertifikat** — einem endlichen, maschinell findbaren Objekt (B1 ✓) mit
billiger Prüfung (B2 ✓) in einem von Menschen bewiesenen Rahmen, nämlich Scheders Analyse
(B3 ✓). Genau dort, wo B1–B3 **lokal** erfüllt sind, gibt es Fortschritt — und er beträgt
**1,6 · 10⁻⁸ in der Basis**. Das ist die Achse in Reinform: Wo der Suchkern endlich ist,
kommt man weiter; wie weit, zeigt die achte Nachkommastelle. Diese Arbeit ist ein
**idealer Kandidat für ein AlphaEvolve-artiges Verfahren** — und zeigt zugleich exakt,
wie klein der Ertrag dort ist, wo die Methode greift.

**Ergänzung zur Achse (Widerspruch/Erweiterung zum Briefing):** Das Briefing kennt
"Zeugensuche", "offene Quantifizierung" und (nach A7) "Autoformalisierung". Der
SAT-Solver-Befund (§3) legt eine **vierte Kategorie** nahe:

> **Verteilungserfolg statt Worst-Case-Fortschritt.**

CDCL-Solver sind der erfolgreichste Fall angewandter NP-Algorithmik überhaupt — und sie
haben die Worst-Case-Schranke um **exakt null** verbessert; Haken 1985 gilt unverändert und
bindet sie unbedingt. Diese Kategorie ist für die Bewertung von KI-Fortschritt bei
NP-Problemen **unverzichtbar**, weil gelernte Heuristiken, ML-gestützte Branching-Regeln und
LLM-optimierte Solver-Codes genau hier landen: Sie verschieben eine Verteilung, nicht eine
Schranke. Prüfkriterium, analog zu M1–M5:

> **M6 (Vorschlag): Verteilung oder Schranke?** Verbessert der Claim eine
> Worst-Case-Garantie über *alle* Eingaben — oder die Trefferquote auf einer
> Instanzverteilung? Wenn Letzteres: Es ist kein Beitrag zu P vs. NP, egal wie groß
> die Instanzen sind.

Dieselbe Unterscheidung entlarvt auch den Knapsack-Fehlschluss (§4.2) und die
Quanten-Berichterstattung (§5.2). Es ist, so mein Befund, **die häufigste Fehlerform im
gesamten Themenfeld**.

---

## 9. Quellen

**Exakte Algorithmen**
- arXiv:2607.10697 — Jiang, Cai, "A Better Analysis For PPSZ For 3-SAT", Juli 2026. https://arxiv.org/abs/2607.10697 `[PREPRINT]`
- Scheder, Steinberger, "PPSZ for General k-SAT and CSP — Making Hertli's Analysis Simpler and 3-SAT Faster", *computational complexity* 33, Art. 13 (2024). https://link.springer.com/article/10.1007/s00037-024-00259-y `[VERIFIZIERT]`
- Scheder, "PPSZ is better than you think", *TheoretiCS* 3, Art. 5 (2024). https://theoretics.episciences.org/13222 `[VERIFIZIERT]`
- Hertli, "3-SAT Faster and Simpler — Unique-SAT Bounds for PPSZ Hold in General", FOCS 2011, arXiv:1103.2165. https://arxiv.org/abs/1103.2165
- Hansen, Kaplan, Zamir, Zwick, "Faster k-SAT algorithms using biased-PPSZ", STOC 2019. https://dl.acm.org/doi/10.1145/3313276.3316359 `[VERIFIZIERT]`
- Xiao, Nagamochi, "Exact Algorithms for Maximum Independent Set", arXiv:1312.6260. https://arxiv.org/abs/1312.6260
- "Faster Graph Coloring in Polynomial Space", arXiv:1607.06201. https://arxiv.org/pdf/1607.06201
- Kullmann, "New methods for 3-SAT decision and worst-case analysis". https://www.sciencedirect.com/science/article/pii/S0304397598000176

**ETH / SETH / fine-grained**
- Nederlof, "An Invitation to 'Fine-grained Complexity of NP-Complete Problems'", arXiv:2601.05044 (Jan. 2026). https://arxiv.org/abs/2601.05044 `[PREPRINT]`
- Vyas, Williams, "Super Strong ETH is False for Random k-SAT", arXiv:1810.06081. https://arxiv.org/pdf/1810.06081
- ECCC 2025/188, "Strong ETH Holds for Bounded-Depth Resolution over Parities". https://eccc.weizmann.ac.il/report/2025/188/ `[PREPRINT]`
- "Exponential time hypothesis", Wikipedia. https://en.wikipedia.org/wiki/Exponential_time_hypothesis
- Bringmann et al., "More Consequences of Falsifying SETH and the Orthogonal Vectors Conjecture", STOC 2018. https://arxiv.org/pdf/1805.08554

**SAT-Praxis**
- SAT Competition 2025, Ergebnisfolien. https://satcompetition.github.io/2025/satcomp25slides.pdf
- SAT Competition 2025, Tracks (400 Instanzen, 5000 s). https://satcompetition.github.io/2025/tracks.html
- SAT Competition 2026. https://satcompetition.github.io/2026/
- MallobSat / KIT SAtRes, SAT Competition 2025 und 2026. https://satres.kikit.kit.edu/news/2025-08-15-satcomp · https://satres.kikit.kit.edu/news/2026-07-28-floc/
- Zhang, Xia, Li, Li, Vardi, Ganesh, "Understanding CDCL Solvers via Scalability Studies and Proofdoors", arXiv:2605.15506 (Aug. 2026). https://arxiv.org/abs/2605.15506 `[PREPRINT]`
- "Proofdoors and Efficiency of CDCL Solvers", arXiv:2603.26286. https://arxiv.org/pdf/2603.26286 `[PREPRINT]`
- Ganesh et al., "On the Hierarchical Community Structure of Practical SAT Formulas". https://www.cs.toronto.edu/~noahfleming/papers/HCS.pdf
- "On the Hardness of SAT with Community Structure", arXiv:1602.08620. https://arxiv.org/pdf/1602.08620
- Haken (1985); vgl. Beame, Pitassi, "Exponential Lower Bounds for the Pigeonhole Principle". https://www.cs.toronto.edu/~toni/Papers/p200-beame.pdf `[VERIFIZIERT]`
- Pipatsrisawat, Darwiche, "On the power of clause-learning SAT solvers as resolution engines" (p-Simulation von Resolution). https://www.semanticscholar.org/paper/abeb638c9f01f60bdafb2a2aac90d557fd633f7f
- Heule, Kullmann, Marek, "Solving and Verifying the Boolean Pythagorean Triples Problem via Cube-and-Conquer", arXiv:1605.00723 (≈200 TB DRAT). https://arxiv.org/abs/1605.00723 `[VERIFIZIERT]`
- Heule, "Schur Number Five", arXiv:1711.08076, AAAI 2018 (2 PB). https://arxiv.org/abs/1711.08076 `[VERIFIZIERT]`
- Ding, Sly, Sun, "Proof of the satisfiability conjecture for large k", *Annals of Mathematics* 196(1), 2022. https://projecteuclid.org/journals/annals-of-mathematics/volume-196/issue-1/Proof-of-the-satisfiability-conjecture-for-large-k/10.4007/annals.2022.196.1.1.short `[VERIFIZIERT]`

**Approximation / Parametrisierung**
- Håstad, "Some Optimal Inapproximability Results" (MAX-3SAT 7/8). https://www.cs.umd.edu/~gasarch/BLOGPAPERS/max3satl.pdf `[VERIFIZIERT]`
- Håstad, "Clique is hard to approximate within n^(1−ε)", *Acta Mathematica*. https://link.springer.com/article/10.1007/BF02392825 `[VERIFIZIERT]`
- Zuckerman, "Linear Degree Extractors and the Inapproximability of Max Clique and Chromatic Number" (Derandomisierung). https://www.cs.utexas.edu/~diz/pubs/
- Khot, Minzer, Safra, "Towards a proof of the 2-to-1 games conjecture?", STOC 2018. https://dl.acm.org/doi/10.1145/3188745.3188804 `[VERIFIZIERT]`
- Barak, "Unique Games Conjecture – halfway there?" (2018). https://windowsontheory.org/2018/01/10/unique-games-conjecture-halfway-there/
- Arora, Barak, Steurer, "Subexponential Algorithms for Unique Games and Related Problems". https://www.boazbarak.org/Papers/ssesubexp.pdf `[VERIFIZIERT]`
- Chen, Huang, Kanj, Xia, "Linear FPT Reductions and Computational Lower Bounds", STOC 2004. https://www.cs.lafayette.edu/~gexia/research/stoc04.pdf
- "Improved Hardness of Approximating k-Clique under ETH", arXiv:2304.02943. https://arxiv.org/pdf/2304.02943
- Lin, "Constant approximating k-Clique is W[1]-hard", STOC 2021. https://dl.acm.org/doi/10.1145/3406325.3451016

**Quanten**
- Aaronson, "NP-complete Problems and Physical Reality". https://www.scottaaronson.com/papers/npcomplete.pdf `[VERIFIZIERT]`
- Aaronson, Ingram, Kretschmer, "The Acrobatics of BQP", CCC 2022, arXiv:2111.10409. https://arxiv.org/pdf/2111.10409 `[VERIFIZIERT]`
- Aaronson, Vorlesungsnotizen zu Grover/BBBV. https://www.scottaaronson.com/qclec/24.pdf
- "Grover-QAOA for 3-SAT: Quadratic Speedup, Fair-Sampling, and Parameter Clustering", arXiv:2402.02585. https://arxiv.org/abs/2402.02585
- "Assessing fault-tolerant quantum advantage for k-SAT with structure", arXiv:2412.13274. https://arxiv.org/html/2412.13274v3
- "Quantum Search Algorithms" (Ambainis), arXiv:quant-ph/0504012. https://arxiv.org/pdf/quant-ph/0504012

**Nicht zu zitieren (`[CLAIM]`, ungeprüft)**
- arXiv:2102.02624, "The #ETH is False, #k-SAT is in Sub-Exponential Time"
- arXiv:2311.05624, "NP-hard problems are not in BQP"
