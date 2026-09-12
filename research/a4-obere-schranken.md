# A4 — Obere Schranken & SAT-Praxis

**Agent:** A4 "Obere Schranken & SAT-Praxis"
**Stand:** 12.09.2026
**Methodenhinweis (verbindlich nach `docs/02-team-briefing.md` §1):** WebFetch ist in dieser
Umgebung gesperrt. Alle Befunde beruhen auf WebSearch-Snippets und sind daher grundsätzlich
mit `[NUR-SNIPPET]` zu lesen, sofern nicht anders markiert. Wir können belegen, *dass* eine
Arbeit existiert — nicht zuverlässig, *was* in ihr steht.

**Forschungsfrage:** Was können wir bei NP-vollständigen Problemen algorithmisch tatsächlich —
und warum widerlegt die Praxisleistung von SAT-Solvern die Vermutung P ≠ NP nicht?

**Kernthese dieses Berichts (in einem Satz):**
Der gesamte algorithmische Fortschritt bei NP-vollständigen Problemen — vom besten
3-SAT-Algorithmus über Held–Karp bis zu CDCL-Solvern mit Millionen Variablen — findet
*innerhalb* des exponentiellen Regimes statt und bewegt sich, quantitativ gemessen,
seit ca. 2011 asymptotisch auf null Fortschritt zu. Die Praxisleistung von SAT-Solvern
ist keine Evidenz für P = NP, sondern ein Befund über die *Struktur realer Instanzen*;
die Solver-Technologie hat sogar eine **unbedingte, nicht von P vs. NP abhängige
untere Schranke** (Haken 1985 via CDCL≈Resolution), die sie nachweislich nicht überwindet.

---

## 1. Exakte Algorithmen: die tatsächlichen Zahlen

### 1.1 3-SAT — der derzeitige Weltrekord

| Ergebnis | Schranke (general 3-SAT) | Schranke (Unique-3-SAT) | Status |
|---|---|---|---|
| Scheder (& Steinberger), 2024 | O*(1.307031594^n) | O*(1.306972377^n) | `[VERIFIZIERT]` (peer-reviewt) |
| Jiang & Cai, Juli 2026, arXiv:2607.10697 | **O*(1.307031578^n)** | **O*(1.306969598^n)** | `[PREPRINT]` `[NUR-SNIPPET]` |

- Die 2026er Arbeit ist **verifiziert existent**: arXiv:2607.10697, "A Better Analysis For
  PPSZ For 3-SAT", Tao Jiang und Shaowei Cai, eingereicht 12.07.2026. Zwei unabhängig
  formulierte Suchanfragen lieferten übereinstimmend dieselben acht bzw. neun signifikanten
  Stellen. Konfidenz, dass die Zahlen so im Abstract stehen: **hoch**. Konfidenz, dass der
  Beweis korrekt ist: **nicht bewertbar** (Preprint, nicht begutachtet).
- Methodisch laut Snippet: Scheders reguläre/irreguläre Schätzungen bleiben unverändert,
  ersetzt wird nur die Endrekombination durch ein explizites **LP-Dualzertifikat**.
  Das ist bemerkenswert: Der Fortschritt ist rein **analytisch**, nicht algorithmisch —
  es ist derselbe PPSZ-Algorithmus von 1998, nur schärfer analysiert. `[NUR-SNIPPET]`
- **Zuordnungsvorbehalt:** Scheder hat 2024 *zwei* einschlägige Arbeiten
  ("PPSZ is better than you think", TheoretiCS Bd. 3, Art. 5, 2024; und mit J. P. Steinberger
  "PPSZ for General k-SAT and CSP — Making Hertli's Analysis Simpler and 3-SAT Faster",
  *computational complexity* 33, Art. 13, 2024, doi:10.1007/s00037-024-00259-y).
  Welche der beiden genau den Wert 1.307031594 liefert, konnte ohne Volltext **nicht**
  sauber getrennt werden. Eine Suchantwort schrieb die 1.307031578 fälschlich der
  *computational-complexity*-Arbeit zu — das ist mit dem Jiang/Cai-Abstract unvereinbar
  und ein Beispiel für Snippet-Synthesefehler. Konfidenz der Zuordnung: **niedrig**.

**Größenordnung des Fortschritts 2024 → 2026:**
1.307031594 → 1.307031578, also Δ = 1,6 · 10⁻⁸ in der **achten Nachkommastelle**.
Für n = 1000 Variablen entspricht das einem Speedup von Faktor
exp(1000 · 1,6·10⁻⁸/1,307) ≈ 1,0000122 — also **0,0012 %**. `[EIGENE EINSCHÄTZUNG]`

### 1.2 Allgemeines k-SAT

- Alle bekannten k-SAT-Algorithmen haben Laufzeit der Form 2^(n(1 − c/k + o(1/k))):
  **c = 1** für PPZ (Paturi–Pudlák–Zane 1997), **c ≈ 1,44** für Schöning (1999),
  **c ≈ 1,64** für PPSZ. `[NUR-SNIPPET]`
- Hansen, Kaplan, Zamir, Zwick, "Faster k-SAT algorithms using biased-PPSZ",
  STOC 2019 — systematische Nutzung von "Bias" bei der Variablenvorhersage,
  exponentielle (aber im Exponenten winzige) Verbesserung. `[VERIFIZIERT]` (peer-reviewt, STOC)
