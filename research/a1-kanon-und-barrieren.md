# A1 — Kanon & Landkarte: Konsensstand zu P vs. NP und die drei Barrieren

**Agent:** A1 "Kanon & Landkarte"
**Stand:** 12. September 2026
**Methode:** ausschließlich WebSearch (WebFetch in dieser Umgebung gesperrt, siehe `docs/02-team-briefing.md` §1).
Rund 50 Suchanfragen über zwei Arbeitsdurchgänge; jeder nichttriviale Claim mit mindestens zwei unterschiedlich formulierten Anfragen trianguliert.

**Revisionshinweis (zweiter Durchgang).** Dieser Bericht wurde nach einer Erstfassung überarbeitet. Vier Punkte haben sich dabei **substanziell geändert**, und die Änderungen sind im Text jeweils als solche kenntlich gemacht statt still eingearbeitet:
1. **Ein Zitat wurde entfernt.** Der Fortnow zugeschriebene Satz »we don't even have a viable approach« ist **nicht belegbar** (§5.1). Die Erstfassung führte ihn als bestätigt. Das war ein Fehler dieses Agenten.
2. **Der Gasarch-Zahlenwiderspruch ist teilweise *härter* geworden**, nicht weicher: Die 66 % sind erklärt, die Differenz 80 % vs. 88 % ist es nicht, und die frühere »plausible Auflösung« ist inzwischen durch einen Gegenbefund belastet (§4.2).
3. **Der Barrieren-Umgehungsmechanismus bei Williams ist jetzt belegt**, nicht nur referiert (§3.5b).
4. **Williams' √-Platz-Simulation (2025) gehört zu P vs. PSPACE**, nicht in eine Randnotiz (§6.6).
5. **Die Leitachse war in der überholten Fassung referiert** (»endliches vs. unendliches Ergebnis«) und ist auf die korrigierte B1/B2/B3-Fassung des Briefings umgestellt; dabei ist ein **teilweiser Gegenbefund** entstanden (§8, Punkt 3).

**Lesehinweis zu den Markierungen:**
- `[KANON]` — Lehrbuchwissen, seit Jahrzehnten in jedem Standardlehrbuch (Sipser, Arora–Barak, Papadimitriou), nicht strittig. Konfidenz hoch. Diese Aussagen sind in dieser Umgebung nicht am Volltext nachgeprüft, aber sie sind auch nicht das, was Nachprüfung braucht.
- `[VERIFIZIERT]` — peer-reviewt, Existenz und Kernaussage durch mehrere unabhängige Suchen bestätigt.
- `[NUR-SNIPPET]` — aus Suchergebnis-Synthesen rekonstruiert, **kein Volltextzugang**. Das ist in diesem Projekt der Normalfall.
- `[CLAIM]` — Behauptung ohne Verifikation.
- `[EIGENE EINSCHÄTZUNG]` — Analyse des Agenten, kein Literaturbefund.

---

## 0. Kurzfassung für eilige Leser

1. P vs. NP ist im September 2026 **offen**. Es gibt keinen anerkannten Beweis in irgendeine Richtung. Lance Fortnow hat im Juni 2026 öffentlich genau die Frage dieses Projekts gestellt — steht ein **KI-erzeugter** Beweis von P ≠ NP bevor? — und sie mit »No, it isn't« beantwortet: Er erwarte zu seinen Lebzeiten keinen Beweis, »by man or machine, separately or working together« (§5).
   **Warnung an die Redaktion:** Der in einer früheren Fassung *dieses* Berichts als bestätigt geführte Wortlaut »we don't even have a viable approach« ist **nicht belegbar** und darf nicht zitiert werden. Er wurde entfernt; die Fehlerbeschreibung steht in §5.
2. Die große Mehrheit der Fachwelt erwartet **P ≠ NP** — von rund 61 % (2002) auf rund 80–88 % (2019), unter ausgewiesenen Fachleuten nochmals deutlich höher. Von den drei kursierenden Zahlen für 2019 ist eine erklärt und eine Differenz **ungelöst**: Die **66 %** beantworten nachweislich eine *andere Frage* (»wird das Problem vor 2100 gelöst?«); die Differenz **80 % vs. 88 %** ließ sich nicht auflösen, und die naheliegende Erklärung ist durch einen Gegenbefund belastet. Das Papier darf hier **keine Einzelzahl** nennen (§4.2).
   Es gibt **keine Umfrage nach 2019** — wer für 2026 Konsenszahlen zitiert, zitiert Zahlen von 2018 (§4.3).
3. Die drei Barrieren (Relativization, Natural Proofs, Algebrization) sind **Sätze über Beweistechniken**, nicht über das Problem. Sie zeigen *nicht*, dass P vs. NP unlösbar oder unabhängig von ZFC ist. Es existieren publizierte Resultate, die alle drei Barrieren nachweislich umgehen (Williams, NEXP ⊄ ACC⁰, JACM 2014, Gödel-Preis 2024).
4. **Methodischer Eigenbefund, der ins Papier gehört.** Zwei der Korrekturen an diesem Bericht hatten dieselbe Ursache: Eine Suchschicht, die *synthetisiert* statt zu zitieren, bestätigt bereitwillig, was die Frage nahelegt — ein plausibel klingendes Fortnow-Zitat (§5.1) und stabil wirkende, in Wahrheit verschmolzene Umfragezahlen (§4.2(3)). Für ein Papier über KI-gestützte Wahrheitsfindung ist das kein Betriebsunfall, sondern Material.
5. Die härteste einzelne Zahl zur Lage des Feldes: Die beste bekannte untere Schranke für die Größe eines *allgemeinen* Booleschen Schaltkreises für eine explizite Funktion liegt bei **3,1n − o(n)** (Li–Yang, STOC 2022). Für P ≠ NP bräuchte man eine *superpolynomielle* Schranke. Der Abstand zwischen »3,1·n« und »n^{ω(1)}« ist die ehrlichste Beschreibung des Forschungsstands.

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

