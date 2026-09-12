# A2 — Untere Schranken & Schaltkreiskomplexität

**Agent:** A2
**Stand:** 12. September 2026
**Forschungsfrage:** Wie weit sind untere Schranken *modellweise* tatsächlich gekommen — und wo verläuft die Grenze zwischen bewiesen und bedingt?

**Methodische Vorbemerkung (verbindlich nach `docs/02-team-briefing.md` §1).**
WebFetch ist in dieser Umgebung gesperrt; es gab **keinen Volltextzugang** zu einer einzigen
der hier besprochenen Arbeiten. Alles, was unten steht, beruht auf WebSearch-Synthesen über
mehrere, unterschiedlich formulierte Anfragen. Der Normalfall ist daher `[NUR-SNIPPET]`:
Ich kann belegen, *dass* eine Arbeit mit einem bestimmten Titel, Autorensatz und Venue
existiert, und ich kann Kernaussagen über mehrere unabhängige Suchen triangulieren —
ich kann nicht garantieren, dass Nebenbedingungen, Basiskonventionen oder o(·)-Terme
in der Originalarbeit exakt so lauten. Wo zwei Suchen sich widersprechen, steht der
Widerspruch als solcher im Text.

---

## 0. Die fünf Kernbefunde

1. **Die beste bewiesene untere Schranke für ein explizites Problem über der vollen binären
   Basis B₂ ist `3,1n − o(n)` (Li & Yang, STOC 2022).** Sie gilt für *affine dispersers* —
   Funktionen, die in **P** liegen, nicht etwa nur in NP. `[VERIFIZIERT]`, Konfidenz hoch.
2. **Die oft zitierte Zahl `5n − o(n)` gehört zu einer anderen Basis**, nämlich U₂
   (alle zweistelligen Booleschen Funktionen **außer** XOR und XNOR), und stammt von
   Iwama & Morizumi (2002). Sie ist mit 3,1n nicht vergleichbar und darf nicht als
   "besserer Stand" dargestellt werden. `[VERIFIZIERT]`, Konfidenz hoch.
   **Verschärfend:** Amano & Tarui haben gezeigt, dass die dort benutzte Beweistechnik
   bei exakt 5n ihre Grenze erreicht — die 5n sind kein Zwischenstand, sondern das
   Ende einer Methode. `[NUR-SNIPPET]`, Konfidenz mittel-hoch.
3. **Für P ≠ NP wird eine superpolynomielle Schranke n^ω(1) gebraucht. Wir können für
   kein explizites Problem über einer allgemeinen Basis auch nur eine *superlineare*
   Schranke beweisen** — nicht 4n, nicht 10n, nicht n·log n. Der Abstand ist nicht
   quantitativ, sondern qualitativ. `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]`, gestützt
   auf mehrfach konsistente Literaturaussagen.
4. **In eingeschränkten Modellen funktionieren untere Schranken hervorragend** —
   AC⁰ (exponentiell), AC⁰[p] (exponentiell), monotone Schaltkreise (exponentiell),
   ACC⁰ (NEXP, NQP). Jeder dieser Erfolge ist an eine strukturelle Schwäche des Modells
   gekoppelt, die beim Übergang zur vollen Klasse verschwindet. Für Monotonie ist das
   sogar *bewiesen*: Tardos (1988) zeigt eine exponentielle Lücke zwischen monotoner
   und nicht-monotoner Komplexität. `[VERIFIZIERT]`, Konfidenz hoch.
5. **Die lebendigste Angriffslinie ist Williams' "algorithmische Methode"** —
   bessere Algorithmen für Schaltkreisanalyse erzeugen untere Schranken. Sie hat
   2011 den einzigen bekannten Barrieredurchbruch geliefert (NEXP ⊄ ACC⁰, Gödelpreis
   2024) und ist 2024–2026 weiter produktiv (Self-Improvement, OVC→Schaltkreisschranken,
   √-Platz-Simulation). Sie ist zugleich **nicht** in Sichtweite von NP gegen P/poly.
   `[VERIFIZIERT]` für Existenz und Struktur, `[EIGENE EINSCHÄTZUNG]` für die Reichweite.
6. **Nachtrag von erheblichem Gewicht — die Schranke hängt fast ausschließlich davon ab,
   wie *mächtig* die Klasse sein darf, in der die harte Funktion liegen soll.**
   Für NP: 3,1n. Für **S₂E** (symmetric exponential time): **2ⁿ/n**, also
   *nahezu maximal* — Chen, Hirahara, Li, Ren (STOC 2024, JACM 73(1), Feb. 2026), über
   einen Algorithmus für das **Range-Avoidance**-Problem. `[VERIFIZIERT]`, Konfidenz hoch.
   Die Differenz zwischen 3,1n und 2ⁿ/n ist kein Modellunterschied, sondern ein
   **Klassenunterschied**. Wer über "den Stand bei unteren Schranken" redet, muss beides
   angeben: *welches Schaltkreismodell* und *welche Klasse die harte Funktion bewohnt*.
7. **Für die gate-elimination-Technik ist der Endpunkt bewiesen, nicht vermutet.**
   Golovnev, Hirsch, Knop, Kulikov, *On the Limits of Gate Elimination* (MFCS 2016,
   JCSS 2018): Die Methode kann Schranken der Form cn nicht über eine gewisse, nur von
   der Zahl der Substitutionen pro Induktionsschritt abhängende Konstante c hinaus
   liefern — und **keine superlinearen Schranken**. `[VERIFIZIERT]`, Konfidenz hoch.
   Da alle bekannten allgemeinen Schranken auf gate elimination beruhen, heißt das:
   Die aktuelle Technikfamilie ist von einem Satz daran gehindert, jemals 3,1n → n·log n
   zu erreichen.

---

## 1. Die modellweise Landkarte

Die zentrale Beobachtung: **Es gibt nicht "den Stand" bei unteren Schranken. Es gibt einen
Stand pro Modell, und die Stände unterscheiden sich um astronomische Faktoren.** Wer eine
Zahl nennt, ohne das Modell zu nennen, sagt nichts.

| Modell | Beste bewiesene Schranke für *explizite* Funktion | Referenz | Status |
|---|---|---|---|
| Allgemeine Schaltkreise, **volle binäre Basis B₂** | **3,1n − o(n)** | Li & Yang, STOC 2022 | `[VERIFIZIERT]` |
| Allgemeine Schaltkreise, **Basis U₂** (ohne XOR/XNOR) | **5n − o(n)** | Iwama & Morizumi, 2002 | `[VERIFIZIERT]` |
| De-Morgan-**Formeln** | ~n³⁻ᵒ⁽¹⁾ | Håstad 1998 (Verbesserungen Tal u.a.) | `[NUR-SNIPPET]` |
| **Monotone** Schaltkreise (CLIQUE) | exponentiell, exp(Ω(√k)) bzw. exp(cn^{1/6−o(1)}) | Razborov 1985; Alon–Boppana 1987; Tardos 1988 | `[VERIFIZIERT]` |
| **AC⁰** (konst. Tiefe d, PARITY) | exp(Ω(n^{1/(d−1)})) | Furst–Saxe–Sipser 1981 / Ajtai 1983 / Håstad 1986 | `[VERIFIZIERT]` |
| **AC⁰[p]**, p prim (MOD_q, q≠p) | exponentiell | Razborov 1987, Smolensky 1987 | `[VERIFIZIERT]` |
| **ACC⁰** | nur: NEXP ⊄ ACC⁰; NQP ⊄ ACC⁰ | Williams 2011/2014; Murray–Williams 2018 | `[VERIFIZIERT]` |
| **TC⁰** (Schwellwert, konst. Tiefe) | **nichts** — nicht einmal n^{1,1} für LTF-Schaltkreise | Razborov–Wigderson n^{log n} nur für Tiefe 3 mit AND unten | `[NUR-SNIPPET]` |
| **P/poly**, harte Funktion in **S₂E** | **2ⁿ/n** (nahezu maximal) | Chen–Hirahara–Li–Ren, STOC 2024 / JACM 2026 | `[VERIFIZIERT]` |
| **P/poly gegen NP** (das Ziel) | **nichts Superlineares** — nicht einmal 10n | — | offen |