- **Entscheidend für die Gesamtfrage:** Die Form 2^(n(1−c/k)) heißt, dass der Speedup
  gegenüber Brute-Force 2^n mit wachsendem k **gegen 1 geht**. Für k = 100 ist der
  Basisgewinn bereits vernachlässigbar. Genau das ist der Inhalt von SETH (§2).

### 1.3 TSP, Graphenfärbung, Maximum Independent Set

| Problem | Beste bekannte exakte Laufzeit | Jahr | Bemerkung |
|---|---|---|---|
| TSP (allgemein) | O(n² · 2^n), Speicher O(n · 2^n) | **1962** | Bellman; Held & Karp. Seit **64 Jahren unverbessert** für allgemeine Graphen. |
| TSP (Grad ≤ 3) | 2^(3n/4)·poly = O(1.682^n) | 2012 | nur Spezialfall |
| Chromatische Zahl | 2^n · poly(n), exponentieller Speicher | 2006/09 | Björklund, Husfeldt, Koivisto (Inklusion–Exklusion) |
| Chromatische Zahl, Polyspace | deutlich schlechter als 2^n | 2017 | "Faster Graph Coloring in Polynomial Space" |
| Maximum Independent Set | O(1.1996^n), **Polyspace** | ~2013/17 | Xiao & Nagamochi, arXiv:1312.6260 |
| MIS, vorher | O(1.2109^n), exp. Speicher | 1986 | Robson |

- **Offenes Problem (explizit in der Literatur):** Ob TSP in O(c^n) für ein **c < 2**
  lösbar ist, ist unbekannt. Das ist eine der prominentesten offenen Fragen der exakten
  Algorithmik (Woegingers Problemliste). `[NUR-SNIPPET]`, triangulationsbedürftig, Konfidenz mittel–hoch.
- MIS: 1986 (1.2109) → 2013 (1.1996) = 0,0113 Basisverbesserung in **27 Jahren**, wobei der
  Hauptgewinn die Reduktion von exponentiellem auf polynomiellen Speicher war.
- Der Befund über alle vier Probleme hinweg ist gleichförmig: **kein einziges
  NP-vollständiges Problem hat einen bekannten Algorithmus mit subexponentieller Laufzeit.**
  Das ist ein Negativbefund von 60 Jahren konzentrierter Forschung.

### 1.4 Historische Entwicklung der 3-SAT-Basis — quantitative Extrapolation

Die Basis b in O*(b^n), und der handlichere Exponentenkoeffizient c = log₂(b)
(d. h. Laufzeit 2^(cn); c = 1 wäre Brute-Force, c = 0 wäre Polynomialzeit):

| Jahr | Autor(en) | Basis b | c = log₂ b |
|---|---|---|---|
| 1985 | Monien & Speckenmeyer | 1,618 (φ) | 0,6943 |
| 1998 | Paturi, Pudlák, Saks, Zane (PPSZ) | ~1,362 | ~0,4458 |
| 1999 | Schöning (random walk) | 1,3334 (= 4/3) | 0,4151 |
| 2011 | Hertli (PPSZ gilt auch für general 3-SAT) | ~1,30704 | ~0,38621 |
| 2024 | Scheder (+ Steinberger) | 1,307031594 | 0,3862055… |
| 2026 | Jiang & Cai `[PREPRINT]` | 1,307031578 | 0,3862055… |

*(Zwischenschritte 1992–2006 — Schiermeyer, Kullmann, Hofmeister/Schöning/Schuler/Watanabe,
Iwama–Tamaki, Rolf — liegen alle zwischen 1,58 und 1,32 und sind hier zusammengefasst;
die Einzelwerte konnten ohne Volltext nicht zuverlässig zugeordnet werden.)*

**Die Rate des Fortschritts, in drei Phasen (eigene Rechnung, `[EIGENE EINSCHÄTZUNG]`):**

| Phase | Dauer | Δc | Δc pro Jahr |
|---|---|---|---|
| 1985 → 1999 | 14 J. | 0,2792 | **2,0 · 10⁻²** |
| 1999 → 2011 | 12 J. | 0,0289 | **2,4 · 10⁻³** |
| 2011 → 2026 | 15 J. | ≈ 9,3 · 10⁻⁶ | **6,2 · 10⁻⁷** |

Der Fortschritt hat sich zwischen den Phasen um **Faktor ~8** und dann um
**Faktor ~3900** verlangsamt; insgesamt ist die aktuelle Rate rund **32 000-mal**
kleiner als in der ersten Phase.

**Extrapolation, ehrlich gerechnet:**
- Naiv-linear über den gesamten Zeitraum (0,3081 Δc in 41 Jahren = 7,5·10⁻³/Jahr):
  c = 0 wäre um **2077** erreicht. Diese Zahl ist jedoch **methodisch wertlos**,
  weil sie die drastische Verlangsamung ignoriert.
- Mit der tatsächlichen Rate der letzten 15 Jahre (6,2·10⁻⁷/Jahr):
  c = 0 wäre in **≈ 620 000 Jahren** erreicht.
- Realistischer als beide: Die Entwicklung sieht nicht wie eine Annäherung an 0 aus,
  sondern wie eine **Konvergenz gegen einen positiven Grenzwert** in der Gegend von
  c ≈ 0,386. Genau das ist die Aussage der ETH (§2). Die Datenreihe ist mit der
  ETH **vollständig verträglich** und liefert keinerlei Hinweis auf einen Weg zu c = 0.

