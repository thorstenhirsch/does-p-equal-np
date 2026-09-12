# A3 — Meta-Komplexität, Proof Complexity & Unabhängigkeit

**Agent:** A3
**Stand:** 12. September 2026
**Forschungsfrage:** Welche Implikationsketten führen am plausibelsten Richtung P ≠ NP — und welche gelten nur unter kryptographischen oder anderen Zusatzannahmen?

---

## 0. Methodische Vorbemerkung (verbindlich mitzulesen)

Gemäß `docs/02-team-briefing.md` §1 ist **WebFetch gesperrt**. Es gab **keinen Volltextzugang** zu
arXiv, ECCC, ACM DL, SIAM, Springer oder IEEE. Alle inhaltlichen Aussagen unten beruhen auf
Suchmaschinen-Snippets und wurden, wo möglich, über **mehrere unterschiedlich formulierte Anfragen**
trianguliert. Entsprechend gilt:

> **Fast alles in diesem Bericht ist `[NUR-SNIPPET]`.** Wir können in der Regel belegen, *dass* eine
> Arbeit existiert, unter welchem Titel, in welchem Venue und welches Jahr — und wir können den
> *Kernsatz* rekonstruieren, wie ihn Abstracts und Sekundärquellen wiedergeben. Wir können **nicht**
> die exakten Voraussetzungen, Quantorenstellungen und Reduktionstypen am Volltext prüfen.
> Bei Meta-Komplexität ist das besonders schmerzhaft, weil dort *genau diese Details* (partiell vs.
> total, deterministisch vs. randomisiert, many-one vs. Turing, relativiert vs. unrelativiert)
> den gesamten Gehalt ausmachen.

Markierungen nach `docs/00-briefing.md`: `[VERIFIZIERT]`, `[PREPRINT]`, `[CLAIM]`,
`[EIGENE EINSCHÄTZUNG]`, ergänzt um `[NUR-SNIPPET]`.

---

## 1. Meta-Komplexität und MCSP

### 1.1 Was ist MCSP?

**Minimum Circuit Size Problem (MCSP).** Eingabe: die **vollständige Wahrheitstabelle** einer
Booleschen Funktion f: {0,1}^n → {0,1}, also ein String der Länge N = 2^n, plus ein Größenparameter s
(binär oder unär). Frage: Gibt es einen Booleschen Schaltkreis der Größe ≤ s, der f berechnet?

Zwei Eigenschaften machen das Problem eigentümlich:

1. **MCSP ∈ NP, trivialerweise.** Der Zeuge ist der Schaltkreis; seine Verifikation kostet
   poly(N) Zeit, weil die Eingabe selbst schon exponentiell in n ist. Die Eingabelänge N = 2^n ist
   also *großzügig bemessen* — genau das ist der Grund, warum MCSP in NP liegt und trotzdem
   niemand einen Polynomialzeitalgorithmus kennt. `[VERIFIZIERT]` (Standardwissen, kanonisch)
2. **MCSP ist ein Problem *über* Komplexität.** Es fragt nach der Komplexität eines Objekts, nicht
   nach einer kombinatorischen Eigenschaft. Das ist der Namensgeber des Teilfelds
   **Meta-Komplexität**: die Komplexität des Berechnens von Komplexität.

**Historische Pointe.** Leonid Levin hat nach eigener Darstellung die Publikation seiner Arbeit zur
NP-Vollständigkeit (1973) verzögert, weil er hoffte, zuvor die NP-Härte von MCSP zeigen zu können.
`[NUR-SNIPPET]`, Konfidenz mittel-hoch (die Anekdote wird in Hiraharas FOCS-2022-Arbeit und in
mehreren Übersichten konsistent wiedergegeben). Das Problem ist damit **so alt wie die
NP-Vollständigkeitstheorie selbst** und bis heute offen.

### 1.2 Warum die Komplexität von MCSP aufschlussreich ist

Drei Gründe, in aufsteigender Bedeutung:

**(a) MCSP verbindet Kryptographie, Lerntheorie und Schaltkreisuntere Schranken.** Wäre MCSP in P,
so gäbe es keine kryptographisch sicheren Pseudozufallsgeneratoren im üblichen Sinn — man könnte
zufällige von pseudozufälligen Strings unterscheiden, indem man ihre Schaltkreiskomplexität misst.
Der Razborov–Rudich-*natural-proofs*-Barriere liegt genau dieses Argument zugrunde: ein „natürlicher"
unterer-Schranken-Beweis ist im Kern ein effizienter Algorithmus für ein MCSP-artiges Problem.
`[VERIFIZIERT]` (kanonisch; vgl. A1 §Natural Proofs)

**(b) MCSP ist der wahrscheinlichste Kandidat für ein „natürliches" NP-Problem von mittlerer
Schwierigkeit** (NP-intermediate), falls P ≠ NP. Neben Faktorisierung und Graphisomorphie
(letzterer durch Babai 2015/2017 quasipolynomiell) ist es das prominenteste Beispiel.

**(c) Der eigentliche Punkt: NP-Härte von MCSP ist kein Geschenk, sondern selbst ein Durchbruch.**
Dies ist die zentrale strukturelle Einsicht des Feldes und wird unten (§1.3) ausgeführt.

### 1.3 Die „Härte-der-Härte": NP-Härte von MCSP impliziert Separationen

Der wichtigste Befund für unser Papier. Die bekannten Konsequenzen (Murray–Williams, *On the (Non)
NP-Hardness of Computing Circuit Complexity*, CCC 2015 / Theory of Computing 13(4), 2017;
sowie Kabanets–Cai, STOC 2000):

| Reduktionstyp SAT → MCSP | Implizierte Konsequenz |
|---|---|
| deterministisch, many-one, polynomiell | **EXP ≠ ZPP** |
| allgemeine Polynomialzeitreduktion | EXP ≠ NP ∩ P/poly (⇒ EXP ≠ ZPP) |
| Logspace-Reduktion | **PSPACE ≠ ZPP** |
| logtime-uniforme AC⁰-Reduktion | **NP ⊄ P/poly** und E ⊄ i.o.-SIZE(2^δn), daraus **P = BPP** |
| „natürliche" Reduktion (Kabanets–Cai) | **E ⊄ P/poly** |

`[NUR-SNIPPET]`, Konfidenz **hoch** für die Existenz und Grundrichtung dieser Resultate (über zwei
unterschiedlich formulierte Suchen konsistent), **mittel** für die exakten Klassenangaben in jeder
Zeile.

**Interpretation (zentral, `[EIGENE EINSCHÄTZUNG]`, Konfidenz hoch):** Diese Tabelle ist der Grund,
warum das Feld sich so verhält, wie es sich verhält. Ein Beweis „MCSP ist NP-vollständig" ist **kein
Zwischenschritt auf dem Weg zu P ≠ NP** — er *enthält bereits* eine Separation, die seit Jahrzehnten
offen ist. Wer MCSP als „naheliegenden nächsten Schritt" verkauft, hat die Richtung der Implikation
verwechselt. Das ist strukturell dasselbe Muster wie bei der Hardness Magnification (§3): eine
scheinbar bescheidene Aussage, die eine unbescheidene enthält.

Umgekehrt gibt es **Nicht-Härte-Hinweise**: Unter glaubwürdigen kryptographischen Annahmen
(*indistinguishability obfuscation* plus subexponentiell sichere Einwegfunktionen) ist **GapMCSP
nicht NP-vollständig unter randomisierten Levin-Reduktionen**. `[NUR-SNIPPET]`, Konfidenz mittel —
diese Aussage konnte ich nur über eine Suchantwort belegen und die Autorenschaft nicht sauber
zuordnen. **Explizit als nicht ausreichend trianguliert gekennzeichnet.**

### 1.4 Die Grenze partiell / total — der Kern des Auftrags

Dies ist der wichtigste inhaltliche Punkt dieses Berichts.

**MCSP\* (partielle Funktionen).** Eingabe ist eine *partielle* Wahrheitstabelle: ein String über
{0, 1, ⋆}, wobei ⋆ „don't care" bedeutet. Gefragt ist, ob ein Schaltkreis der Größe ≤ s existiert,
der auf allen *spezifizierten* Positionen mit der Vorgabe übereinstimmt.

**Was bewiesen ist:**

- **Levin 1973:** NP-Härte der *partiellen* Variante des Minimum DNF Size Problem.
  `[NUR-SNIPPET]`, Konfidenz mittel-hoch.