Zum Kalibrieren gehört die **nicht-explizite** Gegenzahl: Nach Shannon (1949) / Lupanov
benötigen *fast alle* Booleschen Funktionen auf n Variablen rund `2ⁿ/n` Gatter. Für n = 1000
ist das eine Zahl mit etwa 298 Dezimalstellen. Unsere beste bewiesene Schranke für eine
*konkret hinschreibbare* Funktion beträgt bei n = 1000 rund **3100 Gatter**. Zwischen
"es gibt harte Funktionen" (Abzählargument, 1949, eine halbe Seite) und "*diese* Funktion
ist hart" (2022, hochtechnische Fallunterscheidung) liegen 295 Größenordnungen.
`[EIGENE EINSCHÄTZUNG, Konfidenz hoch]` — die Shannon-Zahl ist Kanon, die Rechnung ist meine.

### 1.1 Was "Basis" heißt, und warum die Verwechslung 3,1n ↔ 5n systematisch passiert

- **B₂** = *volle* binäre Basis: erlaubt sind alle 16 zweistelligen Booleschen Funktionen,
  insbesondere **XOR** und **XNOR**.
- **U₂** = B₂ **ohne** XOR und XNOR (die "De-Morgan-artige" Basis).

U₂ ist das **schwächere** Modell: Der Schaltkreis darf weniger. Untere Schranken sind dort
folglich **leichter** zu beweisen und **größer**. Eine U₂-Schranke von 5n impliziert **keine**
B₂-Schranke von 5n; sie impliziert über die Standardsimulation (XOR kostet in U₂ mehrere
Gatter) allenfalls eine deutlich schwächere B₂-Aussage. Die Zahlen 3,1n und 5n messen
verschiedene Dinge.

Warum das im Papier explizit adressiert werden muss: Populärdarstellungen und auch manche
Übersichten nennen "die beste bekannte untere Schranke für Schaltkreise" und geben 5n an,
weil das die größere Zahl ist. Die **relevante** Zahl für P vs. NP ist die über der vollen
Basis — denn P/poly ist über der vollen Basis definiert. Sie lautet **3,1n**.
`[EIGENE EINSCHÄTZUNG, Konfidenz hoch]`

### 1.2 Die Chronologie über B₂ — 38 Jahre für 0,1n

| Jahr | Schranke über B₂ | Autoren |
|---|---|---|
| 1984 | 3n − o(n) | Norbert Blum |
| 2016 | (3 + 1/86)·n − o(n) ≈ **3,0116n** | Find, Golovnev, Hirsch, Kulikov (FOCS 2016) |
| 2021/22 | **3,1n − o(n)** | Jiatu Li, Tianqi Yang (ECCC TR21-023; STOC 2022) |

`[VERIFIZIERT]` über mehrere Suchen; die Autorenschaft von Li **und Yang** (nicht Li allein)
ist bestätigt — der Auftrag nannte nur "Jiatu Li", das ist zu ergänzen.

Blums Schranke stand **32 Jahre** unangetastet. Die Verbesserung 2016 betrug **1/86 ≈ 0,0116**
Gatter pro Variable und galt als hochgradig nichttriviale Arbeit (FOCS-Paper, Oded Goldreich
hat sie in seine "choices" aufgenommen). Die Verbesserung 2022 auf 3,1n brachte weitere
≈ 0,088n. **Gesamtfortschritt in 38 Jahren: 0,1 Gatter pro Eingabebit.**

**Die Technik ist in allen drei Fällen dieselbe: *gate elimination*.** Man belegt Eingänge
geschickt mit Konstanten oder affinen Funktionen, argumentiert, dass dabei jedes Mal mehrere
Gatter verschwinden, und zählt. FGHK 2016 erweiterte das um drei Ideen: Schaltkreise mit
Zyklen für affine Substitutionen zuzulassen, ein sorgfältig gewähltes Komplexitätsmaß,
und quadratische Substitutionen als "verzögerte" affine Substitutionen. Li–Yang gewinnen
laut Abstract-Synthese durch eine **erheblich feinere Fallanalyse**, die die Zahl der
"bottleneck structures" im Eliminationsverfahren senkt. `[NUR-SNIPPET]`, Konfidenz mittel.

**Das ist der entscheidende Punkt für die Bewertung:** Der Fortschritt kommt nicht aus einer
neuen Idee, sondern aus immer aufwendigeren Fallunterscheidungen innerhalb *derselben* Idee
von 1984. Das ist das typische Bild einer ausgereizten Methode.
`[EIGENE EINSCHÄTZUNG, Konfidenz mittel-hoch]`

### 1.3 Und über U₂: eine Methode, die nachweislich am Ende ist

- Schnorr, später Zwick (4n), **Lachish & Raz (4,5n − o(n))**, **Iwama & Morizumi (5n − o(n))**.
  Die Schranken gelten für *k-mixed* bzw. *strongly two-dependent* Funktionen. Eine Funktion
  heißt **k-mixed**, wenn je zwei verschiedene Belegungen derselben k Variablen verschiedene
  Restfunktionen erzeugen. Iwama–Morizumi nutzen (n − o(n))-mixed Funktionen. `[NUR-SNIPPET]`