**Was heißt das absolut?** 1,307^n für n = 1000 Variablen sind 2^386 ≈ 1,5·10^116
Operationen — mehr als das Quadrat der Anzahl Atome im beobachtbaren Universum (~10^80).
Schon n = 500 ergibt 2^193 ≈ 1,3·10^58. Ein realer Industrie-SAT-Benchmark hat
Millionen Variablen. Die exakten Worst-Case-Algorithmen sind für Praxisgrößen
**vollkommen irrelevant** — sie sind Theorieinstrumente. `[EIGENE EINSCHÄTZUNG]`, Konfidenz hoch.

**Zusätzliches Argument gegen naive Extrapolation:** Selbst wenn die Basis weiter
sänke, führt kein stetiger Weg von b > 1 zu Polynomialzeit. b = 1,01 ist für
n = 10⁶ immer noch 2^14355. Die Größe, die auf 0 gehen müsste, ist c, und die
gesamte bekannte Algorithmik operiert in einem Korridor 0,38 < c < 1.

---

## 2. ETH und SETH

### 2.1 Was sie besagen

- **ETH** (Impagliazzo & Paturi 1999/2001): 3-SAT ist nicht in 2^(o(n)) lösbar; formal
  s₃ > 0, wobei s_k = inf{δ : k-SAT in O*(2^(δn)) lösbar}. `[VERIFIZIERT]`
- **SETH** (dieselben Autoren): lim_(k→∞) s_k = 1. Also: für kein ε > 0 gibt es ein
  Verfahren, das *alle* k-SAT in O*((2−ε)^n) löst. `[VERIFIZIERT]`
- **Sparsification Lemma** (Impagliazzo–Paturi–Zane 2001): ETH bezogen auf die
  Variablenzahl n ist äquivalent zu ETH bezogen auf die Klauselzahl m. `[NUR-SNIPPET]`

### 2.2 Verhältnis zu P ≠ NP — die Implikationskette

```
SETH  ⟹  ETH  ⟹  P ≠ NP
```

Beide sind **strikt stärker** als P ≠ NP:
- Wäre P = NP, wären ETH und SETH falsch. Umgekehrt **nicht**: ETH könnte falsch
  sein (3-SAT in 2^(√n)) und P ≠ NP trotzdem gelten. Subexponentiell ist nicht polynomiell.
- Eine Widerlegung von SETH wäre also **kein** Schritt Richtung P = NP; sie würde nur
  besagen, dass die Basis 2 für großes k unterboten wird.
- Praktische Konsequenz für das Papier: Wer ETH/SETH als "Beleg für P ≠ NP" zitiert,
  argumentiert zirkulär — es sind *stärkere unbewiesene Annahmen*, keine Evidenz.
  Sie sind nützlich, weil sie **quantitative** (fine-grained) untere Schranken liefern,
  die P ≠ NP allein nicht hergibt.

### 2.3 Was daraus folgt (fine-grained complexity)

- Unter ETH: kein f(k)·n^(o(k))-Algorithmus für k-Clique (Chen, Huang, Kanj, Xia). `[NUR-SNIPPET]`
- Unter SETH: Orthogonal-Vectors-Vermutung; daraus u. a. Edit Distance nicht in
  O(n^(2−ε)) (Backurs–Indyk 2015), viele weitere Schranken **innerhalb von P**.
- Übersichtsarbeit 2026: **arXiv:2601.05044**, Jesper Nederlof, "An Invitation to
  'Fine-grained Complexity of NP-Complete Problems'", ca. 40 Seiten, Januar 2026,
  **under review**. Verifiziert existent, Inhalt nur aus Snippet: Ausgangsfrage ist,
  ob die naiven Brute-Force-Baselines bereits optimal sind. `[PREPRINT]` `[NUR-SNIPPET]`

### 2.4 Evidenz und Gegenevidenz 2024–2026

**Pro:**
- Die gesamte Algorithmik liefert nur 2^(n(1−c/k)); die 1/k-Form ist genau das,
  was SETH vorhersagt. Konfidenz: **mittel** (Abwesenheit von Gegenbeispielen ist schwache Evidenz).
- **ECCC-Report 2025/188, "Strong ETH Holds for Bounded-Depth Resolution over Parities"**:
  SETH wird in einem **eingeschränkten Beweissystem** bewiesen. Das ist echter, aber
  eng begrenzter Fortschritt — ein unbedingtes Resultat in einem restriktiven Modell,
  nicht ein Beweis von SETH. `[PREPRINT]` `[NUR-SNIPPET]`, Konfidenz der Existenz: hoch;
  Konfidenz der genauen Aussage: mittel.

**Contra / Vorsicht:**
- **Super-Strong ETH ist FALSCH für zufälliges k-SAT** (Vyas & Williams, arXiv:1810.06081,
  "Super Strong ETH is False for Random k-SAT"). Das ist ein reales negatives Datum:
  eine naheliegende Verschärfung von SETH ist widerlegt. `[NUR-SNIPPET]`
- **NSETH** (Carmosino et al. 2016): Unter einer nichtdeterministischen Variante lassen
  sich SETH-basierte untere Schranken für bestimmte Problemklassen *nicht* mit
  deterministischen Reduktionen beweisen — ein Barrieren-Analogon innerhalb der
  fine-grained complexity. `[NUR-SNIPPET]`
