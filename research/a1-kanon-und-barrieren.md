# A1 — Kanon & Landkarte: Konsensstand zu P vs. NP und die drei Barrieren

**Agent:** A1 "Kanon & Landkarte"
**Stand:** 12. September 2026
**Methode:** ausschließlich WebSearch (WebFetch in dieser Umgebung gesperrt, siehe `docs/02-team-briefing.md` §1).
33 Suchanfragen, jeder nichttriviale Claim mit mindestens zwei unterschiedlich formulierten Anfragen trianguliert.

**Lesehinweis zu den Markierungen:**
- `[KANON]` — Lehrbuchwissen, seit Jahrzehnten in jedem Standardlehrbuch (Sipser, Arora–Barak, Papadimitriou), nicht strittig. Konfidenz hoch. Diese Aussagen sind in dieser Umgebung nicht am Volltext nachgeprüft, aber sie sind auch nicht das, was Nachprüfung braucht.
- `[VERIFIZIERT]` — peer-reviewt, Existenz und Kernaussage durch mehrere unabhängige Suchen bestätigt.
- `[NUR-SNIPPET]` — aus Suchergebnis-Synthesen rekonstruiert, **kein Volltextzugang**. Das ist in diesem Projekt der Normalfall.
- `[CLAIM]` — Behauptung ohne Verifikation.
- `[EIGENE EINSCHÄTZUNG]` — Analyse des Agenten, kein Literaturbefund.

---

## 0. Kurzfassung für eilige Leser

1. P vs. NP ist im September 2026 **offen**. Es gibt keinen anerkannten Beweis in irgendeine Richtung, und es gibt — nach Aussage eines der prominentesten Fachvertreter, Lance Fortnow, im Juni 2026 — **nicht einmal einen tragfähigen Ansatz**.
2. Die große Mehrheit der Fachwelt erwartet **P ≠ NP**. Die kursierenden Prozentzahlen für die Gasarch-Umfrage 2019 (66 % vs. ~80 % vs. 88 %) sind **kein Widerspruch in der Sache**: Die 66 % beziehen sich nachweislich auf eine *andere Frage* (»wird das Problem vor 2100 gelöst?«). Details und Beleg in §4.
3. Die drei Barrieren (Relativization, Natural Proofs, Algebrization) sind **Sätze über Beweistechniken**, nicht über das Problem. Sie zeigen *nicht*, dass P vs. NP unlösbar oder unabhängig von ZFC ist. Es existieren publizierte Resultate, die alle drei Barrieren nachweislich umgehen (Williams, NEXP ⊄ ACC⁰, JACM 2014, Gödel-Preis 2024).
4. Die härteste einzelne Zahl zur Lage des Feldes: Die beste bekannte untere Schranke für die Größe eines *allgemeinen* Booleschen Schaltkreises für eine explizite Funktion liegt bei **3,1n − o(n)** (Li–Yang, STOC 2022). Für P ≠ NP bräuchte man eine *superpolynomielle* Schranke. Der Abstand zwischen »3,1·n« und »n^{ω(1)}« ist die ehrlichste Beschreibung des Forschungsstands.

---

## 1. Formale Grundlagen — sauber aufgeschrieben

Dieses Kapitel ist bewusst pedantisch. Fast alle populären Fehlschlüsse über P vs. NP entstehen an genau drei Stellen: an der **Kodierung der Eingabe**, an der Frage, **was Teil der Eingabe ist**, und an der **Bedeutung des Buchstabens N**. Wir bauen die Definitionen so auf, dass diese drei Stellen sichtbar werden.

### 1.1 Das Rechenmodell und der Begriff »Eingabelänge« `[KANON]`

Grundlage ist die **deterministische Turingmaschine** (DTM). Für unsere Zwecke genügt: ein Programm, das auf einem beliebig langen Band arbeitet und pro Schritt eine elementare Operation ausführt.

Entscheidend — und das ist der am häufigsten übersehene Punkt des gesamten Feldes — ist die Definition der **Eingabelänge**:

> Die Eingabe ist ein **Wort über einem endlichen Alphabet**, typischerweise ein Bitstring. Die Eingabelänge *n* ist die **Anzahl der Symbole (Bits)** dieses Wortes — nicht die Anzahl der »Objekte«, nicht der numerische *Wert* einer Zahl in der Eingabe.

Eine natürliche Zahl *W* wird binär kodiert und belegt dann ⌈log₂(W+1)⌉ Bits. Das heißt: Eine Eingabe der Länge *n* kann Zahlen bis zur Größenordnung 2ⁿ enthalten. »Polynomiell in der Eingabelänge« und »polynomiell im Zahlenwert« sind daher **grundverschiedene Aussagen**, die sich exponentiell unterscheiden. Fallstrick (a) in §1.7 lebt genau von dieser Verwechslung.

Ein **Entscheidungsproblem** wird formalisiert als **Sprache** L ⊆ {0,1}\*, also als die Menge derjenigen Eingaben, auf die die Antwort »ja« lautet. Das ist keine Einschränkung der Allgemeinheit: Optimierungsprobleme werden über Schwellwertfragen (»Gibt es eine Lösung mit Wert ≥ k?«) in Entscheidungsprobleme übersetzt, und für die hier relevanten Probleme sind Such-, Optimierungs- und Entscheidungsvariante durch **Selbstreduzierbarkeit** polynomiell äquivalent.

### 1.2 Die Klasse P `[KANON]`

> **P** (auch PTIME) ist die Klasse aller Sprachen L, für die es eine deterministische Turingmaschine M und eine Konstante c gibt, so dass M für **jede** Eingabe x korrekt entscheidet, ob x ∈ L, und dabei höchstens O(|x|^c) Schritte benötigt.

Drei Feinheiten, die im Papier explizit stehen müssen:

- **Worst Case.** Die Schranke muss für *jede* Eingabe gelten, nicht im Mittel, nicht auf Benchmarks, nicht »in der Praxis«. Ein Verfahren, das 99,9999 % aller Instanzen schnell löst, ist damit noch nicht in P.
- **Fester Exponent.** Der Exponent c ist eine Konstante, die *vor* der Eingabe festgelegt ist. Eine Familie von Algorithmen mit wachsenden Exponenten ist kein Polynomialzeit-Algorithmus. Fallstrick (b) in §1.7 lebt hiervon.
- **Robustheit.** P ist gegenüber der Wahl des Maschinenmodells robust (Mehrband- vs. Einband-Turingmaschine, RAM-Modell): Die Übersetzungen kosten nur polynomiellen Mehraufwand, und Polynome sind unter Komposition abgeschlossen. Genau diese Robustheit macht P zur »richtigen« mathematischen Abstraktion von »effizient berechenbar« — trotz der berechtigten praktischen Einwände (ein n¹⁰⁰-Algorithmus ist nutzlos).

### 1.3 Die Klasse NP — zwei äquivalente Definitionen `[KANON]`

**(A) Zertifikats-/Verifier-Definition.**

> L ∈ **NP**, wenn es eine deterministische Polynomialzeit-Turingmaschine V (den »Verifizierer«) und ein Polynom p gibt, so dass für alle x gilt:
> x ∈ L ⟺ es existiert ein Wort w (das »Zertifikat« oder »Zeuge«) mit |w| ≤ p(|x|) und V(x, w) akzeptiert.

In Worten: NP ist die Klasse der Probleme, bei denen eine *vorgelegte Lösung* in Polynomialzeit **überprüfbar** ist. Beispiel SAT: Das Zertifikat ist eine erfüllende Belegung; das Einsetzen und Auswerten dauert linear.

**(B) Nichtdeterministische Definition.**

> L ∈ **NP**, wenn L von einer **nichtdeterministischen** Turingmaschine (NTM) in Polynomialzeit entschieden wird. Eine NTM darf pro Schritt zwischen mehreren Nachfolgekonfigurationen »raten«; sie akzeptiert, wenn *mindestens ein* Rechenweg akzeptiert.

(A) und (B) sind äquivalent: Der Rateweg der NTM *ist* das Zertifikat, und umgekehrt kann eine NTM das Zertifikat raten und dann V simulieren. `[KANON]`

Der Name **NP** steht für **»Nondeterministic Polynomial time«**. Dazu §1.7(c).

**Asymmetrie.** Die Definition ist einseitig: Sie verlangt einen kurzen Beweis für die **Ja**-Antwort. Für die Nein-Antwort ist nichts gefordert. Die Klasse mit kurzen Beweisen für die Nein-Antwort heißt **coNP** = { L : L̄ ∈ NP }. Ob NP = coNP gilt, ist ebenfalls offen (§6.5).

**P ⊆ NP** ist trivial: Wer selbst rechnen kann, braucht kein Zertifikat (V ignoriert w und entscheidet selbst). `[KANON]`

### 1.4 Karp-Reduktionen `[KANON]`

> Eine **Karp-Reduktion** (auch: polynomielle Many-One-Reduktion) von A auf B, geschrieben A ≤ₚ B, ist eine in Polynomialzeit berechenbare Funktion f mit
> x ∈ A ⟺ f(x) ∈ B für alle x.

Intuition: B ist »mindestens so schwer« wie A, denn ein Löser für B ergibt zusammen mit f einen Löser für A.

Zwei technische Punkte, die man beim Beweis in §1.6 braucht:
- **Längenschranke.** Wenn f in Zeit O(n^d) berechenbar ist, dann gilt |f(x)| = O(|x|^d), denn eine Maschine kann in n^d Schritten höchstens n^d Symbole schreiben. Die Reduktion kann die Instanz also nur polynomiell aufblähen.
- **Transitivität/Abgeschlossenheit.** ≤ₚ ist transitiv, weil die Komposition zweier Polynome wieder ein Polynom ist ((n^d)^c = n^{dc}). Das ist der ganze Trick.

Daneben gibt es die allgemeinere **Cook-Reduktion** (Turing-Reduktion: polynomielle Maschine mit Orakelzugriff auf B). Für die NP-Vollständigkeitstheorie ist die Karp-Reduktion der Standard, weil sie feinere Unterscheidungen erhält (z. B. zwischen NP und coNP).

### 1.5 NP-Härte, NP-Vollständigkeit, Cook–Levin, Karp `[KANON]` / `[VERIFIZIERT]`

> L ist **NP-hart**, wenn A ≤ₚ L für **alle** A ∈ NP.
> L ist **NP-vollständig**, wenn L NP-hart ist **und** L ∈ NP.

NP-vollständige Probleme sind also die »schwersten« Probleme in NP: Alle anderen NP-Probleme lassen sich auf sie zurückführen. NP-hart allein sagt *nicht*, dass das Problem in NP liegt — es gibt NP-harte Probleme weit jenseits von NP (z. B. EXPSPACE-vollständige Probleme). Diese Unterscheidung wird in populären Darstellungen regelmäßig verschliffen.

**Satz (Cook 1971 / Levin 1973): SAT ist NP-vollständig.** `[KANON]`
Cook, »The Complexity of Theorem-Proving Procedures«, STOC 1971; Levin unabhängig 1973. Beweisidee: Man kodiert den gesamten Rechenverlauf einer nichtdeterministischen Polynomialzeit-Maschine auf einer Eingabe x als Boolesche Formel (»Tableau«-Konstruktion), deren Erfüllbarkeit genau der Existenz eines akzeptierenden Rechenwegs entspricht. Dass das *überhaupt* geht, ist der eigentliche Inhalt: Ein einziges, konkretes kombinatorisches Problem kodiert die Berechnungen *aller* NP-Maschinen.

**Karp 1972: 21 NP-vollständige Probleme.** `[KANON]` Karp zeigte, dass 21 zentrale kombinatorische Probleme (u. a. CLIQUE, VERTEX COVER, HAMILTONKREIS, KNAPSACK, PARTITION, CHROMATIC NUMBER) NP-vollständig sind — jeweils durch Reduktion von einem bereits als NP-vollständig bekannten Problem. Heute sind mehrere tausend Probleme bekannt. Diese Sammlung ist der Grund, warum NP-Vollständigkeit ein *praktisch* relevanter Begriff ist und nicht nur ein logisches Kuriosum.