- **Ilango, FOCS 2020** (*Constant Depth Formula and Partial Function Versions of MCSP are Hard*):
  MCSP\* und MFSP\* erfordern unter der **Exponential Time Hypothesis (ETH)** Zeit
  N^Ω(log log N) — also eine *quasipolynomielle* untere Schranke, **keine** NP-Härte.
  `[NUR-SNIPPET]`, Konfidenz mittel-hoch.
- **Ilango, CCC 2020** (*NP-Hardness of Circuit Minimization for Multi-Output Functions*):
  erste NP-Härte für Schaltkreisminimierung **totaler** Funktionen bei **allgemeinen
  (unbeschränkten) Booleschen Schaltkreisen** — allerdings für **mehrwertige** (multi-output)
  Funktionen, unter randomisierten many-one-Reduktionen. `[NUR-SNIPPET]`, Konfidenz mittel-hoch.
  **Achtung, häufige Verwechslung:** Multi-Output-MCSP ist ein *anderes Problem* als MCSP.
- **Hirahara, FOCS 2022** (ECCC TR22-119, *NP-Hardness of Learning Programs and Partial MCSP*):
  **NP-Härte von Partial MCSP unter randomisierten Polynomialzeit-many-one-Reduktionen.**
  Zugleich NP-Härte des Lernens effizienter Programme — eine seit Ko offene Frage, für die Ko
  gezeigt hatte, dass es **keinen relativierenden Beweis** gibt. Hirahara überwindet diese
  Relativierungsbarriere. `[VERIFIZIERT]` für Existenz/Venue/Titel (dblp, IEEE FOCS Proceedings,
  ECCC), `[NUR-SNIPPET]` für den genauen Reduktionsbegriff. Konfidenz **hoch**.

**Was NICHT bewiesen ist — und das ist der Kern:**

> **MCSP für totale, einwertige Funktionen ist weiterhin offen.** Es ist nicht bekannt, ob MCSP
> NP-vollständig ist, und es ist nicht bekannt, ob MCSP in P liegt.

**Warum bricht das Argument beim Übergang partiell → total?**
`[EIGENE EINSCHÄTZUNG]` mit Snippet-Stütze, Konfidenz mittel-hoch:

Eine Reduktion auf **MCSP\*** darf ⋆-Positionen setzen. Sie muss die Zielfunktion also nur auf den
Stellen festlegen, die sie kontrollieren will, und überlässt den Rest dem Minimierer. Das gibt der
Reduktion **kombinatorische Freiheit**: sie kann eine SAT-artige Struktur direkt in das Muster der
spezifizierten Positionen einkodieren, ohne je eine Aussage über die Schaltkreiskomplexität eines
*vollständig festgelegten* Objekts machen zu müssen.

Bei **totalem MCSP** entfällt diese Freiheit. Die Reduktion muss die Funktion auf allen 2^n Eingaben
festlegen und dann — für die Korrektheit der Reduktion — **beweisen**, dass die entstehende
Wahrheitstabelle hohe Schaltkreiskomplexität hat, wann immer die SAT-Instanz unerfüllbar war. Das ist
der Punkt: *Die Korrektheit einer Reduktion nach totalem MCSP verlangt explizite untere
Schaltkreisschranken für explizit konstruierte Funktionen* — also genau das, was die gesamte
Komplexitätstheorie seit fünfzig Jahren nicht kann. Die Tabelle in §1.2 formalisiert diese Intuition:
Die Konsequenzen sind kein Zufall, sie sind das Echo dieses Erfordernisses.

Sekundärbelegt aus der Snippet-Lage: „Proving the NP-hardness of circuit minimization for total
functions seems to require a more sophisticated argument"; „one may need new ideas to obtain
NP-hardness of total MCSP via his [Hiraharas] approach"; für DNF-MCSP und Formula-MCSP gelang der
Weg *partiell → total*, „whether one can similarly further reduce MCSP\* to MCSP, however, remains
unclear". `[NUR-SNIPPET]`, Konfidenz mittel-hoch.

**Fazit der Grenze (Konfidenz hoch):** Hiraharas FOCS-2022-Resultat ist ein echter Fortschritt und
überwindet eine bewiesene Barriere (Relativierung). Es ist **kein** Schritt „kurz vor" NP-Härte von
MCSP. Der Abstand partiell → total ist nicht quantitativ, sondern **qualitativ**: er ist genau der
Abstand zwischen „ich darf don't-cares setzen" und „ich muss untere Schranken beweisen".

### 1.5 „SAT Reduces to the Minimum Circuit Size Problem **with a Random Oracle**"

**Was das Papier ist.** ECCC TR23-165 (2023, Revision März 2025), publiziert **FOCS 2023**,
Journalfassung **SIAM Journal on Computing**, DOI 10.1137/24M1652568. `[VERIFIZIERT]` für Existenz,
Venue und Journalfassung (drei Treffer: IEEE Xplore, ECCC, SIAM/DOI). Damit ist dies
**peer-reviewt**, nicht bloß Preprint — eine Korrektur gegenüber der Vorrecherche in
`research/a1-kanon-und-barrieren.md` §478, wo es als `[PREPRINT]` geführt wird.

**Was gezeigt wird.** Mit **Wahrscheinlichkeit 1** über die Wahl eines zufälligen Orakels O gibt es
eine black-box **P/poly**-many-one-Reduktion (ebenso eine P^O-Reduktion) von **unrelativiertem SAT**
auf **MCSP^O** — also MCSP für Schaltkreise, die Zugriff auf das Zufallsorakel O haben. Damit wird
eine Vermutung von Huang, Ilango und Ren (STOC 2023) bestätigt. Die Autoren bezeichnen es als
stärkste bisherige Evidenz für NP-Vollständigkeit von MCSP. `[NUR-SNIPPET]`, Konfidenz hoch für
diese Zusammenfassung (zwei unabhängige Suchen konsistent).

**Was es NICHT zeigt** (der für uns entscheidende Teil, `[EIGENE EINSCHÄTZUNG]`, Konfidenz hoch):

1. **Es zeigt nicht, dass MCSP NP-hart ist.** Die Zielsprache ist MCSP^O, nicht MCSP. Das ist ein
   anderes Problem. Die Aussage lebt in einer relativierten Welt.
2. **Der Titel ist irreführend kurz.** „SAT reduziert auf MCSP" ohne den Zusatz „mit einem
   Zufallsorakel" ist schlicht falsch. Für ein Papier, das populärwissenschaftlich zitiert wird,
   ist das ein Risiko — wir sollten im Papier den vollständigen Titel verwenden.
3. **Die Reduktion ist nicht-uniform (P/poly) und black-box.** Sie fällt damit nicht unter die
   Murray–Williams-Konsequenzen aus §1.3 — was zugleich erklärt, *warum* sie möglich war: Die
   Orakelrelativierung ist exakt das Schlupfloch, das die Barriereresultate offenlassen.
4. **Zufallsorakel-Evidenz ist historisch unzuverlässig.** Die sogenannte *Random Oracle Hypothesis*
   — Aussagen, die relativ zu einem Zufallsorakel mit Wahrscheinlichkeit 1 gelten, gelten auch
   unrelativiert — ist **widerlegt**. Das klassische Gegenbeispiel ist IP vs. PSPACE:
   IP = PSPACE gilt unrelativiert, aber relativ zu einem Zufallsorakel gilt IP^O ≠ PSPACE^O mit
   Wahrscheinlichkeit 1. `[NUR-SNIPPET]` — **noch zu triangulieren**, siehe §7.
   Wenn das zutrifft, ist „mit Wahrscheinlichkeit 1 relativ zu einem Zufallsorakel" genau die
   Evidenzform, bei der die Fachgeschichte bereits einmal danebenlag.

**Bewertung:** ernstzunehmendes, peer-reviewtes Resultat; *Evidenz*, kein Beweis; und eine Evidenzform
mit belegter Fehlschlagshistorie. Konfidenz in diese Bewertung: mittel-hoch.

---

## 2. Heuristica ausschließen

### 2.1 Impagliazzos fünf Welten (kanonisch, `[VERIFIZIERT]`)

Impagliazzo 1995, *A Personal View of Average-Case Complexity*:

| Welt | Charakterisierung |
|---|---|
| **Algorithmica** | P = NP (oder moralisch äquivalent: NP ⊆ BPP) |
| **Heuristica** | P ≠ NP, aber NP ist im Durchschnitt leicht (DistNP ⊆ AvgP) |
| **Pessiland** | NP ist im Durchschnitt hart, aber es gibt **keine** Einwegfunktionen |
| **Minicrypt** | Einwegfunktionen existieren, aber keine Public-Key-Kryptographie |
| **Cryptomania** | Public-Key-Kryptographie existiert |