- Explizite Skepsis in der Literatur: SETH sei "nicht über die Härte *eines* Problems,
  sondern über eine *Folge* von Problemen, für die wir jeweils schon schnellere
  Algorithmen als Brute-Force kennen" — was das Vertrauen in SETH (und damit in die
  darauf gestützten Schranken) mindert. `[NUR-SNIPPET]`, Konfidenz mittel.
- **arXiv:2102.02624, "The #ETH is False, #k-SAT is in Sub-Exponential Time"** —
  `[CLAIM]`, **ungeprüft, sehr wahrscheinlich fehlerhaft**. Nicht zitieren ohne
  Prüfung. → siehe §6 (Crank-Alarm).

**Gesamtbewertung:** ETH gilt als weithin plausibel, SETH als deutlich fragiler.
Kein Resultat 2024–2026 hat beide bewiesen oder widerlegt. Konfidenz: **hoch**.

---

## 3. Das SAT-Solver-Paradox (Kernabschnitt)

### 3.1 Die Beobachtung

Moderne CDCL-Solver (Conflict-Driven Clause Learning) lösen routinemäßig industrielle
Boolesche Instanzen mit **Millionen von Variablen und Klauseln** — obwohl SAT
NP-vollständig ist. Das verblüfft Theoretiker wie Solver-Entwickler seit zwei
Jahrzehnten; die gängige Erklärung ist, dass die Solver eine den Industrieinstanzen
innewohnende **Struktur** ausnutzen. `[NUR-SNIPPET]`, breit bezeugt.

### 3.2 Warum das *kein* Argument für P = NP ist — fünf unabhängige Gründe

**(a) Die unbedingte untere Schranke: CDCL ist Resolution.**
CDCL ist (mit Neustarts) **polynomiell äquivalent zur allgemeinen Resolution**
(Pipatsrisawat–Darwiche; Atserias–Fichte–Thurley). Damit gilt jede untere
Resolutionsschranke als untere Schranke für CDCL:
- **Haken 1985**: jeder Resolutionsbeweis des Schubfachprinzips PHP_n hat
  exponentielle Größe. `[VERIFIZIERT]` (publiziert, breit akzeptiert; verschärft von
  Beame–Pitassi u. a.)
- **Tseitin-Formeln über Expandergraphen**: exponentielle Resolutionsschranken
  (Urquhart; später Ben-Sasson–Wigderson). `[VERIFIZIERT]`

Das ist der entscheidende, oft übersehene Punkt: Hier braucht man **keine Annahme über
P vs. NP**. Es ist unbedingt bewiesen, dass die in der Praxis eingesetzte
Solver-Technologie auf explizit angebbaren, winzigen Formelfamilien exponentiell
scheitert. Ein Solver, der 10 Millionen Variablen einer Industrieinstanz bewältigt,
kann an PHP mit ~20 Tauben scheitern. Die Praxisleistung und die Worst-Case-Härte
**koexistieren nachweislich**.

**(b) Die Benchmark-Zahlen selbst widerlegen die Erzählung "SAT ist gelöst".**
SAT Competition 2025, Main-Sequential-Track: Sieger **AE Kissat MAB mit 327 gelösten
Instanzen**, Zweiter kissat-public mit 321. `[NUR-SNIPPET]`, Konfidenz mittel-hoch.
Im Parallel-Track (64 Threads) gewann **MallobSat**. Der kuratierte Benchmark liegt
also gezielt an der Leistungsgrenze: Ein substanzieller Anteil bleibt auch mit
Stunden-Timeouts **ungelöst** — Jahr für Jahr, seit Jahrzehnten.
*(Gesamtzahl der Benchmark-Instanzen noch zu verifizieren; siehe §7 Lücken.)*

**(c) Der Phasenübergang: künstlich erzeugte Instanzen sind sofort hart.**
Zufälliges 3-SAT hat bei Klauseldichte **m/n ≈ 4,267** einen Phasenübergang von
erfüllbar zu unerfüllbar; genau dort liegen im Mittel die **härtesten** Instanzen.
Man kann also für 200 Variablen — lächerlich klein — Instanzen erzeugen, an denen
Solver scheitern, während Industrieinstanzen mit 10⁶ Variablen fallen.
Nuance, wichtig für Redlichkeit: Der Wert 4,267 stammt aus der **Kavitätsmethode**
der statistischen Physik und ist für k = 3 **nicht bewiesen**. Bewiesen sind Schranken
(u. a. ≥ 3,52). **Ding, Sly, Sun** haben die Schwellenwertvermutung für alle
hinreichend großen k bewiesen (Annals of Mathematics) — für k = 3 bleibt sie offen.
`[VERIFIZIERT]` für Ding–Sly–Sun-Existenz; `[NUR-SNIPPET]` für Details.

**(d) Die Struktur-Erklärung ist selbst noch unvollständig — und kein Polynomialitätsbeweis.**
Was genau ausgenutzt wird, ist **unklar**. Die Forschung hat zwei naheliegende
Kandidaten weitgehend verworfen:
- **Treewidth** korreliert nicht gut mit der Lösezeit.
- **Backdoors** korrelieren empirisch nicht stark mit CDCL-Laufzeit.
Als bester bekannter Kandidat gilt die **hierarchische Community-Struktur** (HCS);
Modularität korreliert "moderat" mit der CDCL-Laufzeit, HCS-Parameter sind
prädiktiver als alle früheren Vorschläge. `[NUR-SNIPPET]`, Quellen: Ganesh et al.,
"On the Hierarchical Community Structure of Practical SAT Formulas".
**Entscheidend:** Selbst die beste dieser Erklärungen ist eine *Korrelationsaussage
über eine Instanzverteilung*, kein Theorem über SAT. Sie sagt: "diese Teilmenge ist
leicht", nicht "SAT ist leicht".