**Historisch/institutionell:** P vs. NP wurde im Jahr 2000 als eines der sieben **Millennium-Probleme** des Clay Mathematics Institute ausgewählt (Preisgeld 1 Mio. USD); die offizielle Problembeschreibung stammt von Stephen Cook. `[VERIFIZIERT]` (claymath.org/millennium/p-vs-np/, trianguliert)

### 1.6 Warum *ein* Algorithmus für *ein* NP-vollständiges Problem genügt

Das ist die formal wichtigste Aussage des Kapitels; sie wird oft zitiert und selten begründet. Der Beweis ist kurz.

**Behauptung.** Sei L NP-vollständig. Wenn L ∈ P, dann P = NP. `[KANON]`

**Beweis.** Sei A ein deterministischer Algorithmus, der L in Zeit O(n^c) entscheidet. Sei L′ ∈ NP beliebig. Da L NP-vollständig (insbesondere NP-hart) ist, existiert eine Karp-Reduktion f von L′ auf L, berechenbar in Zeit O(n^d) für ein festes d. Folgender Algorithmus entscheidet L′ auf Eingabe x mit |x| = n:

1. Berechne y := f(x). Kosten: O(n^d). Nach der Längenschranke aus §1.4 gilt |y| = O(n^d).
2. Rufe A auf y auf. Kosten: O(|y|^c) = O((n^d)^c) = O(n^{dc}).
3. Gib die Antwort von A zurück. Korrekt, weil x ∈ L′ ⟺ f(x) ∈ L.

Gesamtkosten: O(n^d) + O(n^{dc}) = O(n^{dc}) — polynomiell, mit **festem** Exponenten dc. Also L′ ∈ P. Da L′ ∈ NP beliebig war: NP ⊆ P. Mit P ⊆ NP (§1.3) folgt **P = NP**. ∎

**Die Last des Beweises liegt vollständig in den Voraussetzungen.** Wer einen P=NP-Beweis prüft, prüft im Wesentlichen diese fünf Punkte:

| # | Voraussetzung | Typischer Fehler in fehlerhaften Beweisversuchen |
|---|---|---|
| V1 | Der Algorithmus ist **korrekt auf allen Eingaben** | Korrektheit nur auf Zufallsinstanzen, Benchmarks oder einer Instanzfamilie gezeigt |
| V2 | Die Laufzeitschranke gilt im **Worst Case** | Average-Case- oder empirische Laufzeit |
| V3 | Die Laufzeit ist polynomiell in der **Bitlänge** | pseudopolynomiell (→ §1.7a) |
| V4 | Der Exponent ist **fest**, unabhängig von der Instanz | Exponent hängt von einem Eingabeparameter ab (→ §1.7b) |
| V5 | Das gelöste Problem ist **tatsächlich das NP-vollständige** | eine eingeschränkte Variante gelöst (2-SAT statt 3-SAT, planare Instanzen, feste Parameter) |

`[EIGENE EINSCHÄTZUNG, Konfidenz hoch]` Diese Tabelle ist die operative Form der Muss-Kriterien M2/M3 aus `docs/02-team-briefing.md` §4 für die *obere* Schranke (P=NP-Richtung). Die Barrieren in §3 sind das Gegenstück für die *untere* Schranke (P≠NP-Richtung).

**Zusatzbemerkung.** Ein randomisierter Polynomialzeit-Algorithmus für ein NP-vollständiges Problem würde NP ⊆ BPP zeigen — das wäre sensationell (und hätte, mit Standard-Derandomisierungsannahmen, ebenfalls P = NP zur Folge), ist aber nicht *wörtlich* P = NP. Ein Quantenalgorithmus würde NP ⊆ BQP zeigen, was nicht P = NP ist. Diese Unterscheidungen sind bei der Bewertung von Claims wichtig.

### 1.7 Die drei klassischen Verwechslungen

#### (a) Knapsack ist pseudopolynomiell lösbar — warum ist O(n·W) *kein* Polynomialzeit-Algorithmus? `[KANON]`

**Das Problem.** KNAPSACK: Gegeben n Gegenstände mit Gewichten w₁,…,wₙ und Werten v₁,…,vₙ, eine Kapazität W und ein Zielwert V — gibt es eine Teilmenge mit Gesamtgewicht ≤ W und Gesamtwert ≥ V? KNAPSACK ist NP-vollständig (Karp 1972).

**Der Algorithmus.** Dynamische Programmierung über eine Tabelle T[i][j] (i = 1…n Gegenstände, j = 0…W Restkapazität). Die Tabelle hat (n+1)·(W+1) Einträge, jeder in O(1) Zeit berechenbar. Laufzeit: **O(n·W)**. Das ist ein völlig korrekter, in der Praxis oft exzellenter Algorithmus.

**Warum ist das nicht polynomiell?** Die Eingabe enthält die Zahl W. W wird **binär** kodiert und belegt nur b := ⌈log₂ W⌉ Bits. Die Eingabelänge ist also ungefähr

  n · (Bits pro Gegenstand) + b.

Der Algorithmus läuft in O(n·W) = O(n · 2^b) Schritten — das ist **exponentiell in b**, also exponentiell in der Eingabelänge.

**Ein konkretes Zahlenbeispiel macht es unmissverständlich.** Nehmen wir n = 100 Gegenstände und eine Kapazität W = 2¹⁰⁰. Die Zahl W braucht 100 Bits; die gesamte Eingabe hat vielleicht ein paar tausend Bits. Der DP-Algorithmus baut aber eine Tabelle mit 100 · 2¹⁰⁰ ≈ 10³² Einträgen. Das sind mehr Rechenschritte, als in der Lebensdauer des Universums auf jeder denkbaren Hardware ausführbar wären. Verdoppelt man die Bitlänge von W von 100 auf 200 Bits — die Eingabe wächst um 100 Bits, also um wenige Prozent — *quadriert* sich die Laufzeit. Das ist exponentielles Verhalten in Reinform.

**Fachbegriffe dazu, die im Papier stehen sollten:**
- Ein Algorithmus heißt **pseudopolynomiell**, wenn seine Laufzeit polynomiell im *numerischen Wert* der Eingabezahlen ist (also polynomiell, wenn man die Zahlen **unär** kodiert).
- Ein Problem heißt **schwach NP-vollständig** (weakly NP-complete), wenn es NP-vollständig ist, aber einen pseudopolynomiellen Algorithmus besitzt. KNAPSACK, PARTITION und SUBSET-SUM gehören hierher.
- Ein Problem heißt **stark NP-vollständig** (strongly NP-complete), wenn es sogar dann NP-vollständig bleibt, wenn alle Zahlen in der Eingabe unär kodiert sind bzw. durch ein Polynom in der Eingabelänge beschränkt sind. Beispiele: 3-SAT und CLIQUE (die enthalten gar keine großen Zahlen), 3-PARTITION, TSP. **Für ein stark NP-vollständiges Problem würde ein pseudopolynomieller Algorithmus bereits P = NP implizieren** — für ein schwach NP-vollständiges Problem wie KNAPSACK eben nicht.
- KNAPSACK besitzt zudem ein **FPTAS** (fully polynomial-time approximation scheme): Für jedes ε > 0 findet man in Zeit polynomiell in n und 1/ε eine Lösung mit Wert ≥ (1−ε)·OPT. Auch das ist *kein* Schritt in Richtung P = NP — es ist eine Approximation, keine exakte Lösung.

**Die Pointe für das Papier.** »Ich habe einen schnellen Algorithmus für ein NP-vollständiges Problem« ist erst dann eine Aussage über P vs. NP, wenn geklärt ist, *worin* die Laufzeit polynomiell ist. KNAPSACK ist das Lehrbuchbeispiel dafür, dass ein NP-vollständiges Problem einen völlig legitimen, praktisch brauchbaren, exakten Algorithmus haben kann, ohne dass sich an P vs. NP irgendetwas ändert.

#### (b) Clique ist für **festes** k in P — warum ist CLIQUE trotzdem NP-vollständig? `[KANON]`

**Die beiden Probleme sind nicht dasselbe Problem.** Das ist der ganze Punkt, und er ist leicht zu übersehen, weil beide »Clique« heißen.

- **k-CLIQUE für festes k** (z. B. k = 5): Die Sprache ist { G : G enthält eine 5-Clique }. Hier ist k eine **Konstante des Problems**, kein Teil der Eingabe. Brute Force über alle Teilmengen der Größe 5 kostet C(n,5) · O(25) = O(n⁵) — polynomiell. Für jedes feste k ist k-CLIQUE in P, mit Laufzeit O(n^k · k²) (schneller mit Matrixmultiplikation). `[KANON]`
- **CLIQUE**: Die Sprache ist { (G, k) : G enthält eine k-Clique }. Hier ist k **Teil der Eingabe**. Dieses Problem ist NP-vollständig (Karp 1972).

**Warum rettet die erste Aussage die zweite nicht?** Weil P einen **festen** Exponenten verlangt (§1.2). Man hat hier eine *Familie* von Algorithmen A₅, A₆, A₇, … mit Laufzeiten n⁵, n⁶, n⁷, … — aber **keinen einzigen** Algorithmus mit einer Laufzeitschranke n^c für ein festes c, das für alle Instanzen gilt. Bei CLIQUE ist k durch nichts beschränkt: k darf bis zu n groß sein. Der Ausdruck n^k ist dann n^n — vollständig exponentiell.

Formal: Die Definition von P quantifiziert »∃c ∀x«. Die Aussage über festes k liefert nur »∀k ∃c« (nämlich c = k). Die Vertauschung der Quantoren ist der Fehler. `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]`

**Der Begriffsapparat dazu heißt parametrisierte Komplexität:**
- **XP**: Probleme mit Laufzeit n^{f(k)} — der Parameter darf im **Exponenten** stehen. CLIQUE ist in XP (das ist genau die Aussage »für festes k in P«).
- **FPT** (fixed-parameter tractable): Laufzeit f(k) · n^{O(1)} — der Parameter darf nur einen *Vorfaktor* erzeugen, der Exponent ist fest. VERTEX COVER ist FPT (Laufzeit ca. 1,27^k · n^{O(1)}), CLIQUE ist es nach heutigem Kenntnisstand nicht: CLIQUE ist **W[1]-vollständig**, und FPT = W[1] gilt als ähnlich unwahrscheinlich wie P = NP.

**Die allgemeine Lehre**, die im Papier explizit stehen muss: *Jede* NP-vollständige Sprache hat leichte Teilfamilien. 2-SAT ist in P, 3-SAT ist NP-vollständig. KNAPSACK mit kleinen Zahlen ist leicht, KNAPSACK mit großen Zahlen ist hart. Graphenfärbung ist auf Bäumen trivial und im Allgemeinen NP-vollständig. Ein Algorithmus, der eine *Einschränkung* löst, sagt über P vs. NP nichts — das ist Voraussetzung V5 aus §1.6, und es ist der mit Abstand häufigste Konstruktionsfehler in fehlerhaften P=NP-Beweisen.

#### (c) »NP« heißt nicht »nicht-polynomiell« `[KANON]`

**NP = Nondeterministic Polynomial time.** Das »N« steht für *nondeterministic*, nicht für *not*. Die Verwechslung ist so verbreitet, dass sie in fast jeder Einführung eigens dementiert wird.

Warum die falsche Lesart mathematisch unmöglich ist, sieht man in einer Zeile: **P ⊆ NP.** Jedes Problem in P liegt in NP (§1.3). Wäre NP »die nicht-polynomiell lösbaren Probleme«, wäre P ⊆ NP absurd — dann wäre jedes leichte Problem auch schwer. Insbesondere sind Sortieren, Kürzeste-Wege, Primzahltest (AKS 2002) und lineare Programmierung allesamt in NP, weil sie in P sind.