**Historische Fußnote, die sich für die Zielgruppe lohnt.** `[NUR-SNIPPET]`, Konfidenz mittel-hoch (in zwei Suchen konsistent): **John Nash** hat die Frage in einem handschriftlichen Brief an die **NSA von 1955** — also rund 16 Jahre vor Cook — bereits beinahe gestellt: Er argumentierte dort über den Aufwand des Schlüsselbrechens in Begriffen, die der modernen Komplexitätsvermutung sehr nahekommen. Aaronson erwähnt das im Eröffnungskapitel des Nash-Gedenkbands (§4.4) ausdrücklich. Für das Papier ist die Episode nützlich, weil sie zeigt, dass die Frage nicht aus der Logik, sondern aus der **Kryptographie** kommt — dieselbe Kopplung, die in der Natural-Proofs-Barriere (§3.2) wiederkehrt.

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

**Der Satz.** Die in zwei unabhängigen Suchen übereinstimmende Kernformulierung lautet: `[VERIFIZIERT]` für den Aussagegehalt, `[NUR-SNIPPET]` für den Wortlaut

> Nahezu alle großen offenen Probleme der Komplexitätstheorie — **ausdrücklich einschließlich P vs. NP**, ferner P vs. RP und NEXP vs. P/poly — **erfordern nicht-algebrisierende Techniken.**

Technisch: Es gibt algebrische Orakel, relativ zu denen P = NP gilt, und andere, relativ zu denen P ≠ NP gilt. Die Arbeit führt dazu ein Modell der **algebraischen Anfragekomplexität** ein und beweist die nötigen unteren Schranken in diesem Modell.

**Die Entstehungslogik, die man kennen muss, um die Barriere richtig zu lesen** (in der Suchsynthese ausdrücklich so dargestellt, `[NUR-SNIPPET]`): Aaronson und Wigderson gingen davon aus, dass ein P≠NP-Beweis zwei Barrieren überwinden muss — Relativization und Natural Proofs. Da inzwischen Schranken bekannt waren, die **beide gleichzeitig** überwinden, stellten sie die Frage, ob es eine **dritte** Barriere gibt. Algebrization ist die Antwort darauf. Das ist ein wichtiges Detail für das Papier: Die Barrieren sind nicht als geschlossener Katalog entstanden, sondern **nacheinander, jeweils als Reaktion darauf, dass die vorige umgangen wurde**. `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]` Daraus folgt zweierlei: (i) Barrieren werden tatsächlich umgangen, sonst gäbe es die jeweils nächste nicht; (ii) es gibt keinen Grund anzunehmen, dass die Liste mit drei Einträgen vollständig ist — die 2025/2026er Preprints (§3.2, §3.3 unten) sind genau die Fortsetzung dieser Bewegung.

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
- **Warum es die Barrieren umgeht — jetzt mit Mechanismus.** `[VERIFIZIERT]` (über zwei unabhängige Suchen bestätigt; die Gödel-Preis-Laudatio 2024 nennt Relativization und Natural Proofs ausdrücklich, eine zweite Quellensynthese nennt alle drei einschließlich Algebrization)
  - **Relativization und Algebrization** entfallen aus **einem** Grund, und der ist der eigentliche Clou des Ansatzes: Der Beweis hängt an einem **nichttrivialen SAT-Algorithmus für ACC⁰**, und die in zwei Suchen übereinstimmende Begründung lautet — *alle bekannten Satisfiability-Algorithmen, die die erschöpfende Suche schlagen, brechen zusammen, sobald man der Instanz ein Orakel (oder dessen algebraische Fortsetzung) hinzufügt.* Wer schneller als Brute Force ist, **muss** Struktur in der Instanz ausnutzen, die eine Black-Box-Methode nicht sehen kann. Genau deshalb kann ein Beweis, der über einen solchen Algorithmus läuft, prinzipiell nicht relativieren und nicht algebrisieren. `[NUR-SNIPPET]` für den Wortlaut, Konfidenz **hoch** für den Mechanismus.
  - **Natural Proofs** entfällt, weil das Argument **nicht largeness-basiert** ist: Es zeigt nicht »die typische Funktion ist hart«, sondern zielt über die Easy-Witness-Methode auf eine ganz spezifische Sprache in NEXP. `[NUR-SNIPPET]`, Konfidenz mittel-hoch.
  - `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]` **Das ist die pointierteste Aussage des ganzen Berichts für die Projektleitfrage:** Der einzige bekannte Weg an den Orakel-Barrieren vorbei führt über den Bau eines **konkreten, endlichen, messbaren Objekts** — eines Algorithmus. Dazu §8.
- **Weiterführend:** R. R. Williams, *Complexity Lower Bounds from Algorithm Design*, eingeladener Beitrag LICS 2021 — die Selbstdarstellung der Methode durch den Autor. `[VERIFIZIERT]` für Existenz, Inhalt `[NUR-SNIPPET]`.
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

### 4.2 Der dokumentierte Zahlenwiderspruch — teils aufgelöst, teils **härter geworden**

**Das Problem.** Im Umlauf sind für die Umfrage 2019 die Zahlen **66 %**, **~80 %** und **88 %** als »Anteil für P ≠ NP«. Die Projektleitung hat diesen Widerspruch als Testfall für die Triangulationspflicht markiert. Ergebnis nach insgesamt sieben gezielten Suchen: **Ein Teil löst sich auf, ein Teil nicht — und der verbleibende Teil ist nach zusätzlicher Recherche schwerer geworden, nicht leichter.**

#### (1) Die 66 % — aufgelöst `[NUR-SNIPPET]`, Konfidenz hoch

> Die **66 % beziehen sich auf eine andere Frage.** Gasarchs Umfragen fragen nicht nur »P = NP oder P ≠ NP?«, sondern auch »**Wann wird das Problem gelöst?**«. Die 66 % sind der Anteil derjenigen, die eine Lösung **vor dem Jahr 2100** erwarten.

Belegkette (zwei unabhängig formulierte Suchen, konsistent):
- »vor 2100 gelöst?« — **2002: 62 %**, **2012: 53 %**, **2019: 66 %**
- »wird nie gelöst« — **2002: 5 %**, **2012: 3 %**, **2019: 9 %**
- Dazu ein Gasarch zugeschriebener Kommentar sinngemäß: Der Anstieg von 53 % auf 66 % »amazes me because, since 2012, there has been little (no?) progress on resolving P =? NP«. `[NUR-SNIPPET]`