**(e) Selektionseffekt.** Industrieinstanzen entstehen aus Verifikations-, Planungs-
und Konfigurationsproblemen, die Menschen so **modellieren**, dass sie lösbar sind.
Instanzen, an denen der Solver scheitert, erscheinen nicht als Erfolgsmeldung, sondern
als abgebrochenes Projekt oder als umformuliertes Modell. Die sichtbare
Erfolgsstatistik ist überlebensverzerrt. `[EIGENE EINSCHÄTZUNG]`, Konfidenz mittel-hoch.

### 3.3 Die logische Form des Fehlschlusses

P = NP ist eine Aussage über **alle** Instanzen und eine **Worst-Case**-Garantie.
Solver-Erfolg ist eine Aussage über eine **empirische Verteilung**. Von
"∃ viele leichte Instanzen" folgt "∀ Instanzen leicht" nicht — das ist derselbe
Quantorenfehler wie bei Knapsacks pseudopolynomiellem DP (vgl. `00-briefing.md`).
Der SAT-Solver-Fehlschluss und der Knapsack-Fehlschluss sind **strukturgleich**:
Beide ersetzen die Quantifizierung über alle Eingaben durch eine über eine
handlichere Teilmenge (kleine Zahlen W / strukturierte Instanzen).
Das entspricht dem Muss-Kriterium **M3** aus `docs/02-team-briefing.md` §4.

### 3.4 Ein konkreter Preis der Praxisleistung

Selbst dort, wo SAT-Technologie große kombinatorische Fragen *entscheidet*, zeigt
der Ressourcenverbrauch die Exponentialität: Die Pythagoreische-Tripel-Frage
(Heule, Kullmann, Marek 2016) erzeugte einen Beweis von ~200 Terabyte; Schur Number 5
(Heule 2017) ~2 Petabyte. `[NUR-SNIPPET]`, noch zu triangulieren. Das sind keine
Polynomialzeit-Erfolge, sondern hochoptimierte Cube-and-Conquer-Suchen mit
massiver Parallelisierung an der Grenze des physisch Machbaren.

---

## 4. Approximation und Parametrisierung

### 4.1 PCP-Theorem und Inapproximierbarkeit

- **PCP-Theorem** (Arora–Safra; Arora, Lund, Motwani, Sudan, Szegedy 1992/98;
  kombinatorischer Beweis: Dinur 2007): NP = PCP(log n, O(1)). `[VERIFIZIERT]`
  Äquivalent zur **Gap-Version** von MAX-3SAT — und *das* ist der Grund, warum
  Approximation überhaupt hart sein kann: Die Reduktion erzeugt eine Lücke.
- **Håstad**: MAX-3SAT ist nicht besser als 7/8 + ε approximierbar (falls P ≠ NP) —
  und 7/8 erreicht bereits eine **zufällige** Belegung. Der Abstand zwischen
  trivialem Algorithmus und Unmöglichkeit ist null. `[VERIFIZIERT]`
- **Håstad, "Clique is hard to approximate within n^(1−ε)"**: für jedes ε > 0
  gibt es eine Reduktion mit Lücke n^(1−ε). `[VERIFIZIERT]` (ursprünglich unter
  NP ≠ ZPP; derandomisiert von Zuckerman 2007 unter P ≠ NP — noch zu triangulieren).

### 4.2 Warum Knapsack ein FPTAS hat und MAX-CLIQUE nicht

Diese Gegenüberstellung ist didaktisch der beste Hebel gegen die verbreitete
Intuition "NP-vollständig = gleich schwer":

| | Knapsack | MAX-CLIQUE |
|---|---|---|
| Härtegrad | **schwach** NP-hart | stark NP-hart |
| Pseudopolynomielles DP | ja, O(nW) bzw. O(n·OPT) | nein |
| Approximation | **FPTAS** (Ibarra–Kim 1975, Lawler 1979) | kein n^(1−ε)-Faktor |
| Grund | Zielfunktionswerte lassen sich **runden**; der Fehler bleibt multiplikativ klein | PCP erzeugt eine **Lücke**, die keine Rundung schließt |

Der Satz, der beides verbindet: **Ein stark NP-hartes Problem hat kein FPTAS,
außer P = NP** (Garey–Johnson). Knapsacks FPTAS existiert genau deshalb, weil
Knapsack *nicht* stark NP-hart ist — dieselbe Tatsache, die den pseudopolynomiellen
DP ermöglicht. `[VERIFIZIERT]`, Konfidenz hoch.
**Konsequenz für das Papier:** "Knapsack ist NP-vollständig und wir lösen es täglich"
ist ebenso ein Fehlschluss wie das SAT-Solver-Argument, und zwar aus demselben Grund.

### 4.3 APX-Härte

