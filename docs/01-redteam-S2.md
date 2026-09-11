# S2 — Red Team / Devil's Advocate: Bericht

**Agent:** S2 (institutionalisierter Skeptiker)
**Datum:** 11. September 2026
**Auftrag:** Das Projekt vorab zum Scheitern bringen — auf dem Papier, damit es das in der Realität nicht tut.
**Grundlage:** `docs/00-briefing.md` (Grundregeln 1–6 verbindlich befolgt)

---

## 0. Methodenvorbehalt (bitte zuerst lesen)

In dieser Umgebung ist `WebFetch` durch den Egress-Proxy für praktisch alle relevanten
Domains gesperrt: `arxiv.org`, `export.arxiv.org`, `link.springer.com`,
`www.semanticscholar.org`, `huggingface.co`, `journal.hep.com.cn`, `d-nb.info`,
`people.cs.rutgers.edu`, `scottaaronson.blog`, `rjlipton.com`, `en.wikipedia.org`,
`mikinty.github.io`. Auch `curl` bekommt HTTP 403 vom Proxy (Organisationspolicy,
kein Umgehungsversuch unternommen). Der GitHub-MCP-Zugriff ist auf das Projekt-Repo
beschränkt.

**Nutzbar war ausschließlich `WebSearch`** (Titel, URLs, synthetisierte Snippets).

Konsequenzen, die im Abschlusspapier offengelegt werden müssen:

- Alles, was ich nur über Suchsnippets und nicht am Volltext prüfen konnte, ist mit
  `[NUR-SNIPPET]` markiert. Ich habe jede zentrale Behauptung mit 2–3 unterschiedlich
  formulierten Suchanfragen trianguliert.
- **Nicht leistbar unter diesen Bedingungen:** Beweisdetails nachrechnen, Lemma-Ketten
  prüfen, Lean-/Coq-Code kompilieren, Datensätze reproduzieren, Autorenlisten am
  PDF-Titelblatt verifizieren, Zitationsketten auflösen. Wir können in diesem Projekt
  also **keine eigenständige mathematische Verifikation** leisten, sondern nur
  *Rezeptionsanalyse*: wer hat was geprüft, wer hat widersprochen, wo steht es.
- Das ist keine Nebensache, sondern trifft den Kern von Teil D: Ein Akzeptanzprotokoll,
  das wir selbst nicht anwenden können, muss sauber zwischen "wir haben es geprüft" und
  "wir haben geprüft, dass andere es geprüft haben" unterscheiden. Ich tue das unten
  explizit (D.4).

---

## 1. Executive Summary — die schärfsten Einwände gegen dieses Projekt

1. **Die Basisrate ist vernichtend.** Über 30 Jahre dokumentierter Beweisversuche:
   116 gelistete Claims, null Überlebende. Jeder Autor hielt seinen Beweis für richtig.
   Unsere Priorerwartung, etwas beizutragen, muss davon ausgehen, nicht von "wir sind
   sorgfältig".
2. **Die zwei prominentesten KI-nahen "P ≠ NP"-Arbeiten sind derselbe Fehler, zweimal.**
   Das Microsoft-Paper (arXiv:2309.05689) und das FCS-Paper von Xu/Zhou sind
   personell verschränkt: Ke Xu und Guangyan Zhou sind Koautoren *beider*. GPT-4 hat
   also nicht unabhängig "P ≠ NP" gefolgert, sondern das Argument seiner eigenen
   Koautoren reproduziert. Das ist Zirkularität, keine Evidenz.
3. **Der Kernfehler ist klassisch und benennbar:** Man nimmt implizit an, *wie* ein
   Algorithmus vorgehen muss (hier: Instanz der Größe n auf Instanzen der Größe n−1
   reduzieren), und beweist dann eine untere Schranke für genau diese Klasse. Allender
   und Williams sagen das über Xu/Zhou wörtlich; Gasarch sagt dasselbe über den
   gesamten Zustrom an Crank-Papers.
4. **Die Barrieren beweisen nicht, dass P vs. NP unlösbar ist** — aber sie beweisen,
   dass jede Technik, die man "intuitiv" wählt, fast sicher schon ausgeschlossen ist.
   Das ist die härtere Aussage für uns: nicht "unmöglich", sondern "alles, was Ihnen
   einfällt, ist bereits widerlegt".
5. **Die Distanz zum Ziel wird systematisch unterschätzt.** Das stärkste bekannte
   Lower-Bound-Resultat für allgemeine Schaltkreise über der vollen binären Basis ist
   ca. **3,011·n** für eine explizite Funktion (Find–Golovnev–Hirsch–Kulikov 2016).
   Wir können also nicht einmal *4n* beweisen. P ≠ NP verlangt superpolynomiell.
6. **LLMs sind in diesem Problem strukturell schlecht aufgestellt:** Sie interpolieren
   publizierte Mathematik; publizierte Mathematik enthält genau die Argumente, die an
   den Barrieren scheitern. Das Modell reproduziert bevorzugt die Fehlerklasse, die im
   Trainingskorpus überrepräsentiert ist.
7. **Halluzinierte Literatur ist in diesem Feld besonders gefährlich**, weil es viele
   real existierende, aber falsche Paper gibt. Ein erfundenes Zitat ist hier
   *plausibel*, und genau das macht es schwer zu entdecken.

---

# Teil A — Die Barrieren, technisch korrekt

## A.0 Was eine "Barriere" formal überhaupt ist

Eine Barriere ist **kein** Unmöglichkeitssatz über P vs. NP. Sie ist ein Satz der Form:

> *Sei T eine Beweistechnik mit der formalen Eigenschaft E. Dann kann T die Aussage
> P ≠ NP nicht beweisen (ggf.: unter Annahme A).*

Drei Dinge folgen daraus sofort, und das Übersehen dieser drei Punkte ist die
Standard-Überinterpretation:

- Eine Barriere ist **relativ zu einer formal definierten Technikklasse**. Fällt eine
  neue Technik nicht unter E, sagt die Barriere *nichts*.
- Barrieren sind **nicht** Unabhängigkeitsresultate. P vs. NP ist *nicht* als
  unabhängig von ZFC oder PA bekannt.
- Barrieren sind **konstruktive Wegweiser**: Sie sagen, welche Eigenschaft ein
  erfolgreicher Beweis verletzen *muss*. Genau so sind sie historisch auch benutzt
  worden (Williams 2011).

Ein Beweisversuch, der die Barrieren nicht adressiert, ist deshalb nicht "vielleicht
doch richtig" — er ist **mit sehr hoher Wahrscheinlichkeit falsch**, weil er in eine
Klasse fällt, für die Unmöglichkeit bereits bewiesen ist. Das ist der praktische Wert
der Barrieren: sie sind ein extrem effizienter Filter.

---

## A.1 Relativization (Baker–Gill–Solovay 1975)

**Quelle:** T. Baker, J. Gill, R. Solovay, *Relativizations of the P =? NP Question*,
SIAM Journal on Computing 4(4), 1975. `[VERIFIZIERT]` (kanonisch, Briefing bestätigt;
Volltext unter den Umgebungsbedingungen nicht prüfbar) — Konfidenz: **hoch**.

### Was ein Orakel-Argument ist

Eine Orakel-Turingmaschine M^O darf in einem Schritt fragen "ist x ∈ O?" und die Antwort
kostenlos nutzen. P^O und NP^O sind die relativierten Klassen. Eine Beweistechnik
**relativiert**, wenn ihr Argument wortwörtlich weiterläuft, wenn man allen beteiligten
Maschinen dasselbe Orakel O gibt — weil sie Maschinen nur als *Black Boxes* behandelt
(Simulation, Zählen von Schritten, Diagonalisierung über eine Aufzählung).

BGS konstruieren zwei Orakel:

- **A** mit P^A = NP^A. Standardwahl: ein PSPACE-vollständiges Orakel (z.B. TQBF). Dann
  gilt P^A = NP^A = PSPACE, weil das Orakel mächtig genug ist, das nichtdeterministische
  Raten selbst zu erledigen.
- **B** mit P^B ≠ NP^B. Konstruktion per Diagonalisierung: man baut B stufenweise so,
  dass die unäre Sprache L_B = {1^n : ∃x ∈ B mit |x| = n} in NP^B liegt (raten und
  fragen), aber jede polynomielle deterministische Maschine mit Orakel B auf 1^n
  scheitert, weil sie nicht alle 2^n Strings der Länge n abfragen kann.

`[VERIFIZIERT]`, Lehrbuchstandard — Konfidenz: **hoch**.

### Warum das Diagonalisierung ausschließt

Der Beweis von P ≠ EXP (Zeithierarchiesatz) relativiert: für *jedes* Orakel O gilt
P^O ≠ EXP^O. Würde eine analoge Technik P ≠ NP liefern, würde sie P^O ≠ NP^O für alle O
liefern — Widerspruch zu Orakel A. Also: **keine rein simulations-/
diagonalisierungsbasierte Technik kann P vs. NP entscheiden.**

### Was Relativization *nicht* sagt

- Sie sagt **nicht**, dass Diagonalisierung tot ist. Sie sagt, dass *relativierende*
  Diagonalisierung tot ist. Nicht-relativierende Diagonalisierung existiert und ist
  produktiv (siehe A.5: Buhrman–Fortnow–Thierauf, Williams).
- Sie sagt nichts über kombinatorische Argumente, die die *interne Struktur* von
  Schaltkreisen oder Formeln benutzen (Switching Lemma, Approximation durch Polynome) —
  diese relativieren nicht.
- Sie sagt nichts über Arithmetisierung. Genau deshalb war IP = PSPACE (Shamir 1992)
  ein nicht-relativierendes Resultat, das die Barriere bewiesenermaßen durchbrach: es
  gibt Orakel O mit coNP^O ⊄ IP^O.

**Techniken, die nicht relativieren:** Arithmetisierung (IP = PSPACE, MIP = NEXP),
Schaltkreis-interne Argumente, Williams' "algorithmic method" (nutzt spezifische
Struktur von ACC-Schaltkreisen, bricht unter Orakeln zusammen).

> **Randnotiz, unverifiziert:** In den Suchergebnissen taucht ein Preprint
> arXiv:2601.09702, *"Diagonalization Without Relativization: A Closer Look at the
> Baker–Gill–Solovay Theorem"* (Januar 2026) auf. Ich konnte weder Autoren noch Inhalt
> noch Rezeption feststellen. `[PREPRINT]` `[NUR-SNIPPET]` — Konfidenz: **niedrig**,
> nicht als Beleg verwenden.

---

## A.2 Natural Proofs (Razborov–Rudich 1994/97)

**Quelle:** A. Razborov, S. Rudich, *Natural Proofs*, STOC 1994; Journal of Computer and
System Sciences 55(1):24–35, 1997. Gödel-Preis 2007.
`[VERIFIZIERT]` — Konfidenz: **hoch**.

### Die Definition, präzise

Ein Lower-Bound-Beweis gegen eine Schaltkreisklasse C liefert typischerweise eine
**kombinatorische Eigenschaft** P von Booleschen Funktionen, so dass:

- **Usefulness:** Jede Funktion mit Eigenschaft P liegt nicht in C. (Das ist der
  eigentliche Lower Bound.)
- **Constructivity:** P ist effizient entscheidbar, gegeben die *Wahrheitstafel* der
  Funktion (Länge N = 2^n). "Effizient" heißt hier: in Zeit poly(N) = 2^{O(n)}, bzw.
  in P/poly bezüglich N.
- **Largeness:** Ein nicht vernachlässigbarer Anteil *aller* Booleschen Funktionen auf
  n Bits hat die Eigenschaft P — typischerweise mindestens 2^{−O(n)}, in der
  Standardformulierung: eine zufällige Funktion erfüllt P mit Wahrscheinlichkeit
  ≥ 1/2^{O(n)}.

Ein Beweis heißt **natural**, wenn er eine Eigenschaft mit *constructivity + largeness*
benutzt, die *useful* gegen C ist.

### Warum die Barriere an PRFs/OWFs hängt

Eine natürliche Eigenschaft ist ein **effizienter Distinguisher**: sie akzeptiert eine
zufällige Funktion mit nicht vernachlässigbarer Wahrscheinlichkeit (largeness) und
verwirft *jede* Funktion, die von einem kleinen Schaltkreis berechnet wird
(usefulness). Eine pseudozufällige Funktionenfamilie (PRF), die in C berechenbar ist,
ist per Definition von einer zufälligen Funktion nicht effizient unterscheidbar. Beides
zusammen ist ein Widerspruch.

Formal (Razborov–Rudich): Existiert ein 2^{k^ε}-harter PRG in SIZE(k^c), so gibt es
keine P/poly-konstruktive natürliche Eigenschaft mit Dichte 1/2^{n^d}, die useful gegen
SIZE(n^e) ist, sobald e > 1 + cd/ε. `[NUR-SNIPPET]` (Parameter aus Suchsnippet
übernommen, Volltext nicht prüfbar) — Konfidenz: **mittel**.

Da PRFs aus One-Way-Functions konstruierbar sind (Håstad–Impagliazzo–Levin–Luby für
PRGs, Goldreich–Goldwasser–Micali für PRFs), hängt die Barriere letztlich an der
Existenz subexponentiell harter OWFs. `[VERIFIZIERT]`, kanonisch — Konfidenz: **hoch**.

### Die Ironie, die man benennen muss

Die Existenz harter OWFs **impliziert** P ≠ NP. Die Barriere sagt also:

> *Wenn P ≠ NP in einer starken, kryptographisch nutzbaren Form wahr ist, dann kann man
> es nicht mit natürlichen Beweisen zeigen.*