Die Frage »P = NP?« lautet ausgeschrieben: **Ist alles, was man effizient *überprüfen* kann, auch effizient *findbar*?** Sie fragt nicht, ob NP-Probleme schwer sind — sie fragt, ob »Lösung erkennen« und »Lösung finden« dasselbe sind.

Zwei angrenzende Verwechslungen gleich mit:
- **»NP-hart« ≠ »in NP«.** Ein NP-hartes Problem kann weit außerhalb von NP liegen (z. B. das Halteproblem ist NP-hart, aber nicht einmal entscheidbar).
- **»NP-vollständig« ≠ »unlösbar«.** NP-vollständige Probleme sind vollständig entscheidbar — mit Brute Force in Exponentialzeit. Sie sind *vermutlich* nicht effizient lösbar. Das ist ein Unterschied ums Ganze, und moderne SAT-Solver, die Industrieinstanzen mit Millionen Variablen routinemäßig knacken, sind der lebende Beweis dafür, dass »NP-vollständig« und »in der Praxis unlösbar« zwei verschiedene Aussagen sind (→ §7.3, Optiland).

---

## 2. Was P = NP bzw. P ≠ NP formal bedeuten würde — Vorbemerkung zur Beweislast

Bevor wir zu den Barrieren kommen, eine Beobachtung zur Asymmetrie der beiden Richtungen. `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]`

- **P = NP zu zeigen** heißt: **ein** Objekt angeben (einen Algorithmus) und seine Korrektheit und Laufzeit beweisen. Das ist ein **existenzieller** Beweis. Er hat, wenn er gelingt, einen endlichen Zeugen.
- **P ≠ NP zu zeigen** heißt: über **alle** Algorithmen quantifizieren und für jeden von ihnen nachweisen, dass er scheitert. Das ist ein **universeller** Beweis über eine unendliche, nicht aufzählbar strukturierte Klasse. Es gibt kein endliches Objekt, dessen Vorlage die Sache erledigt, und kein billiges Verifikationsorakel.

Genau diese zweite Richtung ist es, gegen die sich die drei Barrieren richten: Sie sind allesamt Aussagen der Form »Beweistechniken vom Typ X können eine solche universelle Aussage nicht erreichen«.

Diese Asymmetrie ist zugleich die direkte Bestätigung der **Leitachse** aus `docs/02-team-briefing.md` §2 (siehe §8).

---

## 3. Die drei Barrieren

### 3.0 Was eine »Barriere« überhaupt ist

Eine Barriere in der Komplexitätstheorie ist ein **Metatheorem**: ein mathematischer Satz nicht über Rechenprobleme, sondern über **Beweise**. Das Schema ist immer dasselbe:

1. Man formalisiert eine Eigenschaft, die alle bekannten Beweise einer bestimmten Art teilen (z. B. »der Beweis bleibt gültig, wenn alle Maschinen Zugriff auf ein Orakel bekommen«).
2. Man zeigt: **Jeder** Beweis mit dieser Eigenschaft kann die Aussage P ≠ NP nicht liefern — meist, indem man eine Welt konstruiert, in der die Eigenschaft gilt, die Aussage aber falsch ist.
3. Folgerung: Wer P ≠ NP beweisen will, braucht eine Technik **ohne** diese Eigenschaft.

Barrieren sind also **Wegweiser**, keine Mauern: Sie sagen, welche Straßen nachweislich in Sackgassen enden. Sie sagen nichts darüber, ob es andere Straßen gibt. Das ist der Punkt, an dem die populäre Rezeption regelmäßig falsch abbiegt (→ §3.4).

### 3.1 Barriere 1: Relativization (Baker–Gill–Solovay 1975)

**Quelle.** T. Baker, J. Gill, R. Solovay: *Relativizations of the P =? NP Question*, SIAM Journal on Computing 4(4), 431–442, 1975. `[VERIFIZIERT]` (Existenz, Venue und Kernaussage über zwei unabhängig formulierte Suchen bestätigt; Volltext nicht zugänglich → `[NUR-SNIPPET]` für Details)

**Der Begriff des Orakels.** Eine **Orakel-Turingmaschine** M^A ist eine gewöhnliche Turingmaschine, die zusätzlich ein »Orakelband« besitzt: Sie kann darauf ein Wort y schreiben und erhält in **einem** Schritt die Antwort auf die Frage »y ∈ A?«. Das Orakel A ist eine beliebige feste Sprache. P^A bzw. NP^A sind die entsprechenden relativierten Klassen.

**Der Satz.**
> Es existiert ein Orakel A mit **P^A = NP^A**, und es existiert ein Orakel B mit **P^B ≠ NP^B**.

Skizze (Standarddarstellung): Für A nimmt man eine hinreichend mächtige Sprache, etwa ein PSPACE-vollständiges Problem (dann kollabieren beide Seiten, weil P^A = PSPACE = NP^A). Für B konstruiert man das Orakel per Diagonalisierung so, dass eine bestimmte Sprache (»enthält B ein Wort der Länge n?«) für NP^B leicht — raten und einmal fragen — und für P^B nachweislich hart ist, weil eine Polynomialzeit-Maschine nur polynomiell viele der 2ⁿ Kandidaten abfragen kann. `[KANON]`

**Welche Technik wird ausgeschlossen?** **Relativierende** Beweise — insbesondere die klassische **Diagonalisierung** und Simulationsargumente. Ein Beweis relativiert, wenn er die Maschinen nur als Black Box behandelt (simuliert, zählt Schritte, diagonalisiert), ohne auf ihre innere Struktur zuzugreifen. Solche Beweise bleiben wortwörtlich gültig, wenn man allen beteiligten Maschinen dasselbe Orakel gibt.

**Das Argument.** Angenommen, es gäbe einen relativierenden Beweis von P ≠ NP. Dann würde er auch P^O ≠ NP^O für **jedes** Orakel O zeigen — insbesondere für das Orakel A, für das nachweislich P^A = NP^A gilt. Widerspruch. Dasselbe Argument in der Gegenrichtung mit B schließt relativierende Beweise von P = NP aus. Also: **P vs. NP ist mit relativierenden Techniken in keiner der beiden Richtungen entscheidbar.**

**Der schmerzhafte Teil:** Genau die Diagonalisierung ist die Technik, mit der alle klassischen Separationsresultate erzielt wurden — Zeithierarchie-Satz, Raumhierarchie-Satz, Unentscheidbarkeit des Halteproblems. Das Werkzeug, das nachweislich funktioniert, ist das Werkzeug, das hier nachweislich nicht funktionieren kann. `[NUR-SNIPPET]`, bestätigt durch mehrere Lehrmaterial-Treffer: »Der Zeithierarchie-Satz gilt in *jeder* relativierten Welt«.

**Was die Barriere NICHT sagt:**
- Sie sagt nicht, dass P vs. NP unlösbar ist.
- Sie sagt nicht, dass Diagonalisierung generell nutzlos ist. Es gibt nicht-relativierende Diagonalisierungsvarianten; ein Preprint von 2026 (arXiv:2601.09702, *Diagonalization Without Relativization: A Closer Look at the Baker–Gill–Solovay Theorem*) arbeitet genau an dieser Grenze. `[PREPRINT]`, Inhalt ungeprüft.
- Sie sagt nichts über die Wahrscheinlichkeit, dass P = NP oder P ≠ NP gilt. Orakelwelten sind Hilfskonstruktionen, keine Evidenz über die reale Welt. (Es gibt in der Literatur die Gegenposition »Orakelresultate sind schwache Evidenz«; sie ist umstritten und sollte im Papier nicht als Konsens dargestellt werden.)

### 3.2 Barriere 2: Natural Proofs (Razborov–Rudich)

**Quelle.** A. A. Razborov, S. Rudich: *Natural Proofs*, Journal of Computer and System Sciences 55(1), 24–35, August 1997 (Konferenzversion STOC 1994). Gödel-Preis 2007. `[VERIFIZIERT]` (Venue, Band, Seiten und die drei definierenden Eigenschaften über zwei unabhängige Suchen bestätigt)

**Der Kontext.** Um P ≠ NP zu zeigen, genügt eine **superpolynomielle untere Schranke für die Schaltkreisgröße** einer Funktion in NP: Wenn ein NP-Problem keine polynomiell großen Booleschen Schaltkreise hat, dann ist es nicht in P (denn P ⊆ P/poly). In den 1980ern gab es dramatische Erfolge für *eingeschränkte* Schaltkreisklassen (Ajtai, Furst–Saxe–Sipser und Håstad für AC⁰ mittels Switching Lemma; Razborov und Smolensky für AC⁰[p]; Razborov für monotone Schaltkreise). Dann kam der Stillstand — und Razborov und Rudich erklärten, warum.

**Die Definition.** Eine Beweistechnik für untere Schranken heißt **natural**, wenn sie sich auf eine Eigenschaft P\* von Booleschen Funktionen stützt, die drei Bedingungen erfüllt: `[VERIFIZIERT]`

1. **Constructivity** (Konstruktivität): P\* ist in Zeit polynomiell in der Länge der *Wahrheitstafel* der Funktion (also in 2ⁿ) testbar. Die Eigenschaft muss algorithmisch effizient erkennbar sein.
2. **Largeness** (Größe): P\* trifft auf einen nicht vernachlässigbaren Anteil *aller* Booleschen Funktionen auf n Variablen zu. Die Eigenschaft ist also »typisch« und nicht auf wenige Ausnahmefunktionen zugeschnitten.
3. **Usefulness** (Nützlichkeit): Keine Funktion mit der Eigenschaft P\* hat kleine Schaltkreise der betrachteten Klasse. Das ist die Eigenschaft, die die untere Schranke überhaupt liefert.

Razborov und Rudich beobachteten: **Praktisch alle bekannten Beweise unterer Schranken in nicht-monotonen Modellen sind in diesem Sinne natural.** `[VERIFIZIERT]`

**Der Satz.**
> Wenn hinreichend starke **Pseudozufallsfunktionen** existieren (eine Standardannahme der Kryptographie, z. B. impliziert durch die Härte des diskreten Logarithmus oder allgemeiner durch die Existenz von Einwegfunktionen), dann kann **keine natural proof** superpolynomielle untere Schranken für allgemeine Boolesche Schaltkreise liefern.

**Das Argument in einem Satz.** Eine Eigenschaft, die *large* und *useful* gegen eine Schaltkreisklasse C ist und *constructive* getestet werden kann, ist automatisch ein effizienter **Unterscheider** (Distinguisher): Sie sagt bei zufälligen Funktionen »ja« (largeness) und bei allen in C berechenbaren Funktionen »nein« (usefulness) — und sie tut das effizient (constructivity). Wären die Pseudozufallsfunktionen selbst in C berechenbar, wäre ihre Sicherheit damit gebrochen. Anders gesagt: **Wer mit natural proofs beweist, dass gewisse Funktionen hart sind, beweist damit zugleich, dass gewisse Funktionen nicht hart genug für Kryptographie sind.** Die Annahme, dass starke Kryptographie existiert, schließt genau diese Beweise aus. `[VERIFIZIERT]`, Formulierung des Mechanismus `[NUR-SNIPPET]`

**Die ironische Pointe für das Papier.** Die Barriere ist eine **Selbstblockade des Feldes**: Die (fast einhellig geglaubte) Vermutung P ≠ NP wäre die Grundlage der Kryptographie, und die (fast einhellig geglaubte) Sicherheit der Kryptographie verbietet den naheliegendsten Weg, P ≠ NP zu beweisen. Je stärker man an die Härte glaubt, desto weniger Beweiswerkzeuge stehen zur Verfügung. `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]`

