# A5 — Algebraische Komplexität & Geometric Complexity Theory

**Agent:** A5
**Forschungsfrage:** Was hat die algebraische Route seit ca. 2020 geliefert, und wie weit
trägt sie Richtung P vs. NP?
**Stand:** 12.09.2026
**Methodik:** ausschließlich WebSearch (WebFetch gesperrt, vgl. `docs/02-team-briefing.md` §1).
Kein Volltextzugang. Alles, was nicht aus dem kanonischen Kontext stammt, ist
`[NUR-SNIPPET]`, sofern nicht anders markiert.

> **Tiefenbegrenzung (verbindlich, vorab deklariert).**
> Dieser Bericht ordnet die algebraische Route **ein** und bewertet ihren **Status**.
> Er beurteilt **nicht** die Mathematik. Weder die Korrektheit der LST-Schranken noch
> die darstellungstheoretischen Argumente von Bürgisser–Ikenmeyer–Panova sind von
> diesem Agenten nachgerechnet oder auch nur im Volltext gelesen worden. Wo die Grenze
> der Verifizierbarkeit erreicht ist, steht das explizit im Text (§7).

---

## 1. Kurzfassung

1. **VP vs. VNP ist offen und die Implikationsrichtung wird in populären Darstellungen
   regelmäßig falsch herum erzählt.** Aus VP ≠ VNP folgt **P ≠ NP nicht**. Belegbar ist
   die *Kontraposition in nichtuniformer Form*: aus VP = VNP folgt (über endlichen Körpern
   unbedingt, in Charakteristik 0 unter GRH) ein Kollaps auf der Booleschen Seite.
   `[VERIFIZIERT]` für die Existenz des Resultats (Bürgisser), `[NUR-SNIPPET]` für die
   genaue Formulierung.
2. **Limaye–Srinivasan–Tavenas (FOCS 2021, Best Paper) ist der einzige echte
   Durchbruch der Periode.** Erste superpolynomielle untere Schranken gegen arithmetische
   Schaltkreise **beliebiger konstanter Tiefe** — nach Jahrzehnten, in denen bei
   Produkttiefe 2 Schluss war. `[VERIFIZIERT]` (FOCS-Award-Seite, JACM, CACM Research Highlight).
3. **LST trägt nicht bis VP ≠ VNP.** Die Depth-Reduction-Kette (Agrawal–Vinay, Koiran,
   Tavenas) macht eine n^ω(√d)-Schranke für homogene Tiefe 4 hinreichend für VP ≠ VNP.
   LST liefert superpolynomiell, nicht n^ω(√d). Die Lücke ist qualitativ, nicht kosmetisch.
4. **GCT hat seinen ursprünglich zentralen Plan verloren.** Ikenmeyer–Panova (2016/2017)
   und Bürgisser–Ikenmeyer–Panova (FOCS 2016 / JAMS 2019) haben bewiesen, dass
   *occurrence obstructions* die Permanent-vs-Determinante-Trennung **nicht** leisten können.
   Das ist ein hartes, peer-reviewtes Negativresultat gegen den in Mulmuley–Sohoni
   formulierten Weg. `[VERIFIZIERT]`
5. **Der konkreteste Messwert ist ernüchternd.** Die beste bekannte untere Schranke für
   die **determinantal complexity** der Permanente ist **n²/2** (Mignon–Ressayre 2004),
   über ℝ verbessert auf (n−1)²+1 (Yabe 2015). Gebraucht wird **superpolynomiell**.
   Seit über 20 Jahren steht die Zahl bei „quadratisch". `[NUR-SNIPPET]`, Konfidenz hoch.
6. **GCT ist 2026 kein totes, aber ein umgebautes und deutlich bescheideneres Programm.**
   Es gibt laufende Aktivität (Publikationen bis 2026, Lehrbuch-Survey 2025, Workshops,
   Drittmittel), aber der Anspruch hat sich von „Weg zu P vs. NP" zu „darstellungstheoretische
   Untersuchung von Orbitabschlüssen" verschoben. `[EIGENE EINSCHÄTZUNG, Konfidenz mittel]`

---

## 2. VP vs. VNP — Valiants Hypothese und was aus ihr *wirklich* folgt

### 2.1 Das Modell

Valiant (1979) verlegt die Frage vom Booleschen ins **algebraische** Modell:

- Gerechnet wird mit **arithmetischen Schaltkreisen** über einem Körper K: Eingaben sind
  Variablen und Körperkonstanten, Gatter sind + und ×. Kosten = Anzahl der Gatter.
  Es gibt kein Verzweigen, kein Vergleichen, keine Bitmanipulation.
- Das Objekt ist eine **Polynomfolge** (f_n), nicht eine Sprache.
- **VP** ≈ Polynomfolgen von polynomiell beschränktem Grad, berechenbar durch
  arithmetische Schaltkreise polynomieller Größe.