Meta-Komplexität ist das Werkzeug, mit dem das Feld seit ~2018 versucht, **Heuristica und Pessiland
auszuschließen** — also die Lücke zwischen worst-case-Härte und den kryptographisch nutzbaren
Härteformen zu schließen.

### 2.2 Hiraharas non-black-box worst-case-to-average-case-Reduktion

**Arbeit:** S. Hirahara, *Non-Black-Box Worst-Case to Average-Case Reductions within NP*, FOCS 2018
(ECCC TR18-138); Journalfassung **SIAM J. Comput.**, DOI 10.1137/19M124705X. `[VERIFIZIERT]` für
Existenz/Venue/Journal (drei konsistente Treffer).

**Was gezeigt wird** `[NUR-SNIPPET]`, Konfidenz mittel-hoch:
Wenn eine **Approximationsversion** von MCSP oder MINKT nicht in BPP liegt, dann liegt die
zugehörige **Average-Case-Version nicht in AvgP**. Es handelt sich um die erste
non-black-box-worst-case-to-average-case-Reduktion innerhalb von NP. Der Begriff *non-black-box* ist
wesentlich: klassische Resultate (Bogdanov–Trevisan, Feigenbaum–Fortnow) zeigen, dass **black-box**
worst-case-to-average-case-Reduktionen für NP-Probleme unter plausiblen Annahmen unmöglich sind.
Hirahara umgeht diese Barriere.

**Die Implikationskette, um die es geht:**

> Wenn **GapMCSP** (bzw. GapMINKT) **NP-hart** ist (unter randomisierten Reduktionen),
> dann existiert **Heuristica nicht** — d. h. DistNP ⊆ AvgP ⟹ NP ⊆ BPP.

`[NUR-SNIPPET]`, Konfidenz mittel-hoch. **Das ist eine bedingte Aussage**, und die Bedingung
(NP-Härte von GapMCSP) ist offen und unterliegt selbst den Konsequenzen aus §1.3.

### 2.3 Huang–Ilango–Ren, STOC 2023 — und eine wichtige Richtigstellung

**Richtigstellung zum Auftrag:** Die Arbeit *NP-Hardness of Approximating Meta-Complexity: A
Cryptographic Approach* (STOC 2023, DOI 10.1145/3564246.3585154, auch IACR ePrint 2023/528) stammt
von **Yizhi Huang, Rahul Ilango und Hanlin Ren** — **nicht** von Hirahara. Der Auftragstext legt
beide STOC-2023-Arbeiten Hirahara nahe; nur *Capturing One-Way Functions via NP-Hardness of
Meta-Complexity* (DOI 10.1145/3564246.3585130, ECCC TR23-037) ist von Hirahara.
`[VERIFIZIERT]` über STOC-2023-Accepted-Papers-Liste und zwei DOI-Einträge. Konfidenz hoch.

**Inhalt Huang–Ilango–Ren** `[NUR-SNIPPET]`, Konfidenz mittel-hoch:
Für jede Konstante c > 1 ist das **Minimum Oracle Circuit Size Problem (MOCSP)** NP-hart zu
approximieren (Ja-Instanzen: Komplexität ≤ s; Nein-Instanzen: ≥ s^c). Das Werkzeug ist eine
**Witness-Encryption**-Konstruktion nach Garg–Gentry–Sahai–Waters. Unter der Annahme
**subexponentiell sicherer injektiver Einwegfunktionen und subexponentiell sicherer Witness
Encryption** ist dasjenige Promise-Problem, dessen NP-Härte nach Hirahara Heuristica ausschließt,
tatsächlich NP-hart unter randomisierten Polynomialzeit-many-one-Reduktionen.

**Kritische Beobachtung** (`[EIGENE EINSCHÄTZUNG]`, Konfidenz mittel-hoch — dies ist der Punkt, den
das Papier herausarbeiten sollte):

> Die Annahme **impliziert die Konklusion bereits direkt.** Wenn subexponentiell sichere
> Einwegfunktionen existieren, dann ist NP im Durchschnitt hart, also befinden wir uns in
> Minicrypt oder Cryptomania — Heuristica ist damit *trivialerweise* ausgeschlossen. Der Wert des
> Resultats liegt also **nicht** im Ausschluss von Heuristica, sondern darin, dass es überhaupt
> gelingt, ein Meta-Komplexitätsproblem als NP-hart-zu-approximieren nachzuweisen, und dass dafür
> ausgerechnet Kryptographie das Werkzeug ist („it may seem counter-intuitive that cryptography
> could be helpful in proving black-box NP-completeness results" — Formulierung aus dem Abstract,
> `[NUR-SNIPPET]`).

**Einschränkung, die wir nicht auflösen konnten:** MOCSP ist die **Orakel**-Variante. Ob die
NP-Härte für MOCSP dasselbe Problem betrifft wie Hiraharas GapMCSP/GapMINKT, ließ sich ohne
Volltext **nicht** klären. Formulierung im Papier daher vorsichtig halten.

### 2.4 Hirahara STOC 2023: Charakterisierung von Einwegfunktionen

*Capturing One-Way Functions via NP-Hardness of Meta-Complexity*, STOC 2023, ECCC TR23-037.
`[VERIFIZIERT]` Existenz/Venue.

**Aussage** `[NUR-SNIPPET]`, Konfidenz mittel-hoch: erste Charakterisierung einer Einwegfunktion
durch **worst-case**-Härteannahmen. Eingeführt wird *distributional Kolmogorov complexity* (eine
Verallgemeinerung zeitbeschränkter bedingter Kolmogorovkomplexität). Dann:

> Eine Einwegfunktion existiert **genau dann**, wenn es NP-hart ist, die distributionelle
> Kolmogorovkomplexität unter randomisierten Polynomialzeitreduktionen zu approximieren, **und**
> NP im worst case hart ist.

Das ist die strukturell sauberste Brücke des Feldes: sie verbindet ein **kryptographisches** Objekt
(OWF) mit **worst-case**-NP-Härte über ein Meta-Komplexitätsproblem.

### 2.5 Liu–Pass: die sauberste verifizierte Äquivalenz

Y. Liu, R. Pass, *On One-Way Functions and Kolmogorov Complexity*, FOCS 2020 (arXiv:2009.11514).
`[VERIFIZIERT]` Existenz/Venue; Inhalt `[NUR-SNIPPET]`, Konfidenz hoch (über zwei Suchen konsistent).

> **Einwegfunktionen existieren ⟺ MK^tP (zeitbeschränkte Kolmogorovkomplexität) ist mild
> average-case-hart** (für jedes Polynom t(n) ≥ (1+ε)n: kein PPT-Algorithmus berechnet K^t auf mehr
> als einem (1 − 1/p(n))-Anteil der n-Bit-Strings).

Ergänzend: **BPP ≠ EXP** ist äquivalent zur *zero-sided* average-case-Härte von MK^tP, während die
Existenz von OWFs zur *two-sided* average-case-Härte äquivalent ist. `[NUR-SNIPPET]`, Konfidenz
mittel.

**Wichtig für unsere Fragestellung:** Diese Äquivalenz ist elegant und unbedingt (keine
Zusatzannahmen) — aber sie führt **nicht** zu P ≠ NP. Sie verschiebt die Frage „gibt es OWFs?" auf
„ist MK^tP average-case-hart?". Beide Seiten sind offen. `[EIGENE EINSCHÄTZUNG]`, Konfidenz hoch.

---

## 3. Hardness Magnification

### 3.1 Das Phänomen

**Oliveira–Santhanam, FOCS 2018** (*Hardness Magnification for Natural Problems*);
**Chen–Jin–Williams, FOCS 2019** (*Hardness Magnification for all Sparse NP Languages*, ECCC
TR19-118); **Oliveira–Pich–Santhanam, CCC 2019 / Theory of Computing 17(11)** (*Hardness
Magnification near State-of-the-Art Lower Bounds*). `[VERIFIZIERT]` für Existenz/Venues.

**Kernaussage** `[NUR-SNIPPET]`, Konfidenz hoch:

> Eine **n^{1+ε}-Schaltkreisuntere-Schranke** für MCSP[2^{o(m)}] oder MKtP[2^{o(m)}] impliziert
> **NP ⊄ P/poly**, **EXP ⊄ P/poly** bzw. **NP ⊄ NC¹**.