**Was die Barriere NICHT sagt:**
- Sie ist **bedingt**. Sie hängt an der Existenz von Pseudozufallsfunktionen. Wären die nicht vorhanden (Impagliazzos Welt »Pessiland« oder darunter, → §7.2), fiele die Barriere. Das ist ein struktureller Unterschied zu Relativization und Algebrization, die unbedingt sind.
- Sie schließt nur **natural** proofs aus. Ein Beweis darf eine der drei Bedingungen verletzen — insbesondere **largeness** (eine Eigenschaft, die nur auf sehr wenige, gezielt konstruierte Funktionen zutrifft) oder **constructivity** (eine nicht effizient testbare Eigenschaft). Genau dort setzen die erfolgreichen Umgehungen an (§3.5).
- Sie sagt **nicht**, dass Schaltkreis-untere-Schranken der falsche Weg sind. Sie sagt, dass eine bestimmte *Art*, sie zu beweisen, nicht reicht.

**Aktuelle Entwicklung 2026.** arXiv:2606.12631, *The Switching Lemma shows what the Switching Lemma cannot prove: an unconditional natural-proofs barrier*. `[PREPRINT]`, Kernaussage laut Suchsynthese `[NUR-SNIPPET]`: Die Arbeit betrachtet **AC⁰-natural proofs** (natural proofs, deren Unterscheider selbst von einem AC⁰-Schaltkreis berechnet wird) und zeigt eine **unbedingte** Schranke — ohne kryptographische Annahme — dass solche Beweise keine Schranken oberhalb von etwa 2^{n^{7/(d−5)}} gegen Tiefe-d-Schaltkreise liefern können; das liegt in derselben quantitativen Region wie das, was das Switching Lemma selbst erreicht (n^{1/(d−1)} im Exponenten). Technisch über eine Lokalisierung des Trevisan–Xue-Pseudozufallsgenerators. Der selbstbezügliche Witz: Die Sicherheit dieses Generators wird ihrerseits mit dem Switching Lemma bewiesen — das Switching Lemma zeigt also seine eigene Grenze. Dass 2026 weiterhin neue, sogar *unbedingte* Barrieren gefunden werden, ist ein aussagekräftiger Indikator für die Lage des Feldes.

### 3.3 Barriere 3: Algebrization (Aaronson–Wigderson 2008)

**Quelle.** S. Aaronson, A. Wigderson: *Algebrization: A New Barrier in Complexity Theory*, STOC 2008, S. 731–740; Journalfassung ACM Transactions on Computation Theory (ToCT) 1(1), 2009, DOI 10.1145/1490270.1490272. `[VERIFIZIERT]` (STOC-Jahr, Seiten, ToCT-Fassung und DOI über zwei unabhängige Suchen bestätigt)

**Der Kontext: Arithmetisierung.** In den frühen 1990ern entstanden Resultate, die die Relativization-Barriere nachweislich durchbrachen — allen voran **IP = PSPACE** (Shamir 1992, aufbauend auf Lund–Fortnow–Karloff–Nisan), obwohl es Orakel O mit coNP^O ⊄ IP^O gibt. Die entscheidende Technik hieß **Arithmetisierung**: Man ersetzt eine Boolesche Formel durch ein Polynom niedrigen Grades über einem endlichen Körper, das auf {0,1} mit ihr übereinstimmt, und nutzt dann algebraische Eigenschaften (Grad, Anzahl der Nullstellen, Schwartz–Zippel). Die Hoffnung der 1990er und 2000er: Arithmetisierung ist der Ausweg aus der Relativization.

**Die Idee der Algebrization.** Aaronson und Wigderson erweiterten den Orakelbegriff so, dass er Arithmetisierung mit erfasst. `[VERIFIZIERT]`

> Beim Relativieren einer Klasseninklusion erhält die simulierende Maschine nicht nur Zugriff auf das Orakel A, sondern zusätzlich auf eine **Low-Degree-Extension** Ã von A über einem endlichen Körper oder Ring — also auf die algebraische Fortsetzung, die die Arithmetisierung erzeugen würde.

Ein Beweis **algebrisiert**, wenn er in diesem erweiterten Modell gültig bleibt. IP = PSPACE algebrisiert (deshalb ist Algebrization eine echte Verschärfung: die Barriere erfasst mehr Techniken als Relativization).

**Der Satz (sinngemäß).** `[NUR-SNIPPET]`
> Es gibt algebrische Orakel, relativ zu denen P = NP gilt, und andere, relativ zu denen P ≠ NP gilt. **Algebrisierende Techniken — also Arithmetisierung in der bekannten Form — reichen für P vs. NP nicht aus.** Die Arbeit führt dazu ein Modell der »algebraischen Anfragekomplexität« ein und beweist die nötigen unteren Schranken in diesem Modell.

**Die Bedeutung.** Das ist die deprimierendste der drei Barrieren, weil sie die einzige Technik trifft, die zuvor nachweislich eine Barriere durchbrochen hatte. Die Aussage lautet im Kern: *Auch der Ausweg aus Barriere 1 führt nicht weiter.* Aaronson und Wigderson formulierten daraus die Forderung nach »non-algebrizing techniques« als Kriterium für ernstzunehmende Ansätze.

**Was die Barriere NICHT sagt:**
- Sie erklärt Arithmetisierung nicht für wertlos — IP = PSPACE, das PCP-Theorem und die gesamte interaktive-Beweise-Landschaft bleiben gültig und wichtig.
- Sie ist an das formale Modell gebunden. Eine Beweistechnik, die stärkere algebraische Struktur nutzt (etwa Darstellungstheorie in der Geometric Complexity Theory), ist nicht automatisch erfasst.
- Sie ist, wie die anderen, **keine** Aussage über die Lösbarkeit des Problems.

**Aktuelle Entwicklung.** Die Algebrization-Linie wird weiterentwickelt: arXiv:2511.14038, *New Algebrization Barriers to Circuit Lower Bounds via Communication Complexity of Missing-String* (Nov. 2025). `[PREPRINT]`, Inhalt ungeprüft.

### 3.4 Korrektur der verbreiteten Überinterpretation

Die Aussage **»Die Barrieren zeigen, dass P vs. NP unlösbar ist«** ist **falsch**. Sie ist in populären Darstellungen weit verbreitet und muss im Papier explizit korrigiert werden. Vier Punkte:

**(1) Barrieren sind Aussagen über Techniken, nicht über das Problem.** Jede der drei Barrieren hat die logische Form: »Beweise mit Eigenschaft X reichen nicht.« Keine hat die Form »Es gibt keinen Beweis.« Die Menge aller mathematischen Beweistechniken ist nicht erschöpft — sie ist nicht einmal aufzählbar formalisiert.

**(2) Barrieren ≠ Unabhängigkeit von ZFC.** Die Frage, ob P vs. NP *formal unabhängig* von ZFC oder von schwächeren Systemen wie Peano-Arithmetik sein könnte, ist eine **eigenständige, offene Frage** mit eigener Literatur — nicht eine Folgerung aus den Barrieren. Scott Aaronson hat dazu die einschlägige Übersicht geschrieben (*Is P Versus NP Formally Independent?*, Bulletin of the EATCS, ca. 2003). `[VERIFIZIERT]` (Existenz; Inhalt `[NUR-SNIPPET]`). Die Forschungslinie »Complexity Barriers as Independence« (Kolokolova) untersucht den Zusammenhang zwischen Barrieren und Unbeweisbarkeit in *schwachen* Systemen (bounded arithmetic) — das ist etwas deutlich Spezifischeres als »unabhängig von ZFC«. `[NUR-SNIPPET]` In der Literatur gibt es außerdem das Ergebnis von Ben-David und Halevi, dass Unabhängigkeit von hinreichend starken Systemen ihrerseits erstaunliche algorithmische Konsequenzen hätte (»NP wäre im Wesentlichen Polynomialzeit«). `[NUR-SNIPPET]`, Konfidenz mittel. **Der Punkt für unser Papier:** Unabhängigkeit ist eine Minderheitenposition, kein Konsens, und die Barrieren belegen sie nicht.

**(3) Es existieren publizierte Resultate, die alle drei Barrieren nachweislich umgehen.** Siehe §3.5. Wenn Barrieren unüberwindlich wären, gäbe es diese Resultate nicht.

**(4) Die korrekte Lesart.** Die Barrieren sind ein **Anforderungskatalog**. Sie sagen einem Beweisversuch, welche Eigenschaften er *nicht* haben darf. Sie sind damit das schärfste verfügbare Prüfinstrument für eingereichte Beweise — und exakt das ist ihre Rolle als Muss-Kriterium M1 in `docs/02-team-briefing.md` §4: Ein Claim, der nicht erklärt, wie er alle drei umgeht, ist mit sehr hoher Wahrscheinlichkeit fehlerhaft, ohne dass man den Beweis lesen muss. `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]`

### 3.5 Resultate, die Barrieren nachweislich umgehen

**(a) IP = PSPACE (Shamir 1992).** Nicht-relativierend. Der historische Beleg dafür, dass Barriere 1 überwindbar ist. `[KANON]`

**(b) Ryan Williams: NEXP ⊄ ACC⁰.** `[VERIFIZIERT]`
- Referenz: R. Williams, *Nonuniform ACC Circuit Lower Bounds*, Journal of the ACM 61(1), Artikel 2, 2014 (Konferenzfassung CCC 2011). Über zwei unabhängige Suchen bestätigt, einschließlich Band-/Artikelnummer.
- **Gödel-Preis 2024** für diese Arbeit — die Auszeichnung würdigte laut Suchsynthese ausdrücklich das »reichhaltige wechselseitige Verhältnis zwischen algorithmischen Techniken und Methoden für untere Schranken«. `[VERIFIZIERT]`
- **Aussage:** NEXP (nichtdeterministische Exponentialzeit) hat keine nicht-uniformen ACC⁰-Schaltkreise polynomieller Größe. ACC⁰ = konstante Tiefe, unbeschränkter Fan-In, mit AND/OR/NOT und MOD_m-Gattern für beliebige feste m. Die Schranke lässt sich laut Suchsynthese auf quasipolynomielle Größe verstärken; zusätzlich: E^NP hat keine ACC⁰-Schaltkreise der Größe 2^{n^{o(1)}}. `[NUR-SNIPPET]`
- **Die Methode (»algorithmische Methode«):** Williams zeigt, dass ein **nichttrivial schnellerer SAT-Algorithmus** für ACC⁰-Schaltkreise — Laufzeit O(2ⁿ/n^k) statt 2ⁿ — bereits eine untere Schranke für NEXP impliziert. Dann konstruiert er einen solchen Algorithmus (über die Reduktion von ACC⁰ auf SYM⁺-Schaltkreise, schnelle rechteckige Matrixmultiplikation und dynamische Programmierung). Der Beweis läuft also über den **Umweg über bessere Algorithmen** — obere Schranken erzeugen untere Schranken. `[NUR-SNIPPET]`, über zwei Suchen konsistent.
- **Warum es die Barrieren umgeht:** Der Beweis nutzt **strukturelle, nicht-relativierende Eigenschaften konkreter ACC⁰-Schaltkreise** (die Normalform-Reduktion auf SYM⁺). Jede Black-Box-Behandlung wäre damit unvereinbar; insbesondere sind alle bekannten SAT-Algorithmen, die Brute Force schlagen, nicht-relativierend. Damit entfallen Relativization und Algebrization. Die Natural-Proofs-Barriere entfällt, weil das Argument **nicht largeness-basiert** ist: Es zeigt nicht »die typische Funktion ist hart«, sondern zielt über die Easy-Witness-Methode auf eine ganz spezifische Sprache in NEXP. `[NUR-SNIPPET]`, Konfidenz mittel-hoch; die Aussage »umgeht alle drei Barrieren« ist in der Community Standarddarstellung.
- **Ehrliche Einordnung der Reichweite:** ACC⁰ ist eine **sehr schwache** Schaltkreisklasse — konstante Tiefe. NEXP ist eine **sehr große** Komplexitätsklasse, gewaltig größer als NP. Für P ≠ NP bräuchte man eine untere Schranke für ein **NP**-Problem gegen **allgemeine** Schaltkreise **polynomieller** Größe. Der Abstand ist enorm. Williams' Resultat ist der Beweis, dass die Barrieren überwindbar sind — nicht, dass man nahe dran ist. `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]`