**Damit ist die 66-%-Zahl aus dem Rennen:** Sie steht nicht in Konkurrenz zu 80 % oder 88 %, sondern beantwortet eine andere Frage. Der ursprünglich vermutete Widerspruch war hier ein Artefakt der Suchsynthese, die zwei Kennzahlen derselben Umfrage nebeneinanderstellte.

#### (2) Die Differenz 80 % vs. 88 % — **nicht** aufgelöst, und die naheliegende Erklärung ist inzwischen **belastet**

Die Erstfassung dieses Berichts hielt fest, die plausibelste Erklärung sei: **80 % bezogen auf alle 124 Befragten, 88 % bezogen auf die Teilmenge derer, die sich überhaupt festlegten** (Rest: »weiß nicht« / »unabhängig« / »nicht wohlgestellt«). Diese Erklärung ist arithmetisch stimmig und war nicht belegt.

**Neuer Befund, der sie untergräbt** `[NUR-SNIPPET]`, Konfidenz mittel: Eine Suchsynthese gibt die Zahlen für 2019 als **88 % für P ≠ NP, 12 % für P = NP** an und fügt ausdrücklich hinzu, **niemand** habe sich für Unabhängigkeit oder »keine Meinung« entschieden. Wenn das stimmt, gibt es **keine Abstentionen** — und dann können »alle Befragten« und »nur Meinungsäußernde« gar nicht auseinanderfallen. Die 80/88-Differenz wäre dann **nicht** durch unterschiedliche Bezugsgrößen erklärbar, sondern schlicht ein Fehler auf einer der beiden Seiten.

**Konsequenz, verbindlich nach `docs/02-team-briefing.md` §1:** Beide Zahlen werden berichtet, der Widerspruch wird gekennzeichnet, und die frühere »plausible Auflösung« wird **zurückgezogen**. Im Papier ist die korrekte Formulierung: *»In der dritten Gasarch-Umfrage (2019, 124 Befragte) sprachen sich rund 80–88 % für P ≠ NP aus; die in Umlauf befindlichen Zahlen differieren, und die Differenz ließ sich ohne Volltextzugang nicht auflösen.«* Eine Einzelzahl darf **nicht** angegeben werden.

#### (3) Ein methodischer Nebenbefund, der ins Papier gehört

`[EIGENE EINSCHÄTZUNG, Konfidenz hoch]` Über sieben Suchen hinweg lieferte die Suchsynthese für die Umfragen **2012** und **2019** wiederholt **dieselben** Kennzahlen — »etwa 80 % für P ≠ NP« und »99 % unter denen, die viel darüber nachgedacht haben«. Für zwei verschiedene Umfragen mit verschiedenen Teilnehmerzahlen ist das mit hoher Wahrscheinlichkeit eine **Verschmelzung zweier Quellen** in der Synthese, nicht ein tatsächlicher Befund. Eine einzelne Suche gab zudem für 2019 die undekodierbare Zahlenfolge **»66 %/9 %/0 %/91 %«** aus, ohne Legende; ich konnte sie nicht zuordnen und berichte sie nur als Beleg dafür, wie instabil diese Schicht ist.

**Die Lehre für das Papier:** Für **grobe** Aussagen (»große Mehrheit erwartet P ≠ NP«) ist die Suchschicht tragfähig. Für **Prozentzahlen auf den Punkt** ist sie es nicht. Das ist kein Nebenproblem dieses Berichts, sondern ein Beleg zur Projektleitfrage: Eine KI-gestützte Rechercheschicht **synthetisiert Plausibles** — genau wie in §5.1. Wer Zahlen aus ihr übernimmt, ohne den Volltext zu sehen, publiziert Vermutungen im Gewand von Daten.

#### (4) Die Zahlen, soweit belastbar

| Jahr | Anteil P ≠ NP | Bezugsgröße | Beleglage |
|---|---|---|---|
| 2002 | **61 von 100** (61 %); davon **7 mit ausdrücklichen Zweifeln**. Für P = NP: **9** | alle Befragten | `[VERIFIZIERT]` über zwei unabhängige Suchen (61 / 7 / 9 in beiden konsistent). Die Aufschlüsselung der restlichen 30 (»keine Meinung«, »Frage nicht wohlgestellt«) **konnte ich nicht verifizieren** |
| 2012 | **ca. 81–83 %**; eine Synthese nennt »125 von 152« (82 %), eine andere »81 %«, eine dritte »etwa 80 %« | alle Befragten | `[NUR-SNIPPET]`, Konfidenz **niedrig-mittel** für die genaue Zahl (siehe Nebenbefund (3)), hoch für die Größenordnung |
| 2019 | **ca. 80 %** *oder* **88 %** — **Widerspruch nicht aufgelöst**, siehe (2) | bei 124 Befragten | `[NUR-SNIPPET]`, Konfidenz **niedrig** für die genaue Zahl, hoch für »große Mehrheit« |
| 2019, Teilmenge »viel darüber nachgedacht« | **99 %** | Experten-Teilmenge | `[NUR-SNIPPET]`, in mehreren Suchen konsistent — **aber** auch für 2012 ausgegeben (Nebenbefund (3)), daher Konfidenz **mittel** und Zuordnung zum Jahr unsicher |

**Zusatzbefund 2012.** Gesondert ausgewiesene Teilgruppe von **21 Preisträgern** (Gödel-Preis, Turing-Award o. ä.): **17 (81 %) für P ≠ NP** (2 davon nur schwach), **2 (9 %) für P = NP**, **2 (9 %) »weiß nicht«**. `[NUR-SNIPPET]`, Konfidenz mittel. Als Kontrollgruppe interessant: Der Wert unterscheidet sich **nicht** nennenswert vom Gesamtwert — das spricht gegen die gelegentlich geäußerte Vermutung, die P≠NP-Mehrheit sei ein Effekt uninformierter Teilnehmer.

**Die inhaltlich belastbare Gesamtaussage** — und mehr sollte das Papier nicht behaupten: `[VERIFIZIERT]` über alle Suchen hinweg konsistent