Die Barriere ist damit **bedingt** — und die Bedingung ist genau das, was wir glauben.
Das macht sie stark, nicht schwach.

### Welche Lower Bounds sind nachweislich *nicht* natural?

`[VERIFIZIERT]` / `[NUR-SNIPPET]`, Konfidenz jeweils angegeben:

| Resultat | Warum nicht natural |
|---|---|
| Zeit-/Raumhierarchiesätze, alle Diagonalisierungsresultate | verletzen **constructivity** (die Eigenschaft "diagonalisiert gegen Maschine i" ist nicht effizient auf Wahrheitstafeln entscheidbar). Konfidenz: hoch |
| MA_EXP ⊄ P/poly (Buhrman–Fortnow–Thierauf 1998) | kombiniert Arithmetisierung (nicht-relativierend) mit Diagonalisierung (nicht-natural). Gilt als erstes nicht-naturalisierendes Schaltkreis-Lower-Bound. `[NUR-SNIPPET]`, Konfidenz: mittel-hoch |
| PP ⊄ SIZE(n^k) (Vinodchandran) | ebenfalls diagonalisierend. `[NUR-SNIPPET]`, Konfidenz: mittel |
| NEXP ⊄ ACC⁰ (Williams 2011) | nutzt die Existenz eines schneller-als-Brute-Force-SAT-Algorithmus für ACC — das ist keine "large" Eigenschaft zufälliger Funktionen. Konfidenz: hoch |

**Wichtige Gegenrichtung, die oft unterschlagen wird:** Die bekannten *erfolgreichen*
Lower Bounds gegen schwache Klassen **sind** natural — Håstads Switching Lemma für AC⁰,
Razborov–Smolensky für AC⁰[p]. Das ist kein Widerspruch: In diesen Klassen existieren
*keine* PRFs (AC⁰-Funktionen sind über Linial–Mansour–Nisan effizient lernbar), die
Barriere greift also gar nicht. Ebenso ist Razborovs monotoner Lower Bound (1985)
natural gegen monotone Schaltkreise — auch dort gibt es keine monotonen PRFs.
`[EIGENE EINSCHÄTZUNG]` auf Basis kanonischen Wissens — Konfidenz: **hoch**.

**Nuance zur Fluchtrichtung:** Williams (*Natural Proofs versus Derandomization*,
arXiv:1212.1891, STOC 2013) zeigt, dass für NEXP-Lower-Bounds die *constructivity*
gerade nicht vermeidbar ist — der Ausweg muss über die Verletzung von *largeness*
laufen. `[NUR-SNIPPET]` — Konfidenz: **mittel**. Für unser Projekt heißt das: Wer sagt
"mein Beweis ist nicht natural", muss angeben **welche der beiden Bedingungen** er
verletzt und **wo genau im Beweis** das passiert. Alles andere ist Handwaving.

---

## A.3 Algebrization (Aaronson–Wigderson 2008)

**Quelle:** S. Aaronson, A. Wigderson, *Algebrization: A New Barrier in Complexity
Theory*, STOC 2008; ACM Transactions on Computation Theory 1(1), 2009; ECCC TR08-005.
`[VERIFIZIERT]` — Konfidenz: **hoch**.

### Was sie gegenüber Relativization hinzufügt

Nach BGS wusste man: relativierende Techniken reichen nicht. Nach IP = PSPACE (1990er)
glaubte man, mit **Arithmetisierung** eine echte nicht-relativierende Technik in der
Hand zu haben. Aaronson–Wigderson zeigen, dass auch das nicht reicht.

**Algebraisches Orakel:** Zu einem Boolean-Orakel O nimmt man eine
**low-degree extension** Õ über einem endlichen Körper: ein Polynom von polynomiellem
Grad, das auf allen Booleschen Eingaben mit O übereinstimmt. `[NUR-SNIPPET]`,
Konfidenz: **hoch**.

**Algebrizing:** Eine Separation C ⊄ D algebrisiert, wenn sie für alle Orakel in der
Form C^O ⊄ D^Õ gilt (die eine Seite bekommt das Boolean-Orakel, die andere die
algebraische Erweiterung); analog für Inklusionen mit vertauschten Rollen.

**Hauptbefunde:**

- **Alle** bekannten arithmetisierungsbasierten nicht-relativierenden Resultate
  algebrisieren: IP = PSPACE, MIP = NEXP, MA_EXP ⊄ P/poly. `[VERIFIZIERT]`,
  Konfidenz: **hoch**.
- **P vs. NP, P vs. RP, NEXP vs. P/poly** erfordern **nicht-algebrisierende** Techniken.
  `[VERIFIZIERT]`, Konfidenz: **hoch**.

Der Fortschritt gegenüber BGS ist also: Die Barriere wurde auf die einzige damals
bekannte Ausbruchstechnik ausgedehnt. Algebrization schließt mehr aus als
Relativization, sie ist die *stärkere* Barriere in diesem Strang.

**Kritik/Verfeinerung:** Impagliazzo, Kabanets, Kolokolova, *An Axiomatic Approach to
Algebrization* (STOC 2009), argumentieren, dass die Aaronson–Wigderson-Definition
asymmetrisch und definitorisch heikel ist (unterschiedliche Begriffe für Inklusionen und
Separationen) und schlagen eine axiomatische Neufassung vor. `[NUR-SNIPPET]` (Existenz
über SFU-Projektseite bestätigt, Inhalt nur via Snippet) — Konfidenz: **mittel**.
Für uns relevant: **Auch Barrieren sind umstritten und werden nachgeschärft.** Wer eine
Barriere zitiert, muss die Version nennen.

---

## A.4 Die Überinterpretation — und ihre saubere Korrektur

**Die falsche Aussage:** "Die drei Barrieren beweisen, dass P vs. NP nie gelöst werden
kann."

**Warum sie falsch ist:**

1. **Kategorienfehler.** Die Barrieren sind Sätze über Beweis*techniken* mit formal
   definierten Eigenschaften (relativierend / natural / algebrisierend), nicht über die
   Lösbarkeit der Frage. Es gibt keinen Satz "P vs. NP ist unentscheidbar".
2. **Keine Unabhängigkeit.** P vs. NP ist *nicht* als unabhängig von ZFC oder PA bekannt.
   Aaronson, *Is P Versus NP Formally Independent?* (Bulletin of the EATCS, Oktober 2003)
   diskutiert die Frage systematisch; das Ergebnis ist, dass die Evidenz für
   Unabhängigkeit schwach ist und dass es erhebliche Hindernisse gibt, Unabhängigkeit
   von starken Theorien überhaupt zu zeigen. `[VERIFIZIERT]` (Venue und Titel
   bestätigt), Inhalt `[NUR-SNIPPET]` — Konfidenz: **mittel-hoch**.
3. **Bedingtheit.** Natural Proofs ist ein *bedingtes* Resultat (unter Existenz harter
   PRFs). Es ist logisch kein Unmöglichkeitssatz, sondern ein Trade-off-Satz.
4. **Empirische Widerlegung.** Barrieren *sind* bereits umgangen worden — von echten,
   peer-reviewten Theoremen (A.5). Wären sie absolute Hindernisse, gäbe es diese
   Resultate nicht.
5. **Barrieren als Kompass.** Kolokolova und andere zeigen, dass man die Barrieren als
   Unabhängigkeit von *schwachen* Theorien rekonstruieren kann, die "alle bekannten
   Techniken" formalisieren (A. Kolokolova, *Complexity Barriers as Independence*, in
   *The Incomputable*, Springer 2017). `[NUR-SNIPPET]` — Konfidenz: **mittel**. Das ist
   die richtige Lesart: Die Barrieren messen, wie weit unsere *aktuelle* Werkzeugkiste
   reicht — und sie zeigen, wo die neue Technik ansetzen muss.

**Die richtige, unbequeme Aussage** (und die für dieses Projekt relevante):

> Die Barrieren beweisen nicht, dass P vs. NP unlösbar ist. Sie beweisen, dass praktisch
> jede Technik, die einem intuitiv einfällt, bereits nachweislich nicht ausreicht. Ein
> Beweisversuch, der nicht explizit sagt, welche Barriere er wie verletzt, ist deshalb
> a priori mit sehr hoher Wahrscheinlichkeit falsch.

`[EIGENE EINSCHÄTZUNG]` — Konfidenz: **hoch**.

---

## A.5 Konkrete Resultate, die Barrieren umgehen

### NEXP ⊄ ACC⁰ (Ryan Williams)

**Quelle:** R. Williams, *Non-Uniform ACC Circuit Lower Bounds*, CCC/STOC 2011; Journal
of the ACM 61(1), Artikel 2, 2014. `[VERIFIZIERT]` — Konfidenz: **hoch**.

- **Aussage:** NEXP hat keine ACC⁰-Schaltkreise polynomieller (sogar
  quasi-polynomieller) Größe. ACC⁰ = konstante Tiefe, unbeschränkter Fan-in, AND/OR/NOT
  plus MOD_m-Gatter für festes m.
- **Warum es nicht relativiert / nicht algebrisiert:** Der Beweis beruht auf einem
  *schneller-als-Brute-Force*-Erfüllbarkeitsalgorithmus für ACC-Schaltkreise, der
  spezifische Struktur ausnutzt (Reduktion auf SYM∘AND-Form). Alle bekannten
  verbesserten SAT-Algorithmen brechen zusammen, sobald man Orakelgatter (oder deren
  algebraische Erweiterungen) in die Instanz einbaut. `[NUR-SNIPPET]` —
  Konfidenz: **hoch**.
- **Warum es nicht natural ist:** "Der Schaltkreis hat einen schnellen
  SAT-Algorithmus" ist keine Eigenschaft, die ein großer Anteil zufälliger Funktionen
  hat — largeness fehlt.
- **Methode ("algorithmic method"):** schneller SAT-Algorithmus für C + Easy Witness
  Lemma + nichtdeterministischer Zeithierarchiesatz ⟹ Lower Bound gegen C.
- **Verschärfungen:** Murray–Williams, *Circuit Lower Bounds for Nondeterministic
  Quasi-Polytime: An Easy Witness Lemma for NP and NQP*, STOC 2018 (NQP ⊄ ACC⁰,
  NQP ⊄ ACC∘THR); Chen–Lyu–Williams, FOCS 2019 / SICOMP: NQP ist sogar *average-case*
  hart für ACC⁰. `[VERIFIZIERT]` (Venues bestätigt) — Konfidenz: **hoch**.

### Weitere nicht-relativierende / nicht-naturalisierende Resultate

- IP = PSPACE (Shamir 1992), MIP = NEXP (Babai–Fortnow–Lund 1991) — nicht-relativierend,
  aber algebrisierend. `[VERIFIZIERT]`, kanonisch.
- MA_EXP ⊄ P/poly (Buhrman–Fortnow–Thierauf 1998) — nicht-relativierend *und*
  nicht-natural, aber algebrisierend. `[NUR-SNIPPET]`, Konfidenz: mittel-hoch.

### Und jetzt die kalte Dusche

Diese Resultate sind echt und wichtig — und **astronomisch weit** von P ≠ NP entfernt:

- ACC⁰ ist eine *sehr* schwache Klasse (konstante Tiefe). Zwischen ACC⁰ und P/poly liegen
  TC⁰, NC¹, LOGSPACE, P — keine davon ist erreicht.
- Die Untergrenze gilt für **NEXP** bzw. **NQP**, nicht für NP. Der Abstand zwischen
  "NEXP hat keine ACC⁰-Schaltkreise" und "NP hat keine polynomiellen Schaltkreise" ist
  qualitativ, nicht quantitativ.
- **Für allgemeine Schaltkreise** über der vollen binären Basis B₂ ist die beste bekannte
  untere Schranke für eine explizite Funktion **(3 + 1/86)·n − o(n)** — Find, Golovnev,
  Hirsch, Kulikov, FOCS 2016, verbessert Blums 3n−o(n) von 1984. Inzwischen 3.1n−o(n)
  (Li, Yang, STOC 2022). `[VERIFIZIERT]` (Venues und Werte über zwei unabhängige Treffer
  bestätigt) — Konfidenz: **mittel-hoch**.

> **Merksatz für das Projekt:** Wir können nicht einmal beweisen, dass irgendeine
> explizite Funktion mehr als ~3,1n Gatter braucht. P ≠ NP verlangt *superpolynomiell*.
> Jede Erzählung, die suggeriert, wir seien "nah dran", ist falsch.

### Zusatz: auch ein Nicht-Barrieren-Programm ist eingebrochen

**Geometric Complexity Theory (GCT).** Mulmuley–Sohoni schlugen 2001 vor, Permanent
vs. Determinant über **occurrence obstructions** zu trennen. 2016 zeigten
Ikenmeyer–Panova und Bürgisser–Ikenmeyer–Panova, dass **keine occurrence obstructions
existieren** — der konkret vorgeschlagene Weg ist tot (arXiv:1604.06431; Journal of the
AMS 32(1), 2019). Der allgemeinere Ansatz über *multiplicity obstructions* bleibt offen.
`[VERIFIZIERT]` (JAMS-Publikation bestätigt) — Konfidenz: **hoch**.

Das ist für uns eine wichtige Lehre: Auch das anspruchsvollste, von einem Top-Theoretiker
über 15 Jahre entwickelte Programm hat in seiner konkret ausformulierten Form nicht
funktioniert. Mulmuley selbst hat die Zeitskala des Programms immer in Jahrzehnten bis
Jahrhunderten beziffert.

---

# Teil B — Basisraten des Scheiterns

## B.1 Woegingers "P-versus-NP page"