**(c) Murray–Williams: NQP ⊄ ACC⁰.** `[VERIFIZIERT]`
- *Circuit Lower Bounds for Nondeterministic Quasi-Polytime from a New Easy Witness Lemma*, STOC 2018; SIAM Journal on Computing. Verschärft (b) von NEXP auf **NQP** = NTIME[n^{polylog n}], also von exponentieller auf quasipolynomielle nichtdeterministische Zeit — ein substanzieller Schritt näher an NP heran, über eine Erweiterung des Easy-Witness-Lemmas von Impagliazzo–Kabanets–Wigderson. Eine Folgearbeit zeigt zudem Average-Case-Härte von NQP gegen ACC⁰ (SIAM J. Comput.). `[NUR-SNIPPET]`

**(d) Weitere.** In der Literatur werden auch Schranken wie »PP hat keine linear großen Schaltkreise« als Beispiel dafür genannt, dass Relativization und Natural Proofs gemeinsam überwunden werden können. `[NUR-SNIPPET]`, Konfidenz mittel.

---

## 4. Der Konsens der Fachwelt — die Gasarch-Umfragen

### 4.1 Übersicht der drei Umfragen

William (Bill) Gasarch (University of Maryland) hat drei Umfragen unter Theoretikerinnen und Theoretikern durchgeführt und in der SIGACT News Complexity Theory Column publiziert:

| Umfrage | Publikation | Teilnehmer |
|---|---|---|
| 1. Umfrage | SIGACT News Complexity Theory Column 36, 2002 | 100 `[VERIFIZIERT]` |
| 2. Umfrage | *Guest Column: The Second P =? NP Poll*, SIGACT News 43(2), 2012 (Column 74), Umfrage 2011 durchgeführt | 152 `[VERIFIZIERT]` |
| 3. Umfrage | *Guest Column: The Third P=?NP Poll*, SIGACT News 50(1), März 2019 (Column 100), DOI 10.1145/3319627.3319636, Umfrage 2018 durchgeführt | 124 `[VERIFIZIERT]` |

Alle drei Angaben zu Venue, Jahr und Teilnehmerzahl sind über mindestens zwei unterschiedlich formulierte Suchen bestätigt.

### 4.2 Der dokumentierte Zahlenwiderspruch — und seine Auflösung

**Das Problem.** Im Umlauf sind für die Umfrage 2019 die Zahlen **66 %**, **~80 %** und **88 %** als »Anteil für P ≠ NP«. Die Projektleitung hat diesen Widerspruch in `docs/00-briefing.md` und `docs/02-team-briefing.md` als Testfall für die Triangulationspflicht markiert.

**Der Befund nach fünf gezielten Suchen.** `[NUR-SNIPPET]`, Konfidenz **hoch**:

> Die **66 % beziehen sich auf eine andere Frage**. Gasarchs Umfragen fragen nicht nur »P = NP oder P ≠ NP?«, sondern auch »**Wann wird das Problem gelöst?**«. Die 66 % sind der Anteil derjenigen, die eine Lösung **vor dem Jahr 2100** erwarten.

Belegkette (zwei unabhängig formulierte Suchen, konsistentes Ergebnis):
- Für die Frage »vor 2100 gelöst?«: **2002: 62 %**, **2012: 53 %**, **2019: 66 %**.
- Für »wird nie gelöst«: **2002: 5 %**, **2012: 3 %**, **2019: 9 %**.
- Dazu ein Gasarch zugeschriebener Kommentar sinngemäß: Der Anstieg von 53 % auf 66 % »amazes me because, since 2012, there has been little (no?) progress on resolving P =? NP«. `[NUR-SNIPPET]`

**Damit ist der Widerspruch in der Sache aufgelöst:** 66 % und 80 % sind Antworten auf **verschiedene Fragen** und stehen nicht in Konkurrenz. Der Widerspruch war ein Artefakt der Suchsynthese, die zwei Kennzahlen derselben Umfrage nebeneinanderstellte.

**Die Zahlen zur eigentlichen Frage P = NP vs. P ≠ NP:**

| Jahr | Anteil P ≠ NP | Bezugsgröße | Beleglage |
|---|---|---|---|
| 2002 | **61 von 100** (61 %) | alle Befragten; davon 7 mit ausdrücklichen Zweifeln; die restlichen 39 verteilen sich auf P = NP (ca. 9), »keine Meinung« und »Frage nicht wohlgestellt« | `[NUR-SNIPPET]`, Konfidenz mittel-hoch für 61; die Aufschlüsselung der restlichen 39 (9/22/8) **konnte ich nicht verifizieren** |
| 2012 | **ca. 81–83 %**; in einer Quellensynthese »125 von 152« (82 %), in einer anderen »81 Prozent von mehr als 150 Befragten« | alle Befragten | `[NUR-SNIPPET]`, Konfidenz mittel. Die genaue Zahl der P=NP-Stimmen **konnte ich nicht verifizieren** |
| 2019 | **ca. 80 %** (über alle 124 Befragten) bzw. **88 %** (in einer Synthese, offenbar bezogen auf die *Meinungsäußernden*, mit 12 % für P=NP als Komplement) | siehe links | `[NUR-SNIPPET]`, Konfidenz mittel |
| 2019, Teilmenge »Experten« | **99 %** unter denjenigen, die angaben, **viel über das Problem nachgedacht** zu haben | Experten-Teilmenge | `[NUR-SNIPPET]`, in zwei Suchen konsistent, Konfidenz mittel-hoch |

**Verbleibende Unschärfe, die im Papier stehen bleiben muss.** Die Differenz **80 % vs. 88 %** für 2019 konnte ich **nicht abschließend auflösen**. Die plausibelste Erklärung — 80 % bezogen auf alle 124 Befragten, 88 % bezogen auf die Teilmenge derer, die sich überhaupt festlegten (Rest: »weiß nicht« / »nicht wohlgestellt«) — ist **arithmetisch stimmig, aber von mir nicht belegt**. Gemäß `docs/02-team-briefing.md` §1 werden **beide Zahlen berichtet und der Widerspruch gekennzeichnet**, nicht geglättet.

**Zusatzbefund 2012.** In der Umfrage 2012 gab es eine gesondert ausgewiesene Teilgruppe von **21 Preisträgern** (Gödel-Preis, Turing-Award o. ä.): davon **17 (81 %) für P ≠ NP** (2 davon nur schwach), **2 (9 %) für P = NP**, **2 (9 %) »weiß nicht«**. `[NUR-SNIPPET]`, Konfidenz mittel. Der Wert ist als Kontrollgruppe interessant: Der Expertenanteil unterscheidet sich in dieser Gruppe **nicht** nennenswert vom Gesamtwert.

### 4.3 Gibt es eine Umfrage 2024–2026?

**Negativbefund.** Zwei gezielte Suchen nach einer vierten Gasarch-Umfrage bzw. einer vergleichbaren Erhebung 2024–2026 ergaben **keinen Treffer**. Die jüngste dokumentierte Umfrage dieser Art ist die dritte (2019). `[NUR-SNIPPET]`, Konfidenz mittel-hoch (Abwesenheitsbeleg per Suche ist schwächer als ein Positivbefund).

**Konsequenz für das Papier.** Es gibt **keine belastbare quantitative Erhebung des Meinungsstandes nach 2019** — insbesondere keine, die den Einfluss der KI-Entwicklung seit 2023 auf die Erwartungen des Feldes messen würde. Das ist eine echte Forschungslücke und sollte als solche benannt werden. Wer für 2026 Prozentzahlen zum Fachkonsens zitiert, zitiert in Wahrheit Zahlen von 2018.

### 4.4 Die maßgeblichen Übersichtsartikel

| Arbeit | Autor | Jahr/Venue | Status |
|---|---|---|---|
| *The Status of the P versus NP Problem* | Lance Fortnow | Communications of the ACM 52(9), 78–86, September 2009 | `[VERIFIZIERT]` — der meistgelesene Übersichtsartikel; wird bis heute in Kursen als Standardlektüre eingesetzt (Treffer bei CMU- und Duke-Kursseiten) |
| *A Status Report on the P versus NP Question* | Eric Allender | Advances in Computers, Bd. 77, Kapitel 4, S. 117–147 | `[VERIFIZIERT]` für Venue/Kapitel/Seiten. **Jahresangabe widersprüchlich:** eine Quelle nennt 2008, eine andere 2009. Beide berichtet, nicht geglättet. Inhalt `[NUR-SNIPPET]`, nicht am Volltext geprüft |
| *P =? NP* | Scott Aaronson | in: J. F. Nash Jr., M. Th. Rassias (Hg.), *Open Problems in Mathematics*, Springer 2016, Kapitel 1, DOI 10.1007/978-3-319-32162-2_1 | `[VERIFIZIERT]` für Venue/Jahr/Herausgeber. **Anmerkung:** Der Auftrag nannte »2017«; die Buchpublikation ist **2016** datiert (Preprint-/Manuskriptfassungen kursieren mit anderen Jahreszahlen). Der Text ist die ausführlichste allgemeinverständliche Bestandsaufnahme, inkl. Barrieren, GCT und Unabhängigkeitsfrage |
| *Fifty Years of P vs. NP and the Possibility of the Impossible* | Lance Fortnow | Communications of the ACM 65(1), 76–85, Januar 2022, DOI 10.1145/3460351 | `[VERIFIZIERT]` für Venue/Band/Seiten/DOI über zwei unabhängige Suchen. Enthält die Optiland-These (§7.3) |

---

## 5. Fortnow 2026 — Wortlautprüfung

**Auftrag:** Verifikation des Wortlauts von Lance Fortnows Aussage zu P vs. NP in Lean (Blogpost »Respect the P v NP Problem«, 10.06.2026).

**Ergebnis: bestätigt, über zwei unabhängig formulierte Suchen.** `[NUR-SNIPPET]`, Konfidenz **hoch** für den Sinngehalt, **mittel-hoch** für den exakten Wortlaut (kein Volltextzugang).