> In allen drei Umfragen (2002, 2012, 2019) erwartet eine **deutliche und über die Zeit wachsende Mehrheit** der befragten Theoretikerinnen und Theoretiker **P ≠ NP**: von rund 61 % (2002) auf rund 80–88 % (2019). Unter denjenigen, die sich intensiv mit dem Problem befasst haben, ist die Zustimmung nochmals deutlich höher. Die Gegenposition P = NP ist eine kleine, aber nicht verschwindende Minderheit, die ausdrücklich auch respektierte Fachvertreter umfasst.

**Wichtige Relativierung, die im Papier stehen muss.** `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]` Eine Umfrage ist **kein mathematisches Argument**. Der Konsens dokumentiert die Erwartung des Feldes, nicht den Wahrheitswert. Historisch gab es Fälle, in denen eine breite Fachmehrheit falsch lag. Der Wert der Zahlen für unser Papier ist ein anderer: Sie zeigen, dass **niemand mit einer baldigen Lösung rechnet** und dass die Erwartungshaltung über 17 Jahre hinweg **stabil** geblieben ist — und das ist der Referenzpunkt, gegen den ein behaupteter KI-Durchbruch sich beweisen müsste.

### 4.3 Gibt es eine Umfrage 2024–2026?

**Negativbefund.** Inzwischen **drei** gezielte, unterschiedlich formulierte Suchen nach einer vierten Gasarch-Umfrage bzw. einer vergleichbaren Erhebung 2024–2026 ergaben **keinen Treffer**; eine davon fragte ausdrücklich nach einer »fourth poll« und lieferte ausschließlich Verweise auf die Umfragen von 2002, 2012 und 2019. Die jüngste dokumentierte Umfrage dieser Art ist die dritte (2019). `[NUR-SNIPPET]`, Konfidenz **hoch** für die Größenordnung des Befundes, mit der prinzipiellen Einschränkung, dass ein Abwesenheitsbeleg per Suche schwächer ist als ein Positivbefund.

**Konsequenz für das Papier.** Es gibt **keine belastbare quantitative Erhebung des Meinungsstandes nach 2019** — insbesondere keine, die den Einfluss der KI-Entwicklung seit 2023 auf die Erwartungen des Feldes messen würde. Das ist eine echte Forschungslücke und sollte als solche benannt werden. Wer für 2026 Prozentzahlen zum Fachkonsens zitiert, zitiert in Wahrheit Zahlen von 2018.

### 4.4 Die maßgeblichen Übersichtsartikel

| Arbeit | Autor | Jahr/Venue | Status |
|---|---|---|---|
| *The Status of the P versus NP Problem* | Lance Fortnow | Communications of the ACM 52(9), 78–86, September 2009 | `[VERIFIZIERT]` — der meistgelesene Übersichtsartikel; wird bis heute in Kursen als Standardlektüre eingesetzt (Treffer bei CMU- und Duke-Kursseiten) |
| *A Status Report on the P versus NP Question* | Eric Allender | Advances in Computers, Bd. 77, Kapitel 4, S. 117–147, **2009** | `[VERIFIZIERT]` für Venue/Band/Kapitel/Seiten/Jahr. **Der in der Erstfassung notierte Widerspruch 2008 vs. 2009 ist aufgelöst:** eine dritte, gezielte Suche liefert übereinstimmend 2009 (dblp Bd. 77, ScienceDirect, Rutgers-Publikationsliste). Inhalt `[NUR-SNIPPET]`, nicht am Volltext geprüft |
| *P =? NP* | Scott Aaronson | in: J. F. Nash Jr., M. Th. Rassias (Hg.), *Open Problems in Mathematics*, Springer **2016**, Kapitel 1, S. 1–122, DOI 10.1007/978-3-319-32162-2_1 | `[VERIFIZIERT]` für Venue/Jahr/Herausgeber/Umfang über drei Suchen. **Anmerkung:** Der Auftrag nannte »2017«; die Buchpublikation ist **2016** datiert. Mit ~116–122 Seiten die **ausführlichste allgemeinverständliche Bestandsaufnahme** überhaupt, ausdrücklich für ein breites Publikum aus Mathematik, Naturwissenschaft und Technik geschrieben, inkl. Barrieren, GCT und Unabhängigkeitsfrage. **Für unsere Zielgruppe die erste Empfehlung.** Inhalt im Einzelnen `[NUR-SNIPPET]` |
| *Fifty Years of P vs. NP and the Possibility of the Impossible* | Lance Fortnow | Communications of the ACM 65(1), 76–85, Januar 2022, DOI 10.1145/3460351 | `[VERIFIZIERT]` für Venue/Band/Seiten/DOI über zwei unabhängige Suchen. Enthält die Optiland-These (§7.3) |

---

## 5. Fortnow Juni 2026 — Wortlautprüfung und **Korrektur eines Pseudo-Zitats**

### 5.1 Der korrigierte Befund

**Eine frühere Fassung dieses Berichts führte hier das Zitat »At this time we don't even have a viable approach to settling the P v NP problem« als über zwei Suchen bestätigt. Das war falsch.** Agent A7 hat den Wortlaut mit Konfidenz hoch als nicht belegbar zurückgewiesen (`docs/02-team-briefing.md` §3); zwei eigene, unterschiedlich formulierte Suchen konnten ihn ebenfalls **nicht** reproduzieren — eine gezielt auf die Phrase gerichtete Suche meldete ausdrücklich, die Wendung komme in den Treffern nicht vor. **Der Wortlaut wird im Papier nicht verwendet.** `[NUR-SNIPPET]`, Konfidenz der Zurückweisung: hoch.