Verallgemeinert (Chen–Jin–Williams): Gibt es ein ε > 0 und eine 2^{n^{o(1)}}-**sparse** Sprache
L ∈ NP mit L ∉ Circuit[n^{1+ε}], dann hat NP keine Schaltkreise der Größe n^k für alle k.

### 3.2 Warum das faszinierend ist

Der Abstand zwischen dem, was verlangt wird (n^{1+ε} für *ein spezielles* Problem) und dem, was
folgt (NP ⊄ P/poly), ist grotesk. Zum Vergleich: die beste bekannte untere Schranke für die
Schaltkreisgröße einer expliziten Funktion über einer vollständigen Basis liegt bei etwa
(3 + 1/86)·n (Find–Golovnev–Hirsch–Kulikov, FOCS 2016) — also **linear**. Verlangt wird
**leicht superlinear**. Die Lücke wirkt wie ein Fingerbreit; sie ist offenbar ein Abgrund.

Ein zweiter Grund für die Faszination: **Magnification umgeht die natural-proofs-Barriere
strukturell.** Die Zielsprachen sind *sparse*; die Largeness-Bedingung von Razborov–Rudich greift
nicht. Damit war Magnification das erste ernsthafte Programm nach 1994, das eine der drei Barrieren
nicht durch Trickserei, sondern durch Wahl des Ziels aushebelte. `[EIGENE EINSCHÄTZUNG]` mit
Snippet-Stütze, Konfidenz mittel-hoch.

### 3.3 Warum es (bislang) scheitert: die *locality barrier*

**Chen, Hirahara, Oliveira, Pich, Rajgopal, Santhanam:** *Beyond Natural Proofs: Hardness
Magnification and Locality*, **ITCS 2020** (arXiv:1911.08297), Journalfassung **Journal of the ACM
69(4), 1–49, 2022** (DOI 10.1145/3538391). `[VERIFIZIERT]` für Existenz, Autoren, beide Venues
(drei konsistente Treffer, inkl. arXiv-Nummer und JACM-Band).

**Die Barriere** `[NUR-SNIPPET]`, Konfidenz mittel-hoch:

Die etablierten Techniken für untere Schranken (insbesondere die **Approximationsmethode**
Razborovs, aber auch Zufallsrestriktionen) sind **lokalisierbar**: die mit ihnen bewiesenen unteren
Schranken gelten automatisch auch gegen Schaltkreise, die zusätzlich **beliebig mächtige
Orakelgatter kleiner Fan-in** verwenden dürfen. Die Magnification-Reduktionen selbst lassen sich
aber **genau mit solchen Gattern** implementieren. Folge: Jede lokalisierbare Technik, die die
verlangte n^{1+ε}-Schranke liefern würde, würde sie auch gegen das orakelangereicherte Modell
liefern — und dort ist sie nachweislich falsch. **Keine lokalisierbare Technik kann die
Magnification-Schwelle erreichen.**

Ergänzend: *Localizability of the approximation method*, computational complexity (Springer),
DOI 10.1007/s00037-024-00257-0, 2024 — vertieft, dass die Approximationsmethode lokalisierbar ist.
`[NUR-SNIPPET]`, Konfidenz mittel.

### 3.4 Bewertung

`[EIGENE EINSCHÄTZUNG]`, Konfidenz hoch:

Hardness Magnification ist das schönste Beispiel dafür, wie das Feld funktioniert. Ein Resultat sagt:
„Eine winzige Verbesserung genügt." Kurz darauf sagt ein zweites Resultat: „Und genau deswegen kann
keine unserer Techniken diese winzige Verbesserung liefern." Die Magnification-Theoreme haben also
**keine neue Angriffsfläche erzeugt**, sondern eine bekannte Härte an eine unerwartete Stelle
verschoben und sie dort sichtbar gemacht. Das ist erkenntnistheoretisch wertvoll — und es ist
**kein Fortschritt Richtung P ≠ NP**.