- **Fundstelle:** Computational Complexity Blog, Post »Respect the P v NP Problem«, URL `https://blog.computationalcomplexity.org/2026/06/respect-p-v-np-problem.html`, Datum 10. Juni 2026. `[VERIFIZIERT]` für Existenz, URL und Datum.
- **Zentrale Aussage, in beiden Suchen übereinstimmend wiedergegeben:**
  > »At this time we don't even have a viable approach to settling the P v NP problem.«
  Der im Team-Briefing als unverifiziert geführte Lead (»we don't even have a viable approach«) ist damit **bestätigt**.
- **Der Lean-Bezug, ebenfalls in beiden Suchen:** Fortnow rät ausdrücklich davon ab, das Problem über eine Formalisierung in Lean angehen zu wollen — sinngemäß »don't waste your time trying a formal approach via Lean«. Begründung laut Synthese: »Computational complexity is very messy to formulate technically.«
- **Zusätzliches Zitat, in einer Suche wörtlich wiedergegeben:**
  > »I can't get an AI willing to give me a full Lean-verified proof of something trivial like P closed under complement, forget the PCP theorem.«
- **Fortnows Schlussfolgerung, sinngemäß:** Wenn jemand oder etwas P ≠ NP beweist, dann über den **richtigen intuitiven Ansatz**, nicht über einen formalistischen.

**Einordnung.** `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]` Die Aussage ist stärker als »das Problem ist schwer«. Sie ist die Feststellung, dass es **keinen Kandidatenplan** gibt — kein Programm, von dem die Fachwelt glaubt, dass es bei genügend Arbeit zum Ziel führt. Das ist der relevante Unterschied zu Problemen wie der Fermat-Vermutung vor Wiles (Taniyama–Shimura war ein bekannter Weg) oder zur Geometric Complexity Theory, wo es zwar einen Plan gibt, dessen Urheber Mulmuley aber selbst Größenordnungen von ~100 Jahren veranschlagt. Für die Leitfrage des Projekts — leistet KI hier substanziellen Fortschritt? — ist das der entscheidende Kontextpunkt: **Ein Werkzeug kann einen Plan beschleunigen; es kann keinen Plan ersetzen, den es nicht gibt.**

Dieser Post steht zudem im Zusammenhang mit Fortnows Post »Navier-Stokes and Lean« (September 2026, `https://blog.computationalcomplexity.org/2026/09/navier-stokes-and-lean.html`) — dort geht es um die Unterscheidung, dass Lean die *Ableitung* verifiziert, nicht die *Angemessenheit der Voraussetzungen* (Muss-Kriterium M4). Die inhaltliche Ausarbeitung liegt bei A7.

---

## 6. Die Landkarte: was bewiesen ist und was offen

### 6.1 Die gesicherte Inklusionskette `[KANON]`

L ⊆ NL ⊆ P ⊆ NP ⊆ PH ⊆ PSPACE ⊆ EXP ⊆ NEXP ⊆ EXPSPACE

Dabei: L/NL = (nicht-)deterministischer logarithmischer Platz; PH = Polynomialzeit-Hierarchie; PSPACE = polynomieller Platz; EXP = deterministische Exponentialzeit 2^{poly(n)}.

Begründungen der wichtigsten Schritte:
- **P ⊆ NP**: trivial (§1.3).
- **NP ⊆ PSPACE**: Man kann alle Zertifikate polynomieller Länge nacheinander durchprobieren und den Platz jedes Mal wiederverwenden. Die *Zeit* ist exponentiell, der *Platz* bleibt polynomiell.
- **PSPACE ⊆ EXP**: Eine Maschine mit polynomiellem Platz hat nur 2^{poly(n)} verschiedene Konfigurationen; sie muss entweder vorher halten oder zykeln.

**Sämtliche Inklusionen in dieser Kette sind offen** — man weiß von keiner einzigen, ob sie echt ist. `[KANON]`

### 6.2 Was bewiesen ist: die Hierarchiesätze

**Zeithierarchie-Satz (Hartmanis–Stearns 1965).** `[VERIFIZIERT]`
> Für »zeitkonstruierbare« Funktionen f, g mit f(n)·log f(n) = o(g(n)) gilt DTIME(f(n)) ⊊ DTIME(g(n)). Mehr Zeit erlaubt strikt mehr.

Beweis per **Diagonalisierung**: Man baut eine Maschine, die jede in Zeit f laufende Maschine simuliert und gegenteilig antwortet — was in Zeit g möglich ist, in Zeit f aber nicht.

**Die wichtigste Folgerung: P ⊊ EXP.** `[KANON]` Also: **P ≠ EXP** ist bewiesen. Analog liefert der Raumhierarchie-Satz L ⊊ PSPACE und NL ⊊ PSPACE, und der nichtdeterministische Zeithierarchie-Satz NP ⊊ NEXP.

**Eine nützliche Konsequenz für das Papier:** Aus P ≠ EXP und der Kette P ⊆ NP ⊆ PSPACE ⊆ EXP folgt sofort: **Mindestens eine** dieser drei Inklusionen ist echt. Wir wissen mit Sicherheit, dass irgendwo in dieser Kette eine Trennung liegt — wir wissen nur nicht, wo. `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]`

### 6.3 Warum die bekannten Separationen nicht auf P vs. NP übertragbar sind

Das ist der Kern der Sache, und er lässt sich in zwei Sätzen sagen. `[VERIFIZIERT]` (über zwei Suchen konsistent bestätigt)

1. **Alle Hierarchiesätze relativieren.** Der Zeithierarchie-Satz gilt in **jeder** relativierten Welt: Man kann die Diagonalisierung wortwörtlich mit Orakel durchführen, weil die simulierende Maschine dem simulierten Programm einfach dasselbe Orakel weiterreicht.
2. Nach Baker–Gill–Solovay (§3.1) **darf** ein Beweis von P ≠ NP aber nicht relativieren. Also **kann** die Hierarchiesatz-Technik P ≠ NP nicht liefern — nicht, weil man sie noch nicht geschickt genug angewandt hätte, sondern aus prinzipiellen Gründen.

Der technische Grund dahinter: Hierarchiesätze vergleichen **gleichartige** Ressourcen (deterministische Zeit gegen deterministische Zeit, Platz gegen Platz). P vs. NP vergleicht **verschiedenartige** Ressourcen (deterministische gegen nichtdeterministische Zeit). Die Standardformulierung lautet: »Die Hierarchiesätze bieten keine Möglichkeit, deterministische und nichtdeterministische Komplexität oder Zeit und Platz zueinander in Beziehung zu setzen.« `[NUR-SNIPPET]`

**Das ist die didaktisch wichtigste Aussage des ganzen Kapitels:** Wir haben ein Werkzeug, das nachweislich Komplexitätsklassen trennt — und es ist nachweislich ungeeignet für diese eine Trennung.

### 6.4 Der Stand bei Schaltkreis-unteren-Schranken — die härteste Zahl

Der aussichtsreichste Weg zu P ≠ NP führt über Schaltkreis-untere-Schranken: Eine superpolynomielle untere Schranke für die Schaltkreisgröße einer NP-Funktion würde genügen.

**Der tatsächliche Stand:** `[VERIFIZIERT]` (über zwei unabhängige Suchen bestätigt)

| Jahr | Schranke | Autoren |
|---|---|---|
| 1984 | 3n − o(n) | Blum |
| 2016 | (3 + 1/86)·n − o(n) | Find, Golovnev, Hirsch, Kulikov (FOCS 2016) |
| 2021/2022 | **3,1n − o(n)** | Li, Yang (STOC 2022; ECCC TR21-023) |

Die Schranken gelten für explizite Funktionen (affine Disperser), die sogar in P liegen. Die Technik heißt **Gate Elimination**: Man belegt Eingänge geschickt mit Konstanten und zeigt, dass dabei jedes Mal mehrere Gatter verschwinden.

**Die Einordnung, die im Papier stehen muss.** `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]` Nach vier Jahrzehnten intensiver Arbeit ist die beste bekannte untere Schranke für allgemeine Boolesche Schaltkreise **linear** — und der Fortschritt von 3n auf 3,1n in 38 Jahren galt als hochgradig nichttrivial. Gebraucht wird eine **superpolynomielle** Schranke, also n^{ω(1)}. Zwischen »3,1·n« und »n^{log n}« liegt kein quantitativer Abstand, den man durch Fleiß überbrückt; es liegt ein qualitativer Methodensprung dazwischen. Das ist die nüchternste verfügbare Antwort auf die Frage »Wie nah sind wir?«. Zugleich sind Schranken dieser Größenordnung ein Zählerbeleg dafür, dass ein »allmählicher Fortschritt bis zur Lösung« kein realistisches Bild ist.

**Aktueller institutioneller Befund:** Vom **7.–11. September 2026** — also in der Woche unmittelbar vor Erstellung dieses Berichts — fand am Isaac Newton Institute in Cambridge ein Workshop **»Frontiers in Complexity Lower Bounds«** statt, der ausdrücklich den Stand der Technik bei unteren Schranken, neue Ansätze für stärkere Modelle **und das Verständnis der verschiedenen Arten von Barrieren** zum Thema hatte. `[VERIFIZIERT]` für Existenz und Zeitraum (cstheory-events.org), Inhalt `[NUR-SNIPPET]`. Das ist der aktuellste greifbare Beleg dafür, dass Barrieren im September 2026 ein aktives Forschungsthema sind — und nicht ein abgeschlossenes Kapitel.

### 6.5 NP vs. coNP

**Status: offen.** `[VERIFIZIERT]` (über zwei Suchen bestätigt)

- **coNP** = Probleme mit kurzen Beweisen für die **Nein**-Antwort. Beispiel: TAUTOLOGIE (»ist diese Formel für jede Belegung wahr?«) ist coNP-vollständig.
- **P = NP ⟹ NP = coNP** (P ist unter Komplement abgeschlossen). Die Umkehrung ist **nicht** bekannt: Es könnte P ≠ NP und trotzdem NP = coNP gelten. `[VERIFIZIERT]`
- Daher ist **NP ≠ coNP eine stärkere Aussage als P ≠ NP**. Wer NP ≠ coNP beweist, hat P ≠ NP mitbewiesen.
- **Der Zugang über Beweiskomplexität (Cook–Reckhow 1979):** NP = coNP genau dann, wenn es ein **polynomiell beschränktes Beweissystem für die Aussagenlogik** gibt. Das »Cook–Reckhow-Programm« will NP ≠ coNP erreichen, indem es für immer stärkere Beweissysteme superpolynomielle untere Schranken zeigt. Erfolge gibt es für schwache Systeme (Resolution: exponentielle Schranken, Haken 1985; bounded-depth Frege). Für **Extended Frege** — ein System, das im Wesentlichen der Stärke normaler mathematischer Argumentation entspricht — ist **keine** nichttriviale untere Schranke bekannt. `[KANON]` / `[NUR-SNIPPET]` für die aktuelle Literaturlage (u. a. arXiv:2312.08163, *Towards P≠NP from Extended Frege lower bounds*, `[PREPRINT]`).
- Die verbreitete Vermutung lautet NP ≠ coNP. Es ist bemerkenswert, dass NP ∩ coNP nicht leer wirkt: Faktorisierung und Graphenisomorphie liegen (bzw. lagen) dort, also in NP ∩ coNP, ohne bekannt in P zu sein — was die Existenz einer interessanten Zwischenschicht nahelegt (vgl. Ladners Satz: Falls P ≠ NP, existieren **NP-intermediate** Probleme, die weder in P noch NP-vollständig sind). `[KANON]`

### 6.6 P vs. PSPACE

**Status: offen.** `[VERIFIZIERT]`

- P ⊆ NP ⊆ PH ⊆ PSPACE; keine dieser Inklusionen ist als echt bekannt.
- Es gilt aber: **P ≠ EXP** und PSPACE ⊆ EXP. Zudem folgt aus dem Raumhierarchie-Satz L ⊊ PSPACE.
- P = PSPACE würde P = NP implizieren (wegen NP ⊆ PSPACE). Die Frage ist also mindestens so schwer wie P vs. NP.
- Es existieren Orakel, die P und PSPACE kollabieren lassen, und Orakel, die L und NP kollabieren lassen — also relativiert auch hier nichts. `[NUR-SNIPPET]`
- **Crank-Warnung:** Es kursiert ein arXiv-Preprint »The Separation of NP and PSPACE« (arXiv:2106.11886, Tianrong Lin, mehrfach revidiert, zuletzt laut Suchtreffer April 2025), der NP ≠ PSPACE per Diagonalisierung zu beweisen behauptet. **`[CLAIM]`, nicht anerkannt.** Schon die Methodenbeschreibung (Diagonalisierung) kollidiert mit §6.3/§3.1. A8 sollte das in die Crank-Auditierung aufnehmen. Analog kursieren »A Homological Separation of P from NP« (arXiv:2510.17829) und weitere. `[CLAIM]`

### 6.7 Wichtige Strukturaussagen unter der Annahme P ≠ NP `[KANON]`

- **Ladners Satz (1975):** Wenn P ≠ NP, gibt es NP-intermediate Probleme.
- **P = NP ⟹ PH kollabiert auf P.** Die gesamte Polynomialzeit-Hierarchie fällt zusammen. Umgekehrt ist »PH kollabiert nicht« eine gängige Verschärfungsannahme.
- **Exponential Time Hypothesis (ETH, Impagliazzo–Paturi):** 3-SAT erfordert Zeit 2^{Ω(n)}; **SETH** verschärft das auf »keine Konstante besser als 2ⁿ«. Diese Annahmen sind **stärker** als P ≠ NP und die Grundlage der Fine-Grained Complexity. Sie sind auch der Grund, warum der Fortschritt bei 3-SAT-Algorithmen (O\*(1,307ⁿ) nach Scheder 2024 laut Vorrecherche der Leitung) als Verbesserung der *Konstanten im Exponenten* einzuordnen ist und nicht als Annäherung an Polynomialzeit.

### 6.8 Angrenzende Bewegung 2025/2026

Zur Einordnung, dass das Feld nicht stillsteht — nur eben nicht bei P vs. NP:
- **Ryan Williams (Februar 2025):** Jede Mehrband-Turingmaschine mit Zeit t ist in Platz O(√(t log t)) simulierbar — die erste substanzielle Verbesserung der Zeit-Platz-Simulation seit rund 50 Jahren. `[NUR-SNIPPET]`, aus der Vorrecherche der Leitung übernommen, von mir nicht eigenständig nachgeprüft. Kein P-vs-NP-Resultat, aber ein Beleg, dass in Nachbarfragen echte Bewegung ist.
- **Meta-Komplexität / MCSP:** »SAT Reduces to the Minimum Circuit Size Problem« (ECCC 2023/165) als bislang stärkster Hinweis auf NP-Vollständigkeit von MCSP; Stichwort »hardness magnification«. `[PREPRINT]`, aus der Vorrecherche übernommen.
- **Geometric Complexity Theory (Mulmuley–Sohoni):** Der einzige ausgearbeitete langfristige Angriffsplan auf P vs. NP, über algebraische Geometrie und Darstellungstheorie. Mulmuley selbst veranschlagt Größenordnungen von ~100 Jahren; zentrale Positivitätshypothesen gelten als »formidable«. `[NUR-SNIPPET]`, aus der Vorrecherche übernommen. Wichtig für M1: GCT ist explizit als nicht-relativierender, nicht-naturaler Ansatz konzipiert.

---

## 7. Konsequenzen

### 7.1 Was P = NP praktisch bedeuten würde

**Der Kern.** Wenn P = NP, dann ist **Finden so leicht wie Prüfen**. Überall dort, wo man eine gute Lösung erkennen kann, sobald man sie sieht, könnte man sie auch effizient konstruieren.

Scott Aaronsons vielzitierte Formulierung, über Suche im Wortlaut bestätigt `[VERIFIZIERT]` (Wortlaut `[NUR-SNIPPET]`):

> »If P = NP, then the world would be a profoundly different place than we usually assume it to be. There would be no special value in 'creative leaps,' no fundamental gap between solving a problem and recognizing the solution once it's found. Everyone who could appreciate a symphony would be Mozart; everyone who could follow a step-by-step argument would be Gauss; everyone who could recognize a good investment strategy would be Warren Buffett.«

**Die konkreten Bereiche:**

**(a) Kryptographie.** Die gesamte Public-Key-Kryptographie (RSA, Diffie–Hellman, elliptische Kurven) beruht darauf, dass bestimmte Funktionen leicht zu berechnen und schwer zu invertieren sind. Bei P = NP mit praktikablem Exponenten wäre das Invertieren von Trapdoor-Funktionen effizient. `[NUR-SNIPPET]` nennt eine Illustration: Ein 3-SAT-Algorithmus mit Laufzeit n² könnte 200-stellige Zahlen in Minuten faktorisieren. Präzise gilt: Faktorisierung liegt in NP ∩ coNP, ist also keinesfalls als NP-vollständig bekannt — aber P = NP würde NP ∩ coNP ⊆ P nach sich ziehen und damit auch Faktorisierung erledigen. Betroffen wären außerdem: digitale Signaturen, TLS, Blockchains (Proof-of-Work wird trivial), Passwort-Hashes. Nicht betroffen: informationstheoretisch sichere Verfahren (One-Time-Pad, Quantenschlüsselaustausch), die nicht auf Komplexitätsannahmen beruhen. `[KANON]`
**(b) Optimierung und Logistik.** Routenplanung, Scheduling, Chipdesign (Platzierung und Verdrahtung), Proteinfaltung, Portfolio-Optimierung — sämtlich NP-harte Probleme, sämtlich exakt und optimal lösbar. Der wirtschaftliche Effekt wäre gewaltig.
**(c) Automatisches Beweisen.** Das ist der philosophisch tiefste Punkt und zugleich der historische Ursprung: Cooks Arbeit von 1971 hieß *The Complexity of Theorem-Proving Procedures*. Die Sprache { (φ, 1^k) : φ hat einen Beweis der Länge ≤ k } ist in NP — man rät den Beweis und prüft ihn. Bei P = NP könnte man Beweise beschränkter Länge **automatisch finden**. Jede Vermutung mit einem Beweis vernünftiger Länge wäre maschinell entscheidbar. Mathematik in ihrer heutigen Form wäre eine andere Tätigkeit. `[NUR-SNIPPET]` bestätigt diese Standarddarstellung.
**(d) Maschinelles Lernen.** Viele Lernprobleme (Finden der kleinsten konsistenten Hypothese, exakte MAP-Inferenz, optimales Entscheidungsbaum-Lernen) sind NP-hart. Bei P = NP entfiele die Notwendigkeit heuristischer Approximation.

**Die Caveats, die dazugehören** `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]`, teilweise `[NUR-SNIPPET]`:
1. **Der Exponent.** Ein Algorithmus mit Laufzeit n¹⁰⁰ oder mit der Konstanten 2^{1000} ist formal polynomiell und praktisch völlig nutzlos (»galactic algorithm«). P = NP würde die *Theorie* umstürzen und die *Praxis* möglicherweise gar nicht berühren.
2. **Nicht-Konstruktivität.** Ein Beweis von P = NP müsste keinen konkreten Algorithmus liefern. Es gibt sogar ein kurioses Ergebnis (Levins universelle Suche), wonach ein expliziter, aber völlig unpraktikabler optimaler Algorithmus konstruierbar ist, *falls* P = NP gilt.
3. Die dramatischen Konsequenzen unter (a)–(d) setzen alle voraus, dass der Exponent klein ist. Diese Voraussetzung wird in populären Darstellungen fast nie genannt.

### 7.2 Was P ≠ NP bedeutet — und was es **nicht** bedeutet

**Was es bedeutet:** Es gibt Probleme, deren Lösungen man effizient prüfen, aber nicht effizient finden kann. Kreativität, Suche, Einsicht wären **prinzipiell** nicht auf Verifikation reduzierbar. Es wäre die mathematische Grundlage dafür, dass Kryptographie überhaupt möglich ist.

**Was es NICHT bedeutet** — das ist der praktisch wichtigere Teil: `[KANON]`

1. **Es bedeutet nicht, dass NP-harte Probleme in der Praxis unlösbar sind.** P ≠ NP ist eine **Worst-Case**-Aussage. Moderne CDCL-SAT-Solver lösen Industrieinstanzen mit Millionen von Variablen; MILP-Solver (Gurobi, CPLEX) lösen TSP-Instanzen mit zehntausenden Städten exakt und optimal. Die harten Instanzen existieren, aber sie treten in der Praxis selten auf.
2. **Es bedeutet nicht, dass Approximation unmöglich ist.** Viele NP-harte Optimierungsprobleme haben gute Approximationsalgorithmen (Vertex Cover: Faktor 2; Knapsack: FPTAS; Euklidisches TSP: PTAS). Die Grenze, **wie gut** man approximieren kann, ist ein eigenes, reiches Forschungsgebiet — und genau das Gebiet, in dem der stärkste KI-Befund des Projekts (AlphaEvolve, arXiv:2509.18057) liegt. Wichtig: Inapproximierbarkeitsresultate **setzen P ≠ NP voraus** und beweisen es nicht.
3. **Es bedeutet nicht, dass Kryptographie automatisch sicher ist.** P ≠ NP ist eine **notwendige**, aber **nicht hinreichende** Bedingung. Kryptographie braucht **Average-Case**-Härte und Einwegfunktionen — beides folgt nicht aus P ≠ NP.

**Impagliazzos fünf Welten** sind die saubere Systematisierung dieses Punktes. `[VERIFIZIERT]` (R. Impagliazzo, *A Personal View of Average-Case Complexity*, 1995; Bestätigung der Welten-Namen und -Definitionen über eine gezielte Suche; eine Quanta-Magazine-Darstellung von 2022 bestätigt die Rezeption):

| Welt | Bedingungen | Bedeutung |
|---|---|---|
| **Algorithmica** | P = NP (oder NP effizient im Praxis-Sinne) | Finden = Prüfen; keine Kryptographie |
| **Heuristica** | P ≠ NP, aber kein NP-Problem ist **average-case** hart | Worst-Case-Härte existiert, ist aber praktisch irrelevant |
| **Pessiland** | NP-Probleme sind average-case hart, aber es gibt **keine Einwegfunktionen** | die schlechteste aller Welten: harte Instanzen, aber unbrauchbar für Kryptographie |
| **Minicrypt** | Einwegfunktionen existieren, aber **keine** Public-Key-Kryptographie | symmetrische Verfahren sicher, Schlüsselaustausch unmöglich |
| **Cryptomania** | Public-Key-Kryptographie existiert (Trapdoor-Einwegfunktionen) | die Welt, in der wir zu leben glauben |

Die Fachwelt vermutet überwiegend, in **Cryptomania** zu leben. Der Punkt für unser Papier: »P ≠ NP« allein unterscheidet zwischen Algorithmica und dem Rest — die vier interessanten Fälle bleiben offen. Es gibt zudem 2026 eine Erweiterung dieser Systematik in der Preprint-Literatur (arXiv:2606.27139, *The Observer World: A Cryptographic Extension of Impagliazzo's Five Worlds*, `[PREPRINT]`, Inhalt ungeprüft).

### 7.3 Fortnows »Optiland«-These

**Quelle.** Lance Fortnow, *Fifty Years of P vs. NP and the Possibility of the Impossible*, CACM 65(1), 76–85, Januar 2022. `[VERIFIZIERT]` für Venue; der Begriff »Optiland« und seine Definition über zwei Suchen bestätigt, Wortlaut `[NUR-SNIPPET]`.

**Die These.** Fortnow beschreibt »Optiland« als eine Welt,

> »where we can almost miraculously gain many of the advantages of P = NP while avoiding some of the disadvantages, such as breaking cryptography.«

Die Begründung, laut Suchsynthese in zwei Varianten konsistent: Fortschritte in maschinellem Lernen und Optimierung — in Software **und** Hardware — erlauben es, Probleme zu lösen, die lange als schwer oder unmöglich galten (Spracherkennung, Proteinfaltung), **während die kryptographischen Protokolle weitgehend unangetastet bleiben**. Wir lösen »die NP-Probleme, die in der Praxis vorkommen«, und die Kryptographie bleibt unversehrt.

**Warum das keine Aussage über P vs. NP ist.** `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]` Optiland ist eine **empirische** These über die Verteilung real auftretender Instanzen und die Leistungsfähigkeit von Heuristiken. Sie berührt die **Worst-Case-Frage** P vs. NP nicht im Geringsten. In der Impagliazzo-Systematik ist Optiland am ehesten eine pragmatische Variante von Heuristica-mit-Cryptomania-Rest: Instanzen aus der Praxis verhalten sich wie in Heuristica, die gezielt konstruierten Instanzen der Kryptographie wie in Cryptomania. Dass dies gleichzeitig möglich ist, liegt daran, dass Kryptographie ihre Instanzen **absichtlich hart wählt** — was in der Optimierung niemand tut.

**Der Wert der These für unser Papier** ist hoch, weil sie eine naheliegende Fehlinterpretation unseres Hauptbefundes präventiv korrigiert: Wenn KI-Systeme spektakuläre Erfolge bei kombinatorischen Problemen erzielen, ist die Versuchung groß, daraus eine Bewegung in Richtung P = NP zu lesen. Fortnows These beschreibt genau, warum das nicht folgt: **Die praktischen Vorteile eines P=NP-Szenarios und die theoretische Aussage P = NP sind entkoppelt.** Wir können immer mehr von Ersterem bekommen, ohne dass sich an Letzterem irgendetwas ändert.

Es lohnt, dies neben Fortnows Aussage vom Juni 2026 (§5) zu lesen: Derselbe Autor, der die praktische Optimierungswelt optimistisch beschreibt, hält die theoretische Frage für so weit von einer Lösung entfernt, dass es »nicht einmal einen tragfähigen Ansatz« gibt. Das ist kein Widerspruch — es ist der Kern der Sache.

---

## 8. Antwort auf die Leitachse (`docs/02-team-briefing.md` §2)

**Die Leitachse lautet:** suchbare endliche Zeugen vs. unendliche Quantifizierung über alle Algorithmen.

**Befund: Der Kanon bestätigt die Achse — und liefert ihr die formale Begründung.** `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]`

1. **Die formale Asymmetrie ist Lehrbuchwissen (§2).** P = NP ist existenziell (ein Algorithmus genügt, §1.6); P ≠ NP ist universell über eine unendliche, nicht endlich parametrisierte Klasse. Nur die erste Richtung hat überhaupt die Form »finde ein Objekt«.
2. **Die Barrieren sind exakt die Formalisierung der Schwierigkeit auf der universellen Seite.** Alle drei sagen dasselbe in verschiedenen Sprachen: Wer über *alle* Algorithmen quantifizieren will, darf die Algorithmen nicht als Black Box behandeln (Relativization), nicht über eine typische Eigenschaft argumentieren (Natural Proofs) und nicht über ihre algebraische Fortsetzung (Algebrization). Er muss die **innere Struktur** ausnutzen — und dafür gibt es kein maschinell prüfbares Erfolgskriterium.
3. **Das einzige Resultat, das die Barrieren überwindet (Williams, §3.5), tut es ausgerechnet über einen algorithmischen Umweg**: Es baut einen *konkreten, endlichen* SAT-Algorithmus für ACC⁰ und wandelt ihn in eine untere Schranke um. Das ist ein bemerkenswerter Teilbeleg **für** die Achse und zugleich ein Hinweis auf ihre Grenze: Die erfolgreichste bekannte Methode für untere Schranken läuft über die Konstruktion eines suchbaren Objekts. `[EIGENE EINSCHÄTZUNG, Konfidenz mittel]` — Hieraus ergibt sich eine konkrete Frage an A6/A8: Kann KI-gestützte Suche **bessere Circuit-SAT-Algorithmen** für eingeschränkte Klassen finden? Das wäre der Ort, an dem KI-Suche und untere Schranken sich tatsächlich berühren — und es ist ein Suchproblem mit maschinell prüfbarem Kriterium (Laufzeit messbar, Korrektheit testbar).
4. **Fortnow (§5) bestätigt die Achse aus der Gegenrichtung.** »We don't even have a viable approach« heißt genau: Es gibt kein Suchproblem, das man einem Suchverfahren vorlegen könnte. Ein Optimierer braucht eine Zielfunktion; für P vs. NP gibt es keine.
5. **Optiland (§7.3) erklärt, warum die Achse leicht übersehen wird.** Die praktischen Erfolge bei suchbaren Problemen sind real und sichtbar; die Nicht-Bewegung bei der universellen Frage ist unsichtbar. Der Eindruck von Fortschritt entsteht aus der Verwechslung beider.

**Kein Gegenbefund.** Ich habe in diesem Bereich nichts gefunden, das die Achse widerlegt oder substanziell relativiert.

---

## 9. Was ich NICHT verifizieren konnte

Vollständige Liste der offenen Punkte dieses Berichts:

1. **Die Differenz 80 % vs. 88 % (Gasarch 2019).** Die Erklärung »alle Befragten vs. nur Meinungsäußernde« ist plausibel und arithmetisch stimmig, aber nicht belegt. Beide Zahlen sind im Bericht stehen geblieben.
2. **Die genaue Aufschlüsselung der Umfrage 2002** (die Zahlen 9 / 22 / 8 für P=NP / keine Meinung / sonstiges). Nur »61 von 100 für P≠NP« ist belegt, und dass 7 davon ausdrückliche Zweifel äußerten.
3. **Die genaue Zahl der P=NP-Stimmen 2012.** Die Synthesen nennen 81 % bzw. »125 von 152« für P≠NP; der Rest ist nicht aufgeschlüsselt.
4. **Das Publikationsjahr von Allenders *Status Report*** — 2008 oder 2009, die Quellen widersprechen sich. Beide berichtet.
5. **Der exakte Wortlaut** sämtlicher Zitate (Fortnow, Aaronson, Gasarch). Kein Volltextzugang; alle Zitate sind Suchsynthesen und entsprechend markiert.
6. **Die Inhalte** von Allenders und Aaronsons Übersichtsartikeln. Nur Existenz und Venue verifiziert.
7. **Der genaue technische Satz der Algebrization-Barriere.** Ich konnte den Mechanismus (Low-Degree-Extension als erweitertes Orakel) verifizieren, nicht aber die exakten Satzformulierungen und Quantoren.
8. **Die Behauptung »Williams umgeht alle drei Barrieren«** im technisch strengen Sinn. Der Teil zu Relativization/Algebrization (nicht-relativierende Eigenschaften von ACC⁰) ist über zwei Suchen bestätigt; der Teil zu Natural Proofs (Verletzung von largeness) ist Community-Standarddarstellung, aber von mir nicht am Volltext geprüft.
9. **Ob es zwischen 2019 und 2026 eine Umfrage gibt.** Negativbefund aus zwei Suchen; ein Abwesenheitsbeleg per Suche ist prinzipiell schwächer als ein Positivbefund.
10. **Die Inhalte der 2026er Preprints** (arXiv:2606.12631, arXiv:2601.09702, arXiv:2511.14038). Nur Existenz, Titel und Abstract-Synthese.

---

## 10. Quellen

**Primärliteratur (Barrieren und Separationen)**
- T. Baker, J. Gill, R. Solovay: *Relativizations of the P =? NP Question*, SIAM J. Comput. 4(4), 431–442, 1975.
- A. A. Razborov, S. Rudich: *Natural Proofs*, J. Comput. Syst. Sci. 55(1), 24–35, 1997 — https://dl.acm.org/doi/10.1006/jcss.1997.1494 · https://www.sciencedirect.com/science/article/pii/S002200009791494X
- S. Aaronson, A. Wigderson: *Algebrization: A New Barrier in Complexity Theory*, STOC 2008, 731–740; ToCT 1(1), 2009, DOI 10.1145/1490270.1490272 — https://www.scottaaronson.com/papers/alg.pdf · https://dblp.org/rec/conf/stoc/AaronsonW08.html
- R. Williams: *Nonuniform ACC Circuit Lower Bounds*, J. ACM 61(1):2, 2014 — https://dl.acm.org/doi/10.1145/2559903 · https://people.csail.mit.edu/rrw/acc-lbs-journal-final.pdf ; Gödel-Preis 2024: https://www.csail.mit.edu/node/11836
- C. Murray, R. Williams: *Circuit Lower Bounds for Nondeterministic Quasi-Polytime from a New Easy Witness Lemma*, STOC 2018 / SIAM J. Comput. — https://dl.acm.org/doi/10.1137/18M1195887 · https://people.csail.mit.edu/rrw/easy-witness-nqp.pdf
- J. Li, T. Yang: *3.1n − o(n) circuit lower bounds for explicit functions*, STOC 2022 — https://dl.acm.org/doi/abs/10.1145/3519935.3519976 · https://eccc.weizmann.ac.il/report/2021/023/download/
- R. Impagliazzo: *A Personal View of Average-Case Complexity*, 1995 — Rezeption: https://www.quantamagazine.org/which-computational-universe-do-we-live-in-20220418/

**Übersichtsartikel**
- L. Fortnow: *The Status of the P versus NP Problem*, CACM 52(9), 2009 — https://dl.acm.org/doi/10.1145/1562164.1562186 · https://www.cs.cmu.edu/~15326-s26/CACM-Fortnow.pdf
- L. Fortnow: *Fifty Years of P vs. NP and the Possibility of the Impossible*, CACM 65(1), 76–85, Januar 2022, DOI 10.1145/3460351 — https://dl.acm.org/doi/10.1145/3460351 · https://lance.fortnow.com/papers/files/pvnp50.pdf
- E. Allender: *A Status Report on the P versus NP Question*, Advances in Computers 77, Kap. 4, 117–147 (2008 oder 2009) — https://people.cs.rutgers.edu/~allender/papers/advances.in.computing.pdf
- S. Aaronson: *P =? NP*, in: Nash/Rassias (Hg.), *Open Problems in Mathematics*, Springer 2016, DOI 10.1007/978-3-319-32162-2_1 — https://www.scottaaronson.com/papers/pnp.pdf
- S. Aaronson: *Is P Versus NP Formally Independent?* — https://www.scottaaronson.com/papers/indep.pdf
- A. Kolokolova: *Complexity Barriers as Independence*, 2016 — https://www.cs.mun.ca/~kol/papers/barriers-incomputable-revised.pdf

**Umfragen**
- W. Gasarch: *The P =? NP Poll*, SIGACT News Complexity Theory Column 36, 2002 — https://www.cs.umd.edu/~gasarch/papers/poll.pdf
- W. Gasarch: *Guest Column: The Second P =? NP Poll*, SIGACT News 43(2), 2012 — https://dl.acm.org/doi/10.1145/2261417.2261434 · https://www.cs.umd.edu/~gasarch/papers/poll2012.pdf
- W. Gasarch: *Guest Column: The Third P=?NP Poll*, SIGACT News 50(1), März 2019, DOI 10.1145/3319627.3319636 — https://dl.acm.org/doi/10.1145/3319627.3319636 · https://www.cs.umd.edu/users/gasarch/BLOGPAPERS/pollpaper3.pdf
- Blogpost zur dritten Umfrage: https://blog.computationalcomplexity.org/2019/03/third-poll-on-p-vs-np-and-related.html

**Fortnow 2026**
- *Respect the P v NP Problem*, 10.06.2026 — https://blog.computationalcomplexity.org/2026/06/respect-p-v-np-problem.html
- *Navier-Stokes and Lean*, 09/2026 — https://blog.computationalcomplexity.org/2026/09/navier-stokes-and-lean.html
- *We Still Can't Beat Relativization*, 01/2016 — https://blog.computationalcomplexity.org/2016/01/we-still-cant-beat-relativization.html

**Institutionell / aktuell**
- Clay Mathematics Institute, P vs NP — https://www.claymath.org/millennium/p-vs-np/
- Workshop *Frontiers in Complexity Lower Bounds*, Isaac Newton Institute, 7.–11.09.2026 — https://cstheory-events.org/2026/05/13/workshop-frontiers-in-complexity-lower-bounds/

**Preprints (Existenz belegt, Inhalt ungeprüft)**
- arXiv:2606.12631 — *The Switching Lemma shows what the Switching Lemma cannot prove: an unconditional natural-proofs barrier*
- arXiv:2601.09702 — *Diagonalization Without Relativization: A Closer Look at the Baker-Gill-Solovay Theorem*
- arXiv:2511.14038 — *New Algebrization Barriers to Circuit Lower Bounds via Communication Complexity of Missing-String*
- arXiv:2312.08163 — *Towards P≠NP from Extended Frege lower bounds*
- arXiv:2606.27139 — *The Observer World: A Cryptographic Extension of Impagliazzo's Five Worlds*

**Nicht anerkannte Claims (zur Weitergabe an A8)**
- arXiv:2106.11886 — *The Separation of NP and PSPACE* (T. Lin) `[CLAIM]`
- arXiv:2510.17829 — *A Homological Separation of P from NP* `[CLAIM]`
- arXiv:2512.11820 — *Toward P vs NP: An Observer-Theoretic Separation via SPDP Rank …* `[CLAIM]`