`[EIGENE EINSCHÄTZUNG, Konfidenz hoch]` Der Vorfall ist selbst ein Befund und gehört ins Papier. Das Zitat klingt fachlich plausibel, passt zur Erwartung des Lesers und ist genau deshalb durch drei Rechercheebenen gewandert (Leitungs-Vorrecherche → Team-Briefing → A1-Erstfassung), bevor es geprüft wurde. Eine Suchmaschine, die eine *Synthese* statt eines Volltextes liefert, bestätigt bereitwillig, was die Frage bereits nahelegt. Das ist dieselbe Fehlermechanik, die das Feld mit fehlerhaften Beweisversuchen füllt, nur eine Etage höher: **Plausibilität wird als Evidenz verbucht.** Wer über KI-gestützte Wahrheitsfindung schreibt, sollte diesen Eigenfehler dokumentieren, nicht still korrigieren.

### 5.2 Was in dem Post tatsächlich steht

**Fundstelle.** Computational Complexity Blog (Fortnow/Gasarch), Post »Respect the P v NP Problem«, `https://blog.computationalcomplexity.org/2026/06/respect-p-v-np-problem.html`. `[VERIFIZIERT]` für Existenz und URL.
**Datumsdifferenz, nicht geglättet:** Eine Suchsynthese nennt den **10. Juni 2026**, eine zweite den **14. Juni 2026**. Gesichert ist nur der Monat (Juni 2026, aus dem URL-Pfad). `[NUR-SNIPPET]`

**Inhalt, über zwei unabhängig formulierte Suchen konsistent** `[NUR-SNIPPET]`, Konfidenz mittel-hoch:

1. **Die Doppelstruktur des Problems.** Fortnow unterscheidet zwei Lesarten von P vs. NP: die **formale mathematische Vermutung** (Clay-Millennium-Problem) und die **intuitive Frage**, ob alles effizient Verifizierbare auch effizient berechenbar ist. Die Pointe des Posts hängt an dieser Trennung.
2. **Optiland, mit KI als Treiber.** Fortschritte in Optimierung und Lernen hätten uns in die Lage versetzt, die *praktisch auftretenden* NP-Probleme zu lösen, **während die kryptographischen Verfahren ungebrochen bleiben**. Die intuitive Lesart bewegt sich also — die formale nicht. (Ausführlich §7.3.)
3. **Die Leitfrage dieses Projekts, vom Fachvertreter selbst gestellt und beantwortet.** Der Post fragt ausdrücklich, ob ein **KI-erzeugter Beweis von P ≠ NP** bevorstehe. Antwort laut Suchsynthese: **»No, it isn't.«** Fortnow erwarte zu seinen Lebzeiten keinen Beweis von P vs. NP — **»by man or machine, separately or working together«**. `[NUR-SNIPPET]`, Konfidenz mittel-hoch für den Sinngehalt, **niedrig für den exakten Wortlaut** (kein Volltextzugang — siehe §5.1 für den Grund, hier besonders vorsichtig zu sein).
4. **Die Begründung ist eine Basisraten-Aussage, keine Prinzipienaussage.** Fortnow erkennt die Widerlegung des **Erdős-Einheitsabstandsproblems** als beeindruckende KI-Leistung an, stellt ihr aber gegenüber, dass auf jeden KI-Mathematikbeweis **Hunderte** von Problemen kommen, an denen KI ohne Fortschritt versucht wurde. `[NUR-SNIPPET]` — Dieser Punkt ist für A8 (Claim-Audit) und die Gesamtsynthese wichtig: Die öffentliche Wahrnehmung sieht die Treffer, nicht den Nenner.
5. **Der Lean-Punkt** (aus dem Team-Briefing §3, dort als belegt geführt, von mir **nicht** eigenständig nachgeprüft): »Don't waste your time trying a formal approach via Lean« und »Computational complexity is very messy to formulate technically«; ferner, dass Fortnow nicht einmal triviale Abgeschlossenheitslemmata für P KI-gestützt in Lean verifiziert bekam. Zuständig ist A7. `[NUR-SNIPPET]`, übernommen, Konfidenz mittel.

### 5.3 Einordnung

`[EIGENE EINSCHÄTZUNG, Konfidenz hoch]` Der korrigierte Befund ist für das Projekt **stärker** als das entfernte Pseudo-Zitat. Statt einer allgemeinen Klage über die Schwierigkeit des Problems liegt eine **direkte, datierte, öffentliche Stellungnahme des einschlägigsten Fachvertreters zur Leitfrage des Projekts** vor — im selben Monat, in dem das Papier entsteht, und mit einer Begründung, die nicht »KI ist schwach« lautet, sondern **»die sichtbaren Erfolge sind eine Auswahl aus einem sehr großen Nenner«**. Zugleich ist die Quelle ein **Blogpost**, also keine peer-reviewte Aussage, sondern eine begründete Expertenmeinung; sie ist im Papier als solche zu kennzeichnen und nicht als Befund.

Der Zusammenhang mit Fortnows Post »Navier-Stokes and Lean« (September 2026, `https://blog.computationalcomplexity.org/2026/09/navier-stokes-and-lean.html`) — Lean verifiziert die *Ableitung*, nicht die *Angemessenheit der Voraussetzungen* (Muss-Kriterium M4) — wird von A7 ausgearbeitet.

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
| 2021/2022 | **3,1n − o(n)** | **Jiatu Li, Tianqi Yang** (Tsinghua), STOC 2022; ECCC TR21-023 `[VERIFIZIERT]`, über zwei unabhängige Suchen einschließlich Autorennamen und Institution |

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
- **Echte Bewegung 2025 — und zwar an genau dieser Frage.** `[VERIFIZIERT]` (über zwei unabhängige Suchen bestätigt, einschließlich ECCC-Nummer) R. Williams, *Simulating Time With Square-Root Space*, ECCC TR25-017, Februar 2025, STOC 2025: **Jede Mehrband-Turingmaschine mit Laufzeit t ist in Platz O(√(t log t)) simulierbar.** Das ist die erste substanzielle Verbesserung gegenüber der Schranke O(t/log t) von Hopcroft–Paul–Valiant aus den 1970ern, also nach rund 50 Jahren. Technisch läuft es über eine Reduktion auf eine implizit definierte **Tree-Evaluation**-Instanz und baut auf dem platzsparenden Tree-Evaluation-Algorithmus von **Cook und Mertz (STOC 2024)** auf. `[NUR-SNIPPET]` für die Beweistechnik.
  **Warum das hierher gehört:** Laut Suchsynthese liefert die Arbeit **direkten Fortschritt bei P vs. PSPACE** — sie identifiziert explizite Probleme, die in Platz O(n) lösbar sind, auf Mehrband-Turingmaschinen aber im Wesentlichen n² Zeit erfordern. `[NUR-SNIPPET]`, Konfidenz mittel-hoch. Damit korrigiere ich die Einordnung der Erstfassung (dort unter §6.8 als bloß »angrenzend« geführt): Es ist ein Resultat **an** der Zeit-Platz-Achse, nicht nur neben ihr. Ein Folge-Preprint (arXiv:2508.14831, *TIME[t] ⊆ SPACE[O(√t)] via Tree Height Compression*) verschärft die Schranke offenbar um den Logarithmusfaktor; `[PREPRINT]`, Inhalt ungeprüft.
  `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]` **Einordnung für das Papier:** Das ist der stärkste Beleg dafür, dass das Feld 2025/26 **nicht** stillsteht — und zugleich dafür, wo es sich bewegt: an Zeit-**Platz**-Fragen, wo Diagonalisierung und Simulation noch greifen, nicht an der Zeit-**Nichtdeterminismus**-Frage P vs. NP, wo sie durch Baker–Gill–Solovay ausgeschlossen sind (§6.3). Der Kontrast ist didaktisch wertvoll: Ein 50-Jahre-Stillstand *kann* gebrochen werden — nur eben dort, wo keine Barriere im Weg steht.