Für die Leitachse aus `docs/02-team-briefing.md` §2: Die locality barrier ist ein **zusätzlicher,
nach 2018 gefundener Beleg für die Diagnose B3** („kein tragfähiger Lifting-Rahmen"). Die drei
klassischen Barrieren sind nicht die vollständige Liste; das Feld produziert **neue** Barrieren,
sobald ein neuer Rahmen entsteht.

---

## 4. Proof Complexity

### 4.1 Cook–Reckhow (kanonisch, `[VERIFIZIERT]`)

S. Cook, R. Reckhow, *The Relative Efficiency of Propositional Proof Systems*, JSL 44(1), 1979:

> **NP = coNP ⟺ es existiert ein polynomiell beschränktes Beweissystem für TAUT** (die Menge der
> aussagenlogischen Tautologien).

Da P = NP ⟹ NP = coNP, würde **NP ≠ coNP** auch **P ≠ NP** liefern. Das **Cook–Reckhow-Programm**
will NP ≠ coNP erreichen, indem es für immer stärkere Beweissysteme superpolynomielle untere
Schranken beweist. Es ist damit der einzige Angriffsweg auf P vs. NP, der **nicht** über
Schaltkreisuntere-Schranken läuft — und der daher von der natural-proofs-Barriere nicht in
derselben Weise betroffen ist.

### 4.2 Stand der unteren Schranken

| Beweissystem | Stand |
|---|---|
| **Resolution** | exponentielle untere Schranken — Haken 1985 (Taubenschlagprinzip), Chvátal–Szemerédi (zufällige CNF). **Erledigt.** |
| **Res(k)** | aktive Front; untere Schranken für zufällige CNF über Expansion (CCC 2025). |
| **Cutting Planes** | exponentielle untere Schranken bekannt (u. a. über *feasible interpolation*, Pudlák 1997). **Erledigt.** |
| **AC⁰-Frege** (bounded-depth Frege) | exponentielle untere Schranken (Ajtai 1988; Krajíček–Pudlák–Woods; Beame–Impagliazzo–Pitassi–Pudlák–Woods). **Erledigt** — aber: untere Schranken für **zufällige Δ-CNF** gelten als „das große langjährige offene Problem", sogar für nichtkonstantes Δ und Tiefe 2. |
| **AC⁰[p]-Frege** | **OFFEN.** Keine superpolynomiellen unteren Schranken bekannt. Direktes Analogon zur Situation bei ACC⁰-Schaltkreisen. |
| **TC⁰-Frege** | **OFFEN.** |
| **Frege** | **OFFEN.** Keine nichttriviale untere Schranke. |
| **Extended Frege** | **OFFEN.** Keine nichttriviale untere Schranke. Entspricht im Wesentlichen der Beweiskraft normaler mathematischer Argumentation über Polynomialzeitbegriffe. |

`[NUR-SNIPPET]` für die Detailzuschreibungen, Konfidenz mittel-hoch; `[VERIFIZIERT]` für die
Grobstruktur (schwache Systeme erledigt, Frege/EF offen) — das ist Konsenswissen und wurde über
mehrere Suchen bestätigt.

**Wichtiger struktureller Befund** `[NUR-SNIPPET]`, Konfidenz mittel-hoch:
Für **schwache** Systeme (Resolution, Cutting Planes) gibt es *feasible interpolation*: man leitet
Beweiskomplexitätsuntere-Schranken aus Schaltkreisunteren-Schranken ab. Für **starke** Systeme
(Frege, Extended Frege) sind **in keiner Richtung** Implikationen zu Schaltkreisunteren-Schranken
bekannt. Der Cook–Reckhow-Weg ist also ab Frege ein *eigenständiges* Problem, keine Umformulierung
des Schaltkreisproblems. Das ist zugleich sein Reiz und sein Fluch.

### 4.3 IPS (Ideal Proof System)

Grochow–Pitassi führten IPS ein: ein algebraisches Beweissystem, in dem eine Widerlegung ein
algebraischer Schaltkreis ist. Die Verbindung: **superpolynomielle untere Schranken gegen IPS
implizieren algebraische Schaltkreisuntere-Schranken** (VNP ≠ VP-artig) — IPS ist damit ein
Beweissystem, dessen Härte an Valiants Programm gekoppelt ist. `[NUR-SNIPPET]`, Konfidenz mittel.

**Neue Resultate (2025/2026):**

- **Elbaz, Govindasamy, Lu, Tzameret**, *Lower Bounds against the Ideal Proof System in Finite
  Fields*, arXiv:2506.17210, **STOC 2026**, DOI 10.1145/3798129.3800721. `[VERIFIZIERT]` für
  Existenz/Venue/DOI (zwei Treffer konsistent).
  **Inhalt** `[NUR-SNIPPET]`, Konfidenz mittel: untere Schranken über **festen endlichen Körpern**
  — bislang gab es sie nur über großen Körpern bzw. in Charakteristik 0. Konkret: eine Variante der
  Knapsack-Instanz hat keine polynomiell großen IPS-Widerlegungen über endlichen Körpern, wenn die
  Widerlegung **multilinear** und als **Schaltkreis konstanter Tiefe** geschrieben ist.
- **Die eigentliche Pointe** `[NUR-SNIPPET]`, Konfidenz mittel-hoch:
  > Jede untere Schranke gegen ein Beweissystem, das mindestens so stark ist wie **constant-depth
  > IPS über endlichen Körpern**, impliziert eine **AC⁰[p]-Frege-untere Schranke**.

  Das ist die exakte Erklärung, warum hier eine Mauer steht: constant-depth IPS über 𝔽_p simuliert
  AC⁰[p]-Frege, und AC⁰[p]-Frege ist wide open (§4.2). Die Fortschritte über endlichen Körpern
  betreffen genau die Fragmente **unterhalb** dieser Schwelle.
- **arXiv:2506.16397**, *New Bounds for the Ideal Proof System in Positive Characteristic*
  `[PREPRINT]`: obere und untere Schranken für IPS-Fragmente in positiver Charakteristik; die
  unteren Schranken gelten für beliebige Charakteristik, verlangen aber **Körpergröße n^{ω(1)}** —
  also gerade *nicht* konstant. `[NUR-SNIPPET]`, Konfidenz mittel.
- Ergänzend: *AC⁰[p]-Frege Cannot Efficiently Prove that Constant-Depth Algebraic Circuit Lower
  Bounds are Hard*, arXiv:2509.16824 `[PREPRINT]` — gehört thematisch zu §5 (Metamathematik).
- *IPS Lower Bounds for Formulas and Sum of ROABPs*, FSTTCS 2025,
  DOI 10.4230/LIPIcs.FSTTCS.2025.22 `[NUR-SNIPPET]`.

**Bewertung** `[EIGENE EINSCHÄTZUNG]`, Konfidenz mittel-hoch: Die IPS-Front bewegt sich real, aber
sie bewegt sich **auf die Schwelle zu**, nicht darüber hinaus. Das Muster ist identisch zu §3:
Sobald ein Fragment stark genug wird, um interessant zu sein, kollabiert die Aufgabe in ein bekannt
offenes Problem (hier AC⁰[p]-Frege).

### 4.4 Refuter-Probleme (STOC 2026)

**Jiawei Li (UT Austin), Yuhao Li (Columbia), Hanlin Ren (IAS):**
*Finding Bugs in Short Proofs: The Metamathematics of Resolution Lower Bounds*, **STOC 2026**,
DOI 10.1145/3798129.3800793; Preprintfassung u. a. arXiv:2411.15515 (*Metamathematics of Resolution
Lower Bounds: A TFNP Perspective*). `[VERIFIZIERT]` für Existenz/Venue/DOI/Autoren.

**Idee** `[NUR-SNIPPET]`, Konfidenz mittel-hoch: Untere Schranken der Beweiskomplexität sagen, dass
*kein* kurzer Beweis existiert. Das **Refuter-Problem** macht daraus eine Suchaufgabe: Gegeben
Query-Zugriff auf einen *angeblichen* Beweis der Länge s für eine harte Tautologie — **finde einen
ungültigen Ableitungsschritt**. Die Komplexität dieser Suchaufgabe ist eng an die **Metamathematik**
der zugrundeliegenden unteren Schranke gekoppelt; die Analyse erfolgt im TFNP-Rahmen.

**Best-Paper-Status:** Die Columbia-CS-Meldung „The Theory Group Wins Big at STOC 2026" berichtet
von **zwei** Best Paper Awards bei STOC 2026 und nennt diese Arbeit im Umfeld. Ob sie **einen der
beiden Awards erhielt**, konnte ich **nicht sauber verifizieren** — siehe §7.
`[CLAIM]`, Konfidenz niedrig-mittel für die Award-Zuschreibung, hoch für Existenz und Venue.

**Warum relevant für uns** `[EIGENE EINSCHÄTZUNG]`, Konfidenz mittel: Refuter-Probleme sind der
Versuch, aus einer **nicht-konstruktiven** Aussage („es gibt keinen kurzen Beweis") einen
**konstruktiven** Kern zu extrahieren. Das ist genau die Bewegungsrichtung, die die Metamathematik
(§5) für Barriereresultate interessant macht: Wenn eine untere Schranke nur nicht-konstruktiv
beweisbar ist, ist das ein Indiz auf die Beweisstärke, die man für sie braucht.

### 4.5 Extended Frege → P ≠ NP: Statusupdate zu A1

`research/a1-kanon-und-barrieren.md` §455 führt arXiv:2312.08163 (*Towards P ≠ NP from Extended
Frege lower bounds*) als `[PREPRINT]`. **Update:** Die Arbeit ist inzwischen im
**Journal of the ACM** erschienen, DOI 10.1145/3801091. `[VERIFIZIERT]` für die Journalfassung
(DOI-Treffer). **Inhalt konnte ich nicht verifizieren** — siehe §7. Vorsicht ist geboten: Der Titel
sagt „Towards", nicht „From"; es handelt sich mit hoher Wahrscheinlichkeit um eine **bedingte**
Implikation mit Zusatzannahmen, nicht um einen Weg, der nur noch abgelaufen werden muss.
`[EIGENE EINSCHÄTZUNG]`, Konfidenz mittel.

---

## 5. Unabhängigkeit und Metamathematik

### 5.1 Die Warnung vorweg

Zu „P vs. NP ist unentscheidbar/unabhängig" existiert eine große populärwissenschaftliche und
Crank-Literatur (Beispiel aus der Trefferliste: arXiv:2404.00468, *On P=NP Either False or
Independent of ZFC* — `[CLAIM]`, ungeprüft, kein Venue erkennbar). Die für unser Papier
entscheidende Unterscheidung:

> **„nicht bekannt unabhängig" ≠ „unabhängig".**
> Es ist **kein** Resultat bekannt, das P vs. NP als unabhängig von ZFC oder auch nur von
> Peano-Arithmetik erweist. Was es gibt, sind (a) Unabhängigkeitsresultate für **sehr schwache**
> Theorien, (b) **bedingte** Unbeweisbarkeitsresultate, und (c) Argumente, die Unabhängigkeit
> **unwahrscheinlich** machen.

### 5.2 Razborov: was genau gezeigt ist

**A. A. Razborov**, *Unprovability of lower bounds on circuit size in certain fragments of bounded
arithmetic*, **Izvestiya: Mathematics 59(1), 205–227, 1995**. `[VERIFIZIERT]` für Existenz, Venue,
Band, Seiten (Mathnet-Eintrag + ADS-Eintrag konsistent).

**Die Aussage** `[NUR-SNIPPET]`, Konfidenz mittel-hoch:

> Wenn **starke Pseudozufallsgeneratoren existieren**, dann ist die Aussage
> „α kodiert einen Schaltkreis der Größe n^{log* n} für SATISFIABILITY"
> **nicht widerlegbar in S₂²(α)**.
> Für S₂¹(α) genügt die schwächere Annahme der Existenz von Generatoren, die gegen
> Schaltkreise kleiner Tiefe sicher sind. Für ein weiteres System, das stark genug ist, um
> exponentielle untere Schranken für Schaltkreise konstanter Tiefe zu beweisen, gilt es **ohne
> unbewiesene Härteannahme**.

**Was das präzise bedeutet — und was nicht** (`[EIGENE EINSCHÄTZUNG]`, Konfidenz hoch, dies ist der
Punkt, an dem in der Popularisierung am meisten schiefgeht):

1. **Die Theorie ist winzig.** S₂² bzw. S₂¹ sind Fragmente der *bounded arithmetic*. Sie sind
   dramatisch schwächer als PA, und PA ist wiederum dramatisch schwächer als ZFC. Ein
   Unbeweisbarkeitsresultat in S₂²(α) sagt über ZFC **nichts**.
2. **Die Theorie ist relativiert.** Das α ist ein freies Prädikatensymbol; S₂²(α) ist die
   relativierte Theorie. Das ist die logische Entsprechung einer Orakelrelativierung — und
   Relativierung ist eine *bekannte* Barriere, keine neue Erkenntnis über die absolute Härte.
3. **Das Resultat ist bedingt** (außer im dritten Fall): Es setzt starke PRGs voraus. Das ist eine
   kryptographische Annahme, die selbst P ≠ NP impliziert. Die Aussage lautet also im Kern:
   *Wenn die Welt kryptographisch hart ist, dann kann eine schwache Theorie das nicht sehen.*
4. **Es geht nicht um „P ≠ NP", sondern um Schaltkreisuntere-Schranken** in einer spezifischen
   Formalisierung mit einer spezifischen Größenschranke (n^{log* n}).

**Der eigentliche Gehalt** (Konfidenz hoch): Razborovs Resultat ist die **logische Schwester der
natural-proofs-Barriere**. Beide sagen: Kryptographische Härte begrenzt, was *schwache,
konstruktive* Methoden über Härte beweisen können. Es ist eine Aussage über **Beweismittel**, nicht
über **Wahrheit** — genau wie die drei Barrieren (vgl. A1 §288).

### 5.3 Neuere metamathematische Arbeiten

- **Pich, Santhanam**, *Unprovability of Strong Complexity Lower Bounds in Bounded Arithmetic*,
  arXiv:2305.15235. `[PREPRINT]` / `[NUR-SNIPPET]`, Venue nicht verifiziert. Thematisch:
  Unbeweisbarkeit *starker* unterer Schranken in Theorien wie PV₁/APC₁.
- **Krajíček, Oliveira**, *Unprovability of circuit upper bounds in Cook's theory PV*,
  arXiv:1605.00263. `[NUR-SNIPPET]`.
- *On the Unprovability of Circuit Size Bounds in Intuitionistic S₂¹*, arXiv:2404.11841.
  `[PREPRINT]` / `[NUR-SNIPPET]`.
- **Pich**, *Circuit Lower Bounds in Bounded Arithmetics* (Annals of Pure and Applied Logic).
  `[NUR-SNIPPET]`.
- **Kolokolova**, *Complexity Barriers as Independence*, 2016 — programmatische Übersicht zum
  Zusammenhang Barrieren ↔ Unbeweisbarkeit in schwachen Theorien. `[NUR-SNIPPET]`.

**Gemeinsamer Nenner** (`[EIGENE EINSCHÄTZUNG]`, Konfidenz mittel-hoch): Diese Literatur zeigt
durchweg Unbeweisbarkeit in **bounded arithmetic** — also in Theorien, deren Beweiskraft ungefähr
dem Polynomialzeitdenken entspricht. Sie zeigt **nie** Unabhängigkeit von PA oder ZFC. Der
Erkenntnisgewinn ist: *Ein Beweis von P ≠ NP wird Mittel brauchen, die über Polynomialzeitdenken
hinausgehen.* Das ist informativ und es ist weit entfernt von „unentscheidbar".

### 5.4 Aaronsons Survey und das Ben-David–Halevi-Argument

**S. Aaronson**, *Is P Versus NP Formally Independent?*, Bulletin of the EATCS, Column 81 (ca. 2003).
`[VERIFIZIERT]` für Existenz (mehrere unabhängige Hostings). Inhalt `[NUR-SNIPPET]`.

**Aaronsons Fazit** (Zitat aus Snippet, Konfidenz mittel-hoch für den Wortlaut):
„one of the few definite conclusions of this survey, that P ≠ NP is either true or false."
Das ist bewusst lakonisch: Aaronson argumentiert gegen die Erwartung formaler Unabhängigkeit.

**Das technisch stärkste Argument gegen Unabhängigkeit — Ben-David & Halevi**
`[NUR-SNIPPET]`, Konfidenz mittel:

> Wäre P vs. NP unabhängig von **PA erweitert um alle im Standardmodell wahren Π₁-Sätze**, so wäre
> NP „im Wesentlichen Polynomialzeit" — NP läge in einer Zeitklasse mit extrem langsam wachsendem
> Exponenten.

**Warum das zählt** (`[EIGENE EINSCHÄTZUNG]`, Konfidenz mittel-hoch): Unabhängigkeit von einer sehr
starken Theorie wäre also **fast so gut wie P = NP** — praktisch fast dasselbe. Wer Unabhängigkeit
als bequemen Ausweg („dann müssen wir es nicht lösen") benutzt, übersieht, dass dieser Ausweg eine
mindestens so überraschende algorithmische Konsequenz hätte wie die Auflösung selbst. Die
Unabhängigkeitshypothese ist **keine Flucht aus dem Problem**.

Hinzu kommt ein struktureller Punkt: P ≠ NP ist ein Π₂-Satz (grob: „für alle Algorithmen existiert
eine Eingabe…"), P = NP dagegen ein Σ₂-Satz mit stark Σ₁-artigem Kern — wäre P = NP wahr, wäre der
Zeuge ein konkreter Algorithmus plus Laufzeitschranke, und dessen Korrektheit müsste beweisbar sein,
sofern sie überhaupt gilt. Unabhängigkeit müsste sich also an der **Π₂**-Seite abspielen.
`[EIGENE EINSCHÄTZUNG]`, Konfidenz mittel — dies ist eine Skizze, kein Resultat, und im Papier
entsprechend zu kennzeichnen.

### 5.5 Wie ernst nimmt die Fachwelt das? (Gasarch-Umfragen)

`[NUR-SNIPPET]`, Konfidenz mittel — und mit ausdrücklichem Vorbehalt nach `02-team-briefing.md` §1
(belegter Fall widersprüchlicher Umfragezahlen).

Aus einer Suchantwort, Anteil „wird **nie** gelöst":
- 2002: **5 %**
- 2012: **3 %**
- 2019: **9 %**

Dieselbe Suchantwort berichtet für 2019, **niemand** habe Unabhängigkeit (von ZFC) für wahrscheinlich
gehalten — also **0 %**. `[NUR-SNIPPET]`, Konfidenz **niedrig-mittel**: Diese Zahl konnte ich **nicht
triangulieren**; sie stammt aus einer einzigen Suchantwort. Die Zahlenreihe 5/3/9 stammt ebenfalls
aus einer einzigen Antwort. **Vor Verwendung im Papier ist eine unabhängige Prüfung durch A1
(Umfragezuständigkeit) erforderlich.** Siehe §7.

Quellen für die Umfragen selbst (Existenz `[VERIFIZIERT]`): Gasarch, *Guest Column: The P=?NP Poll*,
SIGACT News 2002; *Guest Column: The Second P =? NP Poll*, SIGACT News 2012; *Guest Column: The Third
P =? NP Poll*, SIGACT News Complexity Theory Column 100, 2019.

---

## 6. Bewertung: Implikationsketten nach Erfolgsaussicht

### 6.1 Unbedingte Implikationen (keine Zusatzannahmen)

Diese Ketten gelten **ohne** kryptographische Annahmen. Sie sind das Rückgrat — und zugleich zeigen
sie, warum nichts vorangeht.

| # | Kette | Status | Konfidenz |
|---|---|---|---|
| U1 | NP ≠ coNP ⟹ P ≠ NP; NP ≠ coNP ⟺ kein p-beschränktes Beweissystem für TAUT (Cook–Reckhow 1979) | offen ab Frege | hoch |
| U2 | NP ⊄ P/poly ⟹ P ≠ NP | offen | hoch |
| U3 | n^{1+ε}-Schranke für MCSP[2^{o(m)}] ⟹ NP ⊄ P/poly (Magnification) | durch **locality barrier** blockiert | hoch |
| U4 | SAT ≤ₘᵖ MCSP (deterministisch) ⟹ EXP ≠ ZPP (Murray–Williams) | zeigt: NP-Härte von MCSP ist selbst ein Durchbruch | hoch |
| U5 | OWF existieren ⟺ MK^tP mild average-case-hart (Liu–Pass) | bewiesen, führt aber nicht zu P ≠ NP | hoch |

### 6.2 Bedingte Implikationen (kryptographische Zusatzannahmen)

| # | Kette | Annahme | Konfidenz |
|---|---|---|---|
| C1 | GapMCSP/GapMINKT NP-hart ⟹ **Heuristica existiert nicht** (Hirahara FOCS 2018) | Prämisse offen; Reduktionen randomisiert | mittel-hoch |
| C2 | MOCSP NP-hart zu approximieren (Huang–Ilango–Ren STOC 2023) | subexp. sichere injektive OWF **und** subexp. sichere Witness Encryption | mittel-hoch |
| C3 | OWF existiert ⟺ NP-Härte der Approximation distributioneller Kolmogorovkomplexität **und** worst-case-Härte von NP (Hirahara STOC 2023) | Charakterisierung, keine Auflösung | mittel |
| C4 | Razborov 1995: Unbeweisbarkeit gewisser Schaltkreisschranken in S₂²(α) | **starke PRGs** | mittel-hoch |
| C5 | GapMCSP **nicht** NP-vollständig unter randomisierten Levin-Reduktionen | **iO** + subexp. OWF | niedrig-mittel (nicht trianguliert) |
| C6 | SAT ≤ MCSP^O für zufälliges O mit Wahrscheinlichkeit 1 | **Relativierung** — keine kryptographische, aber eine **modelltheoretische** Zusatzannahme | mittel-hoch |

**Wichtigste Beobachtung zu C2** (`[EIGENE EINSCHÄTZUNG]`, Konfidenz mittel-hoch, vgl. §2.3): Die
Annahme in C2 schließt Heuristica bereits direkt aus. Das Resultat ist als
Meta-Komplexitäts-Fortschritt wertvoll, **nicht** als Welt-Ausschluss. Das ist im Papier explizit zu
sagen, sonst entsteht ein Zirkelschluss-Eindruck.

**Wichtigste Beobachtung zu C4/C6:** Beide „Zusatzannahmen" sind von unterschiedlicher Natur —
C4 ist eine Härteannahme (macht die Welt *schwerer*), C6 ist eine Modellwechsel-Annahme
(ändert das *Problem*). Die zweite Sorte ist gefährlicher, weil sie leicht als Fortschritt am
ursprünglichen Problem missverstanden wird.

### 6.3 Rangliste der Angriffslinien

`[EIGENE EINSCHÄTZUNG]`, jeweils mit Konfidenz. Kriterium: Wahrscheinlichkeit, in den nächsten
10–15 Jahren einen *echten* Fortschritt Richtung P ≠ NP zu liefern.

**Rang 1 — Meta-Komplexität (Hirahara-Programm).** Konfidenz mittel.
Einziges Teilfeld mit belegter Serie überwundener Barrieren (Kos Relativierungsbarriere, black-box
worst-case-to-average-case). Realistisches Nahziel ist aber nicht P ≠ NP, sondern der **Ausschluss
von Heuristica und Pessiland** — die Landkarte der fünf Welten zu verkleinern. Die Grenze zu totalem
MCSP (§1.4) ist die entscheidende Blockade, und sie ist qualitativ.

**Rang 2 — Proof Complexity / Cook–Reckhow.** Konfidenz niedrig-mittel.
Vorteil: einziger Weg, der nicht über Schaltkreisuntere-Schranken führt, also nicht in derselben
Weise von natural proofs betroffen ist. Nachteil: seit ~1990 an derselben Stelle blockiert
(AC⁰[p]-Frege, Frege, Extended Frege). Die IPS-Fortschritte 2025/26 bewegen sich auf die bekannte
Schwelle zu und kollabieren dort in AC⁰[p]-Frege (§4.3). Der refuter-/TFNP-Zugang (§4.4) ist
konzeptionell neu, aber noch ohne Schrankenertrag.

**Rang 3 — Hardness Magnification.** Konfidenz niedrig.
Das Programm hat eine eigene Barriere hervorgebracht (locality), bevor es einen Ertrag lieferte.
Solange kein nicht-lokalisierbares Verfahren existiert, ist die Linie blockiert. Der Wert liegt in
der Einsicht, nicht im Fortschritt.

**Rang 4 — Unabhängigkeit / Metamathematik.** Konfidenz niedrig als *Auflösungsweg*, mittel als
*Diagnoseinstrument*.
Niemand erwartet ernsthaft einen Unabhängigkeitsbeweis von ZFC; das Ben-David–Halevi-Argument macht
ihn sogar unattraktiv (§5.4). Wertvoll ist die Linie als **Kalibrierung**: Sie sagt uns, dass ein
Beweis über Polynomialzeitdenken hinaus muss.

**Rang 5 — direkte Schaltkreisuntere-Schranken.** Konfidenz sehr niedrig (Zuständigkeit A1).
Der Stand bei expliziten Funktionen ist linear (~3,1n). Der Abstand zu superpolynomiell ist nicht
überbrückbar mit bekannten Techniken.

### 6.4 Antwort auf die Leitachse (`02-team-briefing.md` §2)

**Befund: Die Leitachse wird bestätigt, und mein Bereich liefert den schärfsten Beleg für B3.**

- **B1 (endliches, repräsentierbares Objekt):** In der Meta-Komplexität ist das gesuchte Objekt
  *eine Reduktion* oder *eine untere Schranke* — beides keine endlichen Objekte fester Größe,
  sondern Konstruktionen über einer unendlichen Familie von Eingabelängen, deren Korrektheit
  bewiesen werden muss. **B1 verletzt.**
- **B2 (billiges Verifikationsorakel):** Es gibt keines. Zu prüfen wäre „diese Reduktion ist
  korrekt" — eine Π₂-Aussage über alle Instanzen. **B2 verletzt.**
- **B3 (menschlich bewiesener Lifting-Rahmen):** Hier ist der schärfste Befund. Meta-Komplexität
  *hat* Lifting-Rahmen — Magnification ist buchstäblich einer („kleine Schranke ⟹ große Schranke").
  Und genau dieser Rahmen wurde binnen zwei Jahren durch die **locality barrier** als unbenutzbar
  für alle bekannten Techniken erwiesen. **Das ist kein Fehlen eines Rahmens, sondern ein
  widerlegter Rahmen** — eine stärkere Aussage als „B3 nicht erfüllt".

**Präzisierungsvorschlag an die Leitung** (`[EIGENE EINSCHÄTZUNG]`, Konfidenz mittel-hoch): Das
Briefing formuliert B3 als „es existiert ein von Menschen bewiesener Lifting-Rahmen". Mein Bereich
legt eine Verschärfung nahe:

> **B3′:** Der Lifting-Rahmen muss nicht nur existieren und bewiesen sein — er muss auch mit den
> Techniken kompatibel sein, die das endliche Objekt liefern sollen. Magnification erfüllt B3 im
> Wortsinn und ist trotzdem unbrauchbar, weil die Techniken, die die Eingangsschranke liefern
> könnten, lokalisierbar sind und der Rahmen Lokalisierbarkeit nicht verträgt.

Das ist relevant für das ganze Papier: Ein KI-System, das nach dem AlphaEvolve-Muster ein endliches
Objekt sucht, würde bei Magnification an **B1/B2** scheitern (es gibt kein endliches Zielobjekt und
kein billiges Orakel) — und selbst bei perfektem Erfolg wäre der Lifting-Schritt durch die
locality barrier blockiert. **Zwei unabhängige Ausfälle, nicht einer.**

**Kein Gegenbefund zur Leitachse gefunden.** Konfidenz hoch.

---

## 7. Was ich NICHT verifizieren konnte

Explizite Negativliste, wie in `00-briefing.md` §4 gefordert:

1. **Autorenschaft von *SAT Reduces to MCSP with a Random Oracle*.** Existenz, Venue (FOCS 2023),
   ECCC-Nummer und SIAM-DOI sind belegt; die Autorennamen konnte ich über Snippets **nicht**
   feststellen.
2. **Die Widerlegung der Random Oracle Hypothesis** (IP vs. PSPACE relativ zu Zufallsorakel). Ich
   halte das für korrektes Kanonwissen, konnte es aber in dieser Sitzung **nicht triangulieren**.
   Vor Verwendung prüfen (Chang, Chor, Goldreich, Hartmanis, Håstad, Ranjan, Rohatgi, ca. 1994 —
   Zuschreibung **ungeprüft**).
3. **Inhalt von *Towards P ≠ NP from Extended Frege lower bounds*** (JACM, DOI 10.1145/3801091).
   Nur Existenz und Journalfassung belegt. Autorenschaft (vermutlich Pich/Santhanam) **ungeprüft**.
4. **Best-Paper-Status** der STOC-2026-Refuter-Arbeit. Zwei Best Papers bei STOC 2026 sind belegt;
   die Zuordnung zu dieser Arbeit **nicht**.
5. **Gasarch-Zahlen** „nie gelöst" (5 % / 3 % / 9 %) und „0 % Unabhängigkeit 2019": je **eine**
   Suchantwort, **nicht trianguliert**. Das Briefing warnt für genau diese Umfrage vor
   widersprüchlichen Zahlen. Abstimmung mit A1 erforderlich.
6. **iO ⟹ GapMCSP nicht NP-vollständig unter randomisierten Levin-Reduktionen** (C5): eine
   Suchantwort, Autorenschaft unbekannt.
7. **Ob MOCSP dasselbe Problem betrifft wie Hiraharas GapMCSP/GapMINKT** (§2.3) — ohne Volltext
   nicht klärbar.
8. **Exakte Formalisierung** in Razborov 1995 (was genau „α kodiert einen Schaltkreis der Größe
   n^{log* n} für SAT" in S₂²(α) bedeutet). Nur die Abstract-Ebene ist belegt.
9. **Die genaue Liste der erledigten vs. offenen Beweissysteme in §4.2** in ihren
   Einzelzuschreibungen (wer hat wann welche Schranke bewiesen). Grobstruktur ist Konsenswissen,
   Details `[NUR-SNIPPET]`.

---

## 8. Quellen

Alle URLs aus WebSearch-Trefferlisten; **kein Volltext abgerufen** (WebFetch gesperrt).

### Meta-Komplexität / MCSP
- Hirahara, *NP-Hardness of Learning Programs and Partial MCSP*, FOCS 2022, S. 968–979 —
  https://dblp.org/rec/conf/focs/Hirahara22.html ; https://eccc.weizmann.ac.il/report/2022/119/
- *SAT Reduces to the Minimum Circuit Size Problem with a Random Oracle*, FOCS 2023;
  ECCC TR23-165 — https://eccc.weizmann.ac.il/report/2023/165/ ;
  SIAM J. Comput., DOI 10.1137/24M1652568 — https://doi.org/10.1137/24M1652568
- Murray, Williams, *On the (Non) NP-Hardness of Computing Circuit Complexity*, Theory of Computing
  13(4) — https://theoryofcomputing.org/articles/v013a004/v013a004.pdf ;
  ECCC TR14-164 — https://eccc.weizmann.ac.il/report/2014/164/
- Ilango, *NP-Hardness of Circuit Minimization for Multi-Output Functions*, CCC 2020,
  DOI 10.4230/LIPIcs.CCC.2020.22 —
  https://drops.dagstuhl.de/storage/00lipics/lipics-vol169-ccc2020/LIPIcs.CCC.2020.22/LIPIcs.CCC.2020.22.pdf
- Ilango, *Constant Depth Formula and Partial Function Versions of MCSP are Hard*, FOCS 2020 —
  https://www.rahulilango.com/papers/FOCS2020.pdf
- Hirahara, *Limits of Minimum Circuit Size Problem as Oracle*, CCC 2016 —
  https://drops.dagstuhl.de/storage/00lipics/lipics-vol050-ccc2016/LIPIcs.CCC.2016.18/LIPIcs.CCC.2016.18.pdf
- Simons Institute, *Meta-Complexity Open Problems* (2023) —
  https://wiki.simons.berkeley.edu/lib/exe/fetch.php?media=mc23%3A91meta-complexity_open-problems.pdf

### Heuristica / Average-Case / Einwegfunktionen
- Hirahara, *Non-Black-Box Worst-Case to Average-Case Reductions within NP*, FOCS 2018;
  SIAM J. Comput., DOI 10.1137/19M124705X — https://eccc.weizmann.ac.il/report/2018/138/
- Huang, Ilango, Ren, *NP-Hardness of Approximating Meta-Complexity: A Cryptographic Approach*,
  STOC 2023, DOI 10.1145/3564246.3585154 — https://eprint.iacr.org/2023/528.pdf
- Hirahara, *Capturing One-Way Functions via NP-Hardness of Meta-Complexity*, STOC 2023,
  DOI 10.1145/3564246.3585130 — https://eccc.weizmann.ac.il/report/2023/037/
- Liu, Pass, *On One-Way Functions and Kolmogorov Complexity*, FOCS 2020 —
  https://arxiv.org/abs/2009.11514
- *Impagliazzo's Worlds Through the Lens of Conditional Kolmogorov Complexity*, ICALP 2024 —
  https://drops.dagstuhl.de/storage/00lipics/lipics-vol297-icalp2024/LIPIcs.ICALP.2024.110/LIPIcs.ICALP.2024.110.pdf
- Quanta Magazine, *Which Computational Universe Do We Live In?* (2022) —
  https://www.quantamagazine.org/which-computational-universe-do-we-live-in-20220418/

### Hardness Magnification
- Oliveira, Santhanam, *Hardness Magnification for Natural Problems*, FOCS 2018 —
  https://ieee-focs.org/FOCS-2018-Papers/pdfs/59f065.pdf
- Chen, Jin, Williams, *Hardness Magnification for all Sparse NP Languages*, FOCS 2019;
  ECCC TR19-118 — https://eccc.weizmann.ac.il/report/2019/118/
- Oliveira, Pich, Santhanam, *Hardness Magnification near State-of-the-Art Lower Bounds*, CCC 2019 /
  Theory of Computing 17(11) — https://theoryofcomputing.org/articles/v017a011/v017a011.pdf
- Chen, Hirahara, Oliveira, Pich, Rajgopal, Santhanam, *Beyond Natural Proofs: Hardness
  Magnification and Locality*, ITCS 2020; JACM 69(4), 2022, DOI 10.1145/3538391 —
  https://arxiv.org/abs/1911.08297 ; https://dl.acm.org/doi/10.1145/3538391
- *Localizability of the approximation method*, computational complexity (2024),
  DOI 10.1007/s00037-024-00257-0 — https://link.springer.com/article/10.1007/s00037-024-00257-0

### Proof Complexity
- Li, Li, Ren, *Finding Bugs in Short Proofs: The Metamathematics of Resolution Lower Bounds*,
  STOC 2026, DOI 10.1145/3798129.3800793 — https://doi.org/10.1145/3798129.3800793 ;
  Preprint: https://arxiv.org/html/2411.15515v2
- Elbaz, Govindasamy, Lu, Tzameret, *Lower Bounds against the Ideal Proof System in Finite Fields*,
  STOC 2026, DOI 10.1145/3798129.3800721 — https://arxiv.org/abs/2506.17210
- *New Bounds for the Ideal Proof System in Positive Characteristic* —
  https://arxiv.org/abs/2506.16397
- *IPS Lower Bounds for Formulas and Sum of ROABPs*, FSTTCS 2025,
  DOI 10.4230/LIPIcs.FSTTCS.2025.22
- *Towards P ≠ NP from Extended Frege lower bounds*, JACM, DOI 10.1145/3801091 —
  https://doi.org/10.1145/3801091 ; Preprint https://arxiv.org/pdf/2312.08163
- *A Lower Bound for k-DNF Resolution on Random CNF Formulas via Expansion*, CCC 2025 —
  https://drops.dagstuhl.de/storage/00lipics/lipics-vol339-ccc2025/LIPIcs.CCC.2025.32/LIPIcs.CCC.2025.32.pdf
- Columbia CS, *The Theory Group Wins Big at STOC 2026* —
  https://www.cs.columbia.edu/2026/the-theory-group-wins-big-at-stoc-2026/
- STOC 2026 Accepted Papers — https://acm-stoc.org/stoc2026/accepted-papers.html

### Unabhängigkeit / Metamathematik
- Razborov, *Unprovability of lower bounds on circuit size in certain fragments of bounded
  arithmetic*, Izv. Math. 59(1), 205–227, 1995 —
  https://www.mathnet.ru/php/archive.phtml?wshow=paper&jrnid=im&paperid=9&option_lang=eng
- Pich, Santhanam, *Unprovability of Strong Complexity Lower Bounds in Bounded Arithmetic* —
  https://arxiv.org/pdf/2305.15235
- Krajíček, Oliveira, *Unprovability of circuit upper bounds in Cook's theory PV* —
  https://arxiv.org/abs/1605.00263
- *On the Unprovability of Circuit Size Bounds in Intuitionistic S₂¹* — https://arxiv.org/pdf/2404.11841
- *AC⁰[p]-Frege Cannot Efficiently Prove that Constant-Depth Algebraic Circuit Lower Bounds are
  Hard* — https://arxiv.org/pdf/2509.16824
- Aaronson, *Is P Versus NP Formally Independent?*, BEATCS Column 81 —
  https://www.scottaaronson.com/papers/indep.pdf
- Kolokolova, *Complexity Barriers as Independence* (2016) —
  https://www.cs.mun.ca/~kol/papers/barriers-incomputable-revised.pdf
- Gasarch, *The P=?NP Poll* (2002) — https://www.cs.umd.edu/~gasarch/papers/poll.pdf
- Gasarch, *The Third P=?NP Poll* (2019) —
  https://www.cs.umd.edu/users/gasarch/BLOGPAPERS/pollpaper3.pdf