**Quelle:** G. J. Woeginger, *The P-versus-NP Page*, TU Eindhoven,
`https://wscor.win.tue.nl/woeginger/P-versus-NP.htm`. `[VERIFIZIERT]` (Existenz und
Zahlen über mehrere unabhängige Treffer bestätigt) — Konfidenz: **hoch**.

- **Stand der Liste:** letzte Aktualisierung **September 2016**.
- **Umfang:** **116 Versuche**, zurück bis **1986**.
- **Verteilung:** **61** Claims "P = NP", **49** Claims "P ≠ NP", **6** sonstige
  (z.B. "das Problem ist unentscheidbar" oder "unabhängig von ZFC").
  `[NUR-SNIPPET]`, über zwei Suchanfragen konsistent — Konfidenz: **mittel-hoch**.
- **Überlebensrate: 0 von 116.** Kein einziger Eintrag wurde von der Community
  akzeptiert.
- **Woeginger ist am 1. April 2022 verstorben** (Alter 57); die Liste wird von ihm nicht
  mehr fortgeführt. `[VERIFIZIERT]` — Konfidenz: **hoch**.

**Aktueller Stand der Liste:** Es existiert eine Fortführung/Spiegelung unter
`https://mikinty.github.io/P-vs-NP/` (gepflegt von "mikinty", basierend auf Woegingers
Original). **Ich konnte weder die Eintragszahl noch das Datum der letzten Aktualisierung
verifizieren** — die Domain ist blockiert, und der GitHub-MCP-Zugriff ist auf unser
Projekt-Repo beschränkt. `[NUR-SNIPPET]`, Konfidenz: **niedrig**.
Ebenfalls existiert `https://github.com/MichaelWehar/P-vs-NP-Community` als Anlaufstelle
für Einreicher und Reviewer. `[NUR-SNIPPET]`, Konfidenz: **niedrig-mittel**.

**Wichtiger Punkt: 116 ist eine massive Untererfassung.** Woeginger hat nur
dokumentierte, halbwegs auffindbare Claims gelistet. Der reale Zustrom ist größer:

- Bill Gasarch berichtet, er erhalte **im Schnitt etwa einen P-vs-NP-Beweisversuch pro
  Monat**, in einer Spitzenphase **sieben in zwei Wochen** — per E-Mail, per X-DM, per
  LinkedIn (auf Spanisch), per Telefonanruf, einer mit 41 Follow-up-Mails.
  (*P v NP Papers Galore*, Computational Complexity Blog, April 2025.)
  `[VERIFIZIERT]` (Post existiert, Inhalt `[NUR-SNIPPET]`) — Konfidenz: **mittel-hoch**.
- Eric Allender berichtet als ehemaliger Editor-in-Chief von *ACM Transactions on
  Computation Theory*, dass solche Einreichungen so regelmäßig kamen, dass er sie oft
  selbst begutachten musste (keine Referees zu finden) und dass ToCT, J.ACM und ACM
  TALG **die Einreichungsfrequenz für solche Arbeiten formal begrenzen**.
  (Gastbeitrag im Computational Complexity Blog, 4. August 2025.) `[VERIFIZIERT]`
  (Post existiert), Inhalt `[NUR-SNIPPET]` — Konfidenz: **mittel-hoch**.
- Es existiert eine ganze Gattung publizierter Widerlegungen, überwiegend aus der
  Rochester-Gruppe um Lane Hemaspaandra: *A Critique of Uribe's "P vs. NP"*
  (arXiv:2205.01189), *A Critique of Du's "A Polynomial-Time Algorithm for 3-SAT"*
  (arXiv:2404.04395), *A Critique of Kumar's ...* (arXiv:2112.06062),
  *On Czerwinski's "P ≠ NP relative to a P-complete oracle"* (arXiv:2312.04395),
  *A Critique of Deng's "P=NP"* (arXiv:2507.09018). `[PREPRINT]`, Existenz über
  Suchtreffer bestätigt — Konfidenz: **mittel-hoch**.

**Ableitung für unser Projekt** `[EIGENE EINSCHÄTZUNG]`, Konfidenz **hoch**:
Die empirische Erfolgsrate von Beweisversuchen liegt bei ≈ 0 bei einer Stichprobe im
dreistelligen Bereich über vier Jahrzehnte. Unsere Priorerwartung, auf diesem Weg etwas
beizutragen, muss dementsprechend gesetzt werden. Der einzige Beitrag, den dieses
Projekt realistisch leisten kann, ist **Bestandsaufnahme und Fehleranalyse** — nicht
Fortschritt am Problem.

---

## B.2 Fallstudie Deolalikar 2010 — die zentrale Lehrgeschichte

**Was passierte** (`[VERIFIZIERT]` im Ablauf, Details `[NUR-SNIPPET]`, Konfidenz
**mittel-hoch**):

| Datum | Ereignis |
|---|---|
| 6. Aug. 2010 | Vinay Deolalikar (HP Labs) mailt einen ~100-seitigen Entwurf "P ≠ NP" an eine Reihe von Forschern. |
| ~7. Aug. | **Stephen Cook** leitet weiter mit der Bemerkung, dies sei "a relatively serious claim to have solved P vs NP". Das ist der Grund, warum die Sache überhaupt Fahrt aufnahm. |
| 8. Aug. | Richard Lipton bloggt "A Proof That P Is Not Equal To NP?" auf *Gödel's Lost Letter and P=NP*. |
| 9.–12. Aug. | Öffentliche, verteilte Prüfung in Blog-Kommentaren (Lipton/Regan), auf dem **Polymath-Wiki** (`michaelnielsen.org/polymath`) und bei Gowers/Tao. Beteiligt u.a. Neil Immerman, Terence Tao, Ryan Williams, Russell Impagliazzo, Cris Moore, Suresh Venkatasubramanian. |
| 11. Aug. | "Deolalikar Responds To Issues About His P≠NP Proof" (Lipton). |
| 12. Aug. | **"Fatal Flaws in Deolalikar's Proof?"** (Lipton/Regan). Konsens: der Beweis ist in der vorliegenden Form fatal fehlerhaft. |
| 13. Aug. | Deolalikar stellt eine dreiseitige Synopsis online. |
| 17. Aug. | Alle Entwürfe verschwinden von seiner Homepage. |
| 15. Sep. | "An Update On Vinay Deolalikar's 'Proof'" (Lipton). |
| danach | **Nie in einem begutachteten Journal erschienen.** Deolalikar erklärte, das Papier sei zur "due process"-Prüfung bei einem Journal. Es ist nie erschienen. |

**Der Ansatz.** Deolalikar wollte zwei Gebiete verbinden:

1. **Finite Model Theory / Deskriptive Komplexität:** Immermans Satz — P entspricht auf
   geordneten Strukturen FO(LFP), Fixpunktlogik erster Stufe. Idee: zeige, dass die
   Lösungsmenge von k-SAT nicht durch FO(LFP) beschreibbar ist.
2. **Statistische Physik zufälliger k-SAT-Instanzen:** die "clustering"/"shattering"-Phase
   nahe der Erfüllbarkeitsschwelle. Idee: Lösungsräume zerfallen dort in exponentiell
   viele, weit getrennte Cluster; ein FO(LFP)-beschreibbarer Lösungsraum wäre dagegen
   "polylog-parametrisierbar", also zu einfach strukturiert.

**Woran es genau scheiterte** — vier klar benennbare Ebenen:

1. **Die XORSAT-Falle (der tödliche Einwand).** Das Argument, wenn es gültig wäre, würde
   sich wortwörtlich auf **k-XORSAT** übertragen — und dort dieselbe
   Cluster-Geometrie vorfinden. Aber k-XORSAT ist ein **lineares Gleichungssystem über
   GF(2)** und in **Polynomialzeit** lösbar (Gauß-Elimination). Der Beweis würde also
   etwas nachweislich Falsches beweisen. Dasselbe gilt abgeschwächt für 2-SAT.
   **Das ist Fehlermuster #1 in Reinform.** `[VERIFIZIERT]` (über mehrere Quellen
   konsistent) — Konfidenz: **hoch**.
2. **Immermans Ordnungs-Einwand.** Immerman zeigte, dass die Arbeit die Rolle der
   **Ordnungsrelation** in der FO(LFP)-Charakterisierung von P nicht korrekt behandelt.
   Der Satz "P = FO(LFP)" gilt auf *geordneten* Strukturen; Deolalikars Lokalitäts- und
   Parametrisierungsargumente brauchten dagegen eine ordnungsfreie Situation. Ohne
   Ordnung ist FO(LFP) echt schwächer als P, das Argument beweist dann schlicht nicht,
   was es beweisen soll. `[VERIFIZIERT]` — Konfidenz: **hoch**.
3. **Die nicht bewiesene Kernannahme.** Das entscheidende Bindeglied — dass die
   physikalisch motivierte Cluster-Struktur ein rigoroses, für den Beweis verwendbares
   Hindernis darstellt — hat Deolalikar nicht bewiesen, sondern aus Resultaten der
   statistischen Physik *inferiert*. Die ganze Beweisstrategie hing an diesem
   unbewiesenen Stück. `[NUR-SNIPPET]` — Konfidenz: **mittel-hoch**.
4. **Der Meta-Einwand (Impagliazzo u.a.):** Die Härte von zufälligem k-SAT zu beweisen
   erscheint als *schwierigeres* Problem als P ≠ NP selbst. Ein Beweis, der P ≠ NP über
   random k-SAT angeht, will also den Berg über den Gipfel dahinter besteigen.
   `[NUR-SNIPPET]` — Konfidenz: **mittel-hoch**.

**Was an dem Fall *funktioniert* hat** (und was wir daraus für Teil D lernen):

- **Offene, verteilte Prüfung ist extrem schnell und extrem effektiv.** Von "Cook nennt
  es ernstzunehmend" bis "Konsens: fatal fehlerhaft" vergingen **sechs Tage**. Kein
  Journal-Review hätte das geschafft.
- **Die Prüfung war öffentlich dokumentiert und ist bis heute nachlesbar** (Polymath-Wiki,
  Lipton/Regan-Blog). Das ist der Goldstandard für Transparenz.
- **Die Prüfer waren benannt.** Immerman, Tao, Williams, Impagliazzo — keine anonymen
  Kommentare, sondern Fachleute mit Namen und Reputation im Einsatz.
- **Es half nicht,** dass der Autor Berufsinformatiker bei HP Labs war, dass Cook es
  zunächst ernst nahm, dass die Presse (TechCrunch, New Scientist) berichtete, oder dass
  der Ansatz zwei respektable Gebiete verband. **Institutionelle Reputation ersetzt keine
  Verifikation.**

**Warnung für unser Projekt** `[EIGENE EINSCHÄTZUNG]`, Konfidenz **hoch**:
Deolalikar war *nicht* ein Crank. Sein Ansatz war der bis dahin seriöseste
öffentlich diskutierte Versuch, und er scheiterte trotzdem an einem Einwand, den man in
zwei Sätzen formulieren kann ("Ihr Argument gilt auch für XORSAT, und XORSAT ist in P").
Wenn wir in diesem Projekt irgendetwas Positives behaupten wollen, ist der erste Test
immer: **auf welche verwandte Aussage, die nachweislich falsch ist, überträgt sich unser
Argument?**

---

## B.3 Katalog der typischen Fehlermuster

Rekonstruiert aus Woeginger/Hemaspaandra-Kritiken, Aaronsons Signs-Listen,
Gasarch/Fortnow und den in Teil C behandelten Fällen. `[EIGENE EINSCHÄTZUNG]` als
Systematisierung, mit Belegen pro Muster — Konfidenz: **hoch**.

### F1 — "Beweist zu viel": das Argument gilt auch für ein Problem in P

Das mit Abstand tödlichste und häufigste Muster. Der Beweis benutzt keine Eigenschaft,
die 3-SAT von 2-SAT, XORSAT, Horn-SAT oder Matching unterscheidet.

- Aaronson, Sign 3: *"The approach seems to yield something much stronger and maybe even
  false (but the authors never discuss that). They've proved 3SAT takes exponential time;
  their argument would go through just as well for 2SAT."* `[NUR-SNIPPET]`
- Deolalikar 2010: XORSAT.
- **Der Standard-Selbsttest:** Wo im Beweis geht ein, dass Klauseln *drei* Literale
  haben? Wenn man das nicht mit Zeilennummer angeben kann, ist der Beweis falsch.

### F2 — Implizite Beschränkung auf eine Algorithmenklasse

Man nimmt an, ein Algorithmus müsse "sinnvoll" oder "auf naheliegende Weise" vorgehen,
und beweist die Schranke für diese Klasse. Das ist ein Lower Bound für ein
**eingeschränktes Berechnungsmodell**, nicht für P.

- Fortnow/Gasarch über den Crank-Zustrom: *"One category of these papers claims to prove
  P ≠ NP by arguing that a polynomial-time machine must search a large space of solutions,
  but to truly prove P ≠ NP, one cannot make assumptions about how the machine might
  work."* (*P v NP Papers Galore*, April 2025) `[NUR-SNIPPET]`
- Allender/Williams über Xu–Zhou: *"the authors flatly assume that any algorithm for SAT
  must proceed by reducing an instance of size n to instances of size n−1 in a particular
  way"* — siehe C.4. `[NUR-SNIPPET]`

### F3 — "SAT braucht erschöpfende Suche" als Intuition ohne formales Modell

Die Aussage "man muss alle 2^n Belegungen durchprobieren" ist **kein mathematischer
Satz**, sondern eine Beschreibung eines bestimmten Algorithmus. Formal korrekt wäre nur:
"In Berechnungsmodell M mit Ressourcenbeschränkung R kostet jeder Algorithmus ≥ f(n)."
Ohne die Angabe von M ist die Aussage leer. Sonderfall von F2, verdient aber eigenen
Eintrag, weil er die Intuitionsquelle fast aller Laienbeweise ist.

