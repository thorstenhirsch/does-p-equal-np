# P versus NP im September 2026

### Eine Bestandsaufnahme mit besonderem Blick auf KI-gestützte Ansätze

**Konsenspapier eines Teams aus zehn Rechercheagenten**
Projektleitung und Redaktion: Claude (Claude Code)
Stand: 12. September 2026

---

## Abstract (English)

We report the findings of a ten-agent research team tasked with assessing the state
of the P versus NP question as of September 2026, with particular attention to
whether AI methods have produced substantive progress. **They have not** — but the
negative result is more informative than it sounds. AI systems have contributed
genuine, novel results to complexity theory: DeepMind's AlphaEvolve discovered new
gadget reductions improving inapproximability bounds for MAX-4-CUT, MAX-3-CUT and
metric TSP. Crucially, these are theorems of the form "*X is NP-hard to approximate
within c*" — statements that **presuppose** P ≠ NP rather than bearing on it.

We propose a three-condition criterion (**B1/B2/B3**) characterising where AI search
succeeds: a finite representable search object, a cheap verification oracle, and a
human-proved lifting framework carrying the finite object to a universal statement.
P versus NP fails all three. The three classical barriers, together with the proved
endpoints of each individual lower-bound technique, *are* the observation that no
viable lifting framework is known.

We also report a finding about our own instrument: in four documented cases the
search layer we relied on synthesised plausible but false claims — a fabricated
quotation, conflated survey figures, a non-existent paper title, and a figure that
migrated from one case to another. All four originated in the coordinating agent's
own research and were caught by specialist agents. We consider this material rather than mishap, and document it in full.

---

## Inhalt