- **Crank-Warnung:** Es kursiert ein arXiv-Preprint »The Separation of NP and PSPACE« (arXiv:2106.11886, Tianrong Lin, mehrfach revidiert, zuletzt laut Suchtreffer April 2025), der NP ≠ PSPACE per Diagonalisierung zu beweisen behauptet. **`[CLAIM]`, nicht anerkannt.** Schon die Methodenbeschreibung (Diagonalisierung) kollidiert mit §6.3/§3.1. A8 sollte das in die Crank-Auditierung aufnehmen. Analog kursieren »A Homological Separation of P from NP« (arXiv:2510.17829) und weitere. `[CLAIM]`

### 6.7 Wichtige Strukturaussagen unter der Annahme P ≠ NP `[KANON]`

- **Ladners Satz (1975):** Wenn P ≠ NP, gibt es NP-intermediate Probleme.
- **P = NP ⟹ PH kollabiert auf P.** Die gesamte Polynomialzeit-Hierarchie fällt zusammen. Umgekehrt ist »PH kollabiert nicht« eine gängige Verschärfungsannahme.
- **Exponential Time Hypothesis (ETH, Impagliazzo–Paturi):** 3-SAT erfordert Zeit 2^{Ω(n)}; **SETH** verschärft das auf »keine Konstante besser als 2ⁿ«. Diese Annahmen sind **stärker** als P ≠ NP und die Grundlage der Fine-Grained Complexity. Sie sind auch der Grund, warum der Fortschritt bei 3-SAT-Algorithmen (O\*(1,307ⁿ) nach Scheder 2024 laut Vorrecherche der Leitung) als Verbesserung der *Konstanten im Exponenten* einzuordnen ist und nicht als Annäherung an Polynomialzeit.

### 6.8 Angrenzende Bewegung 2025/2026

Zur Einordnung, dass das Feld nicht stillsteht — nur eben nicht bei P vs. NP:
- **Ryan Williams (Februar 2025), »Simulating Time With Square-Root Space«:** Der Verifikationslead der Leitung ist **bestätigt** (ECCC TR25-017, STOC 2025, zwei unabhängige Suchen). Wegen des direkten Bezugs zu P vs. PSPACE ist die Darstellung nach **§6.6** verschoben.
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

Es lohnt, dies neben Fortnows Post vom Juni 2026 (§5) zu lesen — dort steht **beides im selben Text**: Derselbe Autor, der Optiland als bereits eingetretene, KI-getriebene Realität beschreibt, verneint im selben Atemzug, dass ein KI-erzeugter Beweis von P ≠ NP bevorstehe. Das ist kein Widerspruch — es ist der Kern der Sache. `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]` Optiland ist die Erklärung dafür, **warum der Eindruck von Fortschritt entsteht, obwohl die formale Frage unbewegt ist**: Die intuitive Lesart von »P = NP« (können wir die Probleme lösen?) bewegt sich sichtbar; die formale Lesart (gilt der Satz?) bewegt sich nicht. Wer beide Lesarten nicht trennt, liest KI-Erfolge als Annäherung an eine Antwort.

---

## 8. Antwort auf die Leitachse (`docs/02-team-briefing.md` §2)

**Die Leitachse lautet** (in der *korrigierten* Fassung des Briefings, §2): **endlicher Suchkern in einem menschlichen Lifting-Rahmen** vs. **Probleme ohne endlichen Suchkern**. Der Schnitt verläuft ausdrücklich **nicht** zwischen »endlichem und unendlichem Ergebnis« — endlich ist allein das *gesuchte Objekt*; die Allgemeinheit kommt über **B3** herein (den von Menschen bewiesenen Lifting-Rahmen). Eine frühere Fassung dieses Abschnitts hat die Achse in der überholten Form referiert; das ist hier korrigiert.

**Befund: Der Kanon bestätigt die Achse — und liefert ihr die formale Begründung.** `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]`

1. **Die formale Asymmetrie ist Lehrbuchwissen (§2) und liefert B1/B2 direkt.** P = NP ist existenziell: **ein** Algorithmus genügt (§1.6) — ein endliches, maschinell repräsentierbares Objekt (B1 erfüllt). P ≠ NP ist universell über die Klasse *aller* Algorithmen; es gibt kein endliches Objekt, dessen Vorlage die Sache erledigt, und damit auch kein billiges Verifikationsorakel (B1 und B2 verletzt).
   **Wichtige Präzisierung, die das Papier nicht verschludern darf:** Auch die P=NP-Richtung ist **nicht** KI-zugänglich, obwohl B1 formal erfüllt ist. Denn zu verifizieren wäre nicht »der Algorithmus läuft auf diesen Instanzen schnell«, sondern »er ist auf **allen** Eingaben korrekt und im **Worst Case** polynomiell« (V1/V2 aus §1.6) — und das ist wieder eine universelle Aussage. **B2 fällt also auf beiden Seiten aus**, nur aus verschiedenen Gründen. Wer das übersieht, hält die P=NP-Richtung fälschlich für ein Suchproblem im Sinne von AlphaEvolve. `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]`