MAX-3SAT ist **APX-vollständig**; ein PTAS für irgendein APX-hartes Problem
implizierte P = NP. Die Klassenlandschaft (FPTAS ⊊ PTAS ⊊ APX ⊊ … ) ist also
**kein Kontinuum von Leichtigkeit**, sondern eine durch das PCP-Theorem erzwungene
Stufung. `[VERIFIZIERT]`, Konfidenz hoch.

### 4.4 Unique Games Conjecture nach dem 2-to-2-Games-Theorem

- **Khot, Minzer, Safra** haben die **2-to-2-Games-Vermutung** (mit unvollständiger
  Completeness) bewiesen. Das gilt als "starke Evidenz" für die UGC und geht in einem
  technischen Sinn **"halbwegs"** (halfway) zur UGC. Folgen u. a.: verbesserte
  NP-Härte für Vertex Cover. `[VERIFIZIERT]` (STOC/FOCS-Ergebnis, breit rezipiert).
- **Aber:** Die UGC selbst ist **offen**. Und es gibt ernstzunehmende Gegenevidenz:
  Arora, Barak und Steurer gaben einen **subexponentiellen** Algorithmus
  (2^(n^ε)) für Unique Games. Ein Problem mit subexponentiellem Algorithmus kann
  unter ETH nicht über lineargroße Reduktionen NP-hart sein — die UGC ist damit
  ein qualitativ anderes Tier als P ≠ NP. `[NUR-SNIPPET]`, noch zu triangulieren.
- **Antwort auf die Auftragsfrage:** Die UGC gilt nach 2018 als *eher wahrscheinlich
  wahr* als davor, aber **nicht als Konsens**. Formulierung im Papier sollte lauten:
  "signifikant gestärkt, nicht entschieden". Konfidenz: **mittel**.

### 4.5 W-Hierarchie und FPT

- FPT ⊆ W[1] ⊆ W[2] ⊆ … ⊆ XP. **k-Clique ist W[1]-vollständig** — eines der
  ersten W[1]-vollständigen Probleme und das Leitproblem der parametrisierten
  Intraktabilität. `[VERIFIZIERT]`
- Unter ETH: kein f(k)·n^(o(k))-Algorithmus für Clique (Chen, Huang, Kanj, Xia). `[NUR-SNIPPET]`
- Neuere Verschärfungen (2021–2024): **konstantfaktor-Approximation von k-Clique ist
  W[1]-hart** (Bingkai Lin, STOC 2021); das Inapproximationsverhältnis wurde auf
  **k^(o(1))** verbessert (Karthik C.S. & Khot; unabhängig Lin, Ren, Sun, Wang, unter ETH).
  Unter Gap-ETH gibt es keine o(k)-FPT-Approximation. `[NUR-SNIPPET]`, Konfidenz mittel-hoch.
- **Befund:** Der Parametrisierungs-Ausweg ist an genau denselben Problemen blockiert
  wie der Approximations-Ausweg. Vertex Cover ist FPT (O*(1,2738^k)), Clique ist es
  nicht — und selbst die *Approximation* von Clique ist es nicht. Die Härte verschwindet
  nicht, sie wandert nur in einen anderen Parameter.

---

## 5. Quantencomputer

### 5.1 Warum NP-vollständige Probleme nicht effizient fallen

**(a) Grover ist nur quadratisch — und für 3-SAT sogar *schlechter* als klassisch.**
Grover (1996) durchsucht N Möglichkeiten in O(√N). Auf die 2^n Belegungen angewandt:
2^(n/2) = **1,4142^n**. Der beste bekannte **klassische** Algorithmus liegt bei
**1,30703^n**. Grover auf Brute-Force ist also *langsamer* als klassische Algorithmik.
Erst die Kombination von Grover mit Schöning/PPSZ schlägt beides (z. B. quantisiertes
Schöning ≈ (4/3)^(n/2) = 1,1547^n; Ambainis 2004) — und bleibt **exponentiell**.
`[EIGENE EINSCHÄTZUNG]` für die Gegenüberstellung (die Einzelzahlen sind belegt),
Konfidenz hoch. **Das ist die wirkungsvollste Einzelzahl des Abschnitts.**

**(b) BBBV-Schranke.** Bennett, Bernstein, Brassard, Vazirani (1997): relativ zu einem
zufälligen Orakel braucht jeder Quantenalgorithmus Ω(√N) Anfragen — Grover ist
**optimal** für Black-Box-Suche. Damit gibt es Orakel-Evidenz für NP ⊄ BQP, und
insbesondere: "Das BBBV-Theorem sagt uns, dass es keinen einfachen Beweis von
NP ⊆ BQP gibt, der das NP-Problem einfach als Black Box behandelt." `[VERIFIZIERT]`

**(c) Keine strukturelle Evidenz in *beide* Richtungen.** Aaronson formuliert den
Stand nüchtern: Es gibt **keine strukturelle Evidenz**, die die Vermutung NP ⊄ BQP
mit vorquantischen Überzeugungen über Komplexitätsklassen verbindet. Niemand weiß,
wie man zeigen könnte, dass aus NP ⊆ BQP auch P = NP folgt. `[NUR-SNIPPET]`,
Quelle: Aaronson, "NP-complete Problems and Physical Reality"; "The Acrobatics of BQP"
(Aaronson, Ingram, Kretschmer, CCC 2022). Konfidenz hoch.