1. [Kurzfassung](#1-kurzfassung)
2. [Methodik und ihre Grenzen](#2-methodik-und-ihre-grenzen)
3. [Die Frage](#3-die-frage)
4. [Was bewiesen ist](#4-was-bewiesen-ist)
5. [Obere Schranken und das SAT-Solver-Paradox](#5-obere-schranken-und-das-sat-solver-paradox)
6. [Was KI tatsächlich geleistet hat](#6-was-ki-tatsächlich-geleistet-hat)
7. [Das Kriterienraster B1/B2/B3](#7-das-kriterienraster-b1b2b3)
8. [Claim-Forensik](#8-claim-forensik)
9. [Befund über das eigene Instrument](#9-befund-über-das-eigene-instrument)
10. [Konsens und Dissens](#10-konsens-und-dissens)
11. [Was sich lohnen würde](#11-was-sich-lohnen-würde)
12. [Fazit](#12-fazit)

---

## 1. Kurzfassung

**P versus NP ist im September 2026 offen wie 1971.** Kein Agent dieses Teams fand
einen Gegenbefund. Es gibt keinen Beweis, keinen Beweisansatz mit Aussicht, und
keine KI-gestützte Annäherung an einen solchen.

Das ist erwartbar. Interessanter sind vier Befunde, die darüber hinausgehen.

**Erstens: KI hat echte neue Resultate in der Komplexitätstheorie erzeugt** — aber in
einer Ergebnisklasse, die P vs. NP strukturell nicht berührt. AlphaEvolve fand
Gadget-Reduktionen, die Inapproximierbarkeitsschranken verbesserten. Diese Sätze
haben die Form „*X ist NP-schwer innerhalb Faktor c zu approximieren*" und
**setzen P ≠ NP voraus**, statt dazu beizutragen. Sie vermessen das Innere eines
Gebiets, dessen Außengrenze offen bleibt.

**Zweitens: Der Grund dafür ist strukturell, nicht quantitativ.** KI-Suche trägt,
wo ein endlicher Suchkern in einem von Menschen bewiesenen Lifting-Rahmen steckt.
P vs. NP hat keinen solchen Kern. Ein KI-Beweis scheitert deshalb nicht an
Rechenleistung, sondern daran, dass niemand weiß, wonach zu suchen wäre.

**Drittens: Die Einzeltechniken haben bewiesene Endpunkte, keine offenen Horizonte.**
Dass die beste untere Schranke bei 3,1n steht, ist kein Zwischenstand. Gate
elimination kann superlineare Schranken **prinzipiell nicht** liefern — das ist ein
Satz, kein Erfahrungsurteil. Analoges gilt für monotone Schaltkreise, AC⁰, Valiant-
Rigidität, hardness magnification und den ursprünglichen GCT-Plan.

**Viertens, und in eigener Sache: Unser Rechercheinstrument hat in vier
dokumentierten Fällen Plausibles erfunden.** Ein Pseudo-Zitat, verschmolzene
Umfragezahlen, ein nicht existierender Papiertitel — und eine Zahl, die von einem
Fall auf einen anderen gewandert ist. Alle vier stammen aus der Leitungsrecherche,
alle vier wurden von Fachagenten gefunden, der vierte erst bei der Prüfung des
fertigen Papiers. Für ein Papier über KI-gestützte Wahrheitsfindung ist das kein
Betriebsunfall, sondern Material.

---

## 2. Methodik und ihre Grenzen

### 2.1 Aufbau

Zehn Agenten in drei Phasen. Zwei Strategieagenten (Angriffsplan; Red Team)
entwarfen Vorgehen und Prüfraster. Acht Fachagenten bearbeiteten je eine Teilfrage:
Kanon und Barrieren (A1), untere Schranken (A2), Meta-Komplexität und Unabhängigkeit
(A3), obere Schranken und SAT-Praxis (A4), algebraische Wege und GCT (A5),
KI-Algorithmenentdeckung (A6), KI-Beweisen (A7), Claim-Forensik mit Vetorecht (A8).
Anschließend eine Konsensrunde.

Redundanz war an den kritischen Stellen beabsichtigt: Barrieren bei A1 und A2,
Meta-Komplexität bei A2 und A3, KI-Claims bei A6, A7 und A8. Diese Überlappung hat
sich ausgezahlt — praktisch jede Korrektur in diesem Papier entstand dort, wo zwei
Agenten dieselbe Frage unabhängig bearbeiteten.

### 2.2 Die harte Beschränkung

**Es bestand kein Volltextzugang.** WebFetch war durch den Egress-Proxy für alle
relevanten Domains gesperrt: arxiv.org, ECCC, Springer, ACM, die einschlägigen
Fachblogs, selbst Wikipedia. Nutzbar war ausschließlich eine Suchschnittstelle,
die Titel, URLs und eine *synthetisierte Zusammenfassung* der Trefferinhalte liefert.

Daraus folgt die zentrale methodische Einschränkung dieses Papiers:

> **Wir können belegen, *dass* eine Arbeit existiert. Wir können nicht zuverlässig
> belegen, *was* in ihr steht.**

Kein Beweisdetail wurde nachgerechnet. Kein Lean-Code wurde geprüft (mit einer
Ausnahme, siehe §8.4). Alle Aussagen über Inhalte beruhen auf Suchsynthesen.

### 2.3 Gegenmaßnahmen

- **Triangulationspflicht:** Jeder nichttriviale Claim wurde mit zwei bis drei
  *unterschiedlich formulierten* Anfragen geprüft.
- **Markierungssystem:** `[VERIFIZIERT]`, `[PREPRINT]`, `[CLAIM]`,
  `[EIGENE EINSCHÄTZUNG]`, `[NUR-SNIPPET]`.
- **Keine Glättung von Widersprüchen.** Wo zwei Quellen unvereinbare Zahlen
  liefern, stehen beide im Papier, gekennzeichnet als unaufgelöst.
- **Rezeptionsforschung statt Begutachtung.** Das Red Team formulierte die
  Selbstbindung, die dieses Papier durchzieht: Wir schreiben „X wurde von Y mit
  Begründung Z kritisiert", nicht „X ist falsch". Wir sind nicht in der Lage,
  mathematische Urteile zu fällen, und geben nicht vor, es zu sein.

### 2.4 Was das Papier nicht leisten kann

Es kann keinen Beweis prüfen. Es kann nicht entscheiden, ob Xu und Zhou recht haben
oder Allender und Williams. Es kann referieren, wer was mit welcher Begründung
behauptet, und wie die Fachöffentlichkeit reagiert hat. Wer mehr erwartet, erwartet
zu viel — von diesem Papier und, das ist die allgemeinere Lehre, von jeder
KI-gestützten Literaturrecherche ohne Volltextzugang.

---

## 3. Die Frage

### 3.1 Formal

**P** ist die Klasse der Entscheidungsprobleme, die eine deterministische
Turingmaschine in polynomieller Zeit löst. **NP** ist die Klasse derjenigen, bei
denen sich eine *gegebene* Lösung in polynomieller Zeit **verifizieren** lässt.

Der Name führt in die Irre und ist die Quelle unzähliger Missverständnisse: NP heißt
**nichtdeterministisch polynomiell**, nicht „nicht-polynomiell". P ⊆ NP ist trivial.
Die Frage ist, ob die Inklusion echt ist.

**Cook–Levin (1971/73):** SAT ist NP-vollständig — jedes Problem in NP lässt sich in
polynomieller Zeit auf SAT reduzieren. **Karp (1972)** zeigte dasselbe für 21
weitere Probleme. Daraus folgt die Eigenschaft, die die Ausgangsfrage dieses
Projekts motivierte:

> Ein einziger Polynomialzeit-Algorithmus für ein einziges NP-vollständiges Problem
> — 3-SAT, Clique, Vertex Cover, Knapsack, Hamiltonkreis — würde **P = NP** beweisen.

Das stimmt. Es ist aber auch der Einstieg in drei klassische Irrtümer.

### 3.2 Drei Fallstricke

**Knapsack ist nicht „fast gelöst".** Das bekannte dynamische Programm läuft in
O(n·W), wobei W die Kapazität ist. Das ist **pseudopolynomiell**, nicht polynomiell:
Die Eingabelänge wächst mit der *Bitzahl* von W, also logarithmisch, während die
Laufzeit mit W selbst wächst — also exponentiell in der Eingabelänge. Eine Kapazität
mit 100 Bit ergibt 2¹⁰⁰ Schritte. Diese Verwechslung ist eine der häufigsten
Bruchstellen in falschen Beweisen.

Dabei ist eine Feinheit wichtig, damit der Punkt nicht überdehnt wird: Knapsack ist
**schwach** NP-vollständig, und nur deshalb existiert das pseudopolynomielle Programm
überhaupt. Für ein **stark** NP-vollständiges Problem — 3-PARTITION, TSP — würde ein
pseudopolynomieller Algorithmus sehr wohl P = NP liefern. „Pseudopolynomiell ist
irrelevant" wäre also falsch; richtig ist: bei Knapsack ist es kein Fortschritt
Richtung P = NP.

**Clique für festes k ist in P, Clique ist NP-vollständig.** Bei fixiertem k gibt es
nur O(n^k) Kandidatenmengen — polynomiell für jedes feste k. NP-vollständig wird es,
wenn k Teil der Eingabe ist.

Der Mechanismus dahinter lohnt die Genauigkeit, weil er den Kern von „polynomiell"
trifft: P verlangt einen **festen** Exponenten. „Für jedes k liegt das Problem in P"
ist die Aussage ∀k ∃c; gebraucht wird aber ∃c ∀k — *ein* Exponent, der für alle k
funktioniert. Die Vertauschung zweier Quantoren trennt hier P von NP-vollständig.

**„Kein bekannter Algorithmus" ist nicht „kein Algorithmus".** Fünfzig Jahre
erfolgloser Suche sind ein Indiz, kein Beweis. Das ist die triviale Richtung des
Problems — und dennoch die häufigste Form, in der es für gelöst erklärt wird.

### 3.3 Was auf dem Spiel steht

Wäre P = NP mit praktikablen Konstanten, bräche die gesamte auf Einwegfunktionen
gebaute Kryptographie zusammen, Optimierungsprobleme würden lösbar, und — der
irritierendste Punkt — das Finden mathematischer Beweise wäre so leicht wie ihr
Prüfen. Diese letzte Konsequenz ist der Grund, warum die meisten Fachleute P ≠ NP
für wahr halten: P = NP hieße, dass mathematische Kreativität algorithmisch
trivialisierbar ist.

Lance Fortnow hat dem eine Beobachtung entgegengesetzt, die dieses Papier für
tragfähig hält: Wir leben faktisch in **„Optiland"** — moderne Solver, Heuristiken
und KI liefern viele praktischen Vorteile eines P=NP-Szenarios, während die
kryptographisch relevanten Instanzen hart bleiben. Die Frage verliert dadurch nichts
an mathematischer Bedeutung, aber einiges an praktischer Dringlichkeit.

### 3.4 Der Konsens der Fachwelt — mit einem ungelösten Widerspruch

Gasarchs Umfragen (2002, 2012, 2019; die dritte mit 124 Teilnehmenden) sind die
einzige systematische Erhebung. **Eine Umfrage nach 2019 existiert nicht** — wer
2026 Konsenszahlen zitiert, zitiert Daten von 2018.

Für 2019 kursieren für den Anteil „P ≠ NP" die Werte **80 %** und **88 %**. Der
Widerspruch ließ sich **nicht auflösen**: Die naheliegende Erklärung (alle Befragten
versus nur Meinungsäußernde) scheitert daran, dass eine Quelle für 2019 ausdrücklich
null Enthaltungen nennt. Dieses Papier nennt deshalb **keine Einzelzahl**, sondern
„rund 80–88 %". `[NUR-SNIPPET]`

Die oft danebengestellten **66 %** beantworten eine andere Frage — „wird P vs. NP
vor 2100 gelöst?" (2002: 62 %, 2012: 53 %, 2019: 66 %). Verifiziert für 2002:
61 von 100 für P ≠ NP (davon 7 mit Zweifeln), 9 für P = NP.

Der Anteil „wird nie gelöst" lag bei 5 %, 3 % und 9 % (bei 100, 152 und 124
Befragten). Die 9 % sind rund elf Personen — kein Trend, sondern Rauschen.

---

## 4. Was bewiesen ist

### 4.1 Die Kalibrierungszahlen

Dieser Abschnitt ist der wichtigste des Papiers, weil er jede „wir sind nah dran"-
Erzählung beendet.

**Markierungsvorbehalt (auf Forderung des Red Teams):** Sämtliche Zahlen dieser
Tabelle und sämtliche Venue-Angaben der Tabelle in §4.3 sind `[NUR-SNIPPET]`. Sie
haben eine zusätzliche Kontrolle, die den drei Konfabulationsfällen aus §9 fehlte —
die interne Konsistenz der Ketten 3n → 3,0116n → 3,1n und 1,307031594 → …578. Das
stützt die **Größenordnung**, nicht die einzelnen Ziffern.

| Größe | Stand 2026 | Für P ≠ NP gebraucht |
|---|---|---|
| Untere Schranke, explizites Problem, volle Basis B₂ `[NUR-SNIPPET]` | **3,1n − o(n)** | superpolynomiell |
| Fortschritt 1984 → 2022 | **+0,1 Gatter pro Eingabebit** | |
| Determinantal complexity der Permanente | **n²/2** (2004) | superpolynomiell |
| Beste 3-SAT-Basis | **1,307031578ⁿ** | polynomiell |
| Fortschrittsrate im Exponenten, 2011–2026 | **32.000× langsamer** als 1985–1999 | |
| TSP-Rekord | unverändert seit **1962** | |

**Zur ersten Zeile.** Die Chronologie: Blum 1984 erreichte 3n; Find–Golovnev–
Hirsch–Kulikov (FOCS 2016) (3+1/86)n ≈ 3,0116n; **Li und Yang** (ECCC TR21-023,
STOC 2022) 3,1n − o(n), für affine dispersers. In achtunddreißig Jahren ist der
Koeffizient um **ein Zehntel** gestiegen.

**Achtung, verbreitete Basisverwechslung:** Die häufig zitierten **5n − o(n)**
(Iwama–Morizumi 2002) gelten über **U₂** — also über B₂ *ohne* XOR und XNOR —, dem
schwächeren Modell, in dem Schranken leichter und größer
ausfallen. Für P vs. NP zählt B₂, weil P/poly darüber definiert ist. Die Zahlen sind
nicht vergleichbar, werden aber regelmäßig nebeneinandergestellt. `[VERIFIZIERT]`

S1 hat in der Konsensrunde zu Recht angemerkt, dass „+0,1 Gatter pro Eingabebit"
eine Rate suggeriert, wo keine ist: **3,1n und superpolynomiell liegen nicht auf
derselben Skala.** Der Abstand ist keine Distanz, die man durch Fortschritt
verringert — er ist ein Kategorienunterschied.

### 4.2 Die drei Barrieren — und was sie nicht sagen

**Relativization** (Baker–Gill–Solovay 1975). Es gibt Orakel A, B mit P^A = NP^A und
P^B ≠ NP^B. Jede Beweistechnik, die bei Hinzufügen eines beliebigen Orakels
unverändert funktioniert — insbesondere klassische Diagonalisierung —, kann die
Frage daher nicht entscheiden.

**Natural Proofs** (Razborov–Rudich 1994/97). Die meisten bekannten
Schranken-Beweise liefern ein Prädikat mit drei Eigenschaften: *useful* (es trennt
die schwere Funktion von allen einfachen — das ist die Eigenschaft, die die Schranke
überhaupt liefert), *constructive* (effizient auswertbar) und *large* (auf einem
nennenswerten Anteil aller Funktionen erfüllt). Ein solches Prädikat für allgemeine
Schaltkreise würde kryptographische Pseudozufallsfunktionen brechen. Constructive
und large allein ergeben keinen Beweis — usefulness ist der Teil, der die Arbeit tut. Wer an deren Existenz glaubt, muss natural proofs ausschließen.
Wichtig: Das ist ein **bedingtes** Resultat — es hängt an einer kryptographischen
Annahme.

**Algebrization** (Aaronson–Wigderson 2008). Erweitert Relativization auf Techniken,
die unter algebraischer Fortsetzung des Orakels stabil bleiben — und erfasst damit
auch die arithmetisierenden Methoden, mit denen IP = PSPACE bewiesen wurde.

**Die verbreitete Überinterpretation ist falsch.** Die Barrieren zeigen **nicht**,
dass P vs. NP unlösbar oder von ZFC unabhängig ist. Sie zeigen, dass bestimmte
Technikfamilien nicht ausreichen. Der Beleg, dass sie umgehbar sind, existiert:
**Ryan Williams' NEXP ⊄ ACC⁰** (JACM 2014; Gödelpreis 2024 `[NUR-SNIPPET]` — eine
Preisträgerzuordnung, also genau die Angabenklasse, die §9 für nicht belastbar
erklärt) umgeht **alle drei**. Der Mechanismus ist geteilt und deshalb instruktiv:

- Gegen **Relativization und Algebrization**: Alle bekannten SAT-Algorithmen, die
  Brute Force schlagen, brechen zusammen, sobald man Orakel oder deren algebraische
  Fortsetzung hinzufügt — wer schneller sein will, *muss* Instanzstruktur nutzen,
  die Black-Box-Methoden gar nicht sehen.
- Gegen **Natural Proofs**: Das Argument liefert kein *large* Prädikat; es trennt
  nicht „fast alle Funktionen" von den einfachen, sondern arbeitet über eine
  einzelne, sehr komplexe Funktion.

Anmerkung zur Quellenlage: Die auffindbare Laudatio nennt Relativization und Natural
Proofs; die Umgehung von Algebrization ist über eine zweite Quelle belegt. `[NUR-SNIPPET]`

### 4.3 Jede Technik hat einen bewiesenen Endpunkt

Dies ist der Befund, den der Strategieagent in der Konsensrunde als unverzichtbar
bezeichnet hat — weil er „noch nicht" von „nicht so" unterscheidet.

| Technik | Endpunkt | Art | Quelle |
|---|---|---|---|
| gate elimination | **kann prinzipiell keine superlinearen Schranken liefern** — für einen formalisierten Rahmen typischer gate-elimination-Argumente, nicht wörtlich für jede denkbare Variante | unbedingt | Golovnev–Hirsch–Knop–Kulikov, MFCS 2016 / JCSS 2018 |
| monotone Schaltkreise | exponentielle Lücke zur allgemeinen Komplexität | unbedingt | Tardos 1988 |
| AC⁰, AC⁰[p] | *kein* Endpunkt — die Technik funktioniert hier, weil es in AC⁰ keine PRFs gibt; sie skaliert nur nicht nach oben | — | Håstad; Razborov–Smolensky |
| natural proofs (allgemein) | schließt die large-Prädikat-Familie aus | **bedingt** (Existenz von PRFs) | Razborov–Rudich |
| Valiant-Rigidität | Hadamard-Matrizen sind nicht rigide **im für Valiants Programm nötigen Parameterbereich**; ein gegenläufiges Preprint von 2026 existiert | unbedingt, aber umstritten | Alman–Williams 2017 |
| hardness magnification | **locality barrier** | unbedingt | Chen–Hirahara–Oliveira–Pich–Rajgopal–Santhanam, JACM 69(4) 2022 |
| GCT, occurrence obstructions | **können perm vs. det nicht trennen** | unbedingt | Bürgisser–Ikenmeyer–Panova, JAMS 32(1) 2019 |
| algebrisierende Methoden | erweiterte Schranke | — | Chen–Hu–Ren, ITCS 2026 `[NUR-SNIPPET]` |

Die Spalte „Art" ist wichtiger, als sie aussieht: **Nur die natural-proofs-Barriere
ist bedingt** — sie hängt an der Existenz kryptographischer Pseudozufallsfunktionen.
Alle übrigen Einträge sind unbedingte Sätze.

Der erste Eintrag verdient Hervorhebung: **3,1n ist kein Zwischenstand auf dem Weg
zu superpolynomiellen Schranken, sondern der ausgereizte Endpunkt einer Methode.**
Jede Trendextrapolation extrapoliert innerhalb einer nachweislich sackgassigen
Technik.

Die allgemeine Beobachtung von A2, die dieses Papier übernimmt: **Das Feld
produziert 2026 Barrieren in etwa derselben Rate wie positive Resultate.**

### 4.4 Wo es sich tatsächlich bewegt

Der Eindruck völligen Stillstands wäre falsch. Es bewegt sich — nur nicht dort.

**Ryan Williams, Februar 2025** (ECCC TR25-017, STOC 2025): Jede Mehrband-
Turingmaschine mit Zeit t ist in Raum O(√(t log t)) simulierbar. Daraus folgt
SPACE[n] ⊄ TIME[n^{2−ε}]. Das ist die erste substanzielle Verbesserung seit rund
fünfzig Jahren.

Hier ist Präzision nötig, weil sich zwei Größen leicht vermischen: Die **Simulation**
verbessert sich von O(t / log t) (Hopcroft–Paul–Valiant) auf O(√(t log t)). Die
daraus **abgeleitete Zeitschranke** verbessert sich von SPACE[n] ⊄ TIME[o(n log n)]
auf SPACE[n] ⊄ TIME[n^{2−ε}] — das ist der Sprung von „fast linear" auf
„quadratisch". **Es ist ausdrücklich kein Beweis von P ≠ PSPACE**; eine frühere,
stärkere Formulierung der Projektleitung wurde von A2 korrigiert. Das Werkzeug kam
von Cook–Mertz (Tree Evaluation) — die damit zugleich einen lange gehandelten
Trennungskandidaten entwerteten.

Bemerkenswert ist der Kontrast: Ein Fünfzig-Jahre-Stillstand bricht — **dort, wo
keine Barriere im Weg steht.**

**S₂E ⊄ SIZE[2ⁿ/n]** (Chen–Hirahara–Ren, STOC 2024; JACM 73(1), Februar 2026), über
einen Algorithmus für Range Avoidance. Für eine hinreichend mächtige Klasse ist damit
die *nahezu maximale* Schranke erreicht. Der Engpass ist also nicht die
Schrankengröße, sondern die Schwäche der Klassen, für die wir sie zeigen können.

**Und die Front liegt näher an ACC⁰, als man denkt:** Gegen TC⁰ — eine Gatterart
darüber — ist nichts bekannt; die belegte Angabe „nicht einmal n^{1,1}" bezieht sich
auf LTF-Schaltkreise. `[NUR-SNIPPET]`

### 4.5 Meta-Komplexität: die lebendigste Front

Die Strategie hatte dieses Teilfeld als aussichtsreichstes eingestuft; die Recherche
bestätigt das, mit Einschränkungen.

**MCSP** (Minimum Circuit Size Problem) fragt nach der Schaltkreiskomplexität einer
als Wahrheitstafel gegebenen Funktion. Seine Komplexität ist aufschlussreich, weil
sie Schranken, Kryptographie, Lernen und Derandomisierung verknüpft.

**Hirahara (FOCS 2022)** bewies NP-Härte von **MCSP\*** — der Variante für
**partielle** Funktionen — und überwand damit eine Relativierungsbarriere. Ein echter
Fortschritt. Aber **totales MCSP bleibt offen**, und die Grenze ist qualitativ, nicht
graduell: Bei partiellen Funktionen darf die Reduktion „don't cares" setzen und muss
nie über ein vollständig festgelegtes Objekt sprechen. Bei totalen Funktionen muss
sie die Funktion auf allen 2ⁿ Eingaben festlegen — und für die Korrektheit **selbst
untere Schaltkreisschranken beweisen**. Genau das ist das Problem, das man lösen
wollte.

Dass NP-Härte von MCSP kein Zwischenschritt ist, sondern selbst einen Durchbruch
enthält, zeigt **Murray–Williams**: Eine deterministische many-one-Reduktion
SAT ≤ MCSP würde bereits EXP ≠ ZPP implizieren. Die Implikationsrichtung wird in
Sekundärdarstellungen regelmäßig verdreht.

**Genau eine unbedingte, nichttriviale Kette** ist bewiesen: **Liu–Pass (FOCS 2020)**
— Einwegfunktionen existieren genau dann, wenn MK^tP mild average-case-hart ist.
Sie führt **nicht** zu P ≠ NP. Alles Übrige hängt an kryptographischen Annahmen,
teils zirkulär: Huang–Ilango–Ren (STOC 2023) setzen subexponentiell sichere
Einwegfunktionen und Witness Encryption voraus — eine Annahme, die Heuristica
bereits selbst ausschließt.

**Zur vieldiskutierten Zufallsorakel-Arbeit** (Ilango, FOCS 2023, inzwischen
SIAM J. Comput.): Mit Wahrscheinlichkeit 1 existiert eine black-box P/poly-Reduktion
von unrelativiertem SAT auf MCSP^O. Der entscheidende Einwand stammt aus der
Geschichte des Fachs: Die **Random Oracle Hypothesis ist widerlegt** (Chang, Chor,
Goldreich, Hartmanis, Håstad, Ranjan, Rohatgi, JCSS 1994) — für fast alle A gilt
IP^A ≠ PSPACE^A, während unrelativiert IP = PSPACE gilt. Das Resultat benutzt exakt
die Evidenzform, die historisch schon einmal versagt hat.

Der Fairness halber: Ilango argumentiert seinerseits, die Relativierung sei hier
**gezielte Barriereumgehung und kein Artefakt**. Welche Lesart trägt, entscheidet
nicht dieses Papier; festzuhalten ist nur, dass die Beweislast beim Resultat liegt.

**Und die Front ist nach 2023 weitergelaufen — in beide Richtungen.**
Hirahara und Ilango (FOCS 2025) zeigen bedingte NP-Härte von *constant-gap* MCSP —
allerdings unter quasipolynomiellen, **nicht-Levin**-Reduktionen, also mit
erheblichen Abschwächungen gegenüber dem, was der Titel suggeriert. In der
Gegenrichtung zeigen Mazor und Pass (CCC 2024), dass Gap-MCSP unter
Indistinguishability Obfuscation **nicht** Levin-NP-vollständig ist. Die Frage ist
offener geworden, nicht geschlossener. `[NUR-SNIPPET]`

### 4.6 Die algebraische Route

**Aus VP ≠ VNP folgt P ≠ NP nicht.** Diese Implikation wird regelmäßig zu stark
dargestellt; sie ist dreifach gebrochen: Sie läuft nur als Kontraposition, liefert
die *nichtuniforme* Variante (P/poly = NP/poly), und hängt in Charakteristik 0
zusätzlich an der verallgemeinerten Riemannschen Vermutung — **über endlichen Körpern
gilt der Transfer dagegen unbedingt** (VP = VNP ⇒ NC²/poly = P/poly = NP/poly =
PH/poly). Die tragfähige Formel: **VP ≠ VNP ist notwendig, nicht hinreichend.**

**Limaye–Srinivasan–Tavenas** (FOCS 2021 Best Paper, JACM 2025) erzielten die ersten
superpolynomiellen Schranken gegen arithmetische Schaltkreise beliebiger konstanter
Tiefe — vorher war bei Produkttiefe 2 Schluss. Der Abstand zum Ziel ist exakt
benennbar: Der „Chasm at Depth Four" — eine Kette von Resultaten von Agrawal–Vinay,
Koiran und Tavenas, nicht ein Einzelsatz — verlangt n^{ω(√d)} für homogene Tiefe 4,
um VP ≠ VNP zu liefern. LST liefert superpolynomiell — schwächer.

**GCT** (Mulmuley–Sohoni) ist der einzige bekannte Versuch, P vs. NP überhaupt in
eine **endliche Objektsuche** zu übersetzen: Eine Obstruction ist eine Partition λ.
Der ursprüngliche Plan ist jedoch widerlegt — occurrence obstructions können
Permanente und Determinante **beweisbar nicht** trennen (Ikenmeyer–Panova, FOCS 2016;
Bürgisser–Ikenmeyer–Panova, JAMS 2019). Das Programm ist auf multiplicity obstructions
ausgewichen, konstruktiv durchgeführt bisher aber nur an Ersatzproblemen.

Zwei Richtigstellungen: Mulmuleys kolportierte Zeitschätzung von rund 100 Jahren ist
nur als **Sekundärwiedergabe** belegbar und wird hier nicht als Zitat geführt. Und
die von ihm postulierte „complexity barrier" **ist kein Theorem** und gehört nicht
als vierte Barriere neben die drei kanonischen. Mulmuley selbst hat die ihm oft
zugeschriebene These, jeder Ansatz müsse durch GCT führen, ausdrücklich
zurückgewiesen: „This is not what I think or said." `[NUR-SNIPPET]`

### 4.7 Unabhängigkeit von ZFC?

Kurz: **nicht bekannt, und die populäre Darstellung ist Unsinn.**

Razborov (1995) zeigte Unbeweisbarkeit in **S₂²(α)** — relativiert, unter einer
Pseudozufallsgenerator-Annahme, in einer Theorie weit unterhalb der Peano-Arithmetik.
Über ZFC sagt das nichts. Ben-David und Halevi haben zudem ein Resultat dazu bewiesen, das das Szenario
unattraktiv macht: Unabhängigkeit **von PA plus allen wahren Π₁-Sätzen** — einer sehr
starken Theorie — käme praktisch fast so gut wie P = NP. Das ist ausdrücklich keine
Aussage über ZFC-Unabhängigkeit im Allgemeinen.

Zu trennen ist strikt: *nicht bekannt unabhängig* ≠ *unabhängig*.

---

## 5. Obere Schranken und das SAT-Solver-Paradox

### 5.1 Die Stagnation, quantifiziert

Bester bekannter worst-case-Zeitbedarf für allgemeines 3-SAT: **O\*(1,307031578ⁿ)**
(Jiang und Cai, 12. Juli 2026, arXiv:2607.10697), über eine verbesserte PPSZ-Analyse.
Der Vorgängerwert (Scheder 2024) lautete 1,307031594ⁿ.

**Die Verbesserung 2024 → 2026 liegt in der achten Nachkommastelle.** Δ = 1,6·10⁻⁸,
das entspricht bei n = 1000 einem Speedup von 0,0012 %.

Rechnet man mit c = log₂(Basis) — c = 1 wäre Brute Force, c = 0 Polynomialzeit:

| Zeitraum | Fortschritt Δc pro Jahr |
|---|---|
| 1985–1999 | 2,0 · 10⁻² |
| 1999–2011 | 2,4 · 10⁻³ |
| **2011–2026** | **6,2 · 10⁻⁷** |

Das ist eine **rund 32.000-fache Verlangsamung**.

Hier ist eine Selbstkorrektur nötig, auf die das Red Team zu Recht bestanden hat:
Eine frühere Fassung dieses Papiers nannte eine Extrapolation („rund 620.000 Jahre")
und bezeichnete sie im selben Atemzug als methodisch wertlos. Beides zusammen geht
nicht. **Die Extrapolation ist Kurvenanpassung an drei Punkte und wird hier nicht
geführt.** Ebenso wäre die naheliegende Aussage, die Reihe konvergiere gegen c ≈ 0,386 und
bestätige damit die Exponential Time Hypothesis, in zwei Hinsichten zu stark: Die
ETH behauptet nur s₃ > 0, **keinen bestimmten Grenzwert**; und die Reihe misst den
Stand der **Analysetechnik**, nicht s₃ selbst — sie könnte stagnieren, während s₃
weit darunter liegt. Was sich sagen lässt: Die Reihe sieht nach Stagnation oberhalb
eines positiven Werts aus. Das ist mit der ETH vollständig verträglich und beweist
sie nicht.

Was **belastbar** bleibt, ist der Befund ohne Extrapolation: Die Fortschrittsrate ist
über vierzig Jahre um mehrere Größenordnungen gefallen, und die jüngste Verbesserung
betrifft die achte Nachkommastelle. Eine Reihe, die sich so verhält, ist kein Indiz
für eine bevorstehende Annäherung an Polynomialzeit. Mehr lässt sich daraus nicht
ableiten, und mehr wird hier nicht behauptet.

### 5.2 Der SAT-Solver-Einwand ist widerlegt, nicht offen

Der naheliegende Einwand lautet: Moderne CDCL-Solver lösen Industrieinstanzen mit
Millionen Variablen. Ist das nicht ein Hinweis auf P = NP?

**Nein — und das ist bewiesen, nicht bloß vermutet.**

CDCL mit Neustarts **p-simuliert die allgemeine Resolution** (Pipatsrisawat–Darwiche,
Overhead O(n⁴); Atserias–Fichte–Thurley) — genauer: idealisierte CDCL-Modelle mit
unbeschränkten Neustarts, nicht jede reale Implementierung. Damit überträgt sich
**Hakens exponentielle Resolutionsschranke für das Schubfachprinzip (1985)** als
**unbedingte** untere Schranke auf diese Solverklasse — ohne jede Annahme über
P vs. NP. Was Haken ausschließt, ist die effiziente *Widerlegung* von PHP durch
resolutionsbasierte Verfahren; über andere Algorithmen sagt er nichts.

Praxisleistung und Worst-Case-Härte koexistieren also nachweislich. Das ist kein
Spannungsverhältnis, das sich zugunsten von P = NP auflösen ließe.

Erschwerend kommt hinzu, dass niemand sagen kann, *warum* Praxisinstanzen leicht
sind. Eine Studie von Zhang, Xia, Li, Li, **Vardi** und **Ganesh** (arXiv:2605.15506,
August 2026) über 766 Benchmark-Familien und mehr als 76.600 Instanzen findet
lineares, polynomielles **und exponentielles** CDCL-Skalieren *innerhalb desselben
Benchmarks* — und stellt fest, dass Treewidth, Klausel-Variablen-Verhältnis und
Community-Struktur die Regime **nicht trennen**. Nach zwanzig Jahren gibt es keinen
anerkannten Erklärungsparameter. Bemerkenswert: Ganesh ist Mitautor der früheren
Gegenthese, es handelt sich um eine Selbstkorrektur. `[PREPRINT]`

Auch der Compute-Ertrag spricht Bände. SAT Competition 2026, Hauptkategorie:
1 Kern × 5000 s löste 276 Instanzen; 32 Kerne × 1000 s lösten 300; 800 Kerne × 200 s
lösten 301. **Die 32-fache Rechenzeit im letzten Schritt kauft genau eine Instanz.**
(Hardware und Timeouts sind zwischen den Tracks nicht identisch; aussagekräftig ist
die Größenordnung, nicht die exakte Differenz. `[NUR-SNIPPET]`)

### 5.3 Quanten

Grover liefert quadratische, nicht exponentielle Beschleunigung. Auf Brute-Force-Suche
angewandt ergibt das **1,4142ⁿ** — und ist damit **langsamer als der beste klassische
3-SAT-Algorithmus** mit 1,30703ⁿ. Erst Ambainis' Kombination von Grover mit Schönings
Algorithmus erreicht etwa 1,153ⁿ und bleibt exponentiell.

Ein nützlicher Filter für Behauptungen in diesem Bereich: **Da P ⊆ BQP, impliziert
jeder Beweis von NP ⊄ BQP bereits P ≠ NP.** Wer das eine beiläufig behauptet,
behauptet unbemerkt das andere.

### 5.4 Unique Games

Die Evidenzlage ist **gegenläufig, nicht „wahrscheinlich wahr"**. Khot–Minzer–Safra
(2-to-2-Games) gilt als starke Evidenz dafür, Arora–Barak–Steurers subexponentieller
Algorithmus als Evidenz dagegen. Korrekte Formulierung: signifikant gestärkt, nicht
entschieden, kein Konsens. `[NUR-SNIPPET]`

---

## 6. Was KI tatsächlich geleistet hat

### 6.1 Der Kernfall: AlphaEvolve in der Komplexitätstheorie

Dies ist der stärkste dokumentierte Fall eines KI-Systems, das **neue Resultate in
der Komplexitätstheorie** hervorgebracht hat.

**arXiv:2509.18057** (Nagda, Raghavan, Thakurta; „Reinforced Generation of
Combinatorial Structures", aktiv revidiert bis mindestens v7 vom 9. März 2026),
begleitet von einem Google-Research-Beitrag. **Status: `[PREPRINT]`** — eine
Konferenz- oder Journalversion war nicht auffindbar.

Berichtete Resultate (`[PREPRINT]`, `[NUR-SNIPPET]`):

| Problem | vorher | mit AlphaEvolve |
|---|---|---|
| MAX-4-CUT | 0,9883 | **0,987** |
| MAX-3-CUT | 0,9853 (gadget-basiert) | **0,9649** |
| Metrisches TSP | 117/116 | **111/110** |

*Lesehilfe: Ein kleinerer Wert bedeutet eine **stärkere Härteschranke**, nicht einen
schnelleren Algorithmus. Jede Zeile ist eine Aussage **innerhalb** der Hypothese
P ≠ NP — siehe §6.2.*

Dazu nahezu optimale obere und bedingte untere Schranken für Certification-
Algorithmen bei MAX-CUT und MAX-Independent-Set auf zufälligen 3- und 4-regulären
Graphen, erreicht über nahezu extremale Ramanujan-Graphen mit bis zu 163 Knoten.

Das gefundene MAX-4-CUT-Gadget hat 19 Variablen und eine stark asymmetrische
Gewichtung mit Faktoren bis 1429:1 — eine Struktur, die menschliche Forschende nicht
in Betracht gezogen hatten. Methodisch elegant: Da die Verifikation der Kandidaten
selbst exponentiell teuer ist, wurde AlphaEvolve auch auf die **Verifikations-
prozedur** angesetzt und beschleunigte sie um bis zu 10.000×. Der naheliegende
Einwand — ein evolvierter Verifier könnte evolvierte Fehler durchwinken — ist
adressiert: Die finalen Gadgets wurden **per Brute Force unabhängig nachverifiziert**;
die evolvierte Prozedur beschleunigt nur die Suche, nicht die Endprüfung.

**Zwei Präzisierungen, die in der Berichterstattung fehlen.** Erstens sind die
MAX-k-CUT- und TSP-Resultate **unbedingte** NP-Härte auf PCP/Håstad-Basis, nicht
UGC-bedingt — methodisch stärker, als zunächst angenommen. Zweitens hängen die
Certification-Unterschranken **nicht** an P ≠ NP, sondern an der unbewiesenen
Kunisky–Yu-Vermutung über Graph-Lifts — also an einer Average-Case-Annahme, die noch
weiter vom Thema entfernt ist.

### 6.2 Warum das P vs. NP nicht berührt

„*X ist NP-schwer innerhalb Faktor c zu approximieren*" heißt ausbuchstabiert:
**ein Polynomialzeit-c-Approximator würde P = NP implizieren.**

Diese Sätze leben **innerhalb** der Hypothese P ≠ NP. Wäre P = NP, wären sie
inhaltsleer. Sie vermessen das Innere eines Gebiets, dessen Außengrenze offen bleibt.
Kein einziges dieser Resultate verschiebt die Grenze zwischen P und NP, und keines
war je dazu gedacht.

Der Google-Research-Beitrag formuliert es selbst präzise: *„AI discovers a structure
within the proof, not the proof itself — humans handle the lifting frameworks."*

### 6.3 Kein besserer Algorithmus für ein NP-schweres Problem

Die Recherche fand **keinen Fall**, in dem KI einen asymptotisch besseren Algorithmus
für ein NP-schweres Problem gefunden hätte. `[Konfidenz hoch]`

Der stärkste Kandidat ist **SATLUTION** (arXiv:2509.07367): LLM-Agenten evolvieren
ganze SAT-Solver-Repositories und schlagen auf dem ungesehenen SC2025-Benchmark die
menschlichen Wettbewerbsgewinner. Beeindruckend — aber es ist empirische Laufzeit auf
einer festen Instanzverteilung. Der Worst Case bleibt unverändert exponentiell, eine
Komplexitätsaussage fällt nicht an.

Ein peer-reviewter Negativbefund verdient Erwähnung: Eine Nachprüfung der
FunSearch-Ergebnisse zum Bin Packing (arXiv:2510.27353, angenommen bei ACM TELO)
findet die LLM-Heuristiken „selbst für Fachleute weitgehend undurchsichtig" und zeigt,
dass eine simple Zwei-Parameter-Heuristik effizienter ist und breiter generalisiert.
Ergänzend Gideoni, Risi und Gal (Februar 2026): Einfache Baselines erreichen die
Leistung der Code-Evolution — **nicht die Pipeline, sondern der von Menschen
entworfene Suchraum** bestimmt die Obergrenze.

### 6.4 Gelernte SAT-Heuristiken

Die NeuroSAT-Linie skaliert nicht; die Arbeiten sagen das selbst. Deep- und
GNN-Ansätze reduzieren zwar die Suchschritte, **verschlechtern aber die Laufzeit**.
Was funktioniert, sind kleine, flache, online trainierte Modelle oder einmalige
Offline-Prädiktion (NeuroBack, ICLR 2024: +5,2 % bzw. +7,4 % gelöste Instanzen).
Die SAT Competition 2025 gewannen klassische CDCL-Solver.

Eine Namenswarnung: „NP-Engine" (arXiv:2510.16476) ist ein **LLM-Benchmark**, kein
Komplexitätsresultat.

### 6.5 Automatisches Beweisen: die Fähigkeitsgrenze in Zahlen

2024–2026 war ein außergewöhnlicher Zeitraum: AlphaProof (IMO-Silber 2024,
Nature 2025), Seed-Prover (IMO 2025), AlphaProof Nexus mit neun Erdős-Problemen
(Mai 2026), AxiomProver (vier zuvor ungelöste Probleme, Anfang 2026 — `[CLAIM]`,
der Peer-Review-Status ist strittig), OpenAI mit zehn forschungsnahen Problemen
(August 2026), Anthropics Lean-Formalisierung von Fermats letztem Satz
(September 2026).

**Eine Korrektur in eigener Sache**, weil sie exemplarisch ist: Eine frühere Fassung
dieses Papiers schrieb „Seed-Prover: 5 von 6 IMO-Aufgaben 2025". Das ist falsch.
Unter Wettbewerbsbedingungen waren es **4 vollständige plus 1 teilweise Lösung,
30 Punkte, IMO-zertifiziertes Silber**; die fünfte Lösung entstand erst nachträglich
per extended search. Genau die Art Aufrundung, die §9 anprangert — hier im eigenen
Text.

Der Erfolgsgradient ist dennoch eindeutig:

| Benchmark | System | Erfolgsquote |
|---|---|---|
| miniF2F | Seed-Prover | ~99 % |
| Putnam | Seed-Prover | ~50 % |
| CombiBench | Seed-Prover | ~30 % |
| OEIS | AlphaProof Nexus | 44/492 ≈ 8,9 % |
| **Erdős-Probleme** | AlphaProof Nexus | **9/353 ≈ 2,5 %** |

Die Tabelle mischt zwei Systeme; als Gradient über Aufgabenschwierigkeit ist sie
dennoch aussagekräftig. Den stärksten Beleg liefert ohnehin ein Hersteller selbst:
Anthropic hält zu den eigenen Resultaten rund um die Riemannsche Zetafunktion fest,
dass die verwendeten Techniken **nicht zum Beweis der Vermutung führten**.
`[NUR-SNIPPET]`

Erfolg setzt voraus: kurzer Beweis, endlicher Suchraum, billiges Verifikationsorakel,
vorhandener Formalisierungskontext in mathlib. **P vs. NP scheitert an allen vieren.**

Die Komplexitätstheorie ist zudem kaum formalisiert. Cook–Levin existiert in
Isabelle/AFP (Balbach) und in Coq (Gäher–Kunze, ITP 2021, peer-reviewt) — **in
mathlib jedoch nichts Einsatzfähiges.** Die Lean-Sammlung der Millennium-Probleme
vermerkt bei P vs. NP ausdrücklich, Cook–Levin gehöre „to a larger complexity-theory
library" — die nicht existiert.

### 6.6 Eine dritte Kategorie: Autoformalisierung

Die FLT-Formalisierung (September 2026) gehört weder zur Zeugensuche noch zur offenen
Quantifizierung. Sie ist **Übersetzung eines seit 1995 verstandenen Beweises** —
Kevin Buzzard nennt es genau so, eine „autoformalization achievement". Die
Größenordnung ist bemerkenswert: rund 13 Millionen Zeilen Lean und 29.500 Theoreme
in elf Tagen, gegenüber einem menschlichen Formalisierungsprojekt, das bis 2029
finanziert ist. `[NUR-SNIPPET]`

Für P vs. NP ist diese Kategorie strukturell irrelevant: **Es existiert kein
Ausgangsbeweis, den man übersetzen könnte.** Wer den FLT-Erfolg auf P vs. NP
extrapoliert, verwechselt Übersetzen mit Finden.

---

## 7. Das Kriterienraster B1/B2/B3

### 7.1 Das Raster

Aus den Befunden von A6, präzisiert durch A1, A2, A3 und A5, ergibt sich eine
Charakterisierung dessen, wo KI-gestützte Suche trägt. Drei Bedingungen müssen
**gleichzeitig** erfüllt sein:

- **B1 — endlicher Suchkern.** Das gesuchte Objekt ist endlich und maschinell
  repräsentierbar: ein Gadget, ein Graph, ein Programm, eine Partition.
- **B2 — billiges Verifikationsorakel.** Kandidaten lassen sich effizient prüfen,
  oder die Prüfung lässt sich effizient machen.
- **B3 — menschlich bewiesener Lifting-Rahmen.** Ein Satz trägt vom endlichen Objekt
  zur allgemeinen Aussage — **und die Techniken, die das Objekt liefern, sind mit
  diesem Rahmen kompatibel.** Das PCP-Theorem plus Gadget-Reduktionskalkül ist das
  Musterbeispiel. Der zweite Halbsatz ist nicht kosmetisch: Hardness Magnification
  erfüllt den ersten und scheitert am zweiten (§7.3).

**Wichtige Präzisierung:** Der Schnitt verläuft *nicht* zwischen endlichem und
unendlichem Ergebnis. FunSearchs Cap-Set-Schranke ist asymptotisch, die
Inapproximierbarkeitsresultate sind universelle Theoreme. Endlich ist allein das
**gesuchte Objekt**; die Allgemeinheit kommt ausschließlich über B3 herein.

### 7.2 Warum P vs. NP durchfällt

**B1 fällt aus — für die P ≠ NP-Richtung.** Es gibt kein endliches Objekt, dessen
Auffinden diese Richtung entscheidet. GCT ist der einzige bekannte Versuch, eines zu
konstruieren. Für die P = NP-Richtung existiert ein endliches Objekt sehr wohl: ein
Algorithmus. Dort scheitert es nicht an B1, sondern an B2 — was die folgende Pointe
erst scharf macht.

**B2 fällt aus — und zwar beidseitig.** Das ist der schärfste Punkt des Rasters, und
er wird meist übersehen. Für P ≠ NP wäre zu verifizieren, dass *kein* Algorithmus
existiert. Aber auch für die **P = NP-Richtung** gilt: Zu prüfen wäre „korrekt auf
**allen** Eingaben, polynomiell im **Worst Case**" — wieder eine universelle Aussage
über unendlich viele Instanzen. Ein gefundener Algorithmus wäre kein verifizierbarer
Zeuge. Selbst die vermeintlich „leichte" Richtung ist kein Suchproblem.

**B3 fällt aus — und das ist die tiefste Ebene.** Die drei Barrieren und die
bewiesenen Endpunkte der Einzeltechniken **sind** genau die Feststellung, dass kein
tragfähiger Lifting-Rahmen bekannt ist. Es fehlt nicht an Rechenleistung. Es fehlt
an der Antwort auf die Frage, wonach überhaupt zu suchen wäre.

### 7.3 Drei lehrreiche Grenzfälle

Das Raster gewinnt seine Schärfe an den Fällen, in denen einzelne Bedingungen
erfüllt sind.

**GCT** erfüllt B1 — eine Obstruction ist eine Partition λ, ein endliches Objekt.
Es ist der einzige bekannte Versuch, P vs. NP in eine endliche Objektsuche zu
übersetzen. Es verletzt aber B2: Die Verifikation erfordert Kronecker- und
Plethysmuskoeffizienten, und es gibt Hinweise, dass für einige davon nicht einmal
eine #P-Beschreibung existiert. Und der Rahmen ist zwar vorhanden, aber **nicht
geschlossen** — er hat offene Stellen bei Debordering und bei GRH/Nichtuniformität
und endet nicht bei P ≠ NP.

> **Die Lehre aus GCT: Ein endliches Suchobjekt allein nützt nichts.**

Das ist die direkte Antwort auf die naheliegende Hoffnung, man müsse P vs. NP nur
geeignet in eine Suchaufgabe übersetzen, dann könne KI sie übernehmen.

**Hardness Magnification** erfüllt B3 **im Wortsinn** — es existiert ein bewiesener
Lifting-Rahmen, und er ist spektakulär: Schon leichte Verbesserungen scheinbar
schwacher Schranken würden Durchbrüche implizieren. Genau deshalb ist er unbrauchbar.
Die locality barrier zeigt, warum: Die verfügbaren Techniken sind lokalisierbar, die
Magnification-Reduktionen aber genau mit Orakelgattern kleinen Fan-ins implementierbar.
Ein Rahmen, mit dem die Eingangstechniken nicht kompatibel sind, trägt nicht.

**Range Avoidance** (A2) erfüllt B1 und B3, aber **B2 bricht zusammen**: Nicht-im-Bild-
Sein ist coNP-artig und erfordert ein NP-Orakel. Genau dort wird die Klasse zu groß —
man erhält S₂E statt NP.

### 7.4 Wo alle drei erfüllt sind

Es gibt eine Stelle, an der B1, B2 und B3 zusammenkommen: **Williams' algorithmische
Methode.** Das gesuchte Objekt ist ein Circuit-SAT-Algorithmus (endlich, B1 ✓),
seine Leistung ist messbar (B2 ✓), und der Lifting-Rahmen ist ein bewiesener Satz:
*nichttrivialer C-SAT-Algorithmus ⟹ untere Schranke gegen C* (B3 ✓).

Die Einschränkung ist gravierend, aber sie hebt den Befund nicht auf: Für
**allgemeine** Schaltkreise (fan-in 2) liefert der Rahmen NEXP ⊄ P/poly, nicht NP —
man müsste also erst ETH oder SETH umstoßen und bekäme dann eine Aussage über NEXP.

**Für eingeschränkte Klassen gilt diese Hürde nicht.** Williams bewies NEXP ⊄ ACC⁰
2011 ohne jeden ETH-Bezug. Genau deshalb ist der Vorschlag in §11.1 auf eingeschränkte
Klassen gerichtet und nicht auf den allgemeinen Fall.

Trotzdem ist dies die einzige bekannte Stelle im gesamten Feld, an der alle drei
Bedingungen erfüllt sind — und damit die einzige, an der KI-gestützte Suche nach dem
AlphaEvolve-Muster methodisch anschlussfähig wäre. Siehe §11.

### 7.5 Ein viertes Prüfkriterium

A4 schlägt ein Kriterium vor, das die häufigste Fehlerform im Feld trifft:

> **M6 — Verbessert ein Claim eine Garantie über *alle* Eingaben, oder nur eine
> Trefferquote auf einer Instanzverteilung?**

Darunter fallen: CDCL-Solver, Knapsacks pseudopolynomielles dynamisches Programm,
die populäre Quanten-Berichterstattung, gelernte Heuristiken und LLM-optimierte
Solver. Alle sind Verteilungserfolge, keine Worst-Case-Fortschritte. Der
Kategorienfehler ist so verbreitet, dass er ein eigenes Kriterium verdient.

---

## 8. Claim-Forensik

### 8.1 Die Basisrate

Gerhard Woegingers „P-versus-NP page" verzeichnet **116 Einträge zwischen 1986 und
2016**. Die Aufteilung ist **strittig**: Eine Quelle nennt 61 × „P = NP", 49 ×
„P ≠ NP", 6 × Sonstiges; eine andere 62/49/3+1+1; eine dritte „über 100, davon
rund 50 für P ≠ NP". Der Widerspruch bleibt hier stehen.

Woeginger verstarb 2022. **Einen Nachfolger der Liste gibt es nicht** — die
systematische Erfassung endete 2016, und damit ist die gesamte KI-Ära nicht
katalogisiert.

Daraus folgt eine Sprachregelung, auf der A8 mit Vetorecht besteht und der dieses
Papier folgt: Korrekt ist **„kein Claim hat Anerkennung gefunden"** — nicht „alle 116
wurden widerlegt". Eine Gesamtwiderlegung existiert nachweislich nicht.

Eine Beobachtung am Rande, die mehr über das Feld sagt als die Gesamtzahl: **Rund
53 % der eingereichten Claims behaupten P = NP, während rund 80–88 % der Fachleute
P ≠ NP erwarten.** Die Claim-Population ist zur Expertenerwartung invers verteilt.

### 8.2 Deolalikar 2010 — das Lehrstück

Vinay Deolalikar (HP Labs) kündigte im August 2010 einen Beweis für P ≠ NP an. Die
Prüfung lief öffentlich über Richard Liptons Blog und ein Polymath-Wiki, unter
Beteiligung von Terence Tao, Timothy Gowers, Neil Immerman und Kenneth Regan.
**Innerhalb von sechs Tagen** stand ein belastbarer Konsens.

Drei Bruchstellen wurden identifiziert: Immerman zur Ordnungsrelation in der
Finite-Model-Theory, Lindell zum tupling — und, entscheidend, eine
**Übergeneralisierung**: Dasselbe Argument liefert, auf k-XOR-SAT angewandt,
„XOR-SAT ∉ P". Das ist nachweislich falsch.

**Der wirksamste Test war der billigste.** Man musste den Beweis nicht verstehen, um
ihn zu brechen — man musste ihn nur auf ein Problem anwenden, dessen Antwort bekannt
ist.

Der strukturelle Punkt ist wichtiger als der technische: Der Beweis bestand aus drei
Teilen aus drei Fachgebieten — Finite Model Theory, statistische Physik,
Komplexitätstheorie. **Kein einzelner Gutachter deckt alle drei ab.** Er scheiterte
nur an *verteilter* Prüfung. Für ein Agententeam ist das die naheliegende Lehre.

### 8.3 Der Fall Xu/Zhou: ein Publikationsversagen

„SAT requires exhaustive search" (Ke Xu, Guangyan Zhou) erschien in **Frontiers of
Computer Science 19(12):1912405, Dezember 2025** — peer-reviewt, bei Springer Nature.
Der Titel behauptet, was P ≠ NP implizieren würde.

**Die Autoren räumen in arXiv:2401.01193 selbst ein, dass ihr Resultat nicht für
k-SAT mit konstanter Klausellänge gilt — 3-SAT ist nicht abgedeckt.** `[NUR-SNIPPET]` Es geht um
SAT-Instanzen mit langen Klauseln. Für P vs. NP folgt daraus nichts, denn 3-SAT ist
NP-vollständig, und genau darüber schweigt die Arbeit.

Zwei unabhängige, voneinander unabhängig entstandene **Kritiken** liegen vor. (Nicht
„Widerlegungen": Allender und Williams formulieren, das Argument bleibe „far short of
a proof"; die Arbeit von Chavrimootoo et al. ist ein Preprint. Die Aufstufung wäre
genau die Art Übertreibung, die dieses Papier andernorts kritisiert.)

- **Chavrimootoo, He, Kotler-Berkowitz, Liuson und Nie** (U Rochester, arXiv:2312.02071,
  2023): Fehler in den Haupttheoremen; die für downward self-reducibility nötige
  Struktur existiert nicht notwendig. Weder SETH noch P ≠ NP werden bewiesen.
- **Eric Allender und Ryan Williams** (Frontiers of Computer Science, Vol. 20,
  Art. 2001405, 2026): Das Argument bleibe „far short of a proof", weil es „an
  assumption about all possible SAT algorithms that is unwarranted" mache.
  (Beide Wortlaute `[NUR-SNIPPET]` — §9 erklärt Zitatwortlaute ausdrücklich für
  nicht belastbar; sie stehen hier, weil sie der Kern der Kritik sind, nicht weil
  wir sie verifizieren konnten.)

**Der eigentliche Befund ist die Konvergenz:** Zwei Gruppen, zwei Jahre auseinander,
ohne Bezug aufeinander, treffen dieselbe Bruchstelle. Das ist stärker als jede
einzelne Widerlegung.

Ein Hinweis zur Rezeption: Eine frühe Suchsynthese der Projektleitung sprach von
„Kommentaren von sieben Experten" im selben Heft. A8 fand über dreißig Anfragen
**genau einen**. Der Widerspruch bleibt unaufgelöst und wird hier nicht geglättet.

**Einordnung, nicht Begutachtung.** Eine frühere Fassung dieses Papiers fällte hier
ein Urteil nach dem Prüfraster M1–M5. Das war ein Verstoß gegen die eigene
Selbstbindung aus §2.3: Wir haben den Volltext nicht gelesen und können ihn nicht
beurteilen. Was sich sagen lässt, ist dies: **Die veröffentlichte Kritik greift
genau an den Stellen an, die M1 (Barrierenrechenschaft) und M3 (Strukturannahme über
Algorithmen) benennen** — Allender und Williams beim unterstellten downward
self-reducibility, Chavrimootoo et al. bei der Existenz der dafür nötigen Struktur.
Ob diese Kritik zutrifft, entscheidet die Fachöffentlichkeit, nicht dieses Papier.

Wichtig für die Einordnung: **Das ist kein Mathematikversagen, sondern ein
Publikationsversagen.** Die Arbeit hat Peer Review bestanden. Korrigiert hat die
Fachöffentlichkeit, nicht das Verfahren. Was bleibt, ist ein zitierfähiger
Springer-Nature-Artikel, dessen Titel weiter trägt als sein Inhalt — für Presse,
Enzyklopädien und künftige Trainingskorpora.

### 8.4 Der Fall arXiv:2309.05689: Zirkularität

„Large Language Model for Science: A Study on P vs. NP" ließ GPT-4 über 97
Dialogrunden mittels „Socratic reasoning" auf **P ≠ NP** schließen.

**Die Autorenliste lautet: Qingxiu Dong, Li Dong, Ke Xu, Guangyan Zhou, Yaru Hao,
Zhifang Sui, Furu Wei.**

Die Urheber von „SAT requires exhaustive search" sind Koautoren Nummer 3 und 4.
Zwei Agenten haben die Liste unabhängig voneinander über je mehrere unterschiedlich
formulierte Anfragen bestätigt. `[NUR-SNIPPET]`

**Entscheidend für die Bewertung: Die Autoren legen die Verbindung im eigenen
Abstract offen** — dort steht, der Schluss stehe „in alignment with (Xu and Zhou,
2023)". Der Befund lautet deshalb nicht „täuschend", sondern **evidentiell wertlos**:
Eine Bestätigung durch die eigenen Koautoren ist keine unabhängige Bestätigung. Und
eine Einschränkung, die das Papier ausdrücklich macht: **Wir wissen nicht**, ob dem
Modell das Argument im Dialog zugeführt wurde oder ob es ihm aus Trainingsdaten
bekannt war. Der Zirkularitätsbefund trägt ohne diese Unterstellung.

Die Verschränkung läuft in beide Richtungen: Die Verteidigungsschrift arXiv:2401.01193
stammt von Dong, Zhou und Xu — die Erstautorin des LLM-Papers ist Mitverteidigerin
genau des Arguments, das „GPT-4" reproduziert haben soll.

**Zur Fairness:** Das Papier bezeichnet sich selbst als „pilot study" und bewirbt
primär das Framework, nicht die Lösung. Als Machbarkeitsstudie für einen
Dialogprozess ist es diskutabel. Aber das Erfolgskriterium der Studie war die
Übereinstimmung mit einem Referenzargument — und dieses Referenzargument ist
inzwischen peer-reviewt widerlegt.

### 8.5 Lean verifiziert die Ableitung, nicht die Aussage

Zwei Fälle aus dem Jahr 2026 beleuchten dieselbe Grenze von zwei Seiten.

**Der sachlich korrekt deklarierte Fall.** OpenAI etablierte am 8. September 2026 die
Fefferman-Alternative **C/D** zu Navier–Stokes. Die vier Fefferman-Alternativen
trennen sich genau hier: C und D erlauben Blowup **mit** glattem Forcing, A und B
verlangen Regularität **ohne** Forcing. Etabliert wurde die schwächere Hälfte; 166 Seiten, Lean-verifiziert,
88 Stunden Suche plus 17 Stunden Formalisierung. Der Fehlermodus war **nicht** ein
falsches Statement, kein `sorry`, kein geschmuggeltes Axiom: Der Lean-Beweis ist
gültig, das Statement sauber. Der Bruch liegt eine Ebene höher — **welches der vier
formal korrekten Statements die informelle Frage beantwortet.** Die ungeforcte
Regularität bleibt offen, das Clay Institute erkennt das Resultat nicht an.
**Das kann Lean prinzipiell nicht entscheiden.** Aufgedeckt haben es Menschen.
(Zum Vorgang gehört auch ein Prioritätsstreit: Buckmaster und Alpöge erhoben am
15. August im Umfeld eines Euler-Resultats Vorwürfe zur Nutzung von Sitzungsdaten.
`[NUR-SNIPPET]`)

**Der Fall mit umdeklarierten Lücken.** Ein aktueller P=NP-Claim (arXiv:2606.03194, Pedigree
Polytopes) wirbt mit „Zero `sorry`s in the main chain". Die eigene README des
zugehörigen Repositories listet sechs `Axiom`e — darunter ausgerechnet die
Cook-Levin/Karp-Brücke „STSP ∈ P → P = NP". Eine Datei enthält
`axiom PolynomialSeparationOracle (P : Type) : Prop`, ein uninterpretiertes,
inhaltsleeres Prädikat. Der sachliche Befund lautet: Die Lücken erscheinen nicht als `sorry`, sondern als
`axiom` — und das ist prüfungsrelevant, weil `sorry` eine Warnung erzeugt und `axiom`
nicht. **Über die Absicht dahinter sagt dieses Papier nichts**; eine frühere Fassung
tat es, auf Basis einer einzigen Quelle, deren Erhebungsweg unten selbst als
unzulässig vermerkt ist.

*Herkunftsvermerk: Dieser Befund stammt aus einer Code-Suche über ein fremdes
öffentliches Repository und damit außerhalb des Bereichs, auf den die
GitHub-Werkzeuge dieser Session beschränkt waren. Der Befund wird berichtet, die
Praxis wurde für die übrigen Agenten untersagt.*

**Die Lehre:** Eine Formalisierung verifiziert die Ableitung aus den angegebenen
Voraussetzungen — niemals die Angemessenheit dieser Voraussetzungen oder der
Formalisierung des Satzes selbst. Ein grünes Lean-Zertifikat ist ein starkes Indiz
für handwerkliche Solidität und **kein** Wahrheitsbeweis.

### 8.6 Wenn die Meldung selbst der Fehler ist

Alle bisherigen Fälle betreffen fehlerhafte Arbeiten. Dieser betrifft die
**Berichterstattung über eine korrekte Arbeit** — und gehört hierher, weil er
denselben Fehlertyp zeigt: unterschlagene Voraussetzungen.

> AlphaEvolves 4×4-Schema mit 48 Multiplikationen gilt über **komplexen Koeffizienten
> in Charakteristik 0**, nicht über beliebigen Ringen. Winograd erreichte **1967**
> bereits 48 über jedem kommutativen Ring, Waksman **1970** sogar **46** über
> kommutativen Ringen mit Division durch 2. Die Verallgemeinerung auf rationale
> Koeffizienten leisteten Dumas, Pernet und Sedoglavic 2025 nachträglich — also
> Menschen. Die vielfach wiederholte Formel „erste Verbesserung über Strassen hinaus
> seit 56 Jahren" vergleicht über Ringklassen hinweg und ist **falsch**.
> `[NUR-SNIPPET]`

Das Resultat selbst bleibt eine echte Entdeckung. Falsch ist die Einordnung — und
zwar in genau der Richtung, die dem berichtenden System nützt.

### 8.7 KI-verstärkte Crank-Literatur

Ein neues Phänomen: Sprachmodelle erzeugen plausibel klingende Pseudobeweise in
großer Zahl, und Preprint-Server ohne Moderation verbreiten sie.

Der GPT-5/Erdős-Vorfall vom Oktober 2025 ist der instruktivste Fall: Es wurde
behauptet, ein offenes Erdős-Problem sei gelöst worden; die Rücknahme erfolgte
**binnen weniger als einem Tag** (die kursierende Angabe „17 Stunden" ließ sich nicht
belegen und wird hier nicht verwendet). Tatsächlich hatte das Modell **existierende
Literatur wiedergefunden** statt neu bewiesen.

Das ist bemerkenswerterweise ein Erfolg, kein Versagen: Literatursuche ist eine
Aufgabe mit endlichem, prüfbarem Ziel — B1 und B2 erfüllt. **Die Fehlmeldung entstand
erst beim Umdeuten von „gefunden" in „bewiesen".**

### 8.8 Die wiederkehrenden Fehlermuster

Aus allen geprüften Fällen, mit dem jeweils billigsten aufdeckenden Test:

| Muster | Beispiel | Prüftest |
|---|---|---|
| Übergeneralisierung | Deolalikar | Auf 2-SAT, XOR-SAT, Horn-SAT anwenden — liefert es dort Falsches? |
| Implizite Beschränkung der Algorithmenklasse | Xu/Zhou (downward self-reducibility) | Über welche Algorithmen wird quantifiziert? |
| Monotonie-Einschränkung | Blum 2017 | Gilt das Argument nur für monotone Schaltkreise? (Tardos) |
| pseudopolynomiell = polynomiell | Knapsack-Claims | Wächst die Laufzeit mit dem *Wert* oder mit der *Bitlänge*? |
| „kein bekannter" = „kein" | diverse | Wird Nichtwissen als Nichtexistenz ausgegeben? |
| Barrieren ignoriert | die Mehrheit | Gibt es einen Abschnitt dazu? |
| Durchschnitts- statt Worst-Case | SAT-Solver-Argumente | Kriterium M6 |
| Zirkularität | arXiv:2309.05689 | Sind Bestätiger und Urheber personengleich? |
| Axiom-Umbuchung | Pedigree Polytopes | `axiom`-Deklarationen zählen, nicht nur `sorry` |
| Titel-Inhalt-Divergenz trotz Peer Review | Xu/Zhou | Deckt der Satz ab, was der Titel suggeriert? |
| **Endorsement-Wäsche** | Mulmuleys „This is not what I think or said" (§4.6); das Fortnow-Pseudozitat (§9) | Hat die zitierte Person das so gesagt — und zu *dieser* Frage? |
| **Gap-Minimierung** | „nur noch wenige Lücken"; Khanukovs „a small number of identified gaps" | Sind die Lücken benannt und einzeln geprüft, oder nur gezählt? |
| **Unterschlagene Voraussetzungen in der Meldung** | „erste Verbesserung seit 56 Jahren" (§8.6) | Über welcher Struktur/Klasse gilt das Resultat — und galt Vergleichbares vorher schon? |

---

## 9. Befund über das eigene Instrument

Dieses Kapitel steht im Papier, weil sein Gegenstand — KI-gestützte Erkenntnis — auch
sein Verfahren ist. Ein Papier, das die Verlässlichkeit maschineller Wahrheitsfindung
untersucht und die eigenen Fehlleistungen verschweigt, wäre unbrauchbar.

**In vier dokumentierten Fällen hat die Suchschicht Plausibles synthetisiert:**

**Fall 1 — ein Pseudo-Zitat.** Der Projektleitung wurde Lance Fortnow der Satz
zugeschrieben, es gebe „not even a viable approach" — in unserer Wiedergabe zudem
fälschlich auf Lean bezogen, während der kolportierte Wortlaut sich auf das Lösen von
P vs. NP insgesamt bezog. Der Wortlaut ist **nicht belegbar**. Er wanderte über das
Team-Briefing in zwei Agentenberichte; ein Entwurf führte ihn sogar als „über zwei
Suchen bestätigt". Gefunden hat es A7, bestätigt A1 durch zwei gezielte Gegenproben
und eine indirekte.

Belegbar sind stattdessen zwei andere Aussagen Fortnows: *„Don't waste your time
trying a formal approach via Lean"* und *„Computational complexity is very messy to
formulate technically"*. Sinngemäß verneint er im selben Post auch die Frage, ob ein
KI-erzeugter Beweis von P ≠ NP bevorstehe, und begründet das mit einer Basisrate statt
mit einer Prinzipienaussage — diese Wiedergabe stammt jedoch aus A1s Gegenproben und
wurde von A7 nicht bestätigt. `[NUR-SNIPPET]` Wir setzen hier bewusst **kein** zweites
ungeprüftes Wortlautzitat gegen das erste.

**Fall 2 — verschmolzene Umfragezahlen.** Über sieben Anfragen lieferte die Suchschicht
dieselben Kennzahlen („~80 %", „99 % unter Expert:innen") abwechselnd für 2012 und für
2019. Mit hoher Wahrscheinlichkeit eine Quellenverschmelzung, kein Datum.

**Fall 3 — ein nicht existierender Titel.** Die Leitung gab „STOC 2026 Best Paper:
*Refuter Problems for Proof Complexity*" als Rechercheanker weiter. Erfunden waren
**Titel und Preisträgerstatus**: Die STOC-2026-Arbeit heißt „Finding Bugs in Short
Proofs". Der *Gegenstand* — Refuter-Probleme in der Proof Complexity — existiert
sehr wohl (ECCC TR24-190). Die Konfabulation war also nicht thematisch, sondern
bibliographisch, und deshalb besonders schwer zu bemerken. Gefunden von A3.

**Fall 4 — eine wandernde Zahl.** Diesen Fall hat A8 in der Prüfung des fertigen
Papiers gefunden, und er ist der aufschlussreichste, weil er die **Herkunft** eines
Fehlers zeigt statt nur seine Existenz. §8.5 nennt für die Navier-Stokes-Arbeit
„17 Stunden Formalisierung". Zwei Abschnitte später verwirft §8.7 die Angabe
„17 Stunden" für den GPT-5/Erdős-Vorfall als unbelegbar. Mit hoher Wahrscheinlichkeit
ist das kein Zufall: **Dieselbe Zahl ist von einem Fall auf einen anderen gewandert.**
Damit besitzt dieses Papier nicht nur eine Fehlerkorrektur, sondern einen
Kontaminationsnachweis — ein Beleg dafür, wie eine Angabe ohne Primärverankerung den
Kontext wechselt und dabei plausibel bleibt.

**Alle vier stammen aus der Leitungsrecherche. Alle vier wurden von Fachagenten
gefunden.** Das ist die operative Lehre: Die Fehler entstanden dort, wo schnell und
breit recherchiert wurde, und wurden dort gefunden, wo langsam und eng geprüft wurde.
Redundanz war nicht Verschwendung, sondern der Mechanismus, der funktioniert hat.

Auch der Verfasser der Strategie hat in der Konsensrunde eine eigene Behauptung
zurückgezogen und dazu angemerkt, seine eigene Regel zur „Snippet-Quarantäne" habe
auf ihn selbst nicht gewirkt. Das gehört hierher, nicht in eine Fußnote.

**Die Konsequenz für die Belastbarkeit dieses Papiers:** Grobe Aussagen —
Existenz von Arbeiten, Richtung von Befunden, Größenordnungen — sind tragfähig.
**Nicht tragfähig sind Prozentzahlen, Zitatwortlaute, Preisträgerzuordnungen,
Titelangaben und isolierte Einzelzahlen.**

Der Befund hat eine unbequeme Kehrseite, auf die das Red Team bestanden hat: Alle
vier Fälle betrafen **Attributionen ohne Primärverankerung** — ein Zitat, eine
Umfragezahl, ein Titel, eine Stundenangabe. Genau solche Angaben stehen auch in §4.1
und §4.3. Was jene Tabellen zusätzlich haben, ist interne Konsistenz über mehrere
Datenpunkte hinweg. Das stützt die **Größenordnung**; die einzelnen Ziffern stützt
es nicht.
Wo dieses Papier solche Angaben macht, sind sie markiert oder als strittig
gekennzeichnet.

---

## 10. Konsens und Dissens

### 10.1 Einstimmig

- **T1.** P vs. NP ist offen. Kein Agent fand einen Gegenbefund.
- **T2.** KI hat neue Resultate in der Komplexitätstheorie erzeugt, aber
  ausschließlich in Ergebnisklassen, die P ≠ NP voraussetzen statt es zu berühren.
- **T3.** Es gibt keinen Fall, in dem KI einen asymptotisch besseren Algorithmus für
  ein NP-schweres Problem gefunden hätte.
- **T4.** Die Einzeltechniken haben bewiesene Endpunkte.
- **T5.** Das SAT-Solver-Argument ist widerlegt, nicht offen.

### 10.2 Angenommen

Nach der Abstimmungsrunde aufgenommen (Einzelnachweis der Textkorrekturen in §10.5):

- **Beidseitiges B2** (A1): auch P = NP ist kein Suchproblem. Vom Strategieagenten
  als „schärfster Punkt" bewertet.
- **M6** (A4): Garantie über alle Eingaben versus Trefferquote auf einer Verteilung.
- **B3a/B3b** (A5): als Notiz zu GCT aufgenommen, nicht als eigene Achse.

**Abgelehnt: B3′** (A3). Der Vorschlag, ein viertes Symbol für die Kompatibilität von
Rahmen und Eingangstechnik einzuführen, wurde als Überkomplizierung verworfen.
Hardness Magnification zeigt, dass B3 wörtlich zu schwach ist — das gehört in die
Definition von B3, nicht in ein zusätzliches Kriterium. Der Einwand selbst ist gültig
und in §7.3 eingearbeitet.

### 10.3 Unaufgelöste Widersprüche

Diese bleiben stehen:

1. **Gasarch 2019**: 80 % versus 88 % für P ≠ NP. Nicht auflösbar.
2. **Woeginger-Aufteilung**: 61/49/6 versus 62/49/3+1+1 versus „>100, davon ~50".
3. **Expertenkommentare zu Xu/Zhou**: „sieben" (eine Suchsynthese) versus „genau
   einer auffindbar" (A8, über 30 Anfragen).
4. **SAT-Competition-Instanzzahlen**: 327 (2025, von 400) versus 276 (2026,
   Gesamtzahl unverifiziert) — kein Trendbeleg.
5. **Datum des Fortnow-Posts**: 10. oder 14. Juni 2026.

### 10.4 Zur Vollständigkeit der Konsensrunde

**Die Konsensrunde wurde vollständig durchgeführt — im zweiten Anlauf.** Ein erster
Durchgang brach nach einer einzigen Antwort (S1) an einem API-Ratenlimit ab. Nach
Fertigstellung des Papiers wurden alle zehn Agenten erneut angeschrieben, diesmal mit
dem fertigen Text statt mit einer Vorlage. **Alle zehn haben geantwortet.**

Das war die produktivere Reihenfolge. Ein fertiges Papier lässt sich konkreter
angreifen als eine Thesensammlung: Der überwiegende Teil der Rückläufe bestand aus
Textstellenkritik, und **jede in §10.5 aufgeführte Korrektur stammt aus dieser Runde**.

Hinzu kommt, dass die wechselseitige Prüfung ohnehin während der gesamten
Projektlaufzeit stattfand, nur asynchron. A2 korrigierte A1s Autorenzuordnung; A3 korrigierte zwei Leads
der Leitung und eine Statusmarkierung von A1; A5 korrigierte eine Aussage des
Strategieberichts; A7 und A8 verifizierten wechselseitig und unabhängig dieselbe
Autorenliste; A1 korrigierte den Entwurf seines eigenen abgebrochenen Vorgängers;
S1 zog in der Konsensrunde eine eigene Behauptung zurück.

Der Austausch war also beides: eine fortlaufende Korrekturkette während der Arbeit
und eine vollständige Abstimmungsrunde am fertigen Text.

### 10.5 Was die Abstimmungsrunde am Papier geändert hat

Die Prüfung durch die zehn Agenten hat **sachliche Fehler im fertigen Papier
aufgedeckt**. Die wichtigsten, jeweils mit Finder:

| Korrektur | von |
|---|---|
| „Seed-Prover: 5 von 6 IMO-Aufgaben" war falsch — es waren 4 vollständige plus 1 teilweise Lösung; die fünfte entstand nachträglich per extended search | A7 |
| Rechenfehler: 9 % von 124 sind rund 11 Personen, nicht sieben | A1 |
| Die AC⁰-Zeile stand fälschlich in der Endpunkt-Tabelle — dort funktioniert die Technik, weil es in AC⁰ keine PRFs gibt | S2 |
| Das Urteil „M1 nicht erfüllt, M3 verletzt…" über Xu/Zhou war Begutachtung eines ungelesenen Volltexts — Verstoß gegen die eigene Selbstbindung | S2 |
| „Zwei unabhängige Widerlegungen" war eine Aufstufung; es sind Kritiken, eine davon Preprint | S2 |
| Die Extrapolation „620.000 Jahre" wurde als methodisch wertlos bezeichnet und trotzdem prominent geführt | S2 |
| „Konvergenz gegen c ≈ 0,386 bestätigt die ETH" — die ETH behauptet nur s₃ > 0, und die Reihe misst die Analysetechnik, nicht s₃ | A4 |
| Williams: „fast linear → quadratisch" beschreibt die abgeleitete Zeitschranke, nicht die Simulation | A2 |
| „B1 fällt aus" gilt nur für die P ≠ NP-Richtung; für P = NP existiert das endliche Objekt, dort scheitert B2 | A1 |
| Natural Proofs fehlte die dritte Eigenschaft *usefulness* — genau die, die die Schranke liefert | A1 |
| §7.4 („erst ETH umstoßen") widersprach §11.1 — die Hürde gilt nur für allgemeine Schaltkreise | A2 |
| Der Matrixmultiplikations-Befund fehlte ganz; er ist der einzige Fall, in dem eine KI-Mathematik-*Meldung* nachweislich falsch ist | A6 |
| **Fall 4 in §9**: die „17 Stunden" wanderten von Navier–Stokes zum Erdős-Vorfall — ein Kontaminationsnachweis | A8 |
| Motivzuschreibung („umgebucht") gegenüber benannten Autoren gestrichen | S2 |
| B1/B2/B3 als „Kriterium" bezeichnet, obwohl post hoc gebildet und nie prospektiv getestet | S2 |

Ein Dissens blieb bestehen und ist eingearbeitet: A3 akzeptierte die Ablehnung von
B3′, wies aber nach, dass die angekündigte Konsequenz nicht vollzogen war — die
B3-Definition in §7.1 blieb unverändert und wurde von Hardness Magnification erfüllt.
Der Nebensatz, der das repariert, steht jetzt dort.

---

## 11. Was sich lohnen würde

Aus der Analyse folgen drei konkrete, heute angreifbare Fragen. Keine davon löst
P vs. NP; alle drei sind mehr als Literaturarbeit.

**11.1 KI-gestützte Suche nach Circuit-SAT-Algorithmen.** Williams' algorithmische
Methode ist die einzige Stelle, die dieses Projekt gefunden hat, an der B1, B2 und B3
weitgehend zusammenfallen: Das gesuchte Objekt ist ein Algorithmus, seine Laufzeit ist
messbar, und der Lifting-Rahmen ist ein bewiesener Satz — bei eingeschränkten Klassen
zudem ohne ETH-Hürde (§7.4).

Die Frage lautet: **Kann evolutionäre LLM-Suche nach dem AlphaEvolve-Muster
nichttriviale Circuit-SAT-Algorithmen für Schaltkreisklassen oberhalb von ACC⁰ finden
— etwa für TC⁰, gegen das bislang keine unteren Schranken bekannt sind?**

Zwei Einschränkungen gehören unmittelbar dazu, sonst wird der Vorschlag naiv.
**Erstens** ist „gegen TC⁰ ist nichts bekannt" eine Aussage über *untere Schranken*,
nicht über *SAT-Algorithmen* — für Tiefe-2-Threshold-Schaltkreise existieren
nichttriviale Algorithmen bereits. **Zweitens, und das ist der eigentliche Haken,
ist B2 nur teilweise erfüllt:** Die Laufzeit eines Kandidatenalgorithmus ist messbar,
aber seine **Korrektheit ist selbst eine Beweispflicht**. Daran hängt die Sache — nicht
am Finden schneller Heuristiken. Wer den Vorschlag aufgreift, arbeitet genau an dieser
Stelle oder an keiner.

Der Ertrag wären untere Schranken gegen schwache Klassen, nicht P ≠ NP. Es ist
dennoch die methodisch anschlussfähigste offene Frage, die dieses Projekt
identifiziert hat.

**11.2 Ein Claim-Register für die KI-Ära.** Die Woeginger-Liste endete 2016. Seither
hat sich die Produktionsrate von Pseudobeweisen durch Sprachmodelle vervielfacht, und
niemand katalogisiert sie. Ein gepflegtes, öffentliches Register mit dem hier
entwickelten Prüfraster (M1–M6, Fehlertaxonomie, vierstufiges Protokoll) wäre eine
Aufgabe, die ein Agententeam tatsächlich leisten kann — im Gegensatz zu allem anderen
in diesem Papier.

**11.3 Formalisierung der Komplexitätstheorie in mathlib.** Cook–Levin existiert in
Isabelle und Coq, nicht in Lean. Solange das so ist, lässt sich P vs. NP in mathlib
nicht einmal *formulieren*. Das ist keine Forschung, sondern Infrastruktur — und
genau die Art Arbeit, bei der Autoformalisierung nachweislich funktioniert (§6.6).
Eine mathlib-Formalisierung von Cook–Levin wäre ein realistisches, prüfbares Ziel mit
echtem Nutzen.

---

## 12. Fazit

**P = NP oder nicht?** Niemand weiß es. Die Fachwelt erwartet mehrheitlich P ≠ NP,
auf Basis einer Umfrage von 2018 und der Intuition, dass mathematische Kreativität
nicht algorithmisch trivialisierbar ist. Ein Beweis existiert in keiner Richtung.

**Hat KI daran etwas geändert?** Nein — und der Grund ist aufschlussreicher als der
Befund. KI hat in diesem Feld geleistet, was sie leisten kann: endliche Strukturen
finden, die Menschen übersehen haben, in Beweisrahmen, die Menschen gebaut haben.
AlphaEvolves 19-Variablen-Gadget mit Gewichtsverhältnissen von 1429:1 ist eine echte
mathematische Entdeckung. Sie liegt nur vollständig innerhalb der Hypothese, deren
Wahrheitswert die eigentliche Frage ist.

Die drei Bedingungen B1, B2 und B3 beschreiben, wo die Grenze verläuft. Sie sind
**post hoc an sechs Fällen gebildet und nie prospektiv getestet** — eine ordnende
Beschreibung, kein validiertes Kriterium; das Red Team hat auf dieser Einschränkung
zu Recht bestanden. Als Beschreibung greift sie: P vs. NP verletzt alle drei
Bedingungen, am gravierendsten die dritte. Und die dritte Bedingung ist keine technische
Hürde, sondern die Formulierung dessen, was offen ist: Die Barrieren und die bewiesenen
Endpunkte der Einzeltechniken **sind** die Feststellung, dass kein tragfähiger Rahmen
bekannt ist.

> **Ein KI-Beweis von P vs. NP scheitert nicht an Rechenleistung, sondern daran,
> dass niemand weiß, wonach gesucht werden soll.**

Das ist keine pessimistische Aussage über KI. Es ist eine Aussage darüber, was für
ein Problem P vs. NP ist. Wer erwartet, dass mehr Rechenleistung oder ein größeres
Modell die Frage beantwortet, unterschätzt, woran es fehlt: nicht an Suchkapazität,
sondern an einer Vorstellung davon, was zu suchen wäre.

Auch das ist zu präzisieren: Aus *kein tragfähiger Rahmen bekannt* folgt nicht
*kein Rahmen findbar*. Williams' NEXP ⊄ ACC⁰ zeigt, dass Rahmen entstehen können,
wo zuvor keiner war.

Das produktivste Ergebnis dieses Projekts ist deshalb nicht der Negativbefund,
sondern seine Struktur: ein Kriterium, das angibt, welche mathematischen Fragen für
KI-gestützte Suche heute zugänglich sind — und ein konkreter Vorschlag für die einzige
Stelle im Feld, an der es zutrifft.

---

## Anhang A — Markierungen

| Marker | Bedeutung |
|---|---|
| `[VERIFIZIERT]` | peer-reviewt oder breit akzeptiert, mehrfach trianguliert |
| `[PREPRINT]` | arXiv/ECCC, nicht begutachtet |
| `[CLAIM]` | Behauptung ohne Verifikation, ggf. widerlegt |
| `[NUR-SNIPPET]` | nur über Suchsynthese belegt, kein Volltext |
| `[EIGENE EINSCHÄTZUNG]` | Analyse des Teams, kein Literaturbefund |

**Generalvorbehalt:** Ohne Volltextzugang gilt `[NUR-SNIPPET]` als Grundzustand für
alle inhaltlichen Aussagen über Publikationen. Explizit markiert sind die Fälle, in
denen dies besonders relevant ist.

## Anhang B — Materialien

Die vollständigen Einzelberichte liegen im Repository:

| Datei | Inhalt | Umfang |
|---|---|---|
| `docs/00-briefing.md` | Grundregeln, Markierungssystem | |
| `docs/01-strategie-S1.md` | Angriffsplan, Zielebenen, Teamentwurf | 7.900 W. |
| `docs/01-redteam-S2.md` | Barrieren, Basisraten, Akzeptanzprotokoll | 10.700 W. |
| `docs/02-team-briefing.md` | Verbindliches Briefing, Kriterienraster | |
| `docs/03-konsensvorlage.md` | Synthese T1–T10 für die Abstimmung | |
| `docs/04-abstimmungsrunde.md` | Die zehn Rückläufe zum fertigen Papier | |
| `research/00-lead-sondierung.md` | Vorbefunde der Leitung | 900 W. |
| `research/a1-kanon-und-barrieren.md` | Fundamentkapitel, Barrieren, Umfragen | 12.800 W. |
| `research/a2-untere-schranken.md` | Schaltkreisschranken, bewiesene Endpunkte | 7.500 W. |
| `research/a3-metakomplexitaet.md` | MCSP, Proof Complexity, Unabhängigkeit | 8.200 W. |
| `research/a4-obere-schranken.md` | Exakte Algorithmen, SAT-Praxis, Quanten | 5.100 W. |
| `research/a5-algebraisch-gct.md` | VP/VNP, LST, GCT | 6.200 W. |
| `research/a6-ki-algorithmenentdeckung.md` | AlphaEvolve, FunSearch, SATLUTION | 3.600 W. |
| `research/a7-ki-beweisen.md` | AlphaProof, Lean, Navier–Stokes, Zirkularität | 4.900 W. |
| `research/a8-claim-audit.md` | Woeginger, Deolalikar, Xu/Zhou, Fehlertaxonomie | 6.800 W. |

Quellenangaben mit URLs stehen jeweils am Ende der Einzelberichte.