2. **Die Barrieren sind exakt die Formalisierung der Schwierigkeit auf der universellen Seite.** Alle drei sagen dasselbe in verschiedenen Sprachen: Wer über *alle* Algorithmen quantifizieren will, darf die Algorithmen nicht als Black Box behandeln (Relativization), nicht über eine typische Eigenschaft argumentieren (Natural Proofs) und nicht über ihre algebraische Fortsetzung (Algebrization). Er muss die **innere Struktur** ausnutzen — und dafür gibt es kein maschinell prüfbares Erfolgskriterium.
3. **Teilweiser GEGENBEFUND — und der wichtigste Einzelbeitrag dieses Berichts zur Leitachse.** `[EIGENE EINSCHÄTZUNG, Konfidenz mittel-hoch]`
   Das prominenteste Resultat, das die Barrieren überwindet (Williams, §3.5b), tut es ausgerechnet über einen **algorithmischen Umweg**: Es baut einen *konkreten, endlichen* SAT-Algorithmus für ACC⁰ und wandelt ihn über das Easy-Witness-Lemma in eine untere Schranke um. Und der jetzt belegte Mechanismus (§3.5b) sagt, dass das **kein Zufall** ist: Wer die erschöpfende Suche schlägt, *muss* Instanzstruktur ausnutzen, die eine Black-Box-Methode nicht sieht — **deshalb** kann ein so gebauter Beweis nicht relativieren und nicht algebrisieren.
   **Das ist genau die B1/B2/B3-Konstellation der Leitachse, angewandt auf untere Schranken:**
   - **B1** ✓ Das gesuchte Objekt ist endlich und maschinell repräsentierbar: ein Algorithmus.
   - **B2** ✓ (eingeschränkt) Korrektheit und Laufzeit eines Circuit-SAT-Algorithmus sind messbar und testbar — ungleich billiger als »kein Algorithmus leistet X«.
   - **B3** ✓ **Der Lifting-Rahmen existiert und ist von Menschen bewiesen**: Williams' Satz »nichttrivialer C-SAT-Algorithmus ⟹ untere Schranke gegen C«, gestützt auf das Easy-Witness-Lemma.
   **Folgerung:** Die Leitachse ist damit **nicht widerlegt, sondern geschärft**. P vs. NP *als Ganzes* hat keinen endlichen Suchkern. Aber es gibt einen **nichtleeren Teilbereich der Lower-Bound-Forschung, der einen hat** — und es ist ausgerechnet derjenige, der als einziger die Barrieren durchbrochen hat.
   **Konkrete, überprüfbare Frage an A6/A8, die aus diesem Befund folgt:** Kann KI-gestützte Suche **bessere Circuit-SAT-Algorithmen für eingeschränkte Schaltkreisklassen** finden? Das ist die strukturell *einzige* mir bekannte Stelle, an der KI-Suche und untere Schranken sich mit erfüllten B1, B2 **und** B3 berühren — also die einzige Stelle, an der der AlphaEvolve-Mechanismus überhaupt greifen könnte. **Wichtige Dämpfung:** Das würde Schranken gegen *schwache* Klassen liefern, nicht P ≠ NP; der Abstand aus §3.5b (ACC⁰/NEXP vs. allgemeine Schaltkreise/NP) bleibt unberührt. Es ist ein Forschungsvorschlag, kein Weg zur Lösung.
4. **Fortnow (§5) bestätigt die Achse aus der Gegenrichtung — allerdings mit einem anderen Argument, als die Erstfassung dieses Berichts annahm.** Seine Begründung ist **nicht** »es gibt keinen Ansatz« (dieser Wortlaut ist nicht belegbar, §5.1), sondern eine **Basisraten-Aussage**: Auf jeden sichtbaren KI-Beweiserfolg kommen Hunderte erfolgloser Versuche. Das ist ein *empirischer* Beleg für die Achse statt eines strukturellen — und damit ein schwächerer, aber ehrlicherer. Das strukturelle Argument muss der Bericht selbst tragen: Ein Optimierer braucht ein maschinell prüfbares Erfolgskriterium (B2); für die universelle Aussage »kein Algorithmus leistet X« gibt es keines, und B3 fehlt, weil kein Lifting-Rahmen bekannt ist. `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]`
5. **Optiland (§7.3) erklärt, warum die Achse leicht übersehen wird.** Die praktischen Erfolge bei suchbaren Problemen sind real und sichtbar; die Nicht-Bewegung bei der universellen Frage ist unsichtbar. Der Eindruck von Fortschritt entsteht aus der Verwechslung beider.

**Zusammenfassend: kein Widerspruch, aber eine Präzisierung in zwei Richtungen.** `[EIGENE EINSCHÄTZUNG, Konfidenz hoch]`
- **Verschärfend:** Auch die P=NP-Richtung ist kein KI-taugliches Suchproblem, weil die *Verifikation* universell ist (Punkt 1). Die Achse trifft beide Richtungen, nicht nur die untere Schranke.
- **Abschwächend:** Innerhalb der Lower-Bound-Forschung existiert ein Teilbereich **mit** endlichem Suchkern und **mit** menschlich bewiesenem Lifting-Rahmen — die algorithmische Methode (Punkt 3). Wer die Achse als »KI kann hier prinzipiell nichts beitragen« liest, überdehnt sie. Die korrekte Lesart ist: **KI kann dort beitragen, wo B1–B3 zugleich erfüllt sind; für P vs. NP als Ganzes sind sie es nicht, für einen schmalen und interessanten Randbereich schon.**

---

## 9. Was ich NICHT verifizieren konnte

Vollständige Liste der offenen Punkte. **Fett** markiert sind die Punkte, die sich gegenüber der Erstfassung dieses Berichts **verändert** haben.