Beachte: Das ist *nicht* trivial widerlegbar — für *Entscheidungsbaum*-Modelle,
*resolution proofs*, *monotone circuits* etc. gibt es echte exponentielle Schranken. Der
Fehler ist nicht die Schranke, sondern die Behauptung, sie gelte für alle Algorithmen.

### F4 — Pseudopolynomiell ≠ polynomiell

Der klassische "P = NP"-Fehler. Subset-Sum/Knapsack haben einen DP-Algorithmus in
O(n·W). Das ist polynomiell in W, aber **exponentiell in der Eingabelänge**, denn W wird
binär mit log W Bits kodiert. Ein Algorithmus ist polynomiell, wenn er polynomiell in der
*Bitlänge* der Eingabe ist. (Briefing, kanonisch. `[VERIFIZIERT]`)
Verwandte Varianten: Laufzeit polynomiell in der *Anzahl der Lösungen*, in der
*Baumweite*, im *Zahlenwert*, in der *Anzahl der Iterationen* ohne Schranke dafür.

### F5 — Verstecktes exponentielles Objekt

Der Algorithmus läuft "polynomiell viele Runden", aber jede Runde manipuliert eine
Datenstruktur, deren Größe nicht polynomiell beschränkt wird (LP-Relaxierungen mit
exponentiell vielen Constraints, Gröbnerbasen, "alle Teilmengen von Größe k" mit
unbeschränktem k, semidefinite Programme ohne Separationsorakel).

### F6 — Verwechslung von average-case, worst-case und heuristischer Praxis

"Moderne SAT-Solver lösen Instanzen mit Millionen Variablen" ist *keine* Aussage über
P vs. NP. Umgekehrt ist "zufällige Instanzen nahe der Schwelle sind hart" weder bewiesen
noch äquivalent zu P ≠ NP.

### F7 — Barrieren werden nicht adressiert

Aaronson (Signs) sinngemäß: Der Beweis *"doesn't 'know about' all known techniques for
polynomial-time algorithms, including dynamic programming, linear and semidefinite
programming, and holographic algorithms"*, und er *"doesn't prove any weaker results
along the way: for example, P ≠ PSPACE, NEXP ⊄ P/poly, NP ⊄ TC⁰, permanent not
equivalent to determinant by linear projections, SAT requires superlinear time."*
`[NUR-SNIPPET]` — Konfidenz: **mittel-hoch**.

**Das ist der schärfste Einzeltest überhaupt:** Eine Technik, die stark genug für
P ≠ NP ist, sollte auf dem Weg dorthin mindestens ein bekanntes offenes Problem
*mit* lösen. Tut sie das nicht, ist sie fast sicher nicht stark genug — oder sie ist so
speziell auf SAT zugeschnitten, dass sie eine versteckte Modellannahme enthält (→ F2).

### F8 — Formale/soziale Marker (Aaronson, "Ten Signs", 2008)

- Sign 1: *"The authors don't use TeX."* Aaronson: fängt bereits ≥ 60 % der falschen
  Durchbrüche. `[NUR-SNIPPET]`
- Sign 2: *"The authors don't understand the question."*
- Sign 4: *"The approach conflicts with a known impossibility result (which the authors
  never mention)."*
- Sign 10: *"The techniques just seem too wimpy for the problem at hand."* — laut
  Aaronson das schwammigste, aber in vielen Fällen entscheidende Kriterium.
  `[NUR-SNIPPET]` — Konfidenz: **mittel**.

### F9 — Beweis durch Umdefinition

Man definiert "Algorithmus", "Polynomialzeit", "Berechnung" oder "Nichtdeterminismus"
neu (oft über Informationstheorie, Physik, Beobachter-Semantik, Kategorientheorie),
beweist etwas über den neuen Begriff und behauptet, P vs. NP gelöst zu haben. Die
Suchtreffer dieser Recherche enthalten mehrere aktuelle Preprints dieses Typs
(z.B. "Observer-Theoretic Separation", "N-Frame Model", "Computation Environments").
Nicht geprüft, nicht zu prüfen, nicht zu zitieren. `[CLAIM]`

### F10 — Nichtkonstruktive Selbstimmunisierung

Der Autor erklärt jede Kritik als Missverständnis, statt sie als Fehler anzuerkennen
oder den Satz abzuschwächen. Siehe C.4: Nach der Kritik erschien "Further Explanations",
das die Kritik als "misinterpretations" rahmt — die Einschränkung auf lange Klauseln wird
darin aber tatsächlich eingeräumt, was in der Sache ein Rückzug ist.

---

# Teil C — Angriff auf KI-gestützte Ansätze

## C.1 Warum "LLM findet cleveren Algorithmus für 3-SAT" a priori extrem unwahrscheinlich ist

### (a) Der Suchraum ist nicht das Problem — das Zielobjekt ist es

Wenn P = NP gilt, existiert ein Polynomialzeit-Algorithmus für 3-SAT. Aber:

- Die algorithmische Community hat seit **1960 (DPLL)** systematisch gesucht. Der beste
  bekannte Worst-Case-Algorithmus für 3-SAT läuft in **O(1,3071^n)** (PPSZ, Paturi–Pudlák–
  Saks–Zane 1998; Hertli 2011/2014; Hansen–Kaplan–Zamir–Zwick STOC 2019; Scheder,
  *PPSZ is better than you think*, TheoretiCS 2024 — Stand ca. 1,307^n).
  `[VERIFIZIERT]` (mehrere unabhängige Treffer) — Konfidenz: **hoch**.
- Der Fortschritt über 25 Jahre bewegt sich in der **dritten Nachkommastelle der Basis**.
  Von 1,3071^n auf n^k ist kein Optimierungsschritt, sondern ein Kategorienwechsel.
- Ein LLM sampelt aus einer durch Trainingsdaten induzierten Verteilung über
  Algorithmenbeschreibungen. Diese Verteilung ist auf publizierten Algorithmen
  zentriert — also genau auf dem Raum, der bereits erschöpfend durchsucht wurde.
  Die Wahrscheinlichkeitsmasse liegt dort, wo definitiv nichts ist.

`[EIGENE EINSCHÄTZUNG]` — Konfidenz: **hoch**.

### (b) Das Trainingsdatenproblem ist schlimmer als "keine Daten"

Es ist nicht so, dass es zu wenig Trainingsdaten zu P vs. NP gäbe. Es gibt **sehr viele**
— und ein großer Teil davon sind **falsche Beweisversuche** und deren Aufbereitung in
Blogs, Foren, Preprints und populärer Literatur. Das Modell lernt also die *Form* eines
P-vs-NP-Arguments überproportional aus der Klasse der fehlerhaften Argumente. Die
naheliegendste Ausgabe eines LLM zu "beweise P ≠ NP" ist deshalb genau ein Text, der wie
ein typischer gescheiterter Beweis aussieht — flüssig, strukturiert, mit korrekt
klingenden Definitionen, und mit dem Fehler an der Stelle, an der er in den Trainingsdaten
auch steht (typischerweise F2/F3). Das ist bei den beiden unten analysierten Papers exakt
eingetreten. `[EIGENE EINSCHÄTZUNG]` — Konfidenz: **mittel-hoch**.

### (c) Die empirische Basis ist schwach

LLMs sind auf SAT-artigen Aufgaben schlecht und skalieren schlecht:

- **SATBench** (EMNLP 2025 Main, arXiv:2505.14615): 2100 aus SAT-Formeln generierte
  Logikrätsel. o4-mini erreicht auf harten **UNSAT**-Fällen **65,0 %** — nahe der
  50-%-Zufallsbaseline. Systematische Fehler: *satisfiability bias*, *context
  inconsistency*, *condition omission*. `[VERIFIZIERT]` (ACL Anthology, peer-reviewt) —
  Konfidenz: **hoch**.
- Mehrere Arbeiten 2025/2026 zeigen abrupten Leistungsabfall, sobald der "reasoning
  horizon" wächst. `[PREPRINT]` — Konfidenz: **mittel**.

Ein Modell, das nicht zuverlässig entscheiden kann, ob eine kleine Formel unerfüllbar
ist, ist kein plausibler Kandidat dafür, die Unerfüllbarkeits-Struktur *im Allgemeinen*
zu durchschauen.

### (d) Die Verifikationsasymmetrie — und warum sie die Richtung des Risikos bestimmt

Das ist der wichtigste methodische Punkt dieses Berichts:

| | Claim "P = NP" (Algorithmus) | Claim "P ≠ NP" (Lower Bound) |
|---|---|---|
| Verifikation | **billig und definitiv**: implementieren, gegen SAT-Competition-Instanzen laufen lassen, Laufzeit messen. Ein falscher Algorithmus fällt in Stunden durch. | **teuer und mühsam**: es gibt kein Experiment. Nur Zeile-für-Zeile-Prüfung durch Fachleute oder Formalisierung. |
| Fehlerrisiko | gering, weil der Realitätstest sofort greift | **hoch**, weil ein subtiler Fehler auf Seite 41 jahrelang unentdeckt bleiben kann |
| KI-Risiko | KI kann hier tatsächlich nützlich sein (Code generieren, testen) | KI ist hier maximal gefährlich: flüssiger, plausibler Prosa-Beweis ohne Realitätstest |

**Konsequenz:** Die realistische Gefahr für dieses Projekt ist *nicht*, dass wir
versehentlich P = NP behaupten. Sie ist, dass wir einen **KI-generierten oder
KI-assistierten P ≠ NP-artigen Plausibilitätstext** für einen Fortschritt halten. Das
Akzeptanzprotokoll in Teil D ist deshalb für Lower-Bound-Claims deutlich strenger.
`[EIGENE EINSCHÄTZUNG]` — Konfidenz: **hoch**.

---

## C.2 Kritische Analyse: arXiv:2309.05689, "Large Language Model for Science: A Study on P vs. NP"

**Bibliographie** `[PREPRINT]`, Konfidenz **mittel-hoch** (über drei Suchanfragen
konsistent; Titelblatt nicht einsehbar):

- Titel: *Large Language Model for Science: A Study on P vs. NP*
- arXiv:2309.05689, September 2023
- **Autoren: Qingxiu Dong, Li Dong, Ke Xu, Guangyan Zhou, Yaru Hao, Zhifang Sui, Furu Wei**
- Affiliationen: Microsoft Research, Peking University, Beihang University, Beijing
  Technology and Business University
- **Kein Nachweis einer Peer-Review-Publikation gefunden.** Es bleibt ein Preprint.

**Was drinsteht:** Ein Framework "Socratic reasoning", in dem GPT-4 rekursiv Teilprobleme
entdeckt, löst und integriert. Fünf Rollen als "collaborative provers" (z.B. "ein
Mathematiker mit Expertise in Wahrscheinlichkeitstheorie"). **97 Dialogrunden** (14 für
ein Beweisschema, 83 für "rigorous reasoning"). Ergebnis: Konstruktion "extrem harter"
NP-vollständiger Instanzen, die angeblich nicht unter O(2^n) lösbar sind; Schluss:
**"P ≠ NP"**.

### Der entscheidende Befund: das ist Zirkularität, keine Entdeckung

Das Paper formuliert seinen Schluss ausdrücklich als *"in alignment with (Xu and Zhou,
2023)"*. Und **Ke Xu und Guangyan Zhou sind Koautoren dieses Papers**. `[PREPRINT]`,
über zwei Suchanfragen konsistent bestätigt — Konfidenz: **mittel-hoch**.

Damit fällt die Schlagzeile in sich zusammen:

> GPT-4 hat nicht selbstständig einen Beweis für P ≠ NP gefunden. GPT-4 wurde von zwei
> Koautoren, die ein eigenes (später als fehlerhaft kritisiertes) P ≠ NP-Argument
> vertreten, in 97 Dialogrunden dahin geführt, dieses Argument zu reproduzieren.

Alle Einwände gegen Xu–Zhou (C.3) treffen damit *automatisch auch* dieses Paper.

### Methodischer Status

`[EIGENE EINSCHÄTZUNG]`, Konfidenz **hoch**:

- **Es ist kein Beweis.** Es ist eine Fallstudie zu einer Prompting-Methodik, bei der der
  Gegenstand zufällig P vs. NP ist. Selbst wenn man die Methodik interessant findet, ist
  die Aussage über P vs. NP inhaltlich exakt so stark wie die Vorlage, aus der sie
  stammt — nämlich nicht.
- **Massives Confirmation-Bias-Design.** Ein Dialogprotokoll, bei dem der menschliche
  Gesprächspartner die Richtung vorgibt und 97 Runden lang nachfasst, ist kein
  Erkenntnisverfahren, sondern ein Führungsverfahren. LLMs sind stark sykophantisch; eine
  83-runden-lange "rigorous reasoning"-Kette unter Anleitung eines Autors, der das Ziel
  kennt, hat keinerlei unabhängige Evidenzkraft.
- **Kein Erfolgskriterium, keine Falsifikationsmöglichkeit.** Es wird nicht angegeben, was
  hätte passieren müssen, damit das Experiment *fehlschlägt*. Das ist das Kennzeichen
  einer nicht-wissenschaftlichen Demonstration.
- **Kein Umgang mit den Barrieren.** Nichts im Paper adressiert Relativization, Natural
  Proofs oder Algebrization. Ein Argument, das O(2^n)-Schranken für konstruierte
  Instanzen behauptet, muss sagen, warum es nicht relativiert. Das tut es nicht.
- **Keine Formalisierung.** Keine Lean-/Coq-Formalisierung, kein maschinenprüfbarer
  Artefakt.