**(d) Die entscheidende Asymmetrie (wichtig für den Crank-Filter):**
Da **P ⊆ BQP**, gilt: **NP ⊄ BQP ⟹ P ≠ NP**. Jede Arbeit, die behauptet zu beweisen,
NP-harte Probleme lägen nicht in BQP, behauptet damit implizit einen Beweis von
P ≠ NP und unterliegt allen drei Barrieren. Das ist ein sofort anwendbares
Ausschlusskriterium. `[EIGENE EINSCHÄTZUNG]`, Konfidenz hoch.
→ Konkreter Fall: **arXiv:2311.05624, "NP-hard problems are not in BQP"** (v3, Okt. 2024).
`[CLAIM]`, nach obiger Logik ein impliziter P-≠-NP-Beweis. Bis zum Nachweis des
Gegenteils als Crank-Kandidat zu behandeln. → §6.

### 5.2 Stand 2025/2026 und seriöse Gegenstimmen

- Die Suche ergab **keine** ernsthafte, peer-reviewte Gegenposition 2025/2026, die
  NP ⊆ BQP für plausibel hielte. Die Konsensformulierung lautet: "heute scheint es
  **extrem unwahrscheinlich**, dass Quantencomputer NP-vollständige Probleme in
  Polynomialzeit lösen." `[NUR-SNIPPET]`, Konfidenz hoch.
- **Negativbefund als Ergebnis:** Zu 2025/2026 spezifisch konnte ich keine relevanten
  neuen Resultate finden. Quantum-Advantage-Demonstrationen betreffen
  Sampling-Probleme, nicht NP-vollständige Entscheidungsprobleme — die Kategorien
  werden in der Berichterstattung regelmäßig vermengt.

---

## 6. Crank-Alarm: zu prüfende Claims aus dieser Recherche

| Claim | Quelle | Einschätzung |
|---|---|---|
| "#ETH is False, #k-SAT is in Sub-Exponential Time" | arXiv:2102.02624 | `[CLAIM]`, ungeprüft. Hätte enorme Konsequenzen; keine Rezeption gefunden. Hohe Crank-Wahrscheinlichkeit. |
| "NP-hard problems are not in BQP" | arXiv:2311.05624 (v3, 2024) | `[CLAIM]`. Impliziert P ≠ NP (da P ⊆ BQP). Muss M1–M5 bestehen; keine Rezeption gefunden. |

Beide sind **nicht** als Belege zu verwenden. Sie werden hier nur dokumentiert,
weil sie in Suchergebnissen zu diesem Themenkomplex auftauchen und ein späterer
Agent sonst über dieselben Treffer stolpert.

---

## 7. Verifikationslücken (ehrlich)

1. **Zuordnung der Zahl 1,307031594** zu einer der beiden Scheder-2024-Arbeiten:
   ungeklärt. Eine Suchantwort war nachweislich falsch.
2. **Zwischenschritte der 3-SAT-Historie 1992–2006** (Schiermeyer, Kullmann, HSSW,
   Iwama–Tamaki, Rolf): Existenz belegt, Einzelwerte nicht.
3. **Gesamtzahl der Instanzen** im SAT-Competition-2025-Main-Track (327 gelöst von wie vielen?).
4. **SAT Competition 2026**: existiert (satcompetition.github.io/2026/), Ergebnisdetails
   nicht über Snippets zugänglich.
5. **Held–Karp-Jahr** (1962) und die Aussage "seither unverbessert": mehrfach bestätigt,
   aber nie am Volltext.
6. **ECCC 2025/188** (Strong ETH für bounded-depth resolution over parities):
   Titel belegt, Aussage nur aus Titel erschlossen.
7. **Arora–Barak–Steurer** subexponentieller Unique-Games-Algorithmus: aus Vorwissen,
   in dieser Recherche nicht direkt bestätigt.

---

## 8. Antwort auf die Leitachse (`docs/02-team-briefing.md` §2)

**Der Befund bestätigt die Achse und schärft sie.**

Die exakten Algorithmen (§1) sind der Reinfall-Testfall für B1/B2/B3:
- **B1 (endliches Objekt):** verletzt. Gesucht ist nicht ein Gadget, sondern ein
  **Algorithmus mit Korrektheitsbeweis für alle Eingaben**.
- **B2 (billiges Verifikationsorakel):** verletzt. Ob eine Analyse eine Basis von
  1,307031578 liefert, ist eine *Beweisfrage*, nicht eine Auswertungsfrage.
- **B3 (Lifting-Rahmen):** verletzt.

Die Jiang/Cai-Arbeit 2026 ist dafür ein **perfektes Kontrastbeispiel**: Ihr Fortschritt
kommt aus einem **LP-Dualzertifikat** — also aus einem endlichen, maschinell findbaren
Objekt in einem von Menschen bewiesenen Rahmen (Scheders Analyse). Genau dort, wo B1–B3
lokal erfüllt sind (finde ein LP-Zertifikat, das die Rekombination verbessert), gibt es
Fortschritt — und er beträgt **1,6 · 10⁻⁸ in der Basis**. Das ist die Achse in
Reinform: Wo der Suchkern endlich ist, kommt man weiter; wie weit, zeigt die achte
Nachkommastelle.