- **Amano & Tarui** konstruieren eine explizite (n − o(n))-mixed Funktion, deren U₂-Komplexität
  **5n + o(n)** beträgt — die k-mixed-Eigenschaft kann also *prinzipiell* keine Schranke
  über 5n liefern. `[NUR-SNIPPET]`, Konfidenz mittel-hoch (über zwei Suchen konsistent,
  Titel: "A Well-Mixed Function with Circuit Complexity 5n ± o(n): Tightness of the
  Lachish–Raz-Type Bounds").

**Das ist methodologisch das interessanteste Einzelresultat dieses Abschnitts:** Es ist ein
*bewiesener* Endpunkt einer konkreten Beweistechnik, keine Vermutung. Es zeigt im Kleinen,
was die drei großen Barrieren im Großen zeigen — dass die verfügbaren Techniken ihre eigene
Obergrenze mitbringen. `[EIGENE EINSCHÄTZUNG, Konfidenz mittel-hoch]`

---

## 2. Eingeschränkte Modelle: wo es funktioniert, und warum es dort funktioniert

### 2.1 AC⁰ — konstante Tiefe, unbeschränkter Fan-In, AND/OR/NOT

**Resultat.** PARITY benötigt in Tiefe d Schaltkreise der Größe exp(Ω(n^{1/(d−1)})).
Furst–Saxe–Sipser (1981) und unabhängig Ajtai (1983) zeigten zunächst nur *superpolynomiell*
(quasipolynomielle Schranke); **Håstad (1986)** brachte mit dem **Switching Lemma** die
im Wesentlichen optimale exponentielle Schranke. `[VERIFIZIERT]`, über zwei Suchen bestätigt.

**Warum es funktioniert.** Zufällige Restriktionen (jede Variable wird unabhängig mit
gewisser Wahrscheinlichkeit auf 0/1 fixiert) machen AC⁰-Schaltkreise *einfacher*: Eine
DNF wird mit hoher Wahrscheinlichkeit zu einer flachen CNF und umgekehrt ("switching"),
sodass sich die Tiefe schrittweise abbauen lässt. PARITY dagegen bleibt unter jeder
Restriktion PARITY (auf weniger Variablen). Der Beweis nutzt also eine **Fragilität des
Modells gegenüber Restriktionen**, die allgemeine Schaltkreise schlicht nicht haben.
`[NUR-SNIPPET]` für die technische Darstellung, Konfidenz hoch (Lehrbuchstoff).

### 2.2 AC⁰[p] — plus MOD_p-Gatter für eine *Primzahl* p

**Resultat (Razborov 1987, Smolensky 1987).** MOD_q ∉ AC⁰[p] für verschiedene Primzahlen
p ≠ q; ebenso MAJORITY ∉ AC⁰[p]. `[VERIFIZIERT]`

**Warum es funktioniert (polynomial method).** Jede AC⁰[p]-Funktion lässt sich durch ein
Polynom **kleinen Grades über einem Körper der Charakteristik p** approximieren. Man wählt
F = GF(p^k) so, dass q | (p^k − 1), womit eine q-te Einheitswurzel ω ∈ F existiert; dann
zeigt man, dass MOD_q keine solche Niedriggrad-Approximation zulässt. `[NUR-SNIPPET]`,
über zwei Suchen konsistent.

**Warum es bei zusammengesetztem m zusammenbricht — und das ist der Kern.** Für m mit
mindestens zwei verschiedenen Primfaktoren (klassisch: m = 6) gibt es **keinen Körper**,
in dem das Argument läuft; Z/6Z ist kein Körper, der Grad-Begriff verliert seine Kraft.
Die Suchsynthese ist hier ungewöhnlich deutlich: "die Techniken von Razborov und Smolensky
versagten bei zusammengesetzten Moduln, und alternative Techniken konnten nicht gefunden
werden". Spätere Arbeiten (polynomial programs, torus polynomials) haben diese Grenze
**formalisiert**, statt sie zu überwinden. `[NUR-SNIPPET]`, Konfidenz mittel-hoch.

**Das ist die schärfste verfügbare Miniatur des Gesamtproblems:** Eine Methode, die für
p = 2, 3, 5, 7, … funktioniert, versagt vollständig bei m = 6 — und der Sprung von "p prim"
zu "m zusammengesetzt" hat von 1987 bis 2011 gedauert und wurde dann mit einer **völlig
anderen** Methode (Williams, §4) und nur für eine **gigantisch größere** Klasse (NEXP statt
NP) erledigt. `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]`

### 2.3 Monotone Schaltkreise — der lehrreichste Fall

**Resultat.** Razborov (1985): CLIQUE hat keine monotonen Schaltkreise polynomieller Größe
(superpolynomiell, Methode der **Approximationen** plus **Sunflower-Lemma** von Erdős–Rado).
Alon & Boppana (1987): Verschärfung auf **exponentiell**. `[VERIFIZIERT]`

**Warum Monotonie entscheidend ist — und zwar bewiesen, nicht vermutet.** Ein monotoner
Schaltkreis darf kein NOT verwenden. Die naheliegende Hoffnung — "für eine monotone Funktion
wie CLIQUE nützen Negationen ohnehin nichts" — ist **falsifiziert**:

- **Razborov (1985b):** Auch das **Matching**-Problem (eine monotone Funktion) hat keine
  polynomiell großen monotonen Schaltkreise (Schranke n^Ω(log n)). Matching liegt aber
  **in P** (Edmonds), hat also polynomiell große *nicht*-monotone Schaltkreise. Damit ist
  die Lücke bereits superpolynomiell. `[VERIFIZIERT]`, Konfidenz hoch.
- **Éva Tardos (1988), "The gap between monotone and non-monotone circuit complexity is
  exponential", Combinatorica 8, 141–142:** Die Lücke ist **echt exponentiell**. Tardos
  nimmt eine clique-artige monotone Funktion, die über die **Lovász-Theta-Funktion**
  (Grötschel–Lovász–Schrijver) in Polynomialzeit berechenbar ist, und wendet auf sie die
  Alon–Boppana-Version des Razborov-Arguments an: monotone Komplexität exp(cn^{1/6−o(1)}),
  nicht-monotone Komplexität polynomiell. `[VERIFIZIERT]`, über zwei Suchen bestätigt.

**Konsequenz für P vs. NP.** Razborovs exponentielle Schranke für monotones CLIQUE sagt über
P vs. NP **nichts** — Tardos' Resultat beweist, dass man aus monotonen Schranken keine
allgemeinen Schranken folgern kann, nicht einmal näherungsweise. Die Methode der
Approximationen ist zudem, soweit bekannt, an Monotonie gebunden: Razborov selbst zeigte,
dass sie im allgemeinen Modell nur triviale (lineare) Schranken liefert.
`[NUR-SNIPPET]` für den letzten Halbsatz, Konfidenz mittel.

**Didaktisch der wichtigste Punkt des ganzen Kapitels:** Hier hat die Community einen
scheinbar riesigen Fortschritt erzielt (exponentielle Schranke für ein NP-vollständiges
Problem!) — und dann *bewiesen*, dass dieser Fortschritt nicht hochskaliert. Das ist die
Vorlage für die Bewertung aller anderen Teilerfolge. `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]`

### 2.4 ACC⁰ — Williams 2011, der einzige Barrieredurchbruch

**Resultat.** NEXP ⊄ ACC⁰ (Williams, CCC 2011 / JACM 61(1):2, 2014; **Gödelpreis 2024**).
Verschärfung: NQP = NTIME[n^polylog n] ⊄ ACC⁰ (Murray–Williams, STOC 2018 / SICOMP).
`[VERIFIZIERT]` — siehe auch A1 §3.5, dort ausführlich.

**Die ehrliche Einordnung** (hier nur kurz, Details bei A1): ACC⁰ ist eine **sehr schwache**
Klasse (konstante Tiefe). NEXP ist eine **gewaltig größere** Klasse als NP. Gebraucht wird
eine Schranke für ein **NP**-Problem gegen **allgemeine polynomielle** Schaltkreise. Williams'
Resultat beweist, dass die Barrieren überwindbar sind — nicht, dass man nahe dran wäre.

### 2.4b TC⁰ — wo die Front *wirklich* endet

TC⁰ (konstante Tiefe, Schwellwert-/Majority-Gatter) ist die Klasse **unmittelbar oberhalb**
von ACC⁰ und **unterhalb** von NC¹. Sie ist das nächste Ziel nach Williams' ACC⁰-Resultat —
und dort ist der Stand schlicht:

- Für allgemeine TC⁰-Schaltkreise ist **keine** superpolynomielle untere Schranke bekannt,
  auch nicht für irgendeine Funktion in EXP^NP. `[NUR-SNIPPET]`
- Die Suchsynthese formuliert es drastisch: Man kennt **nicht einmal untere Schranken der
  Größe n^{1,1} für LTF-Schaltkreise** (Schaltkreise aus linearen Schwellwertfunktionen).
  `[NUR-SNIPPET]`, Konfidenz mittel (eine Suche, Formulierung aus einer Übersichtsarbeit).
- Was es gibt: Razborov–Wigderson, n^{log n} für Schaltkreise der **Tiefe 3** mit AND-Gattern
  in der untersten Schicht; Impagliazzo–Paturi–Saks für PARITY gegen LTF-Schaltkreise
  beschränkter Tiefe (vor über 25 Jahren); superlineare Gatter- und superquadratische
  Drahtzahlschranken für Tiefe 2 und 3 (arXiv:1511.07860). `[NUR-SNIPPET]`

**Das ist die präziseste verfügbare Ortsangabe der Front.** Sie liegt nicht "irgendwo
zwischen AC⁰ und P/poly". Sie liegt **genau eine Gatterart über ACC⁰** — beim Übergang
von MOD-Gattern zu Schwellwertgattern bricht alles zusammen. Und TC⁰ ist immer noch eine
Klasse konstanter Tiefe, unendlich weit von P/poly entfernt.
`[EIGENE EINSCHÄTZUNG, Konfidenz mittel-hoch]`

### 2.5 Warum nichts davon hochskaliert: Natural Proofs und ihre Verwandten

Die Razborov–Rudich-Barriere (JCSS 1997, Gödelpreis 2007) besagt: Ein Beweis, der
(a) **constructive** (das Härtekriterium ist effizient prüfbar) und (b) **large**
(es trifft auf einen nennenswerten Anteil aller Funktionen zu) ist, kann — unter der
Annahme, dass hinreichend harte Pseudozufallsgeneratoren existieren — keine superpolynomielle
Schranke gegen P/poly liefern. **Die AC⁰- und AC⁰[p]-Beweise sind genau in diesem Sinne
natural**: "zufällige Restriktionen vereinfachen" und "hat Niedriggrad-Approximation" sind
effizient prüfbare Eigenschaften, die fast alle Funktionen *nicht* haben. `[VERIFIZIERT]`

Die Erfolge von §2.1–§2.3 sind also keine Zwischenschritte auf einem Weg. Sie sind die
Ausbeute einer Technikfamilie, deren Reichweite anschließend **bewiesen begrenzt** wurde.
**Die Liste der bewiesenen Endpunkte** — sie ist der wichtigste strukturelle Befund dieses
Berichts:

| Technik | Größter Erfolg | Bewiesener Endpunkt |
|---|---|---|
| Approximationsmethode, monoton | CLIQUE exponentiell (Razborov 1985) | **Tardos 1988**: exponentielle Lücke monoton ↔ allgemein |
| Zufällige Restriktionen (AC⁰), polynomial method (AC⁰[p]) | exponentielle Schranken | **Razborov–Rudich 1994/97**: natural proofs |
| **gate elimination** (B₂ *und* U₂) | 3,1n bzw. 5n | **Golovnev–Hirsch–Knop–Kulikov 2016/18**: keine superlinearen Schranken möglich; **Amano–Tarui**: 5n ist für k-mixed tight |
| Matrix rigidity (Valiants Programm) | — | **Alman–Williams 2017**: bester Kandidat (Hadamard) ist nicht rigide |
| hardness magnification | — | **locality barrier** (JACM 2022) |
| algebrisierende Argumente | — | Aaronson–Wigderson 2008; **neu: Chen–Hu–Ren, ITCS 2026**, "New Algebrization Barriers to Circuit Lower Bounds via Communication Complexity of Missing-String" `[NUR-SNIPPET]` |

`[EIGENE EINSCHÄTZUNG, Konfidenz hoch]` für die Zusammenstellung; jede Einzelaussage ist
oben belegt bzw. wird unten belegt. **Das Muster ist bemerkenswert konsistent: Für praktisch
jede erfolgreiche Technik wurde anschließend ihr eigener Endpunkt bewiesen — und das Feld
produziert 2026 immer noch *neue* Barrieren, nicht nur neue Schranken.**

**Besonders hervorzuheben (weil es die Kernzahl dieses Berichts direkt betrifft):**
Golovnev, Hirsch, Knop und Kulikov beweisen in *On the Limits of Gate Elimination*
(MFCS 2016, JCSS 2018), dass ein typisches gate-elimination-Argument — man eliminiert pro
Substitutionsschritt mehrere Gatter und iteriert — **prinzipiell** nicht über eine Schranke
cn hinauskommt, wobei c allein von der Anzahl der Substitutionen pro Schritt abhängt.
Superlineare Schranken sind mit dieser Technik **nicht erreichbar**. `[VERIFIZIERT]`
(über zwei Suchen bestätigt, JCSS-Journalfassung nachgewiesen).
**Damit ist die Aussage "3,1n ist kein Zwischenstand auf dem Weg zu n^{ω(1)}" kein
Erfahrungsurteil mehr, sondern ein Satz.**

---

## 3. Entwicklungen 2024–2026

### 3.1 Ryan Williams, "Simulating Time With Square-Root Space" (Feb. 2025)

**Fundstellen (triangulierend bestätigt):** arXiv:2502.17779; **ECCC TR25-017**
(24. Februar 2025, eccc.weizmann.ac.il/report/2025/017/ — die im Auftrag genannte
Report-Nummer ist damit **bestätigt**); STOC 2025 (dl.acm.org/doi/10.1145/3717823.3718225);
Journalfassung dl.acm.org/doi/10.1145/3798104. `[VERIFIZIERT]` für Existenz und Venue.

**Aussage.** Für alle t(n) ≥ n gilt: **TIME[t] ⊆ SPACE[O(√(t log t))]** für
Mehrband-Turingmaschinen. `[NUR-SNIPPET]`, über drei Treffer konsistent.

**Warum das groß ist.** Der bisherige Stand war Hopcroft–Paul–Valiant (1975):
TIME[t] ⊆ SPACE[t / log t]. Das ist eine Ersparnis um einen **logarithmischen Faktor**;
Williams liefert eine Ersparnis um eine **Quadratwurzel**. Erste substanzielle Verbesserung
seit rund 50 Jahren. `[NUR-SNIPPET]`, Konfidenz hoch.

**Was daraus folgt — und was nicht.**
- **Folgt:** Es gibt explizite Probleme, die in Platz O(n) lösbar sind und auf jeder
  Mehrband-Turingmaschine mindestens **n^{2−ε}** Zeit benötigen, für jedes ε > 0.
  Das ist eine *neue untere Zeitschranke*, gewonnen aus einer *oberen Platzschranke*
  plus Platzhierarchiesatz. `[NUR-SNIPPET]`, Konfidenz mittel-hoch.
  Vorher lieferte HPV auf demselben Weg nur SPACE[n] ⊄ TIME[o(n log n)] — der Sprung geht
  also von "fast linear" auf "quadratisch". `[EIGENE EINSCHÄTZUNG, Konfidenz mittel]`
- **Folgt NICHT:** P ≠ PSPACE. Williams selbst formuliert es als "a little progress on the
  P versus PSPACE problem". Für P ≠ PSPACE bräuchte man eine superpolynomielle Trennung;
  hier ist eine quadratische erreicht. **Und: P ≠ PSPACE ist eine mindestens so schwere
  Frage wie P ≠ NP** (aus P = PSPACE folgte P = NP). `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]`

**Rolle des Cook–Mertz-Algorithmus.** Der Beweis reduziert die Simulation einer
zeitbeschränkten Maschine auf eine Folge implizit definierter **Tree-Evaluation**-Instanzen
und benutzt dann den Algorithmus von **James Cook und Ian Mertz (STOC 2024)**, der
Tree Evaluation in Platz **O(log n · log log n)** löst (Laufzeit n^{O(log log n)}).
Cook–Mertz ist von **catalytic computing** inspiriert: einem Modell, in dem neben dem
Arbeitsband ein großer, mit fremden Daten gefüllter Speicher zur Verfügung steht, der am
Ende unverändert zurückgegeben werden muss. `[VERIFIZIERT]` für Existenz/Venue,
`[NUR-SNIPPET]` für die technische Darstellung, über zwei Suchen konsistent.

**Das ist strukturell bedeutsam:** Tree Evaluation galt als *Kandidat* für eine Trennung
(L ≠ P über den Nachweis, dass Tree Evaluation *nicht* in Logspace liegt). Cook–Mertz haben
diesen Kandidaten weitgehend **entwertet**, indem sie einen unerwartet guten Algorithmus
fanden — und genau dieser Algorithmus wurde dann zum Werkzeug für eine andere untere
Schranke. Ein Musterfall des Algorithmen↔Schranken-Zweiwegeverkehrs (§4).
`[EIGENE EINSCHÄTZUNG, Konfidenz mittel-hoch]`

**Offener Folgeclaim, ungeprüft.** Es existiert ein Preprint arXiv:2508.14831
"TIME[t] ⊆ SPACE[O(√t)] via Tree Height Compression", der den log-Faktor entfernen will.
`[PREPRINT]` / `[CLAIM]` — von mir **nicht** verifiziert, nur als Suchtreffer gesehen.
Nicht als Fakt zitieren.

**Konsolidierung.** Mehrere Folgearbeiten sind sichtbar: "Improved Bounds on the Space
Complexity of Circuit Evaluation" (arXiv:2504.20950), "Catalytic Tree Evaluation From
Matching Vectors" (arXiv:2602.14320, Feb. 2026, auch als eprint.iacr.org/2026/265),
sowie ein polynomialzeitlicher, fast-logarithmischer TreeEval-Algorithmus
(Zeit poly(n), Platz O(log^{1+ε} n), davon nur O(log n) frei). `[NUR-SNIPPET]`.
Das Feld um Tree Evaluation / catalytic computing ist 2024–2026 **das aktivste
Teilgebiet mit echtem Fortschritt** — aber es liegt neben P vs. NP, nicht darauf.

### 3.2 Matrix rigidity — Valiants Programm und seine Erosion

**Das Programm (Valiant 1977).** Eine Matrix heißt *rigide*, wenn man ihren Rang nicht durch
Ändern weniger Einträge pro Zeile stark senken kann. Valiant zeigte: Hinreichend rigide
explizite Matrizen liefern untere Schranken für lineare Schaltkreise (Größe/Tiefe). Jahrzehnte
lang galten **Hadamard-Matrizen** als der aussichtsreichste Kandidat.

**Die Widerlegung (Alman & Williams, STOC 2017, "Probabilistic Rank and Matrix Rigidity",
arXiv:1611.05558).** Die Walsh–Hadamard-Transformation ist **nicht Valiant-rigide**.
Konkret laut Suchsynthese: Für H_n (2ⁿ × 2ⁿ) genügt es, pro Zeile 2^{εn} Einträge zu ändern,
um den Rang unter 2^{n(1−Ω(ε²/log(1/ε)))} zu drücken — **über jedem Körper**.
`[VERIFIZIERT]` für Existenz/Venue, `[NUR-SNIPPET]` für die Parameter, Konfidenz mittel-hoch.

**Das ist ein Negativbefund erster Güte** und gehört ins Papier: Eine über 40 Jahre verfolgte
Angriffslinie wurde nicht etwa "noch nicht erfolgreich" — der beste Kandidat wurde
**nachweislich untauglich**. Verwandte Erosionen: Dvir–Liu, Dvir–Edelman, Kronecker-Produkte
(arXiv:2103.05631, arXiv:2102.11992). `[NUR-SNIPPET]`

**Gegenbewegung 2025/2026.** Das Gebiet ist nicht tot:
- "Low Rank Matrix Rigidity: Tight Lower Bounds and Hardness Amplification", STOC 2025
  (dl.acm.org/doi/10.1145/3717823.3718287; vgl. arXiv:2502.19580). `[NUR-SNIPPET]`
- "Superlogarithmic-Rank Matrix Rigidity for the Walsh–Hadamard Transform"
  (arXiv:2608.06592, Aug. 2026) — offenbar ein *positives* Rigiditätsresultat für
  Hadamard in einem anderen Parameterbereich als dem von Alman–Williams zerstörten.
  `[PREPRINT]`, `[NUR-SNIPPET]`, Konfidenz niedrig bezüglich der genauen Aussage.

### 3.3 "Spiky Rank and Its Applications to Rigidity and Circuits" (arXiv:2602.23503)

**Bestätigt:** existiert, arXiv:2602.23503, eingereicht 2. März 2026. Autoren:
Lianna Hambardzumyan, Konstantin Myasnikov, Artur Riazanov, Morgan Shirley, Adi Shraibman.
`[PREPRINT]`, `[NUR-SNIPPET]`.

**Inhalt laut Abstract-Synthese:** Einführung eines Matrixparameters *spiky rank*, der
*blocky rank* verallgemeinert (blockstrukturierte Matrizen mit beliebigen Rang-1-Blöcken auf
der Diagonale; spiky rank = minimale Anzahl solcher Summanden). Große spiky rank impliziert
**hohe Rigidität**; spiky-rank-Schranken liefern untere Schranken für **Tiefe-2-ReLU-Schaltkreise**.
Ergebnisse: scharfe Schranken für zufällige Matrizen, Rahmen für explizite Schranken,
angewandt auf Hamming-Distanz-Matrizen und Spektralexpander.

**Meine Einordnung.** `[EIGENE EINSCHÄTZUNG, Konfidenz mittel]` Das ist inkrementelle
Grundlagenarbeit im gewohnten Muster: ein neues Maß, scharfe Schranken im *zufälligen* Fall,
ein *Rahmen* für explizite Schranken. Der ReLU-Bezug ist bemerkenswert (Schaltkreiskomplexität
neuronaler Grundbausteine), aber Tiefe 2 ist ein sehr schwaches Modell. Kein P-vs-NP-Bezug,
und die Arbeit behauptet auch keinen.

### 3.4 "Self-Improvement for Circuit-Analysis Problems" (STOC 2024) und
### die Orthogonal Vectors Conjecture (FOCS 2024)

Beide sind Williams-Arbeiten und gehören methodisch zusammen — sie drehen die algorithmische
Methode **um**.

**Self-Improvement (Williams, STOC 2024, dl.acm.org/doi/10.1145/3618260.3649723).**
`[VERIFIZIERT]` für Existenz/Venue. Zwei Beiträge laut Synthese:
(a) neue **unbedingte** untere Schranken gegen *uniforme* Schaltkreise mit symmetrischen
Gattern für Funktionen in deterministischer Linearzeit;
(b) **Selbstverstärkung**: Bestimmte feinkörnige Verbesserungen der Laufzeitexponenten
polynomialzeitlicher Circuit-SAT-Varianten würden subexponentielle Algorithmen für
Circuit-SAT auf 2^{o(n)}-großen Schaltkreisen nach sich ziehen — und damit **die ETH
widerlegen**. `[NUR-SNIPPET]`, Konfidenz mittel.

**OVC → nicht-uniforme Schranken (Williams, FOCS 2024; ECCC TR24-142;
SICOMP, epubs.siam.org/doi/10.1137/24M1718299).** `[VERIFIZIERT]` für Existenz/Venue.
Aussage laut Synthese: Aus der **Orthogonal Vectors Conjecture** (einer Härteannahme über
*uniforme* Algorithmen aus der fine-grained complexity) folgt, dass 2-CNF-Formeln auf n
Variablen **keine** nicht-uniformen Tiefe-2-Exact-Threshold-Schaltkreise der Größe 2^{o(n)}
haben. `[NUR-SNIPPET]`, Konfidenz mittel.

**Warum das konzeptionell interessant ist.** Bisher lautete die Richtung: *bessere
Algorithmen ⇒ untere Schranken*. Hier lautet sie: *Nicht-Existenz guter Algorithmen ⇒
untere Schranken*. Beide Richtungen zusammen ergeben ein **Win-Win**: Entweder man findet
den Algorithmus (und bekommt die Schranke über Williams' Methode), oder man findet ihn
nicht und die Härteannahme gilt (und bekommt die Schranke über die Umkehrung). Williams
weist zusätzlich auf einen möglichen Weg hin, die OVC im O(log n)-dimensionalen Fall zu
**widerlegen**, was für eine Widerlegung von **SETH** genügen würde. `[NUR-SNIPPET]`

**Ehrliche Einordnung:** Die Resultate betreffen Tiefe-2-Threshold-Schaltkreise und uniforme
Schaltkreise mit symmetrischen Gattern — extrem eingeschränkte Modelle. Der Gewinn ist
**konzeptionell** (neue Implikationsrichtung), nicht quantitativ.
`[EIGENE EINSCHÄTZUNG, Konfidenz mittel-hoch]`

### 3.5 Hardness magnification — und die locality barrier

**Die Idee.** *Hardness magnification* reduziert große Separationen (z.B. EXP ⊄ NC¹) auf
**leicht superlineare** untere Schranken für spezielle Probleme (typisch: Varianten von
MCSP, dem Minimum Circuit Size Problem) gegen **schwache** Modelle. Das klingt nach dem
perfekten Hebel: Eine n^{1+ε}-Schranke, die "fast in Reichweite" wirkt, würde eine
Klassentrennung liefern. `[VERIFIZIERT]` für das Konzept.

**Warum es bisher nichts geliefert hat: die locality barrier.**
Chen, Hirahara, Oliveira, Pich, Rajgopal, Santhanam: *"Beyond Natural Proofs: Hardness
Magnification and Locality"*, ITCS 2020 / **JACM 2022** (arXiv:1911.08297). `[VERIFIZIERT]`
Die Magnifikationssätze zeigen *unbedingt*, dass die betreffenden Probleme Q sehr effiziente
Schaltkreise besitzen, **sofern man kleine Orakelgatter mit kleinem Fan-In zulässt**. Die
bekannten Techniken für untere Schranken gegen schwache Modelle lassen sich aber typischerweise
**auf genau solche orakelangereicherten Schaltkreise ausdehnen** — und können daher die
benötigte Schranke prinzipiell nicht liefern. Die Autoren beschreiben die locality barrier
als ein **Schaltkreis-Analogon der Relativization**. `[NUR-SNIPPET]`, Konfidenz mittel-hoch.

**Bemerkenswert:** Hardness magnification **umgeht** nachweislich die natural-proofs-Barriere
(leicht superlineare Schranken für MCSP-Varianten implizieren die Nicht-Existenz naturaler
Beweise). Es hilft nur nichts, weil sofort eine *neue* Barriere auftaucht. Das ist der
vierte Fall desselben Musters aus §2.5. `[EIGENE EINSCHÄTZUNG, Konfidenz mittel-hoch]`

### 3.6 Refuter-Probleme: Konstruktivität als neue Achse (STOC 2026)

Ein 2024–2026 deutlich sichtbar gewordenes Thema: **Sind untere Schranken konstruktiv?**
Eine untere Schranke sagt "jeder zu kleine Schaltkreis irrt sich irgendwo". Das
**Refuter-Problem** fragt: Kann man, gegeben einen zu kleinen Schaltkreis, den Fehler
*effizient finden*? Chen, Jin, Santhanam und Williams nennen eine Schranke **konstruktiv**,
wenn das zugehörige Refuter-Problem in deterministischer Polynomialzeit lösbar ist
(vgl. "Constructive Separations and Their Consequences", arXiv:2203.14379). `[NUR-SNIPPET]`

Zwei Fundstellen mit direkter Relevanz:
- **"Constructive Separations from Gate Elimination"** (Marco Carmosino, Ngu Dang,
  Tim Jackman; arXiv:2604.23958, 27. April 2026). Zeigt laut Abstract-Synthese, dass eine
  Reihe von gate-elimination-Argumenten — **einschließlich Li–Yangs 3,1n** — tatsächlich
  effiziente Refuter liefern. `[PREPRINT]`, `[NUR-SNIPPET]`, Konfidenz mittel.
- **STOC 2026 Best Paper (Auszeichnung über Columbia-Meldung belegt):**
  *"Finding Bugs in Short Proofs: The Metamathematics of Resolution Lower Bounds"*
  (Jiawei Li, UT Austin; Yuhao Li, Columbia; Hanlin Ren, IAS; vgl. arXiv:2411.15515).
  Überträgt die Refuter-Frage auf die **Beweiskomplexität**: Gegeben eine angebliche
  Resolutionswiderlegung, die kleiner ist als die bewiesene untere Schranke — finde den
  ungültigen Ableitungsschritt. Ergebnis laut Synthese: Für viele Resolutions-Größenschranken,
  **einschließlich Hakens klassischer Schranke für das Schubfachprinzip**, ist das
  Refuter-Problem in der TFNP-Unterklasse rwPHP(PLS) lösbar. `[NUR-SNIPPET]`, Konfidenz mittel.
- Das **zweite** STOC-2026-Best-Paper laut derselben Meldung:
  *"Boolean function monotonicity testing requires (almost) n^{1/2} queries"*
  (Mark Chen, Xi Chen, Hao Cui, William Pires, Jonah Stockwell, Columbia). Eine untere
  Schranke im **Property-Testing**-Modell, nicht in der Schaltkreiskomplexität.
  `[NUR-SNIPPET]` **Relevanz für P vs. NP: keine direkte.** Der Auftrag nannte es zur Prüfung;
  das Prüfergebnis ist negativ, und das ist als solches zu berichten.

**Warum die Refuter-Achse für dieses Papier zählt.** Sie ist eine **Präzisierung des
Begriffs "untere Schranke"** und damit genau die Art Meta-Frage, die im KI-Kontext relevant
wird: Ein Refuter ist ein *Algorithmus*, der Gegenbeispiele *produziert* — also ein Objekt
mit billigem Verifikationsorakel (B2 der Leitachse). Zugleich bleibt die *Schranke selbst*
eine universell quantifizierte Aussage. `[EIGENE EINSCHÄTZUNG, Konfidenz mittel]`

---

## 4. Das "Algorithms-to-lower-bounds"-Paradigma

### 4.1 Die Grundidee, für technisch versierte Nicht-Spezialisten

Die naive Erwartung lautet: Untere Schranken beweist man durch *Unmöglichkeitsargumente* —
man zeigt direkt, dass keine kleine Schaltung funktionieren kann. Genau das ist in 50 Jahren
nicht gelungen. Williams' Umkehrung lautet:

> **Um zu zeigen, dass eine Klasse C schwache Schaltkreise hat, baue einen guten Algorithmus,
> der C-Schaltkreise analysiert.**

Konkret (Williams 2010/2011, "algorithmic method" oder "algorithms-to-lower-bounds"): Wenn es
für eine Schaltkreisklasse C mit gutartigen Abschlusseigenschaften einen
**C-SAT-Algorithmus** gibt, der Brute Force auch nur **geringfügig** schlägt — Laufzeit
2ⁿ/n^{ω(1)} statt 2ⁿ —, dann folgt **NEXP ⊄ C**. `[VERIFIZIERT]`, über zwei Suchen konsistent.

**Die Mechanik in drei Schritten** (vereinfachte Standarddarstellung, `[NUR-SNIPPET]`):
1. **Annahme zum Widerspruch:** NEXP ⊆ C, also hat jede NEXP-Sprache kleine C-Schaltkreise.
2. **Easy-Witness-Lemma** (Impagliazzo–Kabanets–Wigderson, von Murray–Williams erweitert):
   Unter dieser Annahme haben NEXP-Sprachen nicht nur kleine Schaltkreise, sondern ihre
   **Zeugen** lassen sich selbst als kleine Schaltkreise komprimieren.
3. **Kollaps:** Dann kann man das Nichtdeterminismus-Zeithierarchie-Theorem mit dem
   schnellen C-SAT-Algorithmus kombinieren und NTIME[2ⁿ] in weniger als 2ⁿ Schritten
   simulieren — Widerspruch zum Hierarchiesatz.

Der zweite Teil ist reine **Algorithmik**: Williams musste den ACC⁰-SAT-Algorithmus
tatsächlich bauen (Reduktion von ACC⁰ auf SYM⁺-Schaltkreise, schnelle rechteckige
Matrixmultiplikation, dynamische Programmierung). `[NUR-SNIPPET]`

### 4.2 Warum die Zwei-Wege-Verbindung methodisch besonders ist

**Richtung 1: Algorithmus ⇒ Schranke.** Wie oben. Der Algorithmus wird als **Blackbox**
verwendet; das Argument gilt für eine ganze Familie natürlicher Schaltkreisklassen.

**Richtung 2 (neu, 2024): Nicht-Existenz eines Algorithmus ⇒ Schranke.** Self-Improvement
(§3.4) und OVC → Tiefe-2-Threshold-Schranken (§3.4) zeigen, dass auch die *negative*
Antwort etwas liefert. Ergebnis: **Win-Win-Schranken** — man muss die algorithmische Frage
gar nicht entscheiden, um etwas zu gewinnen. `[NUR-SNIPPET]`, Konfidenz mittel.

**Richtung 3: Obere Platzschranke ⇒ untere Zeitschranke.** Der √-Platz-Satz (§3.1) nutzt
denselben Dreh auf der Ressourcenachse: Ein *besserer Algorithmus* (Cook–Mertz für
Tree Evaluation) liefert über eine Simulation plus Hierarchiesatz eine *neue untere Schranke*.
`[EIGENE EINSCHÄTZUNG, Konfidenz mittel-hoch]`

**Warum es die Barrieren umgeht.** Die konkreten SAT-Algorithmen nutzen die **innere
Struktur** der Schaltkreise (Normalformen, algebraische Darstellungen). Ein
relativierendes oder algebrisierendes Argument könnte das nicht — es müsste den Schaltkreis
als Blackbox behandeln. Und das Argument ist nicht **large** im Sinne von Razborov–Rudich:
Es zeigt nicht "die typische Funktion ist hart", sondern zielt über das Easy-Witness-Lemma
auf eine ganz bestimmte Sprache. `[NUR-SNIPPET]`, Konfidenz mittel-hoch; in der Community
Standarddarstellung, von mir nicht am Volltext geprüft.

### 4.3 Wie weit trägt es?

**Was es geliefert hat:** den einzigen bekannten Barrieredurchbruch (NEXP ⊄ ACC⁰, 2011),
seine Verschärfung auf NQP (2018), eine Reihe von Folgeschranken (u.a. gegen dünne
symmetrische Funktionen von ACC⁰-Schaltkreisen, arXiv:2001.07788), und ein aktives
Programm 2024–2026.

**Was es nicht geliefert hat und absehbar nicht liefert:**
- Die Methode braucht **oben** eine sehr große Klasse (NEXP, bestenfalls NQP) — nicht NP.
  Der Weg von NQP zu NP ist kein Feintuning; das Hierarchiesatz-Argument braucht
  Nichtdeterminismus-Zeitreserve, die NP nicht hat. `[EIGENE EINSCHÄTZUNG, Konfidenz mittel-hoch]`
- Die Methode braucht **unten** eine Klasse mit brauchbarer Normalform (ACC⁰: SYM⁺).
  Für allgemeine Schaltkreise wäre der benötigte Algorithmus ein nichttrivialer
  **CircuitSAT-Algorithmus** — der würde bereits die SETH und vermutlich die ETH widerlegen.
  `[EIGENE EINSCHÄTZUNG, Konfidenz mittel-hoch]`, gestützt durch die Self-Improvement-Aussage
  in §3.4, die genau diese Selbstblockade quantifiziert.
- Vyas & Williams (ITCS 2023, "On Oracles and Algorithmic Methods for Proving Lower Bounds)
  untersuchen die Grenzen der Methode orakeltheoretisch. `[NUR-SNIPPET]`, nicht näher geprüft.

**Fazit zur Angriffslinie:** Sie ist real, sie lebt, sie hat als einzige eine Barriere
durchbrochen — und sie hat eine **eingebaute Selbstblockade**: Der Algorithmus, den man für
den Zielfall bräuchte, ist selbst ein Objekt, dessen Existenz die zentralen Härteannahmen
des Feldes widerlegen würde. `[EIGENE EINSCHÄTZUNG, Konfidenz mittel]`

---

## 5. Bewertung: Trend der letzten 15 Jahre, quantitativ

### 5.1 Die Kalibrierungstabelle

| Größe | Wert |
|---|---|
| Benötigt für P ≠ NP | superpolynomiell, n^{ω(1)} |
| Bewiesen (explizit, B₂), 1984 | 3n |
| Bewiesen (explizit, B₂), 2016 | 3,0116n |
| Bewiesen (explizit, B₂), 2022 | **3,1n** |
| Fortschritt 1984→2022 | **+0,1n in 38 Jahren** |
| Nicht-explizit (Shannon 1949) | ≈ 2ⁿ/n |
| Bei n = 1000: bewiesen vs. Shannon | ≈ 3 100 vs. ≈ 10²⁹⁸ |

**Die zentrale Formulierung für das Papier:** Wir brauchen n^{ω(1)}. Wir haben 3,1n.
Und wir haben **nicht einmal 4n**. Wir haben für kein explizites Problem über einer
allgemeinen Basis **irgendeine superlineare** Schranke — die Grenze verläuft nicht bei
"polynomiell vs. superpolynomiell", sondern bereits bei **"linear vs. superlinear"**.
`[EIGENE EINSCHÄTZUNG, Konfidenz hoch]`

### 5.2 Was sich in 15 Jahren (2011–2026) tatsächlich bewegt hat

**Echte Fortschritte:**
- 2011: NEXP ⊄ ACC⁰ (Williams) — Barrierendurchbruch. Gödelpreis 2024.
- 2016: 3n → 3,0116n über B₂ (FGHK).
- 2017: Hadamard ist **nicht** rigide (Alman–Williams) — ein wichtiger *Negativbefund*.
- 2018: NEXP → NQP (Murray–Williams).
- 2022: 3,0116n → 3,1n (Li–Yang).
- 2024: Tree Evaluation in O(log n · log log n) Platz (Cook–Mertz).
- 2025: TIME[t] ⊆ SPACE[O(√(t log t))] (Williams); SPACE[n] ⊄ TIME[n^{2−ε}].
- 2024–2026: Refuter-/Konstruktivitäts-Programm, Win-Win-Schranken.

**Was sich nicht bewegt hat:** Keine superlineare Schaltkreisschranke für irgendeine
NP-Funktion. Keine Schranke gegen TC⁰ jenseits des Trivialen. Kein Ansatz, der NP statt
NEXP/NQP erreicht. **Die Zielgröße hat sich in 15 Jahren nicht um einen Exponenten bewegt.**

### 5.3 Ehrlichkeit über Extrapolation

Der naheliegende Fehlschluss wäre eine Trendrechnung: "0,1n in 38 Jahren, also 4n um 2400
und superpolynomiell nie." Diese Zahl wäre **methodisch wertlos**, und zwar aus drei Gründen —
das gehört ausdrücklich ins Papier: `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]`

1. **Der gesuchte Sprung ist qualitativ, nicht quantitativ.** Zwischen 3,1n und n^{log n}
   liegt kein Zahlenintervall, das man abschreitet, sondern ein Methodenwechsel. Gate
   elimination kann *prinzipiell* keine superlineare Schranke liefern: Man eliminiert
   pro Substitution eine konstante Zahl von Gattern bei n Substitutionsschritten — das
   Verfahren ist strukturell auf O(n) begrenzt. Der Amano–Tarui-Befund (§1.3) ist die
   bewiesene Instanz dieses Arguments für U₂.
2. **Durchbrüche im Feld sind historisch nicht trendförmig, sondern sprunghaft.**
   Williams 2011 war aus dem Stand der Technik von 2010 nicht extrapolierbar. Cook–Mertz
   2024 ebenfalls nicht. Eine lineare Extrapolation hätte beide verfehlt — sie unterschätzt
   also mindestens ebenso, wie sie überschätzt.
3. **Die Barrieren sind Sätze über *bekannte* Techniken, nicht über das Problem.**
   Sie sagen nicht, dass es nicht geht. Sie sagen, dass es so nicht geht.

**Die belastbare Aussage ist stattdessen strukturell:** Für jede Technik, die substanzielle
untere Schranken geliefert hat, wurde anschließend ihr eigener Endpunkt **bewiesen**
(Monotonie → Tardos; AC⁰/AC⁰[p] → natural proofs; gate elimination/U₂ → Amano–Tarui;
magnification → locality barrier). Die einzige Technik ohne bewiesenen Endpunkt ist die
algorithmische Methode — und die hat eine *plausible* Selbstblockade (§4.3). Das ist ein
deutlich informativeres Bild als jede Trendrechnung. `[EIGENE EINSCHÄTZUNG, Konfidenz mittel-hoch]`

---

## 6. Antwort auf die Leitachse (`docs/02-team-briefing.md` §2)

**Befund: Der Bereich untere Schranken bestätigt die Achse, und zwar in ungewöhnlich
scharfer Form — mit einer wichtigen Präzisierung.**

Prüfung der drei Bedingungen für den Kern des Gebiets (superpolynomielle Schranke für NP):
- **B1 (endliches, maschinell repräsentierbares Objekt):** **verletzt.** Das gesuchte Objekt
  ist ein Beweis einer universell quantifizierten Aussage über *alle* Schaltkreise aller
  Größen. Es gibt keinen endlichen Zeugen.
- **B2 (billiges Verifikationsorakel):** **verletzt.** Selbst wenn man einen Kandidatenbeweis
  hätte, ist seine Prüfung genau das, woran Fortnow mit Lean scheiterte (vgl. A7).
- **B3 (menschlicher Lifting-Rahmen):** **verletzt** — und das ist die Pointe. Die vier
  bewiesenen Endpunkte aus §5.3 *sind* die präzise Feststellung, dass kein Lifting-Rahmen
  existiert: Monotone Schranken liften nicht (Tardos, bewiesen), AC⁰[p]-Schranken liften
  nicht (natural proofs, bedingt bewiesen), U₂-gate-elimination liftet nicht über 5n
  (Amano–Tarui, bewiesen), magnification liftet nicht (locality barrier, bewiesen).

**Die Präzisierung — ein Teilgegenbefund.** Es gibt in diesem Gebiet **einen** Ort, an dem
B1–B3 *doch* erfüllt sind, und das ist **Williams' algorithmische Methode**:
- **B1 erfüllt:** Das gesuchte Objekt ist ein **Algorithmus** (ein C-SAT-Algorithmus) —
  endlich, als Programm repräsentierbar, maschinell erzeugbar.
- **B2 erfüllt (teilweise):** Laufzeit ist messbar, Korrektheit auf Instanzen testbar.
  Nicht vollständig erfüllt, weil die Korrektheit selbst wieder ein Beweis ist.
- **B3 erfüllt:** Der Lifting-Rahmen existiert und ist **von Menschen bewiesen** —
  er heißt "Williams' Satz" und schließt vom endlichen Algorithmus auf die
  universelle Schrankenaussage. Das ist strukturell exakt die Rolle, die das PCP-Theorem
  im AlphaEvolve-Fall spielt.

Damit ist **Circuit-SAT-Algorithmendesign für eingeschränkte Klassen** der einzige mir
erkennbare Ort im Bereich untere Schranken, an dem KI-gestützte Suche im Prinzip greifen
könnte. Er deckt sich mit der Frage, die A1 an A6/A8 gestellt hat. Die Einschränkung bleibt
hart: Der Lifting-Rahmen liefert **NEXP ⊄ C**, nicht **NP ⊄ P/poly** — man gewinnt eine
Schranke für eine viel zu große Klasse gegen ein viel zu schwaches Modell.
`[EIGENE EINSCHÄTZUNG, Konfidenz mittel]`

**Ein zweiter, schwächerer Teilgegenbefund: Refuter (§3.6).** Auch hier ist das gesuchte
Objekt ein Algorithmus mit testbarem Verhalten. Aber Refuter setzen eine bereits bewiesene
untere Schranke voraus — sie erzeugen keine. `[EIGENE EINSCHÄTZUNG, Konfidenz mittel]`

---

## 7. Was ich NICHT verifizieren konnte

Vollständige Liste, entsprechend `docs/02-team-briefing.md` §1:

1. **Keine einzige Volltextprüfung.** Alle inhaltlichen Aussagen über Beweisinhalte sind
   Suchsynthesen. `[NUR-SNIPPET]` ist der Normalfall dieses Berichts.
2. **Der exakte Wortlaut der Li–Yang-Schranke** (gilt 3,1n für *alle* affinen Disperser
   sublinearer Dimension oder für eine spezielle Konstruktion?) — nur Abstract-Synthese.
3. **Die genauen Amano–Tarui-Parameter** und ob "5n + o(n)" die k-mixed-Methode wirklich
   vollständig ausschöpft oder nur eine spezielle Variante.
4. **Das exakte Verhältnis U₂ ↔ B₂** (welche B₂-Schranke folgt formal aus 5n über U₂?) —
   ich habe keine Quelle gefunden, die das explizit ausrechnet. Meine Aussage "3,1n ist die
   relevante Zahl" ist begründet, aber nicht zitierbelegt.
5. **Der Status von arXiv:2508.14831** (TIME[t] ⊆ SPACE[O(√t)] ohne log-Faktor) —
   `[CLAIM]`, ungeprüft, nicht zitieren.
6. **Der genaue Inhalt von arXiv:2602.23503 (Spiky Rank)** — Existenz und Abstract bestätigt,
   sonst nichts. Ebenso arXiv:2608.06592 (Superlogarithmic-Rank Rigidity).
7. **Die STOC-2026-Best-Paper-Zuordnung** stützt sich auf **eine** Meldung
   (cs.columbia.edu, "The Theory Group Wins Big at STOC 2026") plus den Titelabgleich mit
   der STOC-2026-Programmseite. Die offizielle SIGACT-Best-Paper-Seite habe ich nicht
   eingesehen. Konfidenz mittel. Insbesondere ist unklar, ob beide Arbeiten *Best Paper*
   oder eine davon eine andere Auszeichnung erhielt.
8. **Der TC⁰-Stand.** Ich habe für TC⁰ keine belastbare Zahl recherchiert; die Tabellenzeile
   "im Wesentlichen nichts" ist Erfahrungswissen, nicht belegt. Konfidenz niedrig.
9. **Die Nečiporuk/Håstad-Formelschranken** (n²/log²n für branching programs, n³⁻ᵒ⁽¹⁾ für
   De-Morgan-Formeln) habe ich nicht eigenständig gesucht; sie stehen hier als Kanon.

---

## 8. Quellen

**Allgemeine Schaltkreise, B₂ und U₂**
- N. Blum (1984), 3n − o(n) über B₂ — referiert in: https://eccc.weizmann.ac.il/report/2015/166/revision/1/download/
- M. Find, A. Golovnev, E. Hirsch, A. Kulikov: *A Better-Than-3n Lower Bound for the Circuit Complexity of an Explicit Function*, FOCS 2016 — https://ieeexplore.ieee.org/document/7782921/ · https://eccc.weizmann.ac.il/report/2015/166/revision/1/download/ · https://www.nist.gov/publications/better-3n-lower-bound-circuit-complexity-explicit-function
- J. Li, T. Yang: *3.1n − o(n) Circuit Lower Bounds for Explicit Functions*, STOC 2022 — https://dl.acm.org/doi/abs/10.1145/3519935.3519976 · ECCC TR21-023: https://eccc.weizmann.ac.il/report/2021/023/download/ · Vortrag: https://www.youtube.com/watch?v=54ILPK6JK5c
- K. Iwama, H. Morizumi: *An Explicit Lower Bound of 5n − o(n) for Boolean Circuits* (U₂) — https://link.springer.com/chapter/10.1007/3-540-45687-2_29 · https://www.wisdom.weizmann.ac.il/~ranraz/publications/P5nlb.pdf
- K. Amano, J. Tarui: *A Well-Mixed Function with Circuit Complexity 5n ± o(n): Tightness of the Lachish–Raz-Type Bounds* — https://link.springer.com/chapter/10.1007/978-3-540-79228-4_30
- A. Golovnev, Dissertation *Circuit Complexity: New Techniques and Their Limitations* — https://golovnev.org/theses/phd_nyu.pdf

**Eingeschränkte Modelle**
- Furst–Saxe–Sipser / Ajtai / Håstad, AC⁰ und Switching Lemma — https://en.wikipedia.org/wiki/Switching_lemma · https://www.cs.umd.edu/~jkatz/complexity/f05/switching-lemma.pdf · https://simons.berkeley.edu/sites/default/files/docs/10142/restrictionmethods.pdf
- Razborov / Smolensky, AC⁰[p] — https://users.cs.duke.edu/~reif/courses/complectures/Miltersen/Razborov-Smolensky%20Circuit%20Lower%20Bound.pdf · Grenzen: https://eccc.weizmann.ac.il/report/2001/067/ · https://arxiv.org/pdf/1804.08176 (torus polynomials)
- Razborov 1985 (CLIQUE, monoton), Alon–Boppana 1987 — https://www.cs.tau.ac.il/~nogaa/PDFS/Publications/The%20monotone%20circuit%20complexity%20of%20Boolean%20functions.pdf
- É. Tardos: *The gap between monotone and non-monotone circuit complexity is exponential*, Combinatorica 8 (1988), 141–142 — https://link.springer.com/article/10.1007/BF02122563 · https://www.cs.cornell.edu/~eva/Gap.Between.Monotone.NonMonotone.Circuit.Complexity.is.Exponential.pdf
- R. Williams: *Nonuniform ACC Circuit Lower Bounds*, JACM 61(1):2, 2014 — https://people.csail.mit.edu/rrw/acc-lbs-journal-final.pdf

**2024–2026**
- R. Williams: *Simulating Time With Square-Root Space*, STOC 2025 — https://arxiv.org/html/2502.17779 · https://people.csail.mit.edu/rrw/time-vs-space.pdf · https://dl.acm.org/doi/10.1145/3717823.3718225 · https://dl.acm.org/doi/epdf/10.1145/3798104
- J. Cook, I. Mertz: *Tree Evaluation Is in Space O(log n · log log n)*, STOC 2024 — https://iuuk.mff.cuni.cz/~iwmertz/papers/cm25.tree_evaluation_is_in_space_lognloglogn.pdf
- Kommentar/Einordnung: https://emanueleviola.wordpress.com/2025/11/05/tree-eval-catalytic-computation-simulating-time-with-square-root-space/
- Clay-Institut, Abstracts *P vs NP and Complexity Lower Bounds* (2025) — https://www.claymath.org/wp-content/uploads/2025/04/Abstracts-PvNP.pdf
- J. Alman, R. Williams: *Probabilistic Rank and Matrix Rigidity*, STOC 2017 — https://arxiv.org/abs/1611.05558 · https://dl.acm.org/doi/10.1145/3055399.3055484
- *Low Rank Matrix Rigidity: Tight Lower Bounds and Hardness Amplification*, STOC 2025 — https://dl.acm.org/doi/10.1145/3717823.3718287 · https://arxiv.org/pdf/2502.19580
- *Superlogarithmic-Rank Matrix Rigidity for the Walsh–Hadamard Transform* — https://arxiv.org/html/2608.06592 `[PREPRINT]`
- L. Hambardzumyan, K. Myasnikov, A. Riazanov, M. Shirley, A. Shraibman: *Spiky Rank and Its Applications to Rigidity and Circuits* — https://arxiv.org/abs/2602.23503 `[PREPRINT]`
- R. Williams: *Self-Improvement for Circuit-Analysis Problems*, STOC 2024 — https://dl.acm.org/doi/abs/10.1145/3618260.3649723 · https://dspace.mit.edu/bitstream/handle/1721.1/155669/3618260.3649723.pdf
- R. Williams: *The Orthogonal Vectors Conjecture and Non-Uniform Circuit Lower Bounds*, FOCS 2024 / SICOMP — https://eccc.weizmann.ac.il/report/2024/142/download/ · https://epubs.siam.org/doi/10.1137/24M1718299
- Chen, Hirahara, Oliveira, Pich, Rajgopal, Santhanam: *Beyond Natural Proofs: Hardness Magnification and Locality*, ITCS 2020 / JACM 2022 — https://arxiv.org/abs/1911.08297 · https://dl.acm.org/doi/10.1145/3538391
- M. Carmosino, N. Dang, T. Jackman: *Constructive Separations from Gate Elimination* — https://arxiv.org/abs/2604.23958 `[PREPRINT]`
- Chen, Jin, Santhanam, Williams: *Constructive Separations and Their Consequences* — https://arxiv.org/abs/2203.14379
- J. Li, Y. Li, H. Ren: *Finding Bugs in Short Proofs: The Metamathematics of Resolution Lower Bounds* — https://arxiv.org/html/2411.15515v2
- STOC 2026 Auszeichnungen (Sekundärquelle) — https://www.cs.columbia.edu/2026/the-theory-group-wins-big-at-stoc-2026/ · Programm: https://acm-stoc.org/stoc2026/accepted-papers.html
- Monotonicity Testing (STOC 2026) — vgl. https://arxiv.org/pdf/2410.09235

**Algorithmische Methode**
- R. Williams: *Complexity Lower Bounds from Algorithm Design* (Invited, LICS 2021) — https://people.csail.mit.edu/rrw/LICS21.pdf
- R. Santhanam: *An Algorithmic Approach to Uniform Lower Bounds*, CCC 2023 — https://eccc.weizmann.ac.il/report/2023/028/download/
- N. Vyas, R. Williams: *On Oracles and Algorithmic Methods for Proving Lower Bounds*, ITCS 2023 — https://people.csail.mit.edu/rrw/itcs23-oracles.pdf
- *Lower Bounds Against Sparse Symmetric Functions of ACC Circuits* — https://arxiv.org/abs/2001.07788