### Reaktionen der Community

`[NUR-SNIPPET]`, Konfidenz **mittel**: Ich habe **keine publizierte, begutachtete
Widerlegung** dieses Papers gefunden — was erwartbar ist, weil Fachleute Preprints ohne
Journalanspruch typischerweise ignorieren. Dokumentiert ist Kritik auf der
Hugging-Face-Paper-Seite, sinngemäß: die Aussagen zu P und NP seien *"extremely likely to
be incorrect and should be taken with a grain of salt"*, weil die Arbeit die bekannten
Barrieren nicht adressiere, an denen sich jeder Lösungsversuch messen lassen muss. Ich
konnte diese Seite nicht selbst öffnen (Domain blockiert); die Zuschreibung stammt aus
einem Suchsnippet. **Als Beleg nur mit dieser Einschränkung verwendbar.**

**Fazit C.2** `[EIGENE EINSCHÄTZUNG]`, Konfidenz **hoch**:
Dieses Paper ist für unsere Fragestellung ("hat KI substanziellen Fortschritt gebracht?")
ein **klares Negativergebnis**, und zwar ein besonders instruktives: Der Anschein von
KI-Fortschritt entstand hier durch die Kombination aus (i) einer prominenten Affiliation,
(ii) einem suggestiven Titel und (iii) einer verdeckten Koautorenschaft, die den
"unabhängigen" Schluss des Modells in Wahrheit vorgab.

---

## C.3 Kritische Analyse: "SAT requires exhaustive search" (Xu & Zhou) — der Schlüsselfall

### Bibliographie

`[VERIFIZIERT]` (Journal-Publikation über mehrere Quellen bestätigt) — Konfidenz **hoch**:

- Ke Xu, Guangyan Zhou: *SAT requires exhaustive search*,
  **Frontiers of Computer Science**, Vol. 19, Issue 12, Artikel **1912405**,
  Dezember 2025. DOI **10.1007/s11704-025-50231-4**. Springer Nature / Higher Education
  Press.
- Preprint-Historie: arXiv:2302.09512, erste Fassung Februar 2023 (früher Titel u.a.
  *"Hard Examples Requiring Exhaustive Search do Exist"*), mindestens neun Versionen
  bis v9.
- Nachtrag der Autoren: arXiv:2401.01193, *Further Explanations on "SAT Requires
  Exhaustive Search"*, Januar 2024. `[PREPRINT]`

### Was behauptet wird

Durch Konstruktion extrem harter Beispiele von **CSP (mit großen Domänen)** und
**SAT (mit langen Klauseln)** wird bewiesen, dass diese Instanzen nicht ohne
erschöpfende Suche lösbar sind — was **stärker als P ≠ NP** sei. Konkret: SAT benötige
mindestens 2^{δn} Zeit für jede Konstante δ ∈ (0,1). Die Methode wird als
"white-box diagonalization" über "self-referential" CSPs beschrieben und ausdrücklich in
die Tradition von **Gödels** Unmöglichkeitsresultaten gestellt. `[NUR-SNIPPET]` —
Konfidenz: **mittel-hoch**.

### Der entscheidende Punkt 1: eingeschränktes Berechnungsmodell / unzulässige Annahme

**Eric Allender (Rutgers) und Ryan Williams (MIT)** haben einen formellen Kommentar in
derselben Zeitschrift publiziert:

- *Comment on "SAT requires exhaustive search"*, Frontiers of Computer Science,
  DOI **10.1007/s11704-025-53000-5**, online 17. September 2025, zugeordnet zu
  Vol. 20 (2026), Artikel **2001405**. `[VERIFIZIERT]` — Konfidenz: **hoch**.

Kern ihrer Kritik `[NUR-SNIPPET]`, über zwei Suchanfragen konsistent, Konfidenz
**mittel-hoch**:

> Das Argument *"falls far short of a proof, as it makes an assumption about all possible
> SAT algorithms that is unwarranted"*. Konkret: die Autoren *"flatly assume that any
> algorithm for SAT must proceed by reducing an instance of size n to instances of size
> n−1 in a particular way"*. Da das Theorem beansprucht, dass **jeder** CSP-Algorithmus
> im Wesentlichen d^n Zeit braucht, müssten sie die Existenz **jedes** schnelleren
> Algorithmus ausschließen — und genau das tun sie nicht.

**Das ist Fehlermuster F2 in seiner reinsten publizierten Form.** Es handelt sich de
facto um einen Lower Bound für eine **eingeschränkte Algorithmenklasse** (solche, die
über downward self-reducibility Variable für Variable belegen), nicht für
Polynomialzeit-Algorithmen im Allgemeinen.

**Unabhängige Bestätigung:** M. C. Chavrimootoo, Y. He, M. Kotler-Berkowitz, H. Liuson,
Z. Nie, *Evaluating the Claims of "SAT Requires Exhaustive Search"*, arXiv:2312.02071
(4. Dezember 2023). Befund: *"a flaw in Xu and Zhou's main theorems"* — die für die
downward self-reducibility benötigte Struktur **existiert nicht notwendigerweise**;
folglich beweist das Paper **weder SETH noch P ≠ NP**. `[PREPRINT]`, über zwei
Suchanfragen konsistent — Konfidenz: **mittel-hoch**.

### Der entscheidende Punkt 2: es gilt nicht für 3-SAT

Dies ist die vom Koordinator angefragte Verifikation. **Bestätigt**, und zwar aus dem
Mund der Autoren selbst:

In *Further Explanations* (arXiv:2401.01193) stellen Xu und Zhou klar, dass ihre Arbeit
sich auf das **long clause SAT problem** bezieht, das sich fundamental von 3-SAT
unterscheide, und dass ihre Ergebnisse **nicht auf k-SAT-Probleme mit Klauseln konstanter
Länge, also insbesondere nicht auf 3-SAT, anwendbar sind**. Sie führen sogar aus, dass
untere Schranken für 3-SAT schwer zu beweisen seien, weil dort diverse effektive
Strategien zur Vermeidung erschöpfender Suche existierten; nur bei ihren extrem harten
Beispielen sei erschöpfende Suche die einzige Option. `[PREPRINT]`, über zwei
unterschiedlich formulierte Suchanfragen konsistent — Konfidenz: **mittel-hoch**.

### Was daraus für P vs. NP folgt — und was nicht

`[EIGENE EINSCHÄTZUNG]`, Konfidenz **hoch**:

**Es folgt nichts für P vs. NP.** Und zwar aus einem strukturellen Grund, den man sauber
aussprechen muss:

1. **"Lange Klauseln" ist keine Nebenbedingung, sondern der ganze Punkt.** SAT mit
   Klauseln, deren Länge mit n wächst, hat eine **andere Eingabekodierungs-Ökonomie** als
   3-SAT. Eine Instanz mit n Variablen und Klauseln der Länge Θ(n) hat eine Eingabelänge,
   die selbst schon polynomiell in n mit höherem Grad ist. Eine Schranke "2^{δn}" ist
   dann nicht mehr automatisch eine Schranke "exponentiell in der Eingabelänge".
2. **P ≠ NP ist eine Aussage über eine *Sprache*, nicht über eine *Instanzenfamilie*.**
   Zu zeigen, dass *bestimmte konstruierte Instanzen* hart sind, ist grundsätzlich zu
   schwach. P = NP würde bedeuten, dass es einen Algorithmus gibt, der **alle** Instanzen
   in Polynomialzeit löst. Ein Lower Bound auf einer speziellen Familie schließt das nur
   dann aus, wenn die Familie in Polynomialzeit *konstruierbar* und *Teil der Sprache*
   ist **und** die Schranke gegen alle Algorithmen gilt. Punkt 3 der Allender/Williams-
   Kritik zerstört genau die letzte Bedingung.
3. **Die Selbstauskunft der Autoren zieht dem Claim den Boden weg.** Wenn das Resultat
   für 3-SAT nicht gilt, und 3-SAT NP-vollständig ist, dann trennt das Resultat P nicht
   von NP — denn P = NP würde ja gerade heißen, dass 3-SAT in P liegt, und darüber sagt
   die Arbeit explizit nichts.
4. **Umgekehrt gilt:** Wäre der Satz für *long clause SAT* über *alle* Algorithmen
   korrekt bewiesen, wäre das ein gewaltiges Resultat (stärker als SETH). Genau deshalb
   ist die a-priori-Wahrscheinlichkeit, dass er korrekt ist, sehr klein (Aaronson Sign
   10: die Technik ist zu schwach für den behaupteten Ertrag).

### Der Publikationsvorgang selbst ist ein Datenpunkt

`[VERIFIZIERT]` in den Eckdaten, `[NUR-SNIPPET]` in den Details — Konfidenz
**mittel-hoch**:

- *Frontiers of Computer Science* hat das Paper nach eigenen Angaben "after extensive and
  thorough deliberations" publiziert und **gleichzeitig Kommentare von sieben Experten**
  mitveröffentlicht (mit Zustimmung von Autoren und Gutachtern), samt Appendix mit einem
  Extended Abstract.
- Allender/Williams' Kommentar ist einer davon; er sagt unmissverständlich, das Argument
  sei kein Beweis.
- Eric Allender hat den Vorgang zum Anlass für einen Gastbeitrag im Computational
  Complexity Blog genommen (4. August 2025): *"Some thoughts on journals, refereeing, and
  the P vs NP problem"* — ausgelöst durch *"an (incorrect) P ≠ NP proof recently published
  in Springer Nature's Frontiers of Computer Science"*.

**Bewertung** `[EIGENE EINSCHÄTZUNG]`, Konfidenz **hoch**: Das Vorgehen des Journals ist
zweischneidig. Positiv: maximale Transparenz, die Kritik steht direkt daneben, niemand
kann sich auf "peer-reviewed, also korrekt" berufen. Negativ: **Der Vorgang produziert
einen zitierfähigen Journalartikel in einem Springer-Nature-Journal, der im Titel
behauptet, SAT brauche erschöpfende Suche.** Für jede Sekundärrezeption — Presse,
Wikipedia, LLM-Trainingskorpora, unser eigenes Projekt — ist das ein Falschsignal. Es ist
damit auch ein **Vergiftungsereignis für zukünftige Trainingsdaten**.

**Für unser Papier ist das der wichtigste Einzelbefund:** "Peer-reviewed" ist ein
notwendiges, aber bei weitem **nicht hinreichendes** Kriterium. Das Akzeptanzprotokoll
muss das abbilden.

---

## C.4 Was KI tatsächlich geleistet hat — und was das für P vs. NP heißt

Damit der Bericht nicht nur negativ ist: Es gibt echte, nachprüfbare KI-Erfolge in
angrenzenden Bereichen. Sie sind relevant, weil sie die **Form** möglicher KI-Beiträge
zeigen — und weil sie genau die Grenze markieren.

### AlphaEvolve (Google DeepMind, Mai 2025) `[CLAIM]`/`[VERIFIZIERT]`-gemischt

- **4×4-Matrixmultiplikation mit 48 skalaren Multiplikationen** über nicht-kommutativen
  Ringen — erste Verbesserung gegenüber Strassen (49) seit 1969. Unabhängige
  Verifikationen des konkreten Algorithmus existieren (öffentliche
  Verifikations-Repositories). Konfidenz: **mittel-hoch**.
- Angewandt auf **über 50 offene Probleme** in Analysis, Geometrie, Kombinatorik und
  Zahlentheorie: in **~75 %** der Fälle wurde die beste bekannte Lösung *rekonstruiert*,
  in **~20 %** verbessert. `[NUR-SNIPPET]` — Konfidenz: **mittel**.

**Was das bedeutet** `[EIGENE EINSCHÄTZUNG]`, Konfidenz **hoch**: Das ist *Suche in einem
präzise spezifizierten, endlichen, maschinell verifizierbaren Raum* mit einer
**automatischen Bewertungsfunktion**. Genau diese drei Voraussetzungen fehlen bei
P vs. NP vollständig: Der Raum der Beweise ist nicht endlich parametrisiert, es gibt
keine Bewertungsfunktion für "wie nah ist dieser Beweisversuch", und der Erfolg ist nicht
automatisch testbar. **Die AlphaEvolve-Erfolge sind daher kein Indiz für P-vs-NP-Fortschritt,
sondern ein Beleg dafür, dass KI genau dort hilft, wo P vs. NP nicht ist.**

### Formale Verifikation und AI-Beweise (2025/2026)

- **Ryan Williams, *Simulating Time With Square-Root Space*, STOC 2025 (Best Paper)**:
  TIME[t] ⊆ SPACE[O(√(t log t))]. Verbessert Hopcroft–Paul–Valiant (O(t/log t)) nach
  50 Jahren. `[VERIFIZIERT]` — Konfidenz: **hoch**. Wichtig für uns: **Das ist ein
  menschliches Resultat**, und es ist der markanteste Komplexitätsdurchbruch der jüngsten
  Zeit. Der echte Fortschritt im Feld kam 2025 nicht von KI.
- **Navier–Stokes, September 2026:** OpenAI gab am 8. September 2026 ein Blowup-Resultat
  bekannt, erzeugt von ~10.000 parallelen Agenten über 88 Stunden, in **Lean**
  verifiziert. Unmittelbar darauf Prioritätsstreit mit **Tristan Buckmaster** (NYU) und
  Levent Alpöge; OpenAI trat die Priorität für das 3D-Euler-Resultat ab, beanspruchte sie
  für das Navier–Stokes-Resultat. Quanta Magazine berichtete am 8. September 2026.
  `[VERIFIZIERT]` (mehrere unabhängige Berichte), Details `[NUR-SNIPPET]` — Konfidenz:
  **mittel-hoch**.
  **Entscheidend für unser Protokoll:** Auch mit fertiger Lean-Verifikation blieb die
  Frage offen, ob die **Formalisierung die richtige Aussage** trifft — und das behauptete
  Resultat ist **nicht** das Millennium-Problem (das die Gleichungen ohne äußere Kräfte
  betrifft). Lean beweist Konsistenz der Ableitung, nicht Relevanz des Satzes.