**Der SAT-Solver-Befund (§3) liefert eine Ergänzung zur Achse**, die im Briefing so
noch nicht steht: Es gibt einen vierten Typ scheinbaren Fortschritts, nämlich
**Verteilungserfolg statt Worst-Case-Fortschritt**. CDCL-Solver sind der
erfolgreichste Fall angewandter NP-Algorithmik überhaupt — und sie haben die
Worst-Case-Schranke um **exakt null** verbessert (Haken 1985 gilt unverändert).
Wer KI-Fortschritt bei NP-Problemen bewertet, muss diese Unterscheidung zusätzlich
zu B1–B3 mitführen: **"löst mehr Instanzen" ≠ "verschiebt eine Schranke"**.

---

## 9. Quellen

- arXiv:2607.10697 — Tao Jiang, Shaowei Cai, "A Better Analysis For PPSZ For 3-SAT", Juli 2026. https://arxiv.org/abs/2607.10697 `[PREPRINT]`
- Scheder, Steinberger, "PPSZ for General k-SAT and CSP — Making Hertli's Analysis Simpler and 3-SAT Faster", *computational complexity* 33, Art. 13 (2024). https://link.springer.com/article/10.1007/s00037-024-00259-y `[VERIFIZIERT]`
- Scheder, "PPSZ is better than you think", *TheoretiCS* 3, Art. 5 (2024). https://theoretics.episciences.org/13222 `[VERIFIZIERT]`
- Hansen, Kaplan, Zamir, Zwick, "Faster k-SAT algorithms using biased-PPSZ", STOC 2019. https://dl.acm.org/doi/10.1145/3313276.3316359 `[VERIFIZIERT]`
- Xiao, Nagamochi, "Exact Algorithms for Maximum Independent Set", arXiv:1312.6260. https://arxiv.org/abs/1312.6260
- Bonamy et al., "Faster Graph Coloring in Polynomial Space", arXiv:1607.06201. https://arxiv.org/pdf/1607.06201
- Nederlof, "An Invitation to 'Fine-grained Complexity of NP-Complete Problems'", arXiv:2601.05044 (Jan. 2026). https://arxiv.org/abs/2601.05044 `[PREPRINT]`
- Vyas, Williams, "Super Strong ETH is False for Random k-SAT", arXiv:1810.06081. https://arxiv.org/pdf/1810.06081
- ECCC 2025/188, "Strong ETH Holds for Bounded-Depth Resolution over Parities". https://eccc.weizmann.ac.il/report/2025/188/ `[PREPRINT]`
- Wikipedia, "Exponential time hypothesis". https://en.wikipedia.org/wiki/Exponential_time_hypothesis
- Haken, "The intractability of resolution" (1985); vgl. Beame, Pitassi, "Exponential Lower Bounds for the Pigeonhole Principle". https://www.cs.toronto.edu/~toni/Papers/p200-beame.pdf `[VERIFIZIERT]`
- Ganesh et al., "On the Hierarchical Community Structure of Practical SAT Formulas". https://www.cs.toronto.edu/~noahfleming/papers/HCS.pdf
- "On the Hardness of SAT with Community Structure", arXiv:1602.08620. https://arxiv.org/pdf/1602.08620
- SAT Competition 2025, Ergebnisfolien. https://satcompetition.github.io/2025/satcomp25slides.pdf
- SAT Competition 2026. https://satcompetition.github.io/2026/
- MallobSat, SAT Competition 2025 (Parallel-Track). https://satres.kikit.kit.edu/news/2025-08-15-satcomp
- Ding, Sly, Sun, "Proof of the satisfiability conjecture for large k", *Annals of Mathematics* 196(1), 2022. https://projecteuclid.org/journals/annals-of-mathematics/volume-196/issue-1/Proof-of-the-satisfiability-conjecture-for-large-k/10.4007/annals.2022.196.1.1.short `[VERIFIZIERT]`
- Håstad, "Clique is hard to approximate within n^(1−ε)". https://www.cs.umd.edu/~gasarch/TOPICS/pcp/hastadclique.pdf `[VERIFIZIERT]`
- Khot, Minzer, Safra, "Towards a proof of the 2-to-1 games conjecture?", STOC 2018. https://dl.acm.org/doi/10.1145/3188745.3188804 `[VERIFIZIERT]`
- Barak, "Unique Games Conjecture – halfway there?" (2018). https://windowsontheory.org/2018/01/10/unique-games-conjecture-halfway-there/
- Chen, Huang, Kanj, Xia, "Linear FPT Reductions and Computational Lower Bounds", STOC 2004. https://www.cs.lafayette.edu/~gexia/research/stoc04.pdf
- Lin; Karthik C.S. & Khot; Lin, Ren, Sun, Wang — k-Clique-FPT-Inapproximierbarkeit. https://arxiv.org/pdf/2304.02943
- Aaronson, "NP-complete Problems and Physical Reality". https://www.scottaaronson.com/papers/npcomplete.pdf `[VERIFIZIERT]`
- Aaronson, Ingram, Kretschmer, "The Acrobatics of BQP", CCC 2022. https://arxiv.org/pdf/2111.10409 `[VERIFIZIERT]`
- Aaronson, Vorlesungsnotizen zu Grover/BBBV. https://www.scottaaronson.com/qclec/24.pdf
- `[CLAIM]` arXiv:2102.02624, "The #ETH is False, #k-SAT is in Sub-Exponential Time" — **ungeprüft, nicht zitieren**
- `[CLAIM]` arXiv:2311.05624, "NP-hard problems are not in BQP" — **ungeprüft, nicht zitieren**