**Aufgelöst (nicht mehr offen):**
- **Publikationsjahr von Allenders *Status Report*: 2009.** Eine dritte gezielte Suche liefert übereinstimmend 2009 (dblp Bd. 77, ScienceDirect, Rutgers). Der Widerspruch 2008/2009 ist erledigt.
- **Aufschlüsselung der Umfrage 2002: 61 für P ≠ NP (davon 7 mit Zweifeln), 9 für P = NP.** Über zwei unabhängige Suchen konsistent. Offen bleibt nur die Aufteilung der restlichen 30.
- **»Williams umgeht alle drei Barrieren«.** Jetzt mit Mechanismus belegt (§3.5b): Verbesserte SAT-Algorithmen brechen zusammen, sobald man Orakel oder deren algebraische Fortsetzungen hinzufügt — daher kann ein darauf gestützter Beweis weder relativieren noch algebrisieren. Konfidenz hoch. **Einschränkung:** Die offizielle Gödel-Preis-Laudatio 2024 nennt laut Suchsynthese ausdrücklich nur Relativization und Natural Proofs; die Algebrization-Aussage stammt aus einer zweiten Quelle.
- **Williams 2025 (√-Platz-Simulation).** Der Lead der Leitung ist bestätigt (ECCC TR25-017, STOC 2025).

**Weiterhin offen:**
1. **Die Differenz 80 % vs. 88 % (Gasarch 2019) — ungelöst, und die frühere »plausible Auflösung« wurde zurückgezogen** (§4.2(2)): Eine Suchsynthese nennt für 2019 ausdrücklich **null** Abstentionen, was die Erklärung »alle Befragten vs. nur Meinungsäußernde« arithmetisch unmöglich macht. Beide Zahlen bleiben im Bericht, ohne Auflösung.
2. **Die genaue Zahl der P≠NP-Stimmen 2012.** Synthesen nennen 81 %, 82 % (»125 von 152«) und »etwa 80 %«. Konfidenz niedrig-mittel.
3. **Die Jahreszuordnung der »99 % unter Expert:innen«.** Dieselbe Kennzahl wurde in verschiedenen Suchen sowohl 2012 als auch 2019 zugeschrieben (§4.2(3)).
4. **Der exakte Wortlaut** sämtlicher Zitate (Fortnow, Aaronson, Gasarch). Kein Volltextzugang. Für Fortnows »No, it isn't« / »by man or machine« gilt das verschärft: Sinngehalt mittel-hoch, **Wortlaut niedrig** — nach dem Vorfall in §5.1 ist bei genau dieser Quelle besondere Zurückhaltung geboten.
5. **Das genaue Datum des Fortnow-Posts** (10. oder 14. Juni 2026; zwei Synthesen widersprechen sich). Gesichert ist nur »Juni 2026«.
6. **Ob der Wortlaut »we don't even have a viable approach« irgendwo existiert.** Ich konnte ihn in zwei Anläufen nicht reproduzieren und weise ihn mit A7 zurück; ein Abwesenheitsbeleg bleibt prinzipiell schwächer als ein Positivbefund. Die Konsequenz ist gleichwohl eindeutig: **nicht zitieren.**
7. **Die Inhalte** von Allenders und Aaronsons Übersichtsartikeln. Nur Existenz und Venue verifiziert.
8. **Die exakten Satzformulierungen und Quantoren der Algebrization-Barriere.** Mechanismus (Low-Degree-Extension als erweitertes Orakel) und Aussagegehalt (»P vs. NP erfordert nicht-algebrisierende Techniken«) sind verifiziert; die formale Satzfassung nicht.
9. **Die Inhalte der 2025/2026er Preprints** (arXiv:2606.12631, arXiv:2601.09702, arXiv:2511.14038, arXiv:2508.14831, arXiv:2606.27139). Nur Existenz, Titel und Abstract-Synthese.
10. **Aaronsons Übersichtsartikel »P =? NP«: Jahr 2016 oder 2017?** Der Auftrag nannte 2017, die Springer-Buchpublikation ist 2016 datiert. Ich habe die Buchangabe übernommen; Manuskript- und Preprint-Fassungen kursieren mit abweichenden Jahren. Geringe Relevanz, aber nicht geglättet.

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

**Neu im zweiten Durchgang verifiziert**
- R. Williams: *Simulating Time With Square-Root Space*, ECCC TR25-017, Februar 2025; STOC 2025 — https://eccc.weizmann.ac.il/report/2025/017/ · https://people.csail.mit.edu/rrw/time-vs-space.pdf
- J. Li, T. Yang: *3.1n − o(n) Circuit Lower Bounds for Explicit Functions*, STOC 2022; ECCC TR21-023 — https://eccc.weizmann.ac.il/report/2021/023/
- R. R. Williams: *Complexity Lower Bounds from Algorithm Design*, eingeladener Beitrag LICS 2021 — https://people.csail.mit.edu/rrw/LICS21.pdf
- Gödel-Preis 2024, offizielle Laudatio (SIGACT) — https://sigact.org/prizes/g%C3%B6del/citation2024.html
- R. Williams: *Nonuniform ACC Circuit Lower Bounds*, JACM 61(1), Art. 2, 2014 — https://dl.acm.org/doi/10.1145/2559903 · Konferenzfassung https://www.cs.cmu.edu/~ryanw/acc-lbs.pdf
- S. Aaronson, A. Wigderson: *Algebrization: A New Barrier in Complexity Theory* — https://www.scottaaronson.com/papers/alg.pdf · ToCT-Fassung https://dl.acm.org/doi/pdf/10.1145/1490270.1490272
- E. Allender: *A Status Report on the P versus NP Question*, Advances in Computers 77, Kap. 4, S. 117–147, **2009** — https://people.cs.rutgers.edu/~allender/papers/advances.in.computing.pdf
- L. Fortnow: *Fifty Years of P vs. NP and the Possibility of the Impossible*, CACM 65(1), 2022 — https://lance.fortnow.com/papers/files/pvnp50.pdf
- Cook, Mertz: platzsparender Tree-Evaluation-Algorithmus, STOC 2024 (Grundlage von Williams 2025) `[NUR-SNIPPET]`

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