- **Lance Fortnow, *Respect the P v NP Problem*, Computational Complexity Blog,
  10. Juni 2026.** Er stellt ausdrücklich die Frage *"Is an AI-generated proof of P ≠ NP
  around the corner?"* Die in Suchsnippets wiedergegebene Antwort lautet sinngemäß
  **"No, it isn't"**, verbunden mit der Aussage, er glaube nicht, zu seinen Lebzeiten
  einen P-vs-NP-Beweis zu sehen, *"proven by man or machine, separately or working
  together"*. `[NUR-SNIPPET]` — **Konfidenz: mittel.** Die Existenz und das Datum des
  Posts sind bestätigt; die wörtlichen Zitate stammen aus Suchsnippets, die ich nicht am
  Volltext prüfen konnte. **Im Abschlusspapier bitte nur mit diesem Vorbehalt zitieren.**

---

## C.5 Halluzination von Literatur — das operative Risiko dieses Projekts

Das ist kein abstraktes Risiko. Es ist die wahrscheinlichste Art, wie *dieses Projekt*
scheitert.

### Die Datenlage

`[PREPRINT]`/`[VERIFIZIERT]`-gemischt, `[NUR-SNIPPET]`, Konfidenz **mittel**:

- GPT-3.5 bzw. GPT-4 erzeugten in kurzen Literaturübersichten über 42 Themen **55 %**
  bzw. **18 %** erfundene Zitate.
- Eine 2025er Studie fand **19,9 %** vollständig erfundene Zitate bei GPT-4o über sechs
  simulierte Literaturübersichten; bei weniger sichtbaren Themen stieg die Rate auf
  **28–29 %**, bei sehr prominenten Themen sank sie auf **~6 %**.
- Ein Benchmark ("GhostCite") über 13 LLMs, 40 Domänen und 375.000 Zitate fand
  Halluzinationsraten von **14 % bis 95 %**.
- Für **Informatik**-Referenztitel wurden Raten von **47 % (GPT-4)** bis **77 %
  (Llama 2 7B)** berichtet.
- Eine systematische Untersuchung von ACL/NAACL/EMNLP 2024–2025 fand **fast 300
  angenommene Papers mit mindestens einem halluzinierten Zitat**, mit steigender Tendenz.
- Lance Fortnow/Bill Gasarch, *AI and Research Papers* (Januar 2026): KI sei nützlich für
  Grammatik und LaTeX-Tabellen, aber beim Schreiben *"it may make stuff up and create
  citations out of thin air"*. `[NUR-SNIPPET]`

Diese Zahlen stammen aus unterschiedlichen Studien mit unterschiedlichen Methoden und
sind nicht direkt vergleichbar. Die Größenordnung ist aber robust: **zweistellige
Prozentsätze.**

### Warum P vs. NP der Worst Case ist

`[EIGENE EINSCHÄTZUNG]`, Konfidenz **hoch**:

1. **Es gibt viele real existierende falsche Paper.** Ein halluziniertes Zitat der Form
   "X, *A Polynomial-Time Algorithm for SAT*, arXiv 2021" ist hier **nicht implausibel** —
   solche Paper gibt es wirklich. Der Plausibilitätsfilter, der in anderen Feldern greift,
   greift hier nicht.
2. **Autorennamen sind wiederverwendbar.** "Razborov", "Williams", "Aaronson",
   "Impagliazzo" an einen erfundenen Titel zu hängen, ergibt einen perfekt plausiblen
   Eintrag.
3. **Venue-Namen sind eng und bekannt** (STOC, FOCS, CCC, JACM, SICOMP, ECCC) und daher
   leicht korrekt-klingend zu halluzinieren.
4. **Der Long Tail ist genau unser Arbeitsbereich.** Die Halluzinationsrate ist bei
   *wenig sichtbaren* Themen am höchsten — und Barrieren-Details, Widerlegungspapers und
   Kommentare sind genau das.

### Absicherungsprotokoll für dieses Projekt (verbindlich)

`[EIGENE EINSCHÄTZUNG]`, Konfidenz **hoch**:

- **Z1 — Zwei-Quellen-Regel.** Jede zitierte Arbeit muss über **zwei voneinander
  unabhängige, unterschiedlich formulierte Suchanfragen** mit **übereinstimmendem Titel
  UND mindestens einem übereinstimmenden Autor** auffindbar sein. Ansonsten:
  `[UNVERIFIZIERT]`.
- **Z2 — Persistenter Identifikator.** Für jede Quelle muss mindestens eines vorliegen:
  DOI, arXiv-ID, ECCC-Nummer, ACM-DL-Link, stabile Autoren-Homepage. Ein bloßer Titel
  reicht nie.
- **Z3 — Venue-Jahr-Konsistenz.** Venue und Jahr müssen zusammenpassen (keine
  STOC-Publikation in einem Jahr ohne STOC-Zitation; keine Journalversion vor der
  Konferenzversion). Widersprüche ⟹ `[UNVERIFIZIERT]`.
- **Z4 — Nie aus dem Gedächtnis zitieren.** Kein Agent dieses Projekts darf eine
  Referenz aus Modellwissen einfügen, ohne sie über Z1 bestätigt zu haben. Das gilt auch
  für "kanonische" Arbeiten, denn genau dort ist die Versuchung am größten.
- **Z5 — Seitenzahlen, Nummern, Zitate markieren.** Bandnummern, Seitenzahlen,
  Theoremnummern und wörtliche Zitate sind in dieser Umgebung **nicht** verifizierbar und
  müssen als `[NUR-SNIPPET]` gekennzeichnet oder weggelassen werden.
- **Z6 — Quantitative Werte doppelt belegen.** Jede Zahl (Prozentwert, Laufzeitschranke,
  Anzahl von Claims) braucht zwei unabhängige Treffer oder die Markierung
  "unbestätigter Einzelwert".

**Selbstanwendung:** Ich habe in diesem Bericht nach Z1–Z6 gearbeitet. Alles, was ich
nicht triangulieren konnte, ist markiert. Die Zahlen zur Woeginger-Verteilung (61/49/6),
die Razborov–Rudich-Parameter, die Fortnow-Zitate und die genaue
Frontiers-Artikelnummerierung sind `[NUR-SNIPPET]`.

### Die Metaebene: der Erdős-Vorfall, Oktober 2025

`[VERIFIZIERT]` (mehrere unabhängige Berichte), Konfidenz **hoch**:

OpenAI-Mitarbeiter (Kevin Weil, Mark Sellke) kündigten öffentlich an, GPT-5 habe
**10 zuvor ungelöste Erdős-Probleme** gelöst und bei elf weiteren Fortschritte erzielt.
**Thomas Bloom**, Betreiber von `erdosproblems.com`, widersprach sofort: "open" auf seiner
Seite bedeute nur, dass **er** die Lösung nicht kenne — nicht, dass das Problem ungelöst
sei. GPT-5 hatte im Wesentlichen **bereits publizierte Lösungen aus der Literatur
wiedergefunden**. Die Posts wurden gelöscht. Demis Hassabis (DeepMind) nannte den Vorgang
öffentlich *"embarrassing"*. Gary Marcus dokumentierte den Vorgang als "Erdősgate".

**Warum das hierher gehört** `[EIGENE EINSCHÄTZUNG]`, Konfidenz **hoch**:
Der Fehler war nicht, dass das Modell halluzinierte. Der Fehler war, dass **kompetente
Menschen mit starken Anreizen** eine Modellausgabe nicht auf ihre *Semantik* prüften
("was heißt 'open' in dieser Datenbank?"), bevor sie sie öffentlich als Durchbruch
verkauften. Genau dieser Fehlertyp — **nicht Halluzination, sondern unkritische
Interpretation der Ausgabe** — ist das realistische Risiko für unser Projekt.

---

# Teil D — Akzeptanzprotokoll: Was würde mich überzeugen?

## D.0 Grundprinzip

`[EIGENE EINSCHÄTZUNG]`, Konfidenz **hoch**.

> **Die Beweislast liegt vollständig beim Claim.** Die Nullhypothese lautet: *Jeder
> behauptete Fortschritt an P vs. NP ist falsch.* Diese Nullhypothese hat eine empirische
> Basis von ≥ 116 zu 0. Sie wird nicht durch Plausibilität, Eleganz, Affiliation,
> Presseresonanz oder die Länge eines Dokuments erschüttert, sondern ausschließlich durch
> die unten genannten Gates.

Die Gates sind **konjunktiv**, nicht als Punktesystem zu lesen. Es gibt keine
Kompensation: Ein fehlendes K3 wird nicht durch ein besonders starkes K7 aufgewogen.

## D.1 Die Gates

### K0 — Präzise Aussage (Ausschlussgate)
Der Claim muss die **Standarddefinitionen** benutzen: Turingmaschine (oder ein
nachweislich polynomiell äquivalentes Modell), Laufzeit in der **Bitlänge** der Eingabe,
Worst Case, eine konkret benannte NP-vollständige Sprache. Jede Neudefinition von
"Algorithmus", "Berechnung" oder "Polynomialzeit" ⟹ **sofortige Ablehnung** (F9). Die
behauptete Aussage muss in einem Satz formal hinschreibbar sein.

### K1 — Explizite Barrieren-Adressierung (Ausschlussgate)
Ein eigener, benannter Abschnitt muss für **jede** der drei Barrieren angeben:
- **Relativization:** An welcher Stelle benutzt der Beweis eine Eigenschaft der Maschine
  bzw. des Schaltkreises, die unter einem Orakel zusammenbricht? Zeilengenau.
- **Natural Proofs:** Welche der beiden Bedingungen — **constructivity** oder
  **largeness** — wird verletzt, und **wo**? "Mein Beweis ist nicht natural" ohne diese
  Angabe ist wertlos.
- **Algebrization:** Warum überträgt sich das Argument nicht auf algebraische Orakel
  (low-degree extensions)?

Fehlt dieser Abschnitt, lese ich das Papier nicht weiter. Das ist kein Snobismus, sondern
Ökonomie: die Barrieren sind bewiesene Sätze, und ein Beweis, der sie nicht verletzt,
ist beweisbar falsch.

### K2 — Der Differenzierungstest (Ausschlussgate, F1)
Der Claim muss **explizit benennen**, wo im Beweis die Eigenschaft eingeht, die
3-SAT von **2-SAT**, **XORSAT** und **Horn-SAT** unterscheidet — und warum das Argument
für diese, in P liegenden Probleme **nicht** durchgeht. Diese drei Fälle sind
Pflichtprüfungen; weitere je nach Ansatz (Matching, planare Instanzen, beschränkte
Baumweite).
Für P=NP-Claims analog: Warum funktioniert der Algorithmus nicht auch für ein
PSPACE-vollständiges oder unentscheidbares Problem?

### K3 — Der Modellumfang-Test (Ausschlussgate, F2/F3)
Falls eine untere Schranke behauptet wird: **Über welche Klasse von Algorithmen wird
quantifiziert?** Die Antwort muss lauten "alle Turingmaschinen mit polynomieller Laufzeit"
und muss **bewiesen** sein, nicht angenommen.
Rote Flaggen, die zur Ablehnung führen:
- "ein Algorithmus muss die Variablen nacheinander belegen"
- "eine Instanz der Größe n muss auf Instanzen der Größe n−1 reduziert werden"
- "der Algorithmus muss den Lösungsraum durchsuchen"
- "erschöpfende Suche ist die einzige Möglichkeit"
Wird die Schranke tatsächlich nur für ein eingeschränktes Modell bewiesen, ist das ein
**legitimes Resultat**, muss aber als solches deklariert werden — und es ist **kein**
Fortschritt an P vs. NP (siehe C.3).

### K4 — Der Zwischenresultat-Test (starkes Indiz, F7)
Eine Technik, die stark genug für P ≠ NP ist, sollte auf dem Weg **mindestens ein
bekanntes offenes Problem mitlösen**. Kandidaten: P ≠ PSPACE, NEXP ⊄ P/poly, NP ⊄ TC⁰,
Permanent nicht linear auf Determinante projizierbar, SAT braucht superlineare Zeit,
NP ⊄ ACC⁰, eine Schranke > 4n für allgemeine Schaltkreise.
Wenn ein Ansatz das *schwierigste* Problem des Feldes löst und **kein einziges** der
leichteren, ist er mit sehr hoher Wahrscheinlichkeit falsch.

### K5 — Peer Review, aber qualifiziert
Publikation in einem begutachteten Venue ist **notwendig, nicht hinreichend**. Der Fall
Xu/Zhou in *Frontiers of Computer Science* (Springer Nature, Dezember 2025) zeigt, dass
ein falscher P≠NP-Beweis den Prozess passieren kann — inklusive gleichzeitig
publizierter Widerlegung durch Allender und Williams.
Anerkannt werden: STOC, FOCS, CCC, ITCS, SODA, ICALP, J.ACM, SICOMP, *Computational
Complexity*, *Theory of Computing*, *TheoretiCS*, ACM ToCT, *Annals of Mathematics*.
**Zusätzlich verlangt:** eine **namentliche, positive Stellungnahme** von mindestens zwei
Fachleuten mit eigener Publikationsbilanz im Bereich Schaltkreiskomplexität oder
Beweiskomplexität, die erklären, den Beweis **gelesen** zu haben.