- **VNP** ≈ Polynomfolgen, die sich als exponentielle Summe („Summe über alle
  Belegungen eines Hilfsvariablenblocks") über etwas aus VP schreiben lassen —
  das algebraische Analogon der Existenzquantifizierung.
- **Vollständige Probleme:** Die **Determinante** det_n ist das Leitobjekt für VP
  (genauer: VP_ws / VBP, die Klasse der algebraischen Branching Programs; det ist dafür
  vollständig), die **Permanente** perm_n ist VNP-vollständig (in Charakteristik ≠ 2).
- **Valiants Hypothese:** VP ≠ VNP. Äquivalent (bis auf die üblichen Feinheiten):
  perm_n ist nicht durch polynomiell große arithmetische Schaltkreise berechenbar;
  in der *determinantalen* Variante: die kleinste Matrix M mit affin-linearen Einträgen
  und det(M) = perm_n hat superpolynomielle Größe („determinantal complexity").

Die Permanente unterscheidet sich von der Determinante formal nur durch das fehlende
Vorzeichen sgn(σ) — und ist trotzdem #P-vollständig (Valiant 1979), während die
Determinante in polynomieller Zeit berechenbar ist. Dieser minimale formale Abstand bei
maximalem Komplexitätsabstand ist der Grund, warum das Paar als Musterfall gilt.
`[VERIFIZIERT]` — kanonischer Lehrbuchstoff.

### 2.2 Warum „leichter"

`[EIGENE EINSCHÄTZUNG, Konfidenz hoch]` — die Begründung ist in der Literatur Standard:

- **Kein Gegenbeispielproblem durch Bit-Tricks.** Arithmetische Schaltkreise sind ein
  strukturell armes Modell. Man darf Grad, Homogenität, Multilinearität, Symmetrie
  ausnutzen — lauter algebraische Invarianten, die im Booleschen Modell schlicht fehlen.
- **Es gibt bereits nichttriviale untere Schranken.** Für eingeschränkte Modelle
  (monotone, multilineare, depth-3 über endlichen Körpern, homogene depth-4,
  seit 2021 konstante Tiefe) existieren superpolynomielle bis exponentielle Schranken.
  Im Booleschen Modell steht man bei allgemeinen Schaltkreisen weiterhin bei
  linearen Schranken (Größenordnung ~3,1n bzw. 5n − o(n) je nach Modell und Jahr).
- **Die Natural-Proofs-Barriere greift nicht in derselben Form.** Razborov–Rudich lebt
  von Pseudozufallsgeneratoren gegen die Schaltkreisklasse; algebraische
  Schranken argumentieren über Rang- und Dimensionsmaße, die nicht „largeness" erfüllen
  müssen. Das ist einer der Hauptgründe, warum die Community die algebraische Route
  überhaupt für aussichtsreicher hält.

### 2.3 Die Implikationsrichtung — der kritische Punkt

**Falsch ist:** „VP ≠ VNP impliziert P ≠ NP."
**Ebenfalls falsch (und der subtilere Fehler):** „VP ≠ VNP ist ein Spezialfall von P ≠ NP,
also folgt es aus P ≠ NP." Auch das gilt nicht in dieser Einfachheit.

Belegbar ist folgendes Bild `[NUR-SNIPPET]`, Kern aus mehreren übereinstimmenden Quellen
(Bürgisser, *Completeness and Reduction in Algebraic Complexity Theory*, Springer 2000,
Kapitel „P Versus NP: A Nonuniform Algebraic Analogue"; Zusammenfassungen im
Complexity Zoo und in Vorlesungsnotizen von Chi-Ning Chou):

> **Wenn VP = VNP** (über ℂ), **dann** — unter der **generalized Riemann hypothesis** (GRH) —
> gilt **P/poly = NP/poly**, ja sogar NC³/poly = P/poly = NP/poly = PH/poly, und
> #P/poly = FP/poly; die Polynomialzeithierarchie kollabiert (auf Σ₂P bzw. die
> nichtuniforme Entsprechung).
> **Über endlichen Körpern** gilt die analoge Aussage **ohne GRH**:
> VP = VNP ⇒ NC²/poly = P/poly = NP/poly = PH/poly.

Was daraus logisch folgt:

- **Kontraposition, korrekt:** Wenn P/poly ≠ NP/poly (was aus P ≠ NP *nicht* folgt,
  sondern stärker ist) und GRH gilt, dann VP ≠ VNP über ℂ. Also: **VP ≠ VNP ist eine
  notwendige Vorstufe** für die nichtuniforme Separation — modulo GRH.
- **Was NICHT folgt:** VP ≠ VNP ⇒ P ≠ NP. Diese Richtung ist **nicht bekannt** und
  gilt nach verbreiteter Auffassung als nicht zu erwarten. Ein Beweis von VP ≠ VNP
  ließe P = NP formal weiterhin offen.
- **Drei Abschwächungen stecken gleichzeitig in der Brücke:**
  1. **Nichtuniformität** — die Konsequenz ist P/poly vs. NP/poly, nicht P vs. NP.
     P/poly ≠ NP/poly ist echt stärker als P ≠ NP. Wer „P ≠ NP" sagt, meint hier etwas anderes.
  2. **GRH** — in Charakteristik 0 hängt die Brücke an einer offenen zahlentheoretischen
     Vermutung. Das ist eine Zusatzannahme im Sinne von **M5** (`docs/02-team-briefing.md` §4).
  3. **Richtung** — die Brücke läuft von „algebraisch leicht" zu „boolesch leicht",
     also verwertbar nur als Kontraposition, und diese endet in der nichtuniformen Welt.

`[EIGENE EINSCHÄTZUNG, Konfidenz hoch]` Für das Papier ist die saubere Formulierung:
**VP ≠ VNP ist notwendig (modulo GRH, in nichtuniformer Lesart), aber nicht hinreichend
für P ≠ NP.** Wer VP ≠ VNP bewiese, hätte ein Millennium-Problem *nicht* gelöst — er hätte
ein bekanntes Hindernis auf einem von mehreren Wegen beseitigt. Diese Unterscheidung ist
die zentrale Fehlschlusskorrektur dieses Berichts (vgl. `docs/01-strategie-S1.md`, Zeile 244
und Arbeitspaket 1.5).

---

## 3. Limaye–Srinivasan–Tavenas (LST) — der Durchbruch von 2021

### 3.1 Was gezeigt wurde

**N. Limaye, S. Srinivasan, S. Tavenas, *Superpolynomial Lower Bounds Against Low-Depth
Algebraic Circuits*, FOCS 2021, S. 804–814.** `[VERIFIZIERT]` für Existenz, Venue, Autoren,
Best-Paper-Award (FOCS-2021-Award-Seite; Meldung der Universität Aarhus).
Journalfassung: **J. ACM 72(4), 2025**; zusätzlich **CACM Research Highlight** mit
Technical Perspective. ECCC TR21-081.

Inhalt `[NUR-SNIPPET]`, aus mehreren übereinstimmenden Snippets:

- Superpolynomielle untere Schranken für **set-multilineare** arithmetische Schaltkreise
  **konstanter Produkttiefe** — und zwar für *alle* konstanten Produkttiefen, sogar für
  Produkttiefen, die asymptotisch kleiner als log log d wachsen.
- Anschließend **„hardness escalation" / Lifting**: Die set-multilinearen Schranken werden
  auf **allgemeine** arithmetische Schaltkreise konstanter Tiefe gehoben.
- Zielpolynom ist im Kern die **iterierte Matrixmultiplikation** IMM_{n,d} (in VP),
  daneben Aussagen über die Permanente.
- Gültigkeit zunächst für Körper der Charakteristik 0 oder hinreichend großer Charakteristik.

**Warum Durchbruch:** Vor LST waren superpolynomielle (non-FPT) untere Schranken für
**keine** Produkttiefe > 2 bekannt — selbst unter der Set-Multilinearitätsrestriktion.
Depth-3 über ℂ und homogene Depth-4 waren die Fronten; darüber hinaus war jahrzehntelang
nichts zu holen. LST durchbricht diese Wand für alle konstanten Tiefen auf einen Schlag.
`[NUR-SNIPPET]`, Konfidenz hoch (mehrfach unabhängig formuliert in Snippets und in der
CACM Technical Perspective).

### 3.2 Wie weit trägt LST — und wie weit nicht

Der entscheidende Kontext ist die **Depth-Reduction-Kette**:

- **Agrawal–Vinay (FOCS 2008), „Arithmetic Circuits: A Chasm at Depth Four"**, verbessert
  von **Koiran (2012)** und **Tavenas (2013)**: Jeder arithmetische Schaltkreis der Größe s,
  der ein Polynom vom Grad d berechnet, lässt sich in einen **homogenen Tiefe-4-Schaltkreis**
  der Größe s^O(√d) umbauen (Erweiterung der Depth-Reduction von Valiant–Skyum–Berkowitz–Rackoff).
- **Folge:** Eine untere Schranke von **n^ω(√n)** für homogene Tiefe-4-Schaltkreise, die
  perm_n berechnen, würde **VP ≠ VNP** liefern. `[NUR-SNIPPET]`, Konfidenz hoch —
  Standardaussage, u.a. in Wigdersons/Shpilkas Übersichten.
- **Analog „Chasm at Depth Three"** (Gupta–Kamath–Kayal–Saptharishi u.a.) für Tiefe 3
  über Körpern der Charakteristik 0.

`[EIGENE EINSCHÄTZUNG, Konfidenz hoch]` **Damit lässt sich der Abstand exakt benennen:**
Der Chasm sagt, dass konstante Tiefe der richtige Ort ist — aber er verlangt eine Schranke
der Form n^ω(√d), also **eine bestimmte quantitative Stärke**. LST liefert
*superpolynomiell*, was schwächer ist. Die spätere Verschärfung von
**Bhargav–Dutta–Saxena** (Form n^Ω(d^{1/φ^{2Δ}}) für Produkttiefe Δ, φ = goldener Schnitt;
„Improved Lower Bound, and Proof Barrier, for Constant Depth Algebraic Circuits",
ACM Trans. Comput. Theory, DOI 10.1145/3689957) verbessert die Abhängigkeit, **erreicht
die Schwelle aber nicht** — und trägt, wie schon der Titel sagt, eine **eigene
Beweisbarriere** bei. Das ist ein Musterfall für M1/M3 aus `docs/02-team-briefing.md` §4.

**Formulierung für das Papier:** LST hat das Feld an eine Stelle gebracht, an der die
Reduktion „konstante Tiefe genügt" nicht mehr nur theoretisch ist — aber der Weg von dort
zu VP ≠ VNP erfordert eine *quantitative* Verstärkung, für die bereits eine dokumentierte
Barriere existiert. Und **selbst VP ≠ VNP ergäbe P ≠ NP nicht** (§2.3). Es sind also
mindestens zwei offene Stufen bis zum Millennium-Problem, davon eine mit eigener Barriere.

### 3.3 Was 2023–2026 daraus geworden ist

`[NUR-SNIPPET]`, Existenz der Arbeiten belegt, Inhalte nur aus Snippets:

| Arbeit | Venue | Beitrag (Snippet-Ebene) |
|---|---|---|
| **Improved Low-Depth Set-Multilinear Circuit Lower Bounds** | CCC 2022 | Verbesserung der set-multilinearen Kernschranke |
| **Low-Depth Arithmetic Circuit Lower Bounds: Bypassing Set-Multilinearization** | ICALP 2023 | Umgeht den set-multilinearen Zwischenschritt |
| **M. Forbes, Low-Depth Algebraic Circuit Lower Bounds over Any Field** | CCC 2024 | Erweitert LST auf **kleine Charakteristik** — LST galt nur für char 0 bzw. große char. Damit: Permanenten-Schranken gegen konstante Tiefe über endlichen Körpern |
| **Bhargav–Dutta–Saxena, Improved Lower Bound, and Proof Barrier, for Constant Depth Algebraic Circuits** | ACM ToCT (DOI 10.1145/3689957) | Quantitative Verbesserung **plus** explizite Beweisbarriere |
| **LST, Journalfassung** | J. ACM 72(4), 2025 | Peer-reviewte Endfassung |
| **AC⁰[p]-Frege Cannot Efficiently Prove that Constant-Depth Algebraic Circuit Lower Bounds are Hard** | ITCS 2026, arXiv 2509.16824 / ECCC TR25-134 | **Metakomplexität**: Beweisbarkeit der LST/Forbes-Schranken in schwachen Beweissystemen |
| **Meta-Mathematics of Algebraic Complexity** | LICS 2026 | Metamathematische Einordnung des Feldes |
| **Separation Results for Constant-Depth and Multilinear Ideal Proof Systems** | arXiv 2601.06299 | IPS-Beweiskomplexität im Anschluss |

**Debordering / border complexity.** **P. Dutta, V. Lysikov, *Recent Advances in
Debordering Methods*, arXiv:2510.13049** (Oktober 2025), 54 Seiten, eingeladener Survey,
under review für Texts & Monographs in Symbolic Computation (TMSC), Special Issue RTCA'23
Paris. `[PREPRINT]` / `[NUR-SNIPPET]`.

Worum es geht: **border complexity** misst, was sich durch Objekte niedriger Komplexität
**approximieren** lässt (Grenzwerte von Schaltkreisfamilien; Klassen wie VP-quer / VP̄).
GCT arbeitet konstruktionsbedingt mit Orbit**abschlüssen**, also automatisch mit
Grenzwerten — die GCT-Variante von Valiants Vermutung ist eine Aussage über
**border** determinantal complexity, nicht über die gewöhnliche.
**Debordering** = obere Schranke eines gewöhnlichen Komplexitätsmaßes durch ein
Border-Maß, also „die Grenzwerte wieder loswerden". Laut Snippet liegt Debordering
„im Kern" der Verbindung zwischen Valiants Determinante-vs-Permanente-Vermutung (1979)
und ihrer GCT-Variante (Mulmuley–Sohoni 2001); historischer Präzedenzfall ist Binis
Debordering der Matrixmultiplikationstensoren (1980).

`[EIGENE EINSCHÄTZUNG, Konfidenz mittel]` **Warum das für unsere Frage zählt:** Die
Existenz eines 54-seitigen Übersichtsartikels zu Debordering-Methoden Ende 2025 zeigt,
dass das Border-Problem als *eigenständiges technisches Hindernis* etabliert ist — es ist
die Stelle, an der die GCT-Formulierung und die klassische Valiant-Formulierung
auseinanderfallen können. Ein GCT-Erfolg beweist zunächst die **Border-Variante**; ob
daraus die eigentliche Vermutung folgt, ist Gegenstand der Debordering-Forschung.
Das ist eine weitere, oft übersehene Zwischenstufe zwischen „GCT gelingt" und „VP ≠ VNP".

### 3.4 Der konkreteste Messwert: determinantal complexity der Permanente

`[NUR-SNIPPET]`, mehrfach triangulierend bestätigt, Konfidenz hoch:

- **Beste bekannte untere Schranke:** dc(perm_n) ≥ **n²/2** — **Mignon–Ressayre (2004)**.
- **Verbesserung über ℝ:** (n−1)²+1 — **Yabe (2015)**, über den Begriff des
  „bi-polynomial rank" (arXiv:1504.00151).
- **Border-Variante:** **Landsberg–Manivel–Ressayre** zeigen, dass die Permanente nicht
  einmal im Abschluss der Polynome mit determinantal complexity < n²/2 liegt — bewiesen
  über die Dimension dualer Varietäten.
- **Benötigt für Valiants Hypothese:** **superpolynomiell** in n.

`[EIGENE EINSCHÄTZUNG, Konfidenz hoch]` **Das ist die nüchternste verfügbare Kennzahl
für den Abstand der algebraischen Route zum Ziel.** Zwischen n²/2 und n^ω(1) liegt kein
Faktor, sondern ein Größenordnungssprung in der Beweistechnik. Die Zahl hat sich seit 2004
im Wesentlichen nicht bewegt. Sie ist das algebraische Gegenstück zu den ~5n-Schranken für
Boolesche Schaltkreise: gleiche Struktur des Problems, nur auf einem höheren Sockel.

**2026-Randnotiz:** K. Sheshadri, *A near-quadratic lower bound on the border determinantal
complexity of Σᵢ xᵢⁿ via conormal specialization*, arXiv:2606.13628, 11.06.2026,
`[PREPRINT]`. Laut Abstract-Snippet die **ersten** border-determinantalen unteren Schranken
für eine explizite Familie, die **superlinear in der Variablenzahl** sind — und zwar
passend zu den bekannten O(n²)-oberen Schranken. **Wichtig für die redliche Einordnung:**
Das Zielpolynom ist die **Potenzsumme** Σxᵢⁿ, **nicht die Permanente**; es ist ein
Einzelautor-Preprint ohne erkennbares Peer Review; und die Schranke ist quadratisch, also
weiterhin himmelweit von superpolynomiell entfernt. Nicht als Fortschritt Richtung
VP ≠ VNP verkaufen.

---

## 4. Geometric Complexity Theory (GCT)

### 4.1 Grundidee

Mulmuley–Sohoni, *Geometric Complexity Theory I: An Approach to the P vs. NP and Related
Problems*, SIAM J. Comput. 31(2), 2001 (Teil II 2008; weitere Teile bis GCT V u.a.).
`[VERIFIZIERT]` für Existenz/Venue.

Die Idee in der für uns verantwortbaren Tiefe:

1. **Von der Komplexitätsaussage zur Geometrie.** „perm_m ist determinantal-komplex" heißt:
   das (mit einer Hilfsvariablen auf Grad n gepolsterte, „padded") Permanentenpolynom
   ℓ^{n−m}·perm_m liegt **nicht** im Bild der linearen Substitutionen von det_n.
   Nimmt man den **Abschluss** (Zariski-Abschluss der GL_{n²}-Bahn), werden aus beiden
   Objekten **projektive Varietäten**: die Orbitabschlüsse
   Ω(det_n) = \overline{GL_{n²}·det_n} und Ω(perm_m^{pad}).
   Die Vermutung wird zur geometrischen Aussage: **Ω(perm^{pad}) ⊄ Ω(det)**.
2. **Warum Abschlüsse?** Weil Bahnen selbst nicht abgeschlossen sind und die Methoden der
   algebraischen Geometrie (Koordinatenringe, Ideale) abgeschlossene Mengen brauchen.
   Preis: man beweist die **Border-Variante** der Vermutung (vgl. §3.3).
3. **Warum Darstellungstheorie?** Beide Varietäten sind **GL-stabil**. Damit sind ihre
   **Koordinatenringe** ℂ[Ω] Darstellungen von GL_{n²}(ℂ) und zerfallen in irreduzible
   Bestandteile (Weyl-Moduln S_λ, indiziert durch Partitionen λ). Eine Inklusion
   Ω(perm^{pad}) ⊆ Ω(det) erzwingt eine **Surjektion** ℂ[Ω(det)] ↠ ℂ[Ω(perm^{pad})],
   also für **jedes** λ: mult_λ(ℂ[Ω(perm^{pad})]) ≤ mult_λ(ℂ[Ω(det)]).
4. **Obstruction.** Findet man ein λ, das diese Ungleichung verletzt, ist die Inklusion
   widerlegt — und die Trennung bewiesen. Ein solches λ heißt **obstruction**.
   - **Occurrence obstruction** (der starke, ursprünglich propagierte Fall):
     mult_λ(ℂ[Ω(perm^{pad})]) > 0, aber mult_λ(ℂ[Ω(det)]) = **0**.
     Also: ein irreduzibler GL-Modul, der in dem einen Koordinatenring **vorkommt**
     und in dem anderen **gar nicht**.
   - **Multiplicity obstruction** (der schwächere, allgemeinere Fall):
     0 < mult_λ(det) < mult_λ(perm^{pad}) — beide kommen vor, aber mit falscher Vielfachheit.
5. **Die „Obstruction Hypothesis"** ist im Kern die Annahme, dass solche λ (a) **existieren**
   und (b) **hinreichend explizit** sind: konstruierbar, verifizierbar, im Idealfall mit
   positiv-kombinatorischer Beschreibung der beteiligten Multiplizitäten (Kronecker-,
   Plethysmus-, „GCT"-Koeffizienten). Dieses Explizitheitsverlangen ist die Brücke zu
   Mulmuleys „explicit proofs" (§5) — die Obstruction soll das **Hardness-Zertifikat** sein.

`[EIGENE EINSCHÄTZUNG, Konfidenz mittel]` — Diese Darstellung folgt den Standardquellen
(Bläser–Ikenmeyer-Survey, Mulmuleys Übersichtsarbeiten). Verifiziert ist sie nur auf
Snippet-Ebene; die Feinheiten (Stabilizer, Padding, Dimensionsbedingungen, Fußpunkt der
Reduktion auf VBP statt VP) sind hier bewusst weggelassen.

### 4.2 Das entscheidende Negativresultat

**C. Ikenmeyer, G. Panova, *Rectangular Kronecker coefficients and plethysms in geometric
complexity theory*, FOCS 2016 / Advances in Mathematics 319 (2017)**
und
**P. Bürgisser, C. Ikenmeyer, G. Panova, *No occurrence obstructions in geometric complexity
theory*, FOCS 2016 / J. Amer. Math. Soc. 32(1), 2019, S. 163–193.**
`[VERIFIZIERT]` — JAMS ist ein Spitzenjournal der reinen Mathematik, peer-reviewt;
Venue, Jahrgang, Seitenzahlen und Autorenschaft sind über mehrere Quellen bestätigt
(AMS-Seite, arXiv:1604.06431, Liverpool Repository).

**Aussage** `[NUR-SNIPPET]`, aber in mehreren Snippets gleichlautend:

> Mulmuley und Sohoni hatten vorgeschlagen, eine verschärfte Fassung der
> Permanent-vs-Determinante-Vermutung über ℂ zu untersuchen, die darauf hinausläuft,
> die Orbitabschlüsse von Determinante und gepolsterter Permanente durch
> **occurrence obstructions** zu trennen — irreduzible GL_{n²}(ℂ)-Darstellungen, die im
> einen Koordinatenring vorkommen und im anderen nicht.
> **Bürgisser–Ikenmeyer–Panova beweisen, dass dieser Ansatz unmöglich ist.**

Technischer Kern (nur so weit, wie belegbar): Es wird gezeigt, dass Positivität gewisser
**Plethysmus-Koeffizienten** die Positivität der zugehörigen **rectangular Kronecker-**
bzw. **GCT-Koeffizienten** nach sich zieht — die gesuchten Null-Multiplizitäten treten also
schlicht nicht auf. Ikenmeyer–Panova hatten kurz zuvor den analogen Schlag gegen die
Variante mit rechteckigen Kronecker-Koeffizienten geführt.

`[EIGENE EINSCHÄTZUNG, Konfidenz hoch]` **Einordnung.** Das ist kein „Rückschlag" im
weichen Sinn, sondern ein **Unmöglichkeitsresultat gegen den namentlich benannten,
zentralen Plan der Originalarbeiten**. Es gehört in dieselbe Kategorie wie die drei
klassischen Barrieren: nicht „wir haben es noch nicht geschafft", sondern „auf diesem
Weg geht es beweisbar nicht". Der Vergleich, den A1 mit Wiles/Taniyama–Shimura zieht,
lässt sich hier zuspitzen: GCT war „ein Plan" — und die Version des Plans, die explizit
ausformuliert war, ist widerlegt.

### 4.3 Die Reaktion des Programms

`[VERIFIZIERT]` für Existenz, `[NUR-SNIPPET]` für Inhalt:

**J. Dörfler, C. Ikenmeyer, G. Panova, *On Geometric Complexity Theory: Multiplicity
Obstructions Are Stronger Than Occurrence Obstructions*, ICALP 2019 /
SIAM J. Applied Algebra and Geometry (DOI 10.1137/19M1287638), arXiv:1901.04576.**

Laut Snippet liefert diese Arbeit **erstmals eine Situation, in der eine Trennung mit
Multiplizitäten gelingt, während die Trennung mit Occurrences beweisbar unmöglich ist** —
und zwar für die **Chow-Varietät** (Produkte homogener Linearformen) und für Polynome
beschränkten **border Waring rank** (höhere Sekantenvarietät der Veronese-Varietät).

Das ist die inhaltliche Rechtfertigung des Übergangs: **occurrence → multiplicity
obstructions**. Wichtig für die redliche Darstellung:

- Es ist ein **Existenzbeweis in Modellsituationen**, nicht für perm vs. det.
- Es zeigt, dass der Übergang **nicht leer** ist — die stärkere Methode kann strikt mehr.
- Es zeigt **nicht**, dass multiplicity obstructions für perm vs. det existieren oder
  gar auffindbar wären.
- Der Preis ist erheblich: Multiplizitäten zu vergleichen ist kombinatorisch weit
  schwerer als Nichtverschwinden festzustellen. Genau hier hängen die
  **Positivitätshypothesen** (Kronecker-, Plethysmuskoeffizienten kombinatorisch
  deuten), die als „formidable" gelten.

**Die konstruktive Linie: Obstructions aus Symmetrien.**
`[VERIFIZIERT]` für Existenz, `[NUR-SNIPPET]` für Inhalt:

- **C. Ikenmeyer, U. Kandasamy, *Implementing geometric complexity theory: On the
  separation of orbit closures via symmetries*, STOC 2020** (arXiv:1911.03990).
  Laut Snippet wird der Orbitabschluss der **Potenzsumme** von dem des **Produkts der
  Variablen** getrennt, indem die **Symmetriegruppen** beider Polynome und deren
  darstellungstheoretische Zerlegungskoeffizienten ausgenutzt werden. Die Konstruktion
  liefert eine **multiplicity obstruction, die weder eine occurrence obstruction noch eine
  „vanishing ideal occurrence obstruction" ist**.
- **P. Dutta, F. Gesmundo, C. Ikenmeyer, G. Jindal, V. Lysikov, *Geometric complexity
  theory for product-plus-power*, Journal of Symbolic Computation 132, 2026**
  (arXiv:2211.07055). Laut Snippet wird der GCT-Ansatz gegen die Potenzsumme
  **vollständig durchgeführt**, indem Ikenmeyer–Kandasamy (STOC'20) auf einen neuen
  Orbitabschluss verallgemeinert wird; es entstehen **neue multiplicity obstructions,
  konstruiert allein aus den Symmetrien der Polynome**.

`[EIGENE EINSCHÄTZUNG, Konfidenz mittel]` **Das ist die beste Nachricht, die GCT in der
Periode zu bieten hat — und sie ist zugleich das präziseste Maß für die Verkleinerung des
Anspruchs.** Die Methode funktioniert **nachweislich und konstruktiv** — aber an
Modellpaaren wie Potenzsumme vs. Produkt von Linearformen, die komplexitätstheoretisch
harmlos sind. Es ist der Übergang von „wir haben einen Plan für perm vs. det" zu
„wir üben die Technik an kleineren Objekten". Das ist legitime und gute Mathematik.
Es ist kein Fortschritt bei perm vs. det.

**Zusätzliche Erschwernis aus der algebraischen Kombinatorik.** Ikenmeyer–Pak
(FOCS) und Ikenmeyer–Pak–Panova (SODA) gehören laut Igor Paks Blog zu der sehr kleinen
Gruppe von **„not in #P"-Resultaten**: Es gibt Hinweise, dass zentrale
darstellungstheoretische Multiplizitäten **keine** positive kombinatorische
(#P-)Beschreibung besitzen. `[NUR-SNIPPET]`, Konfidenz mittel.
`[EIGENE EINSCHÄTZUNG, Konfidenz mittel]` Falls sich das erhärtet, trifft es GCT an einer
empfindlichen Stelle: Mulmuleys Programm will **explizite** Zertifikate, und positive
kombinatorische Formeln sind der natürliche Weg dorthin. Vergleiche Panovas Vortrag
„Computational Complexity in Algebraic Combinatorics" (Yale, 24.10.2025), der
Multiplizitäten explizit in den Kontext der Suche nach multiplicity obstructions für
VP vs. VNP stellt. `[NUR-SNIPPET]`

### 4.4 Mulmuleys Zeitschätzung und die Haltung der Community

- **Zeitschätzung:** Mulmuley selbst veranschlagt für das Programm — falls tragfähig —
  Größenordnungen von **~100 Jahren** bis zur Entscheidung von P vs. NP.
  `[NUR-SNIPPET]`, mehrfach bestätigt (u.a. Wikipedia-Artikel „Geometric complexity theory",
  bereits in `research/00-lead-sondierung.md` und `research/a1-kanon-und-barrieren.md`
  übernommen). Konfidenz hoch für die Existenz der Aussage, niedrig für ihren exakten
  Wortlaut und Kontext.
- **Mulmuleys eigene Positionierung ist moderater, als sie oft wiedergegeben wird.**
  In einem Gastbeitrag auf Lance Fortnows Blog *Computational Complexity*
  („Ketan Mulmuley Responds", 21.04.2008) korrigiert Mulmuley ausdrücklich die ihm
  zugeschriebene Behauptung, jeder Ansatz zur Trennung von P und NP müsse durch GCT gehen:
  **„This is not what I think or said."** Er hält fest: „One cannot really say that GCT is
  the only way to separate P from NP or that any approach must go through it" — es gebe aber
  „good mathematical reasons to believe why it may well be among the 'easiest' approaches
  to the P vs. NP problem". `[NUR-SNIPPET]`, Konfidenz mittel für den exakten Wortlaut.
  **Für das Papier relevant:** Die starke Lesart („GCT ist *der* Weg") ist eine
  Fremdzuschreibung, die der Urheber selbst zurückgewiesen hat. Sie darf ihm nicht
  untergeschoben werden — auch nicht, um ihn zu kritisieren.
- **Programmatische Darstellung für ein Informatikpublikum:** K. Mulmuley,
  *The GCT Program Toward the P vs. NP Problem*, **CACM 55(6), Juni 2012**
  (DOI 10.1145/2184319.2184341) — laut Snippet als Statusaktualisierung zu Fortnows
  CACM-Übersicht gedacht. `[NUR-SNIPPET]`
- **Haltung der Community:** `[EIGENE EINSCHÄTZUNG, Konfidenz mittel]` Aus den zugänglichen
  Signalen (siehe §6) lässt sich ein differenziertes Bild rekonstruieren:
  **Respekt für die Mathematik, Skepsis gegenüber dem Zeitplan, Desinteresse an der
  Rhetorik.** Die Arbeiten von Bürgisser, Ikenmeyer, Panova, Gesmundo, Lysikov, Dutta
  erscheinen in JAMS, Advances in Mathematics, CCC, ICALP, FOCS — das ist erstklassige
  Publikationstätigkeit. Sie sind aber ganz überwiegend **Resultate über GCT-Objekte**
  (Koeffizienten, Varietäten, Debordering), nicht **Fortschritte auf dem Weg zu P vs. NP**.
  Bezeichnend: Die zentralsten Resultate der Periode 2016–2019 sind **negativ**, und sie
  stammen von Leuten **innerhalb** des Programms.

---

## 5. Mulmuleys „complexity barrier"

**K. D. Mulmuley, *On P vs. NP, Geometric Complexity Theory, Explicit Proofs and the
Complexity Barrier*, arXiv:0908.1932, 13.08.2009, 65 Seiten.** `[PREPRINT]`, Existenz
`[VERIFIZIERT]` (arXiv-Listing cs.CC 2009-08, dblp). Verwandte Arbeiten:
arXiv:0908.1936 („…and the Riemann Hypothesis"), *Explicit Proofs and The Flip*
(arXiv:1009.0246).

**Die These, so korrekt wie snippet-belegbar:**

> Mulmuley identifiziert eine fundamentale Wurzelschwierigkeit, die er **„complexity
> barrier"** nennt und die **jede** Beweistechnik überwinden müsse, die diese Probleme
> entscheiden soll; die gesamte mathematische Arbeit in GCT ziele darauf, diese Barriere
> zu überqueren. Die früher bekannten Barrieren (relativization, natural proofs) seien
> zu überwinden, wobei die complexity barrier als der **fundamentalere** Hinderungsgrund
> positioniert wird.
>
> Ein **„explicit proof"** ist bei Mulmuley ein Beweis, der **Hardness-Zertifikate**
> konstruiert, die **leicht zu verifizieren, zu konstruieren und zu dekodieren** sind.

`[NUR-SNIPPET]`, Konfidenz mittel für die Formulierung, hoch für die Grundstruktur
der These.

**Grundgedanke (so weit nachvollziehbar, `[EIGENE EINSCHÄTZUNG, Konfidenz niedrig–mittel]`):**
Die Barriere ist selbstbezüglich: Um zu beweisen, dass ein Problem hart ist, braucht man
ein Zertifikat für Härte; ein solches Zertifikat muss selbst *effizient* sein, sonst ist
der Beweis nicht durchführbar; aber ein effizient konstruierbares und verifizierbares
Härtezertifikat ist genau das, was nach Razborov–Rudich pseudozufällige Funktionen
ausschließt bzw. was die Selbstbezüglichkeit der Komplexitätstheorie erschwert. Mulmuleys
„flip" ist der Vorschlag, diese Schwierigkeit umzudrehen: statt sie zu umgehen, das
Problem gerade in ein **Konstruktionsproblem für explizite Objekte** zu übersetzen (die
Obstructions), und deren Explizitheit über Positivitätsresultate zu erzwingen.

**Akzeptanzgrad** `[EIGENE EINSCHÄTZUNG, Konfidenz mittel]`:
- Die complexity barrier ist **kein Theorem** im Sinne von Baker–Gill–Solovay,
  Razborov–Rudich oder Aaronson–Wigderson. Diese drei sind formalisierte Aussagen mit
  Beweis in einem präzisen Modell. Mulmuleys Barriere ist eine **informelle These /
  ein konzeptuelles Argument** in einem 65-seitigen Übersichtstext.
- Sie ist **nicht** in den kanonischen Barrierenkanon aufgenommen worden. Die
  Standardliteratur (Aaronson, *P =? NP*, 2016; Fortnow, CACM 2022) zählt weiterhin drei
  Barrieren. Vgl. `research/a1-kanon-und-barrieren.md`.
- Sie ist aber auch **nicht widerlegt** — es ist keine Aussage, die man widerlegen könnte.
- Für **M1** (Barrierenrechenschaft, `docs/02-team-briefing.md` §4) ist der relevante Punkt:
  GCT ist eines der wenigen Programme, das explizit darlegt, **warum** es die drei
  klassischen Barrieren nicht trifft (nicht relativierend, weil zutiefst nicht-blackbox;
  nicht natural, weil die Obstruction-Eigenschaft nicht „large" ist). Das ist ein echter
  Pluspunkt — und unabhängig davon, ob man die complexity barrier als vierte Barriere
  akzeptiert.

---

## 6. Bewertung: Lebendiges Programm oder Randerscheinung?

**Belege statt Spekulation.** Was sich verifizieren ließ:

### 6.1 Belege für laufende Aktivität

| Signal | Beleg | Status |
|---|---|---|
| **Lehrbuch-Survey** | Bläser–Ikenmeyer, *Introduction to Geometric Complexity Theory*, Theory of Computing, **Graduate Surveys 10, S. 1–166, publiziert 31.05.2025** (eingereicht 04.08.2018, revidiert 05.04.2021) | `[VERIFIZIERT]` Existenz/Umfang |
| **Aktuelle Forschungsarbeit** | van den Berg, Dutta, Gesmundo, Ikenmeyer, Lysikov, *Algebraic Metacomplexity and Representation Theory*, **CCC 2025** | `[VERIFIZIERT]` Existenz/Venue |
| **Debordering-Survey** | Dutta–Lysikov, arXiv:2510.13049, **Okt. 2025**, 54 S., invited, TMSC under review | `[PREPRINT]` |
| **GCT-Fachartikel 2026** | Dutta, Gesmundo, Ikenmeyer, Jindal, Lysikov, *Geometric complexity theory for product-plus-power*, **J. Symbolic Computation 132 (2026)** | `[VERIFIZIERT]` Existenz/Venue |
| **Border-Schranke 2026** | K. Sheshadri, arXiv:2606.13628, 11.06.2026 (Potenzsumme, nicht Permanente) | `[PREPRINT]` |
| **Workshop** | **„Frontiers in Complexity Lower Bounds", Isaac Newton Institute, Cambridge, 07.–11.09.2026.** Organisation: **Igor Carboni Oliveira, Nutan Limaye, Rahul Santhanam**. Registrierung £225 / £175 Studierende, Anmeldeschluss 19.07.2026. Thema laut Ankündigung: „revisit the state of the art in complexity lower bounds, including recent work on lower bounds in weak models and new approaches to showing lower bounds for stronger models, as well as work on formulating and understanding various kinds of barriers" | `[VERIFIZIERT]` (newton.ac.uk/event/lfcw01, cstheory-events.org, DMANET-Ankündigung 05/2026) |
| **Workshopreihe** | Workshop on Algebraic Complexity Theory (WACT), 7. Auflage 2023 Warwick, fortlaufend (Ikenmeyer-Vortragsfolien „Algebraic and geometric complexity theory" für WACT25) | `[NUR-SNIPPET]` |
| **Drittmittel** | DFG-Projekt „geometric complexity theory" (GEPRIS-Projektnummer 408113219) | `[NUR-SNIPPET]` |
| **Vorträge/Nachwuchs** | Panova, „Computational Complexity in Algebraic Combinatorics", Yale, 24.10.2025; Ikenmeyer-Lehrmaterial (Liverpool, ICTS-Vorlesungsreihe) | `[NUR-SNIPPET]` |

### 6.2 Einschränkende Beobachtungen

`[EIGENE EINSCHÄTZUNG, Konfidenz mittel]`:

- **Der Newton-Institute-Workshop ist kein GCT-Workshop.** Er heißt „Frontiers in
  Complexity Lower Bounds", die drei Organisierenden (Oliveira, Limaye, Santhanam) stehen
  für Meta-Komplexität, algebraische Schaltkreise und Beweiskomplexität — **nicht** für GCT.
  GCT taucht in der zugänglichen Themenbeschreibung **nicht namentlich** auf. Das ist ein
  belastbares Signal dafür, wo das Feld 2026 seine Fronten sieht: bei **schwachen Modellen,
  Meta-Komplexität und Barrieren**, nicht bei Orbitabschlüssen.
  `[NUR-SNIPPET]` für die Programmbeschreibung — ein vollständiges Vortragsprogramm konnte
  nicht eingesehen werden (WebFetch gesperrt). Über talks.cam ließ sich lediglich ein
  Einzelvortrag belegen (Halley Goldberg, Warwick, „Asymmetry and Complexity of
  Nondeterministic Computations", Mo. 07.09.2026, 15:30, Seminar Room 1) — ein
  Meta-Komplexitäts-Thema. **Diese Einschränkung ist relevant:**
  Es ist möglich, dass GCT-Vorträge im Programm stehen; ausschließen kann ich es nicht.
- **Das Publikationsaufkommen ist real, aber umgewidmet.** Die aktiven Arbeiten sind
  Metakomplexität, Debordering, algebraische Kombinatorik, Highest-Weight-Vektoren —
  Untersuchungen **der GCT-Werkzeuge**, nicht Etappen eines Separationsplans.
- **Personelle Konzentration.** Praktisch alle GCT-Kernarbeiten der letzten zehn Jahre
  tragen einen der Namen Bürgisser, Ikenmeyer, Panova, Gesmundo, Lysikov, Dutta, Dörfler.
  Das ist eine funktionierende, aber kleine Gruppe. Ein Anzeichen für breite
  Nachwuchsrekrutierung über diesen Kreis hinaus ließ sich **nicht** belegen — das heißt
  nicht, dass es sie nicht gibt; es heißt, dass ich sie mit WebSearch nicht nachweisen konnte.
- **Der 166-Seiten-Survey hat eine lange Latenz.** Eingereicht 2018, revidiert 2021,
  publiziert 2025. `[EIGENE EINSCHÄTZUNG, Konfidenz niedrig]` — man kann das als Zeichen
  eines konsolidierenden, nicht eines stürmisch wachsenden Feldes lesen. Es kann auch
  schlicht am Umfang liegen. Ich werte es nicht.

### 6.3 Antwort auf die Frage

`[EIGENE EINSCHÄTZUNG, Konfidenz mittel]`

**GCT ist 2026 ein lebendiges mathematisches Forschungsgebiet und zugleich ein
weitgehend erledigtes P-vs-NP-Programm.** Beides ist wahr und der Unterschied ist der
ganze Punkt:

- **Lebendig als Mathematik:** kontinuierliche Publikationen in Spitzenvenues,
  Drittmittel, Lehrbuch, Workshops, ein produktiver Kern von ~8–10 Forschenden.
  „Randerscheinung" wäre sachlich falsch.
- **Erledigt als Fahrplan:** Der konkret ausformulierte Plan (occurrence obstructions) ist
  **widerlegt**. Der Nachfolgeplan (multiplicity obstructions) ist in **Modellsituationen**
  (Chow-Varietät, Potenzsumme vs. Produkt, product-plus-power) **konstruktiv durchgeführt**
  — und nirgends sonst. Die Zeitschätzung des Urhebers liegt bei ~100 Jahren. Es gibt
  **kein** Zwischenresultat der Form „Etappe k von n erreicht" auf dem Weg zu perm vs. det;
  die dafür einschlägige Kennzahl (determinantal complexity, §3.4) steht seit 2004 bei n²/2.

Die Formel für das Papier: **GCT ist ein Programm ohne Zwischenstände.** Genau das
unterscheidet es von Wiles/Taniyama–Shimura, wo Zwischenetappen existierten und
abgearbeitet wurden.

---

## 7. Was ich nicht verifizieren konnte (explizit)

Vorab deklarierte Tiefenbegrenzung, hier eingelöst:

1. **Keinen einzigen Volltext.** WebFetch ist gesperrt. Alle inhaltlichen Aussagen über
   LST, BIP, Dörfler–Ikenmeyer–Panova, Dutta–Lysikov und Mulmuley stammen aus
   Suchmaschinen-Snippets, jeweils triangulierend über 2–3 verschieden formulierte Anfragen.
2. **Die exakte Aussage von Bürgissers VP=VNP-Transferresultat.** Die Snippets nennen
   übereinstimmend die Konsequenzen (NC²/poly bzw. NC³/poly = P/poly = NP/poly = PH/poly,
   GRH in char 0), aber Quantoren, Körperabhängigkeit und die genaue Fassung des
   PH-Kollapses habe ich nicht am Original geprüft. Für das Papier sollte hier zitiert
   werden, nicht paraphrasiert.
3. **Die genauen quantitativen Schranken bei LST und Bhargav–Dutta–Saxena.** Die
   Exponentenform n^Ω(d^{1/φ^{2Δ}}) stammt aus einem Snippet; sie könnte in der
   Wiedergabe verzerrt sein. Ebenso die Angabe „Produkttiefen kleiner als log log d".
4. **Ob eine n^ω(√d)-Schranke *genau* die Schwelle ist.** Die Depth-Reduction-Literatur
   hat mehrere Varianten (homogen/nicht-homogen, Tiefe 3 vs. 4, char 0 vs. beliebig).
   Meine Darstellung gibt die Standardversion wieder; Feinheiten sind ungeprüft.
5. **Mulmuleys „~100 Jahre" im Originalwortlaut und Kontext.** Belegt ist nur die
   Sekundärwiedergabe. Vgl. den im Team-Briefing dokumentierten Fall des
   Pseudo-Fortnow-Zitats: **Diese Zahl darf im Papier nicht als wörtliches Zitat erscheinen.**
6. **Das Vortragsprogramm des Newton-Institute-Workshops.** Ob GCT dort vorkommt, ist offen.
   Belegt ist ein einziger Einzelvortrag (Goldberg, Meta-Komplexität); die Vollliste war
   nicht einsehbar.
7. **Die Mathematik selbst.** Ich kann nicht beurteilen, ob multiplicity obstructions
   aussichtsreich sind, ob die Positivitätshypothesen erreichbar sind, oder ob die
   complexity barrier ein tiefes oder ein rhetorisches Argument ist. Das war auftragsgemäß
   auch nicht die Aufgabe.
8. **KI-Beteiligung.** Ich habe **keinen** Hinweis darauf gefunden, dass LLMs,
   evolutionäre Suche oder automatisches Beweisen an irgendeinem der hier referierten
   Resultate beteiligt waren. Alle genannten Arbeiten sind konventionelle
   Menschenmathematik. Das ist ein **Negativbefund** und als solcher zu berichten.
   Konfidenz: mittel — Abwesenheit von Evidenz, nicht Evidenz der Abwesenheit; ich habe
   nicht gezielt nach „KI + algebraische Komplexität" gesucht.

---

## 8. Antwort auf die Leitachse (`docs/02-team-briefing.md` §2)

**Der Befund bestätigt die Achse und schärft sie an einer Stelle.**

Prüfung der drei Bedingungen für die algebraische Route:

- **B1 (endliches, maschinell repräsentierbares Suchobjekt):** In GCT ist das
  *scheinbar* erfüllt — eine **Obstruction ist eine Partition λ**, also ein endliches,
  vollständig maschinell repräsentierbares kombinatorisches Objekt. Das ist die
  **strukturell interessanteste Beobachtung dieses Berichts**: GCT ist der einzige
  bekannte Versuch, P vs. NP überhaupt in ein **Suchproblem nach einem endlichen Objekt**
  zu übersetzen. Genau das ist Mulmuleys „flip".
- **B2 (billiges Verifikationsorakel):** **Verletzt.** Zu prüfen wäre für ein
  Kandidaten-λ, ob mult_λ(perm-Seite) > mult_λ(det-Seite). Diese Multiplizitäten sind
  Kronecker-/Plethysmuskoeffizienten — deren Berechnung ist selbst
  komplexitätstheoretisch hart, und es gibt Resultate (Ikenmeyer–Pak,
  Ikenmeyer–Pak–Panova), die nahelegen, dass für einige von ihnen **nicht einmal eine
  #P-Beschreibung existiert**. Es gibt also kein billiges Orakel, und der Verdacht steht
  im Raum, dass es prinzipiell keines geben kann.
- **B3 (von Menschen bewiesener Lifting-Rahmen):** **Teilweise vorhanden, aber
  löchrig — und das ist der Unterschied zum AlphaEvolve-Fall.** Der Rahmen existiert
  formal: Obstruction ⇒ Orbitabschluss-Nichtinklusion ⇒ border determinantal complexity
  hoch ⇒ (Debordering nötig!) ⇒ VP ≠ VNP ⇒ (GRH + Nichtuniformität nötig!) ⇒
  P/poly ≠ NP/poly. **Aber:** dieser Rahmen hat **zwei ungeschlossene Stellen**
  (Debordering, GRH/Nichtuniformität) und endet **nicht** bei P ≠ NP. Beim PCP-Theorem
  plus Gadget-Reduktionskalkül gibt es diese Löcher nicht — dort ist der Lifting-Rahmen
  ein geschlossener Satz.

`[EIGENE EINSCHÄTZUNG, Konfidenz mittel]` **Präzisierungsvorschlag an die Leitung:**
Die Achse sollte **B3 zweiteilen** in
- **B3a: Existiert ein Lifting-Rahmen?**
- **B3b: Ist er geschlossen, oder enthält er selbst offene Vermutungen?**

GCT erfüllt B3a und verletzt B3b (Debordering offen, GRH offen, Zielaussage nichtuniform).
Das ist ein qualitativ anderer Fall als P vs. NP ohne jeden Plan (B3a verletzt) und als
AlphaEvolve (B3a und B3b erfüllt). Ohne diese Zweiteilung würde man GCT entweder
zu positiv („es gibt ja einen Rahmen") oder zu negativ („kein Rahmen") einordnen.

**Konsequenz für die Leitfrage nach KI:** Selbst wenn man B1 großzügig als erfüllt ansieht
und ein KI-System Kandidaten-λ generieren ließe — ohne B2 gibt es kein Fitness-Signal, das
eine Suche steuern könnte, und ohne geschlossenes B3 wäre ein Treffer kein Beweis von
P ≠ NP. Die algebraische Route ist damit **das lehrreichste Gegenbeispiel des Projekts**:
Sie zeigt, dass ein „endliches Suchobjekt" allein nichts nützt.

---

## 9. Widerspruch / Ergänzung zu Annahmen des Briefings

1. **Ergänzung zu `docs/01-strategie-S1.md`, Zeile 244** („selbst VP ≠ VNP impliziert
   P ≠ NP nicht direkt"): Das ist richtig, aber unterbestimmt. Die präzise Lage ist, dass
   die einzige bekannte Brücke (a) in die **Gegenrichtung** läuft, (b) in der
   **nichtuniformen** Welt landet und (c) in Charakteristik 0 an **GRH** hängt. Alle drei
   Punkte gehören ins Papier, sonst bleibt „nicht direkt" ein Vagheitswort.
2. **Kein Widerspruch zu A1.** Die dortige Einordnung (GCT = einziger ausgearbeiteter
   Langfristplan, ~100 Jahre, explizit nicht-relativierend/nicht-natural) wird bestätigt
   und um das zentrale Negativresultat ergänzt, das A1 nur streift.
3. **Präzisierung zu `docs/01-redteam-S2.md`, Zeile 375:** Die dortige Formulierung
   („keine occurrence obstructions") ist korrekt. Ergänzend wichtig: Ikenmeyer–Panova
   (Adv. Math. 2017) und Bürgisser–Ikenmeyer–Panova (JAMS 2019) sind **zwei
   aufeinanderfolgende** Resultate, nicht eines.
4. **Zur Barrierenzählung:** Mulmuleys „complexity barrier" sollte im Papier **nicht** als
   vierte Barriere neben Relativization / Natural Proofs / Algebrization geführt werden.
   Sie hat nicht denselben logischen Status (These vs. Theorem). Wenn sie erwähnt wird,
   dann als **Positionierung eines Programms**, nicht als Resultat.

---

## 10. Quellen

**Valiants Hypothese / VP vs. VNP**
1. P. Bürgisser: *Completeness and Reduction in Algebraic Complexity Theory*,
   Algorithms and Computation in Mathematics 7, Springer 2000; darin Kap. „P Versus NP:
   A Nonuniform Algebraic Analogue" — https://link.springer.com/chapter/10.1007/978-3-662-03338-8_21 `[NUR-SNIPPET]`
2. Complexity Zoo, Eintrag VP/VNP — https://complexityzoo.uwaterloo.ca/Complexity_Zoo:V `[NUR-SNIPPET]`
3. Chi-Ning Chou: *VP = VNP and GRH implies P/poly = NP/poly* (Notizen) —
   https://cnchou.github.io/notes/VP=VNP.html `[NUR-SNIPPET]`

**Low-depth arithmetic circuit lower bounds**
4. N. Limaye, S. Srinivasan, S. Tavenas: *Superpolynomial Lower Bounds Against Low-Depth
   Algebraic Circuits*, FOCS 2021, S. 804–814; J. ACM 72(4), 2025; ECCC TR21-081 —
   https://eccc.weizmann.ac.il/report/2021/081/ · https://dl.acm.org/doi/10.1145/3734215 `[VERIFIZIERT]` (Existenz)
5. FOCS 2021 Awards (Best Paper an Limaye/Srinivasan/Tavenas) —
   https://focs2021.cs.colorado.edu/awards/ `[VERIFIZIERT]`
6. CACM Research Highlight + Technical Perspective „Low-Depth Arithmetic Circuits" —
   https://cacm.acm.org/research-highlights/superpolynomial-lower-bounds-against-low-depth-algebraic-circuits/ ·
   https://cacm.acm.org/research/technical-perspective-low-depth-arithmetic-circuits/ `[NUR-SNIPPET]`
7. M. A. Forbes: *Low-Depth Algebraic Circuit Lower Bounds over Any Field*, CCC 2024 —
   https://drops.dagstuhl.de/entities/document/10.4230/LIPIcs.CCC.2024.31 `[VERIFIZIERT]` (Existenz)
8. *Improved Low-Depth Set-Multilinear Circuit Lower Bounds*, CCC 2022 —
   https://drops.dagstuhl.de/entities/document/10.4230/LIPIcs.CCC.2022.38 `[NUR-SNIPPET]`
9. *Low-Depth Arithmetic Circuit Lower Bounds: Bypassing Set-Multilinearization*, ICALP 2023 —
   https://drops.dagstuhl.de/entities/document/10.4230/LIPIcs.ICALP.2023.12 `[NUR-SNIPPET]`
10. C. S. Bhargav, S. Dutta, N. Saxena: *Improved Lower Bound, and Proof Barrier, for
    Constant Depth Algebraic Circuits*, ACM Trans. Comput. Theory —
    https://dl.acm.org/doi/full/10.1145/3689957 `[NUR-SNIPPET]`
11. M. Agrawal, V. Vinay: *Arithmetic Circuits: A Chasm at Depth Four*, FOCS 2008 —
    https://ieeexplore.ieee.org/document/4690941/ `[NUR-SNIPPET]`
12. A. Wigderson: *Low-Depth Arithmetic Circuits* (Übersicht) —
    https://www.math.ias.edu/~avi/PUBLICATIONS/Wigderson2017ACM.pdf `[NUR-SNIPPET]`

**Border complexity / Debordering**
13. P. Dutta, V. Lysikov: *Recent Advances in Debordering Methods*, arXiv:2510.13049, Okt. 2025 —
    https://arxiv.org/abs/2510.13049 `[PREPRINT]`

**Geometric Complexity Theory**
14. K. Mulmuley, M. Sohoni: *Geometric Complexity Theory I: An Approach to the P vs. NP
    and Related Problems*, SIAM J. Comput. 31(2), 2001 —
    https://epubs.siam.org/doi/10.1137/S009753970038715X `[VERIFIZIERT]` (Existenz)
15. P. Bürgisser, C. Ikenmeyer, G. Panova: *No occurrence obstructions in geometric
    complexity theory*, FOCS 2016 / J. Amer. Math. Soc. 32(1), 2019, 163–193 —
    https://www.ams.org/jams/2019-32-01/S0894-0347-2018-00908-7/ · https://arxiv.org/abs/1604.06431 `[VERIFIZIERT]`
16. C. Ikenmeyer, G. Panova: *Rectangular Kronecker coefficients and plethysms in geometric
    complexity theory*, FOCS 2016 / Advances in Mathematics 319 (2017) —
    https://arxiv.org/abs/1512.03798 `[NUR-SNIPPET]`
17. J. Dörfler, C. Ikenmeyer, G. Panova: *On Geometric Complexity Theory: Multiplicity
    Obstructions Are Stronger Than Occurrence Obstructions*, ICALP 2019 /
    SIAM J. Appl. Algebra Geom. — https://epubs.siam.org/doi/abs/10.1137/19M1287638 ·
    https://arxiv.org/abs/1901.04576 `[VERIFIZIERT]` (Existenz)
18. M. Bläser, C. Ikenmeyer: *Introduction to Geometric Complexity Theory*,
    Theory of Computing, Graduate Surveys 10, 1–166, 31.05.2025 —
    https://theoryofcomputing.org/articles/gs010/ `[VERIFIZIERT]` (Existenz)
19. M. van den Berg, P. Dutta, F. Gesmundo, C. Ikenmeyer, V. Lysikov:
    *Algebraic Metacomplexity and Representation Theory*, CCC 2025 —
    https://drops.dagstuhl.de/entities/document/10.4230/LIPIcs.CCC.2025.26 `[VERIFIZIERT]` (Existenz)
20. K. Mulmuley: *On P vs. NP, Geometric Complexity Theory, Explicit Proofs and the
    Complexity Barrier*, arXiv:0908.1932 (2009) — https://arxiv.org/abs/0908.1932 `[PREPRINT]`
21. K. Mulmuley: *On P vs. NP, Geometric Complexity Theory, and the Riemann Hypothesis*,
    arXiv:0908.1936 `[PREPRINT]`
22. K. Mulmuley: *Explicit Proofs and The Flip*, arXiv:1009.0246 `[PREPRINT]`
23. Wikipedia, *Geometric complexity theory* (Sekundärquelle für Mulmuleys Zeitschätzung) —
    https://en.wikipedia.org/wiki/Geometric_complexity_theory `[NUR-SNIPPET]`
23a. K. Mulmuley: *The GCT Program Toward the P vs. NP Problem*, CACM 55(6), Juni 2012,
    DOI 10.1145/2184319.2184341 — https://dl.acm.org/doi/10.1145/2184319.2184341 ·
    https://cacm.acm.org/research/the-gct-program-toward-the-p-vs-np-problem/ `[NUR-SNIPPET]`
23b. L. Fortnow (Hg.): *Ketan Mulmuley Responds*, Computational Complexity Weblog,
    21.04.2008 — https://blog.computationalcomplexity.org/2008/04/ketan-mulmuley-responds.html `[NUR-SNIPPET]`
23c. C. Ikenmeyer, U. Kandasamy: *Implementing geometric complexity theory: On the
    separation of orbit closures via symmetries*, STOC 2020, DOI 10.1145/3357713.3384257 —
    https://arxiv.org/abs/1911.03990 `[VERIFIZIERT]` (Existenz)
23d. P. Dutta, F. Gesmundo, C. Ikenmeyer, G. Jindal, V. Lysikov: *Geometric complexity
    theory for product-plus-power*, J. Symbolic Computation 132 (2026) —
    https://arxiv.org/abs/2211.07055 ·
    https://www.sciencedirect.com/science/article/pii/S0747717125000409 `[VERIFIZIERT]` (Existenz)

**Determinantal complexity**
23e. T. Mignon, N. Ressayre: *A quadratic bound for the determinant and permanent problem*
    (IMRN 2004) — Sekundärbeleg: M. Bläser, *Determinant versus permanent*, ADFOCS-17
    Lecture Notes, https://conferences.mpi-inf.mpg.de/adfocs-17/material/MB_LN.pdf `[NUR-SNIPPET]`
23f. Y. Yabe: *Bi-polynomial rank and determinantal complexity*, arXiv:1504.00151 —
    https://arxiv.org/abs/1504.00151 `[PREPRINT]`
23g. M. Kumar, B. L. Volk: *A Lower Bound on Determinantal Complexity*, CCC 2021 /
    arXiv:2009.02452 — https://arxiv.org/abs/2009.02452 `[NUR-SNIPPET]`
23h. K. Sheshadri: *A near-quadratic lower bound on the border determinantal complexity of
    Σᵢ xᵢⁿ via conormal specialization*, arXiv:2606.13628, 11.06.2026 —
    https://arxiv.org/abs/2606.13628 `[PREPRINT]`

**Metakomplexität / Anschlussarbeiten**
24. *AC⁰[p]-Frege Cannot Efficiently Prove that Constant-Depth Algebraic Circuit Lower
    Bounds are Hard*, ITCS 2026 / arXiv:2509.16824 / ECCC TR25-134 —
    https://arxiv.org/abs/2509.16824 `[PREPRINT]`
25. *Meta-Mathematics of Algebraic Complexity*, LICS 2026 —
    https://drops.dagstuhl.de/entities/document/10.4230/LIPIcs.LICS.2026.49 `[NUR-SNIPPET]`

**Feldaktivität**
26. Isaac Newton Institute: *Frontiers in Complexity Lower Bounds*, Cambridge,
    07.–11.09.2026, Org. I. C. Oliveira, N. Limaye, R. Santhanam —
    https://www.newton.ac.uk/event/lfcw01/ `[VERIFIZIERT]`
27. CS Theory Events, Ankündigung desselben Workshops (13.05.2026) —
    https://cstheory-events.org/2026/05/13/workshop-frontiers-in-complexity-lower-bounds/ `[VERIFIZIERT]`
28. Workshop on Algebraic Complexity Theory (WACT), 7. Auflage 2023, Warwick —
    https://www.dcs.warwick.ac.uk/~u2270030/wact/ `[NUR-SNIPPET]`
29. DFG GEPRIS, Projekt „geometric complexity theory", Nr. 408113219 —
    https://gepris.dfg.de/gepris/projekt/408113219 `[NUR-SNIPPET]`
30. G. Panova: *Computational Complexity in Algebraic Combinatorics*, Yale, 24.10.2025 —
    https://calendar.math.yale.edu/node/25185 `[NUR-SNIPPET]`
30a. talks.cam, Einzelvortrag im Workshopprogramm: H. Goldberg (Warwick),
    *Asymmetry and Complexity of Nondeterministic Computations*, 07.09.2026 —
    https://talks.cam.ac.uk/talk/index/272850/ `[NUR-SNIPPET]`
31. I. Pak, Blogeintrag zu „not in #P"-Resultaten —
    https://igorpak.wordpress.com/2023/09/14/the-power-of-negative-thinking-combinatorial-and-geometric-inequalities/ `[NUR-SNIPPET]`