### K6 — Maschinengeprüfte Formalisierung (bei Lower-Bound-Claims: Pflicht)
Wegen der Verifikationsasymmetrie (C.1d) verlange ich bei jedem P≠NP-artigen Claim eine
**vollständige Formalisierung in Lean 4, Rocq/Coq oder Isabelle**, mit:
- öffentlich verfügbarem, reproduzierbar kompilierendem Repository,
- **null** `sorry`/`admit`/`axiom`-Lücken (vollständige Audit-Liste der Axiome),
- **und — entscheidend — einer separaten, von Fachleuten geprüften Begründung, dass das
  formalisierte Statement tatsächlich P ≠ NP ist.**

Der letzte Punkt ist der Kern. Der Navier–Stokes-Vorgang vom September 2026 zeigt genau
das: *"A Lean-verified proof gives confidence in logical consistency, but mathematicians
still need to confirm the formalization actually matches the statement that counts."*
`[NUR-SNIPPET]` Eine Lean-Datei, die eine subtil falsche Definition von "polynomial time"
verwendet, ist ein maschinengeprüfter Beweis von gar nichts. **Formalisierung verlagert
die Vertrauensfrage von der Beweisführung auf die Definitionen — sie eliminiert sie
nicht.**

### K7 — Unabhängige Reproduktion
- **Bei P = NP:** Lauffähige Implementierung + Laufzeitmessungen auf
  SAT-Competition-Instanzen und auf Instanzen mit wachsendem n, mit veröffentlichtem
  Fit an ein Polynom. Diese Prüfung ist billig und muss **vor** jeder weiteren Diskussion
  stattfinden. Zusätzlich: Anwendung auf Faktorisierung eines RSA-Challenge-Moduls oder
  auf eine kryptographische Instanz — ein echter P=NP-Algorithmus ist sofort
  demonstrierbar.
- **Bei P ≠ NP:** mindestens zwei unabhängige Gruppen, die den Beweis vollständig
  durchgearbeitet und ihre Prüfung öffentlich dokumentiert haben.

### K8 — Offene, dokumentierte Prüfung
Der Deolalikar-Prozess ist das Vorbild: öffentliche Prüfung auf Blog/Wiki/Zulip,
namentlich, nachlesbar, mit dokumentierten Einwänden und Antworten. Ein Claim, der nur
per Pressemitteilung und ohne offene Diskussion existiert, wird nicht bewertet.

### K9 — Die Clay-Uhr
Für die stärkste Akzeptanzstufe gilt die Clay-Regel: Publikation in einem begutachteten
Journal von Weltrang **plus zwei Jahre allgemeiner Akzeptanz in der mathematischen
Community** danach, bevor das Scientific Advisory Board überhaupt eine detaillierte
Prüfung erwägt. `[VERIFIZIERT]` (Clay-Regeln), Konfidenz **hoch**.
Diese Regel existiert genau deshalb, weil die Community weiß, dass Fehler oft erst nach
Monaten gefunden werden.

## D.2 Sofort-Disqualifikatoren

Bei einem dieser Merkmale beende ich die Prüfung ohne weitere Begründung:

| # | Merkmal |
|---|---|
| D-1 | Barrieren werden nicht erwähnt (K1) |
| D-2 | Das Argument gilt erkennbar auch für 2-SAT / XORSAT / Horn-SAT (K2) |
| D-3 | "SAT/NP braucht erschöpfende Suche" ohne formal spezifiziertes Berechnungsmodell (K3) |
| D-4 | Pseudopolynomiell als polynomiell ausgegeben (F4) |
| D-5 | Neudefinition zentraler Begriffe (F9) |
| D-6 | Der Autor antwortet auf Kritik ausschließlich mit "Missverständnis", ohne Satz oder Beweis zu ändern (F10) |
| D-7 | Zitate im Papier sind nicht auffindbar (Halluzinationsverdacht, C.5) |
| D-8 | Der Beweis existiert nur als LLM-Dialogprotokoll (C.2) |
| D-9 | Ein behaupteter P=NP-Algorithmus wird nicht implementiert oder ist nicht lauffähig |
| D-10 | Es gibt keine benannte Person, die erklärt, den Beweis vollständig gelesen zu haben |

## D.3 Asymmetrisches Protokoll

| | P = NP behauptet | P ≠ NP behauptet |
|---|---|---|
| Erste Prüfung | **Implementieren und laufen lassen** (Stunden) | Barrieren-Abschnitt lesen (Minuten) |
| Formalisierung | wünschenswert | **Pflicht (K6)** |
| Warum | empirisch falsifizierbar | nicht empirisch falsifizierbar |
| Typischer Fehler | F4, F5 (versteckte Exponentialität) | F1, F2, F3 (zu viel bewiesen / Modell eingeschränkt) |
| Zeit bis Widerlegung | Stunden bis Tage | Wochen bis Jahre — oder nie |

## D.4 Was dieses Projekt unter den gegebenen Bedingungen *nicht* leisten kann

`[EIGENE EINSCHÄTZUNG]`, Konfidenz **hoch**. Dieser Abschnitt gehört ins Abschlusspapier.

Ohne `WebFetch`, ohne Volltextzugriff und ohne die Möglichkeit, Code auszuführen oder
Lean zu kompilieren, gilt:

1. **Wir können K6 nicht anwenden.** Wir können keine Lean-Entwicklung auschecken,
   kompilieren, auf `sorry` prüfen oder die Definitionen im Formalisat lesen. Jede
   Aussage von uns zu einer Formalisierung wäre reine Sekundärrezeption.
2. **Wir können K7 nicht anwenden.** Wir können keinen behaupteten SAT-Algorithmus
   implementieren und messen.
3. **Wir können K2, K3, K4 nur mittelbar anwenden.** Ob ein Argument auch für XORSAT
   durchgeht, lässt sich nur am Volltext entscheiden. Wir können nur berichten, ob
   *andere* diesen Test gemacht haben (was wir bei Deolalikar und bei Xu/Zhou tatsächlich
   konnten).
4. **Wir können Zitate nur bis auf Titel- und Autorenebene prüfen** (Z1–Z3), nicht bis
   auf Seiten-, Theorem- oder Wortlautebene (Z5).
5. **Wir können keine wörtlichen Zitate garantieren.** Alle in diesem Bericht in
   Anführungszeichen gesetzten englischen Passagen stammen aus Suchmaschinen-Snippets und
   sind entsprechend markiert. Sie könnten Paraphrasen sein.

**Konsequenz für die Formulierung des Abschlusspapiers:** Jede unserer Aussagen über
einen konkreten Beweisversuch muss die Form haben
*"X wurde von Y (Venue, Datum) mit der Begründung Z kritisiert"*
und **nicht**
*"X ist falsch, weil Z"*.
Der Unterschied ist nicht kosmetisch. Wir sind in diesem Projekt **Rezeptionsforscher,
nicht Gutachter.** Wer das verwischt, wiederholt genau den Fehler, den die OpenAI-
Mitarbeiter im Erdős-Vorfall gemacht haben: eine Modellausgabe als Prüfung auszugeben.

## D.5 Selbstbindung des Projekts

Ich schlage folgende Regeln für das Gesamtprojekt vor:

- **S-1:** Das Papier behauptet an keiner Stelle Fortschritt an P vs. NP. Die
  Fragestellung "hat KI hier substanziellen Fortschritt gebracht?" wird mit einem
  begründeten **Nein** beantwortet — das ist nach Grundregel 4 des Briefings ein
  vollwertiges Resultat.
- **S-2:** Jeder KI-bezogene Claim in der Literatur wird mit der Frage geprüft:
  *Wer hat es unabhängig verifiziert, und was genau hat diese Person geprüft?*
- **S-3:** Die Verifikationsasymmetrie (C.1d) wird im Papier explizit erklärt. Sie ist
  der nützlichste einzelne Gedanke dieses Berichts für Leser.
- **S-4:** Die Methodenbeschränkung (Abschnitt 0 und D.4) wird im Papier offengelegt, mit
  der Liste der blockierten Domains.
- **S-5:** Die Fehlermuster F1–F10 werden als Prüfliste abgedruckt. Das ist der
  praktische Nutzen, den ein Leser mitnehmen kann.
- **S-6:** Kein Agent zitiert eine Quelle, die nicht nach Z1–Z3 bestätigt ist.

---

## Anhang: Bewertungstabelle der behandelten Claims

| Claim | Status | Geprüft von | Befund | Konfidenz |
|---|---|---|---|---|
| Deolalikar, P ≠ NP (2010) | `[CLAIM]`, widerlegt, nie publiziert | Immerman, Tao, Williams, Impagliazzo u.a., öffentlich | XORSAT-Gegenbeispiel; Ordnungsproblem in FO(LFP); Kernannahme unbewiesen | hoch |
| Xu & Zhou, *SAT requires exhaustive search* (FCS 19(12), 2025) | `[CLAIM]`, publiziert, kritisiert | Allender & Williams (FCS-Kommentar 2025/26); Chavrimootoo et al. (arXiv:2312.02071) | unzulässige Annahme über alle SAT-Algorithmen; gilt laut Autoren **nicht für 3-SAT**; beweist weder SETH noch P ≠ NP | mittel-hoch |
| Dong et al., *LLM for Science: P vs. NP* (arXiv:2309.05689) | `[PREPRINT]`, kein Beweis | keine formale Widerlegung gefunden; informelle Kritik | reproduziert das Argument der eigenen Koautoren Xu & Zhou; kein Barrieren-Abschnitt; keine Formalisierung | mittel-hoch |
| Williams, NEXP ⊄ ACC⁰ (JACM 2014) | `[VERIFIZIERT]` | Peer Review, breite Akzeptanz | echtes Resultat, umgeht alle drei Barrieren — aber astronomisch weit von P ≠ NP | hoch |
| Williams, TIME[t] ⊆ SPACE[O(√(t log t))] (STOC 2025) | `[VERIFIZIERT]` | Best Paper Award | echter Durchbruch, **menschlich erbracht** | hoch |
| Bürgisser–Ikenmeyer–Panova (JAMS 2019) | `[VERIFIZIERT]` | Peer Review | GCT via occurrence obstructions ist unmöglich | hoch |
| AlphaEvolve 4×4-Matrixmultiplikation (48) | `[CLAIM]`, unabhängig nachgerechnet | öffentliche Verifikations-Repos | echt, aber in einem Bereich mit endlichem Suchraum und automatischer Bewertung | mittel-hoch |
| GPT-5 "löst 10 Erdős-Probleme" (Okt. 2025) | `[CLAIM]`, zurückgezogen | Thomas Bloom, Community | Modell fand publizierte Lösungen; Posts gelöscht | hoch |
| OpenAI Navier–Stokes (Sep. 2026) | `[CLAIM]`, Lean-verifiziert, umstritten | Buckmaster/Alpöge (Priorität), Community (Formalisierungs-Semantik) | betrifft **nicht** das Millennium-Problem im engeren Sinn | mittel-hoch |

---

## Quellen

Alle URLs wurden über WebSearch aufgefunden. **Kein Volltext konnte abgerufen werden**
(siehe Abschnitt 0). Die Markierungen geben den erreichten Verifikationsgrad an.

### Barrieren und Komplexitätstheorie

1. T. Baker, J. Gill, R. Solovay, *Relativizations of the P =? NP Question*, SIAM J. Comput. 4(4), 1975. `[VERIFIZIERT]` (kanonisch) — Lehrmaterial: https://pages.cs.wisc.edu/~jyc/02-810notes/lecture08.pdf
2. A. Razborov, S. Rudich, *Natural Proofs*, STOC 1994 / JCSS 55(1), 1997. `[VERIFIZIERT]` — https://dl.acm.org/doi/pdf/10.1145/195058.195134
3. S. Aaronson, A. Wigderson, *Algebrization: A New Barrier in Complexity Theory*, STOC 2008 / ACM ToCT 1(1), 2009; ECCC TR08-005. `[VERIFIZIERT]` — https://www.scottaaronson.com/papers/alg.pdf , https://eccc.weizmann.ac.il/report/2008/005/
4. R. Impagliazzo, V. Kabanets, A. Kolokolova, *An Axiomatic Approach to Algebrization*, STOC 2009. `[NUR-SNIPPET]` — https://www.cs.sfu.ca/~kabanets/Research/algebrization.html
5. R. Williams, *Non-Uniform ACC Circuit Lower Bounds*, CCC 2011 / JACM 61(1):2, 2014. `[VERIFIZIERT]` — https://www.cs.cmu.edu/~ryanw/acc-lbs.pdf , https://dl.acm.org/doi/10.1145/2559903
6. R. Williams, *A Casual Tour Around a Circuit Complexity Bound*, arXiv:1111.1261. `[PREPRINT]` — https://arxiv.org/abs/1111.1261
7. C. Murray, R. Williams, *Circuit Lower Bounds for Nondeterministic Quasi-Polytime: An Easy Witness Lemma for NP and NQP*, STOC 2018. `[VERIFIZIERT]` — https://dl.acm.org/doi/10.1145/3188745.3188910 , https://people.csail.mit.edu/rrw/easy-witness-nqp.pdf
8. L. Chen, X. Lyu, R. Williams, *Non-deterministic Quasi-Polynomial Time is Average-Case Hard for ACC Circuits*, FOCS 2019 / SICOMP. `[VERIFIZIERT]` — https://doi.org/10.1137/20M1321231
9. R. Williams, *Natural Proofs Versus Derandomization*, arXiv:1212.1891 / STOC 2013. `[NUR-SNIPPET]` — https://arxiv.org/pdf/1212.1891
10. R. Williams, *Simulating Time With Square-Root Space*, STOC 2025 (Best Paper). `[VERIFIZIERT]` — https://dl.acm.org/doi/10.1145/3717823.3718225 , https://people.csail.mit.edu/rrw/time-vs-space.pdf
11. M. Find, A. Golovnev, E. Hirsch, A. Kulikov, *A Better-Than-3n Lower Bound for the Circuit Complexity of an Explicit Function*, FOCS 2016. `[NUR-SNIPPET]` — https://www.nist.gov/publications/better-3n-lower-bound-circuit-complexity-explicit-function
12. J. Li, T. Yang, *3.1n − o(n) Circuit Lower Bounds for Explicit Functions*, STOC 2022. `[NUR-SNIPPET]` — https://dl.acm.org/doi/abs/10.1145/3519935.3519976 , https://eccc.weizmann.ac.il/report/2021/023/
13. P. Bürgisser, C. Ikenmeyer, G. Panova, *No occurrence obstructions in geometric complexity theory*, FOCS 2016 / J. Amer. Math. Soc. 32(1), 2019. `[VERIFIZIERT]` — https://arxiv.org/abs/1604.06431 , https://www.ams.org/journals/jams/2019-32-01/S0894-0347-2018-00908-7/viewer/
14. S. Aaronson, *Is P Versus NP Formally Independent?*, Bulletin of the EATCS, Okt. 2003. `[VERIFIZIERT]` (Venue) / `[NUR-SNIPPET]` (Inhalt) — https://www.scottaaronson.com/papers/indep.pdf
15. A. Kolokolova, *Complexity Barriers as Independence*, in *The Incomputable*, Springer 2017. `[NUR-SNIPPET]` — https://www.cs.mun.ca/~kol/papers/barriers-incomputable-revised.pdf
16. L. Fortnow, *The Status of the P versus NP Problem*, CACM 52(9), 2009. `[VERIFIZIERT]` — https://dl.acm.org/doi/10.1145/1562164.1562186 , https://lance.fortnow.com/papers/files/pnp-cacm.pdf
17. L. Fortnow, *Fifty Years of P vs. NP and the Possibility of the Impossible*, CACM, 2022. `[VERIFIZIERT]` — https://cacm.acm.org/research/fifty-years-of-p-vs-np-and-the-possibility-of-the-impossible/
18. *Diagonalization Without Relativization: A Closer Look at the Baker–Gill–Solovay Theorem*, arXiv:2601.09702, Jan. 2026. `[PREPRINT]` **unverifiziert, Autoren und Rezeption unbekannt — nicht zitieren** — https://arxiv.org/pdf/2601.09702

### Basisraten und gescheiterte Beweisversuche

19. G. J. Woeginger, *The P-versus-NP Page*, TU Eindhoven, letzte Aktualisierung Sep. 2016 (116 Einträge; 61 × "P=NP", 49 × "P≠NP", 6 × sonstige). `[VERIFIZIERT]` / Verteilung `[NUR-SNIPPET]` — https://wscor.win.tue.nl/woeginger/P-versus-NP.htm
20. Fortführung/Spiegel der Liste. `[NUR-SNIPPET]`, Eintragszahl und Stand **nicht verifiziert** — https://mikinty.github.io/P-vs-NP/
21. M. Wehar, *P-vs-NP-Community*. `[NUR-SNIPPET]` — https://github.com/MichaelWehar/P-vs-NP-Community
22. Polymath-Wiki, *Deolalikar P vs NP paper*. `[VERIFIZIERT]` (Existenz) / `[NUR-SNIPPET]` (Inhalt) — https://michaelnielsen.org/polymath/index.php?title=Deolalikar_P_vs_NP_paper
23. R. Lipton, *A Proof That P Is Not Equal To NP?*, 8.8.2010 — https://rjlipton.com/2010/08/08/a-proof-that-p-is-not-equal-to-np/
24. R. Lipton, *Deolalikar Responds To Issues About His P≠NP Proof*, 11.8.2010 — https://rjlipton.com/2010/08/11/deolalikar-responds-to-issues-about-his-p%E2%89%A0np-proof/
25. R. Lipton, K. Regan, *Fatal Flaws in Deolalikar's Proof?*, 12.8.2010 — https://rjlipton.com/2010/08/12/fatal-flaws-in-deolalikars-proof/
26. R. Lipton, *An Update On Vinay Deolalikar's "Proof"*, 15.9.2010 — https://rjlipton.com/2010/09/15/an-update-on-vinay-deolalikars-proof/
27. HP Labs News, August 2010 — https://www.hpl.hp.com/news/2010/jul-sep/deolalikar.html
28. TechCrunch, *Attempt At P ≠ NP Proof Gets Torn Apart Online*, 12.8.2010 — https://techcrunch.com/2010/08/12/fuzzy-math/
29. S. Aaronson, *Eight Signs A Claimed P≠NP Proof Is Wrong*, 2010. `[NUR-SNIPPET]` — https://scottaaronson.blog/?p=458
30. S. Aaronson, *Ten Signs a Claimed Mathematical Breakthrough is Wrong*, 2008. `[NUR-SNIPPET]` — https://scottaaronson.blog/?p=304
31. B. Gasarch, L. Fortnow, *P v NP Papers Galore*, Computational Complexity Blog, April 2025. `[NUR-SNIPPET]` — https://blog.computationalcomplexity.org/2025/04/p-v-np-papers-galore.html
32. E. Allender (Gastbeitrag), *Some thoughts on journals, refereeing, and the P vs NP problem*, Computational Complexity Blog, 4.8.2025. `[NUR-SNIPPET]` — https://blog.computationalcomplexity.org/2025/08/some-thoughts-on-journals-refereeing.html
33. Kritik-Serie (Hemaspaandra-Gruppe u.a.), `[PREPRINT]`: arXiv:2205.01189 (Uribe), arXiv:2404.04395 (Du), arXiv:2112.06062 (Kumar), arXiv:2312.04395 (Czerwinski), arXiv:2507.09018 (Deng)
34. W. Gasarch, *Guest Column: The Third P=?NP Poll*, SIGACT News 50(1), März 2019 (124 Antwortende; ≈ 80 % P ≠ NP; unter Vielbefassten deutlich höher). `[VERIFIZIERT]` (Venue) / Zahlen `[NUR-SNIPPET]` — https://dl.acm.org/doi/10.1145/3319627.3319636 , https://www.cs.umd.edu/users/gasarch/BLOGPAPERS/pollpaper3.pdf
35. W. Gasarch, *Guest Column: The Second P =? NP Poll*, SIGACT News, 2012 (152 Antwortende). `[NUR-SNIPPET]` — https://www.cs.umd.edu/~gasarch/papers/poll2012.pdf
36. Clay Mathematics Institute, *Rules for the Millennium Prize Problems* (begutachtete Journalpublikation + zwei Jahre allgemeine Akzeptanz). `[VERIFIZIERT]` — https://www.claymath.org/millennium-problems/rules/ , https://www.claymath.org/wp-content/uploads/2022/03/millennium_prize_rules_0.pdf

### KI-Ansätze und die beiden Schlüsselpapers

37. K. Xu, G. Zhou, *SAT requires exhaustive search*, Frontiers of Computer Science 19(12):1912405, Dez. 2025, DOI 10.1007/s11704-025-50231-4. `[VERIFIZIERT]` (Publikation) — https://journal.hep.com.cn/fcs/EN/10.1007/s11704-025-50231-4 , https://dl.acm.org/doi/10.1007/s11704-025-50231-4
38. K. Xu, G. Zhou, *SAT Requires Exhaustive Search*, arXiv:2302.09512 (v1 Feb. 2023, bis v9). `[PREPRINT]` — https://arxiv.org/abs/2302.09512
39. E. Allender, R. Williams, *Comment on "SAT requires exhaustive search"*, Frontiers of Computer Science, DOI 10.1007/s11704-025-53000-5, online 17.9.2025, Vol. 20 (2026) Art. 2001405. `[VERIFIZIERT]` (Publikation) / Inhalt `[NUR-SNIPPET]` — https://link.springer.com/article/10.1007/s11704-025-53000-5 , https://people.cs.rutgers.edu/~allender/papers/allender.williams.pdf
40. M. C. Chavrimootoo, Y. He, M. Kotler-Berkowitz, H. Liuson, Z. Nie, *Evaluating the Claims of "SAT Requires Exhaustive Search"*, arXiv:2312.02071, 4.12.2023. `[PREPRINT]` — https://arxiv.org/abs/2312.02071
41. K. Xu, G. Zhou, *Further Explanations on "SAT Requires Exhaustive Search"*, arXiv:2401.01193, 2.1.2024 (enthält das Eingeständnis, dass das Resultat **nicht für 3-SAT** gilt). `[PREPRINT]` — https://arxiv.org/abs/2401.01193 , Review: https://www.themoonlight.io/en/review/further-explanations-on-sat-requires-exhaustive-search
42. Q. Dong, L. Dong, K. Xu, G. Zhou, Y. Hao, Z. Sui, F. Wei, *Large Language Model for Science: A Study on P vs. NP*, arXiv:2309.05689, Sep. 2023. `[PREPRINT]`, keine Peer-Review-Publikation nachgewiesen — https://arxiv.org/abs/2309.05689 , https://huggingface.co/papers/2309.05689
43. *SATBench: Benchmarking LLMs' Logical Reasoning via Automated Puzzle Generation from SAT Formulas*, EMNLP 2025 Main / arXiv:2505.14615. `[VERIFIZIERT]` — https://aclanthology.org/2025.emnlp-main.1716/
44. R. Paturi, P. Pudlák, M. Saks, F. Zane (PPSZ, 1998); T. Hertli (2011/2014); Hansen–Kaplan–Zamir–Zwick, STOC 2019; D. Scheder, *PPSZ is better than you think*, TheoretiCS 2024 — Stand der Technik ≈ O(1,307^n) für 3-SAT. `[VERIFIZIERT]`/`[NUR-SNIPPET]` — https://theoretics.episciences.org/13222 , https://dl.acm.org/doi/10.1145/3313276.3316359 , https://link.springer.com/article/10.1007/s00037-024-00259-y

### KI, Halluzination, Rezeption 2025/2026

45. Google DeepMind, *AlphaEvolve: A Gemini-powered coding agent for designing advanced algorithms*, Mai 2025. `[CLAIM]` — https://deepmind.google/blog/alphaevolve-a-gemini-powered-coding-agent-for-designing-advanced-algorithms/ ; unabhängige Verifikation: https://github.com/PhialsBasement/AlphaEvolve-MatrixMul-Verification
46. G. Marcus, *Erdősgate*, Marcus on AI, Okt. 2025. `[VERIFIZIERT]` (Vorgang, mehrfach belegt) — https://garymarcus.substack.com/p/erdosgate
47. The Decoder, *Leading OpenAI researcher announced a GPT-5 math breakthrough that never happened*, Okt. 2025 — https://the-decoder.com/leading-openai-researcher-announced-a-gpt-5-math-breakthrough-that-never-happened/
48. L. Fortnow, *Respect the P v NP Problem*, Computational Complexity Blog, 10.6.2026. Existenz und Datum `[VERIFIZIERT]`; Zitate `[NUR-SNIPPET]`, **Konfidenz mittel** — https://blog.computationalcomplexity.org/2026/06/respect-p-v-np-problem.html
49. L. Fortnow / B. Gasarch, *AI and Research Papers*, Jan. 2026. `[NUR-SNIPPET]` — https://blog.computationalcomplexity.org/2026/01/ai-and-research-papers.html
50. L. Fortnow / B. Gasarch, *The Future of Mathematics and Mathematicians*, Feb. 2026. `[NUR-SNIPPET]`, Inhalt nicht erschlossen — https://blog.computationalcomplexity.org/2026/02/the-future-of-mathematics-and.html
51. L. Fortnow, *Navier-Stokes and Lean*, Sep. 2026. `[NUR-SNIPPET]` — https://blog.computationalcomplexity.org/2026/09/navier-stokes-and-lean.html
52. OpenAI, *On the Navier–Stokes Millennium Prize Problem*, 8.9.2026. `[CLAIM]` — https://openai.com/index/navier-stokes-solution/
53. Quanta Magazine, *AI Has Solved One of Math's $1 Million Millennium Prize Problems*, 8.9.2026. `[NUR-SNIPPET]` — https://www.quantamagazine.org/ai-has-solved-one-of-maths-1-million-millennium-prize-problems-20260908/
54. *Navier–Stokes priority controversy*, Wikipedia (Prioritätsstreit Buckmaster/OpenAI). `[NUR-SNIPPET]` — https://en.wikipedia.org/wiki/Navier%E2%80%93Stokes_priority_controversy
55. Studien zu Zitationshalluzination (Auswahl, alle `[NUR-SNIPPET]`): arXiv:2604.03173 (*Detecting and Correcting Reference Hallucinations*), arXiv:2603.03299 (*How LLMs Cite and Why It Matters*), arXiv:2604.03159 (*BibTeX Citation Hallucinations*), PMC12658395 (*Citation Fabrication in Mental Health Research*) — https://pmc.ncbi.nlm.nih.gov/articles/PMC12658395/ ; Pressemitteilung: https://www.eurekalert.org/news-releases/1106130

---

*Ende des Berichts. Abgelegt unter `docs/01-redteam-S2.md` gemäß Auftrag. Hinweis:
`docs/00-briefing.md` sieht als Ablage `research/<agent-id>-<thema>.md` vor; die explizite
Anweisung des Koordinators (docs/01-redteam-S2.md) hat Vorrang. Bei Bedarf bitte eine
Kopie unter `research/S2-redteam.md` anlegen oder das Briefing angleichen.*
