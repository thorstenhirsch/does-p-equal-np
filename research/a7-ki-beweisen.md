# A7 — KI für automatisches Beweisen: Stand September 2026

**Agent:** A7 · **Datum:** 2026-09-12 · **Repo:** does-p-equal-np

**Methodische Vorbemerkung.** WebFetch war gesperrt; alle Web-Belege stammen aus
WebSearch-Synthesen und sind daher grundsätzlich `[NUR-SNIPPET]`. **Ausnahme:** Für zwei
Lean-Repositories konnte ich über die GitHub-Code-Suche **Quelltext im Wortlaut** lesen.
Diese Befunde sind mit `[QUELLTEXT]` markiert und haben deutlich höhere Belastbarkeit —
sie sind der härteste Teil dieses Berichts. Triangulation: jeder nichttriviale Claim wurde
mit ≥2 unterschiedlich formulierten Anfragen geprüft.

---

## 0. Kernthese

KI-gestütztes Beweisen hat 2024–2026 einen realen, großen Sprung gemacht — aber **exakt
entlang der Leitachse aus §2 des Team-Briefings**. Jeder verifizierbare Erfolg liegt in
einer der beiden Klassen:

- **(A) Suchbare endliche Zeugen:** ein konkretes Objekt (Beweisterm, Gadget, Schranke)
  wird gefunden, und ein *billiges Orakel* (der Lean-Kernel) sagt ja/nein.
- **(B) Übersetzung vorhandenen Wissens:** ein von Menschen bereits verstandener Beweis
  wird formalisiert (Autoformalisierung).

P vs. NP liegt in **keiner** der beiden Klassen: Es gibt kein endliches entscheidendes
Objekt, und es gibt keinen zu übersetzenden Beweis. Der Befund **bestätigt die Leitachse
ohne Gegenbefund**. Konfidenz: **hoch**.

Der zweite Kernbefund ist unbequemer: Der Lean-Kernel wird 2026 systematisch als
Wahrheitsgarantie missverstanden. Ich kann **zwei unabhängige Fälle am Quelltext bzw. an
der Sache belegen**, in denen eine "grüne" Formalisierung etwas anderes verifiziert, als
die Überschrift behauptet (Abschnitt 2). Das ist die empirische Untermauerung von
Muss-Kriterium **M4**.

---

## 1. Bestandsaufnahme 2024–2026

### 1.1 Übersicht

| System | Datum | Leistung | Autonomie | Peer Review | Formal? |
|---|---|---|---|---|---|
| AlphaProof + AlphaGeometry 2 | Jul 2024 | IMO 4/6, Silber-Niveau | angeleitet (Problem-Formalisierung durch Menschen) | **ja**, Nature 11/2025 | Lean |
| AlphaProof Nexus | Mai 2026 | 9/353 Erdős, 44/492 OEIS | weitgehend autonom | Preprint | Lean |
| Seed-Prover (ByteDance) | Jul 2025 | IMO 4 voll + 1 teilw. = 30 P. (Silber); 5/6 erst *nachträglich* | autonom im Lean-Setting | Preprint | Lean |
| DeepSeek-Prover-V2 | Apr 2025 | miniF2F-valid 90,6 % | autonom | Preprint | Lean |
| Goedel-Prover-V2 | 2025 | PutnamBench 64/657 | autonom | Preprint | Lean |
| AxiomProver (Axiom Math) | Feb 2026 | 4 zuvor offene Probleme | "zero human guidance" (Firmenclaim) | **strittig** | Lean |
| OpenAI "Astra" | 01.08.2026 | 10 Probleme ohne Fortschritt seit ≥10 J. | autonom | Preprint | Lean 4 |
| Anthropic / Claude | Aug 2026 | Riemann-Zeta-Schranke 41,6 % → 67,2 % | angeleitet (Ko-Autor E. Easley) | extern begutachtet, nicht publiziert | Lean |
| Anthropic / Claude | 04.09.2026 | FLT-Formalisierung, 13 Mio. Zeilen Lean | "largely autonomously" | nein | Lean 4 |
| OpenAI | 08.09.2026 | Navier–Stokes Fefferman **C/D** | autonom, 88 h | **nein** | Lean |

### 1.2 Einzelbefunde

**AlphaProof/AlphaGeometry** `[VERIFIZIERT]`. Der einzige *peer-reviewte* Eintrag der
Liste: "Olympiad-level formal mathematical reasoning with reinforcement learning", Nature,
12.11.2025, doi 10.1038/s41586-025-09833-y. IMO 2024: 4 von 6 Problemen, davon 3 der 5
Nicht-Geometrie-Aufgaben durch AlphaProof, P4 (Geometrie) durch AlphaGeometry 2.
**Wichtige Einschränkung:** die Probleme wurden *von Menschen nach Lean formalisiert*;
das System löste ein bereits präzise gestelltes Problem. Konfidenz hoch.

**AlphaProof Nexus** `[PREPRINT]`, arXiv ca. 21.05.2026. Die berichteten Zahlen sind als
*Quoten* aussagekräftiger als als Absolutzahlen: **9 von 353** offenen Erdős-Problemen
(≈ 2,5 %) und **44 von 492** OEIS-Vermutungen (≈ 8,9 %). Zwei der Erdős-Probleme waren
56 Jahre offen. Inferenzkosten "wenige hundert Dollar pro Problem". Beweise auf GitHub.
`[NUR-SNIPPET]` — die Trefferquote ist der eigentliche Befund: **rund 97,5 % der offenen
Erdős-Probleme blieben offen.** Das System erntet die kurzen, isolierten Probleme ab.

**Seed-Prover** `[PREPRINT]`, arXiv:2507.23726. **Korrektur am Auftragstext:** Die
Formulierung "IMO 2025, 5/6" ist nur mit Nachlauf richtig. Unter Wettbewerbsbedingungen:
4 vollständig + 1 teilweise = 30 Punkte (IMO-zertifiziert, Silber). Die fünfte Lösung kam
erst durch *extended search* nach dem Wettbewerb. Benchmarks: miniF2F-test 99 %
(243/244), PutnamBench 331/657, **CombiBench nur 30 %** — die Kombinatorik-Lücke ist der
interessante Datenpunkt, weil sie zeigt, wo der Suchraum aufhört, handhabbar zu sein.

**DeepSeek-Prover-V2 / Goedel-Prover-V2** `[PREPRINT]`. arXiv:2504.21801 bzw. 2025.
Reine Benchmark-Systeme auf Olympiade-Niveau, kein Anspruch auf Forschungsresultate.
Relevanz hier nur als Kalibrierung: Was heute "gelöst" heißt, ist Olympiade-Mathematik mit
kurzen Beweisen.

**AxiomProver (Axiom Math)** `[CLAIM]`, **Widerspruch nicht glättbar.** Zwei Quellen
widersprechen sich zum Peer-Review-Status:
- Axios (26.05.2026) und cryptobriefing titeln, die Beweise seien **in peer-reviewten
  Journals** gelandet.
- Eine andere Zusammenfassung hält fest: Preprints im Februar 2026 auf arXiv, und
  **"as of late May 2026, no peer-reviewed journal publications have been confirmed"**.

Beide Zahlen stehen hier nebeneinander; ich kann den Widerspruch mit WebSearch nicht
auflösen. Inhaltlich genannt: Fels Vermutung über Syzygien numerischer Halbgruppen und die
Chen–Gendron-Vermutung (2021). Der Firmenclaim lautet "zero human guidance"; das ist
Selbstauskunft eines finanzierten Startups (200 Mio. USD) und als solche zu behandeln.
Konfidenz zur Autonomie: **niedrig**.

**OpenAI "Astra", 01.08.2026** `[CLAIM]`. Zehn Resultate in Mathematik und *theoretischer
Informatik*, Auswahlkriterium: "no progress on the main result for at least a decade".
Jedes Argument mit maschinenprüfbarem **Lean-4-Zertifikat**. Tokenkosten für alle zehn
zusammen ca. **2 000 USD**. `[NUR-SNIPPET]`. Keine dieser zehn Arbeiten ist nach meiner
Recherche ein Millennium- oder Barrieren-Problem; "kein Fortschritt seit einem Jahrzehnt"
ist eine ganz andere Kategorie als "seit 55 Jahren offen mit drei bewiesenen Barrieren".

**Anthropic, Riemann-Zeta, Aug 2026** `[CLAIM]`, gut dokumentiert. Verbesserung der
unteren Schranke für den Anteil der Nullstellen auf der kritischen Geraden von **41,6 %
auf 67,2 %**. Lean-Formalisierung gemeinsam mit Eric Easley (Anthropic), besteht das
Standard-Validierungswerkzeug *comparator*. Zwei interne Mathematiker plus zwei externe
(Brian Conrey, Dan Goldston) haben kurzfristig geprüft — das ist **kein Peer Review**.
Entscheidend und von Anthropic selbst gesagt: **"the techniques Claude used are not
expected to lead to proving the Riemann hypothesis."** Eine Quelle charakterisiert den
Beitrag als Kombination zweier bereits vorliegender Arbeiten, die niemand kombiniert
hatte. Das ist exakt Klasse (A)/(B): quantitative Optimierung im vorhandenen
Formalisierungskontext, nicht der Durchbruch. Konfidenz mittel-hoch.

**Anthropic, FLT-Formalisierung, 04.09.2026** `[CLAIM]`, quantitativ der wichtigste
Datenpunkt dieses Berichts (siehe §5). Behauptet: erste durchgängige, maschinengeprüfte
Formalisierung von Fermats letztem Satz in Lean 4; **13 Mio. Zeilen Lean, 29 500
Zwischentheoreme, ca. 11 Tage Wall-Clock, mehrere Dutzend parallele Agenten,
6 Mrd. Token**. Der erste Versuch **scheiterte**; erst die mittendrin ergänzte
Koordinationsplattform *Prove2Me* (T. Peng, Columbia) machte den Abschluss möglich.
Kevin Buzzard, der seit 2024 das menschliche FLT-Projekt leitet (EPSRC-gefördert bis
09/2029, Blueprint allein für Phase 1: 86 Seiten), nennt es "an extraordinary
**autoformalization** achievement" — und genau dieses Wort ist die Einordnung: Es wurde
**kein neuer Satz bewiesen**, sondern ein bekannter Beweis übersetzt. Klasse (B).

---

## 2. Kernauftrag: Navier–Stokes als Lehrstück zu M4

### 2.1 Rekonstruktion des Vorgangs

`[CLAIM]`, mehrfach trianguliert (Quanta, Fortnow-Blog, Fachpresse):

- **15.08.2026** — Tristan Buckmaster (NYU) und Levent Alpöge (Anthropic) zeigen
  finite-time blowup mit glattem Forcing für die **3D-Euler**-Gleichungen, aufbauend auf
  einer Forcing-Technik, die Diego Córdoba und Luis Martínez-Zoroa über ein Jahr
  entwickelt hatten.
- **22.08.2026** — Buckmaster/Alpöge verifizieren ihr eigenes Resultat in Lean.
- **01.–05.09.2026** — OpenAI lässt ein internes Multiagentensystem (ca. 10 000
  nebenläufige Agenten) 88 Stunden laufen; 17 weitere Stunden für Lean-Formalisierung
  und -Verifikation.
- **08.09.2026, 12:00** — OpenAI veröffentlicht ein **166-seitiges Manuskript** plus
  Lean-Formalisierung, ca. 12 Stunden nach Buckmasters öffentlichem Post.
- **Danach** — Prioritäts- und Ethikstreit. Buckmaster fragt öffentlich, ob OpenAI-Modelle
  auf seinen Codex-Sitzungen trainiert hatten bzw. darauf zugriffen, und wirft OpenAI vor,
  "customer's data to try to scoop their customer" genutzt zu haben. OpenAI tritt die
  Priorität für das **Euler**-Resultat an Buckmaster/Alpöge ab und beansprucht sie für
  **Navier–Stokes**. Charles Fefferman: die Helden der Geschichte seien Córdoba und
  Martínez-Zoroa.

### 2.2 Was Lean hier verifiziert hat — und was nicht

**Das ist der entscheidende Punkt, und er ist sauber belegbar.**

Das Clay-Problem zu Navier–Stokes ist von Fefferman in **vier Alternativen** A, B, C, D
gefasst. A und B verlangen globale glatte Lösungen **ohne äußere Kraft**. C und D erlauben
ein Blowup-Beispiel **mit glatter äußerer Kraft (Forcing)**.

OpenAI hat **C/D** etabliert: Eine anfangs glatte, ruhende Flüssigkeit entwickelt unter
einem glatten Forcing in endlicher Zeit eine Singularität, bei endlicher Energie. Die
Lean-Formalisierung ist, soweit erkennbar, **korrekt** — sie verifiziert die Ableitung
genau dieser Aussage.

Nur: Die **ungeforcte** globale Regularität — das, was die Fachwelt seit Jahrzehnten als
"das Navier–Stokes-Problem" meint — bleibt offen. Das **Clay Mathematics Institute hat das
Resultat nicht anerkannt** und führt Navier–Stokes weiterhin als ungelöstes
Millennium-Problem.

**`[QUELLTEXT]` — unabhängige Bestätigung aus dem Lean-Ökosystem selbst.** Das Repository
`lean-dojo/LeanMillenniumPrizeProblems` (Lean 4, mathlib v4.31.0) führt in seiner README
eine Tabelle "Propositions that count as a solution". Für Navier–Stokes stehen dort
**vier getrennte Propositionen** — `MillenniumNavierStokes.FeffermanA`, `FeffermanB`,
`FeffermanC`, `FeffermanD` — und der Status lautet **"Open"**. `Problems/Registry.lean`
kommentiert ausdrücklich, dass Navier–Stokes "Fefferman's four" Ausgänge hat und "a proof
of any one of them settles the problem" — was die Statement-Datei formal so modelliert,
aber eben nicht die Frage beantwortet, welche Alternative das *Preisproblem* im gemeinten
Sinn erledigt.

**Damit ist die zu prüfende Lehre belegt — in einer präziseren Form, als der Auftrag sie
formuliert.** Der Fehlermodus war hier **nicht** ein falsch formalisiertes Statement, kein
`sorry`, kein geschmuggeltes Axiom. Der Lean-Beweis ist gültig und das Statement ist
sauber. Der Bruch liegt **eine Ebene höher**: bei der Frage, *welches* der vier formal
korrekten Statements die informelle Frage "Ist Navier–Stokes gelöst?" beantwortet. Lean
kann diese Frage **prinzipiell nicht** beantworten, weil sie außerhalb des formalen
Systems liegt. Die Formulierung aus M4 — "eine Formalisierung verifiziert die Ableitung
aus den angegebenen Voraussetzungen, nicht deren Angemessenheit" — trifft es genau. Die
Diskrepanz wurde hier **nicht** durch Lean aufgedeckt, sondern durch menschliche
Fachurteile (Fefferman, Clay Institute).

Fortnow formuliert in "Navier-Stokes and Lean" (09/2026) sinngemäß dasselbe: Ein
erfolgreicher Lean-Check etabliert ein Resultat *innerhalb seines formalen Setups*;
Fachleute müssen weiterhin beurteilen, ob dieses Setup dem gemeinten Problem entspricht
und was der Satz tatsächlich sagt. `[NUR-SNIPPET]`

**Fairnesshalber:** OpenAI hat nach allem, was ich sehe, in der Sache nicht gelogen — die
Ankündigung bezog sich auf Fefferman C/D. Die Überdehnung entstand in der
Kommunikationskette ("AI has solved a Millennium Prize Problem"). Der Fall ist deshalb
kein Betrugs-, sondern ein **Angemessenheits**-Lehrstück. Genau das macht ihn für M4
wertvoll.

### 2.3 `[QUELLTEXT]` Zweiter Fall — hier greifen M5 und die `sorry`-Lücke wirklich

Der Auftrag fragt, wie ein Fehler *trotz* grüner Formalisierung entsteht. Der
Navier–Stokes-Fall liefert die Angemessenheits-Variante. Die Variante
"Axiomschmuggel + Buchführungstrick" liefert ein anderer Fall, den ich **im Quelltext
nachgelesen** habe:

**arXiv:2606.03194, "Lean 4 Machine-Verified Proof of P = NP via the Pedigree Polytope
Membership Problem"** (T. S. Arthanari, Univ. Auckland, 02.06.2026), Repo
`TiruArt/Pedigree-Polytopes-Lean4`. `[CLAIM]` — ein P = NP-Claim mit Lean-Siegel.

Das Repo wirbt: **"Proved in Lean 4 / Mathlib4. Zero `sorry`s in the main chain.
2968/2968 jobs clean."** Das Paper behauptet, ein von Lean 4 akzeptierter Beweis sei
"correct by construction with no possibility of subtle errors".

Die **README desselben Repos** listet die Beweiskette jedoch selbst so auf:

| # | Schritt | Datei | Status laut README |
|---|---|---|---|
| 1 | MCF(n−1) feasible → X ∈ conv(Pₙ) | `N_Sufficiency.lean` | ✅ Proved |
| 2 | MCF ist kombinatorisches LP → M3P ∈ P (Tardos 1986) | `N_Complexity.lean` | ✅ Proved |
| 3 | conv(Aₙ) volldimensional | `N_FullDimensional.lean` | ✅ Proved |
| 4 | M3P ∈ P → separation oracle (Maurras 2002) | `N_PEqualsNP.lean` | **Axiom** |
| 5 | Separation → Optimisation (GLS 1988) | `N_PEqualsNP.lean` | **Axiom** |
| 6 | Pedigree-Optimisation = STSP | `N_PEqualsNP.lean` | ✅ Proved |
| 7 | **STSP ∈ P → P = NP (Cook 1971, Karp 1972)** | `N_PEqualsNP.lean` | **Axiom** |

`HANDOVER_NOTES.md` benennt **sechs Axiome**: `tardos_strongly_polynomial`,
`maurras_separation`, `gls_optimisation`, `cook_np_completeness` u. a.

Das ist **M5 in Reinform**, und zwar in einer besonders instruktiven Ausprägung:

1. **"Zero `sorry`s" ist wahr und zugleich irreführend.** Die Lücken sind nicht als
   `sorry` offengelassen, sondern als `axiom` *geschlossen*. Ein `sorry` erzeugt eine
   Lean-Warnung; ein `axiom` erzeugt keine. Die Buchhaltung ist grün, weil die Schulden
   umgebucht wurden.
2. **Der Kernschritt selbst ist ein Axiom.** Schritt 7 — die Cook-Levin/Karp-Brücke von
   "STSP ∈ P" zu "P = NP" — ist *postuliert*, nicht bewiesen. Damit verifiziert Lean nie
   die Aussage "P = NP", sondern nur "aus meinen Axiomen folgt ein Symbol namens P = NP".
3. **Ein Axiom ist sogar inhaltsleer.** In `N_PEqualsNP.lean` steht wörtlich
   `axiom PolynomialSeparationOracle (P : Type) : Prop`. Das deklariert ein
   **uninterpretiertes Prädikat** — ein opakes Symbol ohne jede Definition. Aussagen
   darüber haben in Lean keinen mathematischen Inhalt. Ebenso
   `noncomputable axiom partialPedigree ... : Pedigree (k-1)` in `N_EdgeInHC.lean`:
   die *Existenz* des zentralen Objekts wird postuliert statt konstruiert.
4. **Das `Backup/`-Verzeichnis dokumentiert den Weg dorthin.** Es enthält Dutzende
   `sorry`s an zentralen Stellen, u. a. `adjacency_iff_gr_connected` und — besonders
   aufschlussreich — `theorem adjacency_iff_rigidity_graph_connected ... :
   AdjacentInPolytope P Q ↔ True := by sorry`: ein Theorem, dessen rechte Seite
   buchstäblich `True` ist, also nichts behauptet. In `ExperimentWithLean.lean` steht der
   Vorgang im Klartext als Kommentar: *"We replace the failing omega tactic with a clean
   local sorry placeholder. This stops Lean from throwing an error here … so your
   environment remains green."*

Ob `Backup/` Teil der Build-Kette ist, kann ich nicht entscheiden; formal mag "zero
`sorry`s in the main chain" korrekt sein. Das ändert nichts: Die Axiome in Schritt 4, 5
und 7 liegen **in** der main chain und sind vom Autor selbst so ausgewiesen. Eine
Widerlegung des Papers habe ich nicht gefunden — sie ist auch nicht nötig, weil das Repo
sich selbst entkräftet. Konfidenz: **hoch** (Quelltext).

**Zusammengenommen** liefern §2.2 und §2.3 die vier vom Auftrag genannten Fehlerwege in
zwei realen Fällen: *unangemessene Statement-Wahl* (Navier–Stokes), *versteckte
Zusatzannahmen* und *axiomatische Ergänzungen* (Pedigree, main chain), *`sorry`-Lücken*
(Pedigree, Backup).

---

## 3. Der Direktversuch an P vs. NP

### 3.1 Hypothese H — **bestätigt**, dreifach trianguliert

**arXiv:2309.05689, "Large Language Model for Science: A Study on P vs. NP"** (Sept. 2023).

**(a) Vollständige Autorenliste** — über drei unterschiedlich formulierte Anfragen
(Titelsuche; "Qingxiu Dong P vs NP"; "Ke Xu" + "SAT requires exhaustive search")
übereinstimmend:

> **Qingxiu Dong, Li Dong, Ke Xu, Guangyan Zhou, Yaru Hao, Zhifang Sui, Furu Wei**
> (Microsoft Research / Peking University u. a.)

**Ke Xu** und **Guangyan Zhou** sind Positionen 3 und 4. Sie sind zugleich die Autoren von
**arXiv:2302.09512, "SAT Requires Exhaustive Search"**. Hypothese H ist damit
**bestätigt**. Konfidenz: **hoch**.

**(b) Die "in alignment with"-Formulierung — bestätigt.** Zwei unabhängige Anfragen geben
den Abstract wörtlich gleichlautend wieder:

> "…GPT-4 successfully produces a proof schema and engages in rigorous reasoning
> throughout 97 dialogue turns, concluding **'P ≠ NP', which is in alignment with
> (Xu and Zhou, 2023)**."

Das Paper räumt die Übereinstimmung also **im eigenen Abstract** ein. Konfidenz: hoch.

**Verschärfung, die über den Auftrag hinausgeht.** Die Personalunion geht in die
Gegenrichtung weiter: **arXiv:2401.01193, "Further Explanations on 'SAT Requires
Exhaustive Search'"** (Jan. 2024) — die Verteidigungsschrift für Xu/Zhou — ist verfasst
von **Qingxiu Dong, Guangyan Zhou und Ke Xu**. Die *Erstautorin des LLM-Papers* ist also
Mitverteidigerin des Arguments, das "GPT-4" angeblich unabhängig reproduziert hat. Die
Autorengruppe ist über alle drei Arbeiten hinweg dieselbe. Konfidenz: hoch.

### 3.2 (c) Methodische Einordnung — fair, aber eindeutig

**Was gegen den Zirkularitätsvorwurf spricht.** Das Paper nennt sich selbst eine
**"pilot study"** und bewirbt als Beitrag primär das *Framework* "Socratic reasoning"
(rekursives Entdecken, Lösen und Integrieren von Teilproblemen mit Selbstevaluation) —
nicht die Lösung von P vs. NP. Als Machbarkeitsstudie gelesen — "kann ein LLM über 97
Dialogrunden einen kohärenten Beweisschemaentwurf durchhalten?" — ist die Arbeit
legitim, und die Offenlegung der Übereinstimmung im Abstract ist ehrlicher als das
Kolportierte suggeriert.

**Was dagegen spricht, und schwerer wiegt.**

1. **Der Zeuge ist befangen.** Zwei Koautoren sind die Urheber der Zielaussage. Ein
   LLM, das mit Koautoren des Zielarguments dialogisiert, ist kein unabhängiger Prüfer.
   Ob GPT-4 das Argument aus Trainingsdaten kannte (2302.09512 lag seit Feb. 2023 auf
   arXiv) oder es im Dialog zugeführt bekam, ist für die Bewertung **gleichgültig** —
   in beiden Fällen ist es Reproduktion, nicht Herleitung.
2. **Es gibt keinen Erfolgsmaßstab.** "In alignment with (Xu and Zhou, 2023)" ist als
   Validierung nur brauchbar, wenn Xu/Zhou 2023 richtig ist. Damit hängt der Erfolg der
   Machbarkeitsstudie an der Korrektheit des Referenzarguments.
3. **Und das Referenzargument ist widerlegt — peer-reviewt.** Das ist der entscheidende
   Punkt:
   - `[PREPRINT]` **arXiv:2312.02071**, "Evaluating the Claims of 'SAT Requires
     Exhaustive Search'" (Chavrimootoo, He, Kotler-Berkowitz, Liuson, Nie, Dez. 2023):
     identifiziert einen Fehler in den Haupttheoremen. Xu/Zhou setzen voraus, dass ein
     Algorithmus für Model RB eine bestimmte Struktur hat, die *downward
     self-reducibility* ausnutzt — diese Annahme ist nicht gerechtfertigt. Damit ist
     weder SETH bewiesen noch P ≠ NP. **Das ist M3 (Strukturannahme über Algorithmen)
     in Reinform.**
   - `[VERIFIZIERT]` **Eric Allender und Ryan Williams, "Comment on 'SAT requires
     exhaustive search'"**, *Frontiers of Computer Science*,
     doi 10.1007/s11704-025-53000-5 — eine **peer-reviewte Widerlegung im selben
     Journal**, in dem Xu/Zhou erschienen sind (FCS 2025, 19(12): 1912405), von zwei der
     prominentesten lebenden Komplexitätstheoretiker. Kernkritik laut Snippet: Das
     Argument macht eine ungerechtfertigte Annahme über *alle möglichen*
     SAT-Algorithmen; die Behauptung widerspricht bekannten Resultaten, wonach 3-SAT
     ohne exhaustive search (wenn auch exponentiell) lösbar ist. **Das ist M2
     (Übergeneralisierungs-Test) — das Argument beweist zu viel.**

**Fazit zu 3.** Der prominenteste "KI-Beweis" zu P vs. NP ist kein Beweis. Er ist die
Reproduktion eines Arguments eigener Koautoren, und dieses Argument ist inzwischen
peer-reviewt widerlegt. Als Machbarkeitsstudie für Dialogführung bleibt die Arbeit
diskutabel; als Evidenz für KI-Fähigkeit bei P vs. NP ist sie **wertlos**, weil ihr
Erfolgskriterium zusammengebrochen ist. Konfidenz: **hoch**.

Bemerkenswert für das Crank-Kapitel des Gesamtprojekts (A8/A1): Xu/Zhou **hat** ein
Peer-Review-Verfahren bestanden. Peer Review ist hier also kein zuverlässiger Filter
gewesen; korrigiert hat die Fachöffentlichkeit, nicht das Verfahren.

### 3.3 Nachfolger 2026: "Khanukov"

`[CLAIM]`, Konfidenz **niedrig-mittel**, zwei Anfragen. Dmitry Khanukov betreibt 2026 ein
öffentliches GitHub-Projekt mit dem Ansatz **"human + specialized LLM orchestra + formal
verification in Lean"** und beansprucht, der Erste mit dieser Kombination zu sein. Nach
Selbstauskunft liegt vor: eine Formalisierung des P ≠ NP-Problems, alle wesentlichen
Publikationen analysiert und in Code übersetzt, eine Gesamtbeweisstruktur — und **"a small
number of identified gaps — assumptions under which the problem is already solved"**; die
Arbeit bestehe darin, diese Annahmen in bewiesene Aussagen zu verwandeln.

**Das ist genau das offene Ende.** "Annahmen, unter denen das Problem bereits gelöst ist"
ist strukturell dieselbe Konstruktion wie die Axiome in §2.3 — die gesamte Schwierigkeit
sitzt in den Lücken. Fortnow erwähnt das Projekt skeptisch und merkt an, ohne Verständnis
des Lean-Codes sei die Strategie unklar. **Kein Resultat, keine Publikation, kein Review.**
Der Briefing-Lead "Khanukov 2026" existiert also, ist aber **kein Direktversuch mit
Ergebnis**, sondern ein laufendes Einzelprojekt.

---

## 4. Formalisierung von Komplexitätstheorie

### 4.1 Was existiert

`[VERIFIZIERT]` **Cook–Levin ist formalisiert — aber nicht in Lean.**

- **Isabelle/HOL:** Frank J. Balbach, "The Cook-Levin theorem", *Archive of Formal
  Proofs* (isa-afp.org/entries/Cook_Levin.html), gepflegt bis mind. Juni 2026. Enthält
  deterministische Mehrband-Turingmaschinen, **die Komplexitätsklassen P und NP**,
  polynomielle many-one-Reduktion und SAT. Basiert auf Arora–Barak. Laut Eintrag die
  erste Formalisierung, in der die komplexitätstheoretischen Begriffe *und* der
  Polynomialitätsnachweis der Reduktion dasselbe Berechnungsmodell verwenden.
- **Coq/Rocq:** Lennard Gäher & Fabian Kunze, "Mechanising Complexity Theory: The
  Cook-Levin Theorem in Coq", ITP 2021, doi 10.4230/LIPIcs.ITP.2021.20. Peer-reviewt.

### 4.2 Was in Lean/mathlib fehlt

`[QUELLTEXT]` **Das ist der belastbarste Befund dieses Abschnitts.** In
`lean-dojo/LeanMillenniumPrizeProblems`, Datei `Problems/PVersusNP/Millennium.lean`,
steht wörtlich:

> "Note: the Clay PDF also mentions standard results and examples (e.g. AKS: `PRIME ∈ P`,
> Cook–Levin: `SAT` is NP-complete). **Those results belong to a larger complexity-theory
> library; this file focuses on the definitions and the Clay statement itself.**"

Der Verweis geht auf eine "larger complexity-theory library", **die es in mathlib nicht
gibt**. Das Projekt formuliert also das *Statement* und muss alles Weitere auslagern.

Weiterer Quelltextbefund: Die README führt für P vs. NP
`Millennium.ClayPVersusNP` (P = NP) bzw.
`Millennium.ClayPVersusNP.Formulations.NegativeBranch` (P ≠ NP) als lösungsrelevante
Propositionen, Status **"Open"**. `Problems/Registry.lean` erklärt die Modellierung:
Probleme mit mehreren Ausgängen bekommen getrennte Propositionen, es gibt **bewusst keine
Aggregatdeklaration**, weil die Disjunktion der Alternativen eine klassische Tautologie
ist — ein induktiver Typ mit einem Konstruktor pro Alternative wäre durch klassische
Fallunterscheidung trivial bewohnt. Das ist eine saubere, aber vielsagende Designnotiz:
Schon das *Aufschreiben* der Frage erfordert Sorgfalt gegen triviale Scheinlösungen.
Es wird ein konkretes Turingmaschinenmodell über endlichem Alphabet und Cooks
Verifizierer-Definition mit String-Zertifikaten verwendet.

`[NUR-SNIPPET]` Eine Review im September 2026, mit eigenständigem Beitrag von **Kevin
Buzzard (PR #9)**, fand, dass **zwei der formalen Statements "interface sketches" sind,
deren Daten durch die angegebenen Eigenschaften nicht festgelegt werden**. Welche zwei,
konnte ich nicht klären. Selbst am Statement-Level ist die Formalisierung also noch nicht
stabil — ein weiterer M4-Datenpunkt: auch Statements müssen begutachtet werden.

Ergänzend `[NUR-SNIPPET]`: Maximilian Keßler arbeitet an einer Lean-Formalisierung von
Komplexitätstheorie mit Ziel Cook–Levin; es existiert eine Zulip-Diskussion
"Computational Complexity Theory" im leanprover-community-Archiv, in der angemerkt wird,
dass Balbachs Isabelle-Arbeit viel Aufwand in Dinge steckt, die in mathlib bereits
vorliegen (etwa Arithmetik auf Bitdarstellungen). Ein einsatzfähiger, in mathlib
integrierter Komplexitätsteil existiert im September 2026 **nicht**.

### 4.3 Fortnows Aussage — **Lead teilweise bestätigt, Wortlaut abweichend**

Der Briefing-Lead lautet, Fortnow habe im Juni 2026 sinngemäß gesagt: "we don't even have
a viable approach". Ich habe den Post identifiziert:

**Lance Fortnow, "Respect the P v NP Problem", Computational Complexity Blog,
10.06.2026.** Über zwei Anfragen belegte Aussagen `[NUR-SNIPPET]`:

- **"Don't waste your time trying a formal approach via Lean."**
- **"Computational complexity is very messy to formulate technically."**
- Er könne kein KI-System dazu bringen, einen vollständigen Lean-verifizierten Beweis
  selbst für etwas Triviales zu liefern (Beispiel: Abgeschlossenheit von P unter einer
  Operation).
- Der Post fragt explizit, ob KI-Fortschritte bei Beweisen ändern, was wir über die
  formale Aussage P ≠ NP wissen, und ob ein KI-erzeugter Beweis bevorsteht.

**Bewertung:** Die *Stoßrichtung* des Leads ist bestätigt — Fortnow hält den
Lean-Zugang zu P vs. NP im Juni 2026 für aussichtslos. Den **exakten Wortlaut "we don't
even have a viable approach" konnte ich nicht verifizieren**; die belegbaren Zitate sind
die drei oben. Ich empfehle der Leitung, im Papier die belegten Formulierungen zu
verwenden und den Lead-Wortlaut zu verwerfen. Konfidenz zur Stoßrichtung: hoch; zum
Wortlaut: **nicht bestätigt**.

Der dritte Punkt ist dabei der aussagekräftigste: Wenn selbst triviale
komplexitätstheoretische Lemmata 2026 nicht KI-automatisiert in Lean bewiesen werden —
während dieselben Systeme 13 Mio. Zeilen FLT-Formalisierung produzieren —, dann liegt das
Hindernis nicht an der Beweislänge, sondern am **fehlenden Formalisierungskontext**.

---

## 5. Bewertung: Welche Problemklasse löst KI-Beweisen?

### 5.1 Das Erfolgsprofil

Aus der Bestandsaufnahme lässt sich ein scharfes Profil ablesen. KI-Beweisen funktioniert
2026, wenn **alle vier** Bedingungen erfüllt sind:

1. **Kurzer Beweis.** Olympiadeaufgaben, isolierte Erdős-Probleme, Verbesserung einer
   numerischen Schranke. Größenordnung: Seiten, nicht Bände.
2. **Klar umrissener Suchraum.** Ein endliches oder gut parametrisiertes Objekt wird
   gesucht.
3. **Billiges Verifikationsorakel.** Der Lean-Kernel sagt in Sekunden ja/nein. Das
   erlaubt massives Parallel-Sampling — 10 000 Agenten bei Navier–Stokes, mehrere Dutzend
   bei FLT, 60 Subagenten bei Riemann-Zeta. **Ohne billiges Orakel kollabiert das
   gesamte Verfahren**, weil dann nicht mehr verworfen werden kann.
4. **Vorhandener Formalisierungskontext.** mathlib deckt Analysis, Zahlentheorie,
   Algebra, algebraische Geometrie gut ab. Wo mathlib endet, endet die Leistung.

### 5.2 Quantitativ

| Größe | Wert | Quelle |
|---|---|---|
| Erdős-Trefferquote (AlphaProof Nexus) | **9 / 353 ≈ 2,5 %** | arXiv 05/2026 |
| OEIS-Trefferquote | **44 / 492 ≈ 8,9 %** | ebd. |
| miniF2F-test (Seed-Prover) | 99 % (243/244) | arXiv:2507.23726 |
| PutnamBench (Seed-Prover) | 331 / 657 ≈ 50 % | ebd. |
| **CombiBench (Seed-Prover)** | **30 %** | ebd. |
| Kosten, 10 Astra-Resultate | ca. 2 000 USD | OpenAI 08/2026 |
| Kosten pro Erdős-Problem | wenige hundert USD | DeepMind 05/2026 |
| Navier–Stokes: Suche / Formalisierung | 88 h / 17 h | OpenAI 09/2026 |
| **FLT-Formalisierung: Zeilen Lean** | **13 000 000** | Anthropic 09/2026 |
| FLT: Zwischentheoreme | 29 500 | ebd. |
| FLT: Token | 6 Mrd. | ebd. |
| FLT: Wall-Clock | 11 Tage (dt. Dutzend Agenten) | ebd. |
| FLT: menschliches Vergleichsprojekt | seit 2024, EPSRC-gefördert **bis 09/2029**; Blueprint Phase 1 = 86 S. | Buzzard |

Die Zahlenreihe ist in sich konsistent und sagt dasselbe: Der Gradient läuft von
Benchmark-Mathematik (99 %) über Putnam (50 %) und Kombinatorik (30 %) zu offenen
Forschungsproblemen (2,5–9 %). Je weniger vorstrukturiert, desto steiler der Abfall.

### 5.3 Warum P vs. NP nicht darunterfällt

Die vier Erfolgsbedingungen scheitern **einzeln und gemeinsam**:

1. **Beweislänge unbekannt und vermutlich groß.** Es gibt keinen Anhaltspunkt, dass ein
   P-vs-NP-Beweis kurz ist. Der Vergleich zu FLT ist instruktiv — und genau deshalb
   irreführend, wenn man ihn falsch zieht: Die 13 Mio. Lean-Zeilen entstanden für einen
   Beweis, den Menschen seit 1995 **kennen und verstehen**, mit einem 86-seitigen
   Blueprint und einer über Jahre entwickelten Route ("R = T" nach Taylor). Die Leistung
   war **Übersetzung**. Für P vs. NP gibt es nichts zu übersetzen. Die Analogie
   "13 Mio. Zeilen in 11 Tagen, also schaffen sie auch P vs. NP" ist ein Kategorienfehler:
   Sie verwechselt Autoformalisierungsdurchsatz mit mathematischer Entdeckung.
2. **Kein endliches Zeugenobjekt.** Das ist die Leitachse. P ≠ NP quantifiziert über
   **alle** Algorithmen — eine unendliche, nicht aufzählbar-durchsuchbare Klasse. Es gibt
   kein Gadget, keinen Graphen, keine Konstruktion, deren Auffinden die Sache erledigt.
   Die drei Barrieren (Relativization, Natural Proofs, Algebrization) sind genau der
   Beweis dafür, dass die naheliegenden *suchbaren* Beweisformen nicht reichen. Eine
   KI-Suche, die in diesen Formen sucht, ist **beweisbar** zum Scheitern verurteilt (M1).
3. **Kein billiges Orakel für den entscheidenden Schritt.** Lean prüft einen *fertigen*
   Beweis. Es liefert kein Signal, ob eine *Teilstrategie* aussichtsreich ist. Bei kurzen
   Beweisen kompensiert Brute-Force-Sampling das; bei einem Beweis unbekannter, großer
   Tiefe gibt es kein verwertbares Zwischensignal — das Belohnungssignal ist über die
   gesamte Suchtiefe null.
4. **Kein Formalisierungskontext.** §4: Cook–Levin existiert in Isabelle und Coq, **nicht
   in Lean/mathlib**. Das Millennium-Repo verweist auf eine Bibliothek, die es nicht gibt.
   Fortnow bekommt nicht einmal triviale P-Abgeschlossenheitslemmata Lean-verifiziert.
   Ein KI-System, das P vs. NP in Lean angreifen wollte, müsste **zuerst die
   Komplexitätstheorie-Bibliothek bauen** — und selbst dann wäre nur die *Frage*
   formuliert.

**Zusatzbefund gegen den Optimismus.** Die beiden 2026er Ereignisse, die dem "KI löst
ein Millennium-Problem"-Narrativ am nächsten kommen, stützen es bei Prüfung nicht:
Navier–Stokes betraf Fefferman C/D bei offenem A/B und wird vom Clay Institute nicht
anerkannt (§2.2); Riemann-Zeta war eine Schrankenverbesserung, von der Anthropic selbst
sagt, die Technik führe **nicht** zum Beweis der Vermutung (§1.2). In beiden Fällen wurde
ein *anderes, benachbartes, endlich-strukturiertes* Problem gelöst. Das ist genau das
Muster der Leitachse.

**Kontext aus der Fachdiskussion** `[NUR-SNIPPET]`: Terence Tao verlagert in "Mathematics
in the Age of AI" (ICM 2026) die Frage bewusst weg von "kann KI Forschungsmathematik?" hin
zu den Folgen einer Beweis-Schwemme: verifizierte Beweise, die **niemand versteht**,
Aufwertung von Exposition, Refereeing und "canonicalization". Für uns relevant: Selbst der
optimistische Pol der Debatte argumentiert über *Menge* korrekter Resultate, nicht über
konzeptuelle Durchbrüche bei Barrieren-Problemen.

### 5.4 Antwort auf die Leitachse (§2 Team-Briefing)

**Bestätigt, ohne Gegenbefund.** Jeder belastbare Erfolg 2024–2026 betrifft suchbare
endliche Zeugen oder Autoformalisierung vorhandenen Wissens. Kein einziger Fall betrifft
eine unendliche Quantifizierung über alle Algorithmen. Der Direktversuch an P vs. NP
(§3) war keine Herleitung, sondern Reproduktion eines inzwischen widerlegten Arguments.
Der einzige laufende Lean-Direktversuch (§3.3) hat seine gesamte Schwierigkeit in
unbewiesenen Annahmen geparkt.

**Ergänzungsvorschlag zur Leitachse.** Die Achse "suchbare Zeugen vs. unendliche
Quantifizierung" sollte um eine **dritte, empirisch dominante Kategorie** erweitert
werden: **Autoformalisierung** (Klasse B). FLT und die Riemann-Zeta-Formalisierung fallen
weder unter "Zeugensuche" noch unter "unendliche Quantifizierung" — sie sind Übersetzung
existierenden menschlichen Wissens in ein formales System. Das ist 2026 die
beeindruckendste KI-Leistung überhaupt, und sie ist für P vs. NP **strukturell irrelevant**,
weil kein Ausgangsbeweis existiert. Ohne diese Kategorie droht das Papier, den
FLT-Erfolg entweder zu unterschätzen oder falsch zu extrapolieren.

### 5.5 Prognose `[EIGENE EINSCHÄTZUNG]`

Ein maschineller Beweis zu P vs. NP ist im Zeithorizont der nächsten Jahre **nicht in
Sicht**. Die realistische nahe Zukunft von KI in der Komplexitätstheorie ist das, was
A-Lead bereits verifiziert hat (arXiv:2509.18057, AlphaEvolve): bessere
Inapproximierbarkeitsschranken durch Gadget-Suche — also wieder Klasse (A). Eine
plausible, nützliche Zwischenetappe wäre der **Aufbau einer
Komplexitätstheorie-Bibliothek in mathlib** inklusive Cook–Levin, portiert aus
Balbach/Gäher–Kunze. Das wäre eine echte Autoformalisierungsaufgabe von der Art, die 2026
nachweislich gelingt, und sie würde die Frage überhaupt erst formulierbar machen. Sie
würde sie nicht beantworten. Konfidenz: mittel-hoch.

---

## 6. Was ich nicht verifizieren konnte

- **Wortlaut** "we don't even have a viable approach" (Fortnow, Juni 2026). Stoßrichtung
  bestätigt, Zitat nicht. → Lead-Wortlaut verwerfen.
- **Peer-Review-Status von AxiomProver.** Zwei Quellen widersprechen sich direkt
  (Axios/cryptobriefing: in Journals erschienen; andere Zusammenfassung: Ende Mai 2026
  keine bestätigt). Beide berichtet, nicht geglättet.
- **Welche zwei** Statements Buzzards Review (PR #9, 09/2026) als "interface sketches"
  beanstandet.
- **Ob `Backup/` im Pedigree-Repo Teil der Build-Kette ist.** Für den Befund unerheblich,
  da die Axiome in der main chain liegen.
- **Volltext** sämtlicher arXiv-Preprints — WebFetch gesperrt. Alle inhaltlichen Angaben
  zu Preprints sind `[NUR-SNIPPET]`.
- **Unabhängige Prüfung** der OpenAI-Navier–Stokes-Lean-Dateien. Niemand hat sie
  öffentlich nachvollzogen; der Community-Konsens fehlt (Stand 12.09.2026).
- **Plausibilität der FLT-Zahlen** (13 Mio. Zeilen / 11 Tage). Firmenangabe, von Buzzard
  positiv kommentiert, aber nicht unabhängig nachgezählt.
- **Substanz des Khanukov-Projekts.** Nur Selbstauskunft plus Fortnows Skepsis.

---

## 7. Quellen

**Peer-reviewt / Journal**
- AlphaProof, "Olympiad-level formal mathematical reasoning with reinforcement learning",
  *Nature*, 12.11.2025 — https://www.nature.com/articles/s41586-025-09833-y
- E. Allender, R. Williams, "Comment on 'SAT requires exhaustive search'",
  *Frontiers of Computer Science* — https://link.springer.com/article/10.1007/s11704-025-53000-5
  · Preprint: https://people.cs.rutgers.edu/~allender/papers/allender.williams.pdf
- K. Xu, G. Zhou, "SAT requires exhaustive search", *FCS* 2025, 19(12):1912405 —
  https://journal.hep.com.cn/fcs/EN/10.1007/s11704-025-50231-4
- L. Gäher, F. Kunze, "Mechanising Complexity Theory: The Cook-Levin Theorem in Coq",
  ITP 2021 — https://drops.dagstuhl.de/entities/document/10.4230/LIPIcs.ITP.2021.20
- F. J. Balbach, "The Cook-Levin theorem", *Archive of Formal Proofs* —
  https://isa-afp.org/entries/Cook_Levin.html

**Preprints**
- arXiv:2309.05689 — Dong, Dong, **Xu**, **Zhou**, Hao, Sui, Wei, "Large Language Model
  for Science: A Study on P vs. NP" — https://arxiv.org/abs/2309.05689
- arXiv:2302.09512 — Xu, Zhou, "SAT Requires Exhaustive Search" — https://arxiv.org/abs/2302.09512
- arXiv:2401.01193 — **Dong**, Zhou, Xu, "Further Explanations on 'SAT Requires
  Exhaustive Search'" — https://arxiv.org/abs/2401.01193
- arXiv:2312.02071 — Chavrimootoo et al., "Evaluating the Claims of 'SAT Requires
  Exhaustive Search'" — https://arxiv.org/pdf/2312.02071
- arXiv:2507.23726 — "Seed-Prover: Deep and Broad Reasoning for Automated Theorem
  Proving" — https://arxiv.org/pdf/2507.23726
- arXiv:2504.21801 — "DeepSeek-Prover-V2" — https://arxiv.org/pdf/2504.21801
- arXiv:2606.03194 — Arthanari, "Lean 4 Machine-Verified Proof of P = NP via the
  Pedigree Polytope Membership Problem" — https://arxiv.org/abs/2606.03194 `[CLAIM]`

**Quelltext (im Wortlaut gelesen, GitHub-Code-Suche)**
- `lean-dojo/LeanMillenniumPrizeProblems` — https://github.com/lean-dojo/LeanMillenniumPrizeProblems
  (README; `Problems/PVersusNP/Millennium.lean`; `Problems/Registry.lean`;
  `Problems/PVersusNP/PolynomialHierarchy.lean`; `lakefile.toml`, mathlib v4.31.0)
- `TiruArt/Pedigree-Polytopes-Lean4` — https://github.com/TiruArt/Pedigree-Polytopes-Lean4
  (README; `MembershipProject/README.md`; `HANDOVER_NOTES.md`; `INSTALL.md`;
  `MembershipProject/Core/N_PEqualsNP.lean`; `MembershipProject/Core/N_EdgeInHC.lean`;
  `Backup/*.lean`)

**Blogs / Primärankündigungen**
- L. Fortnow, "Navier-Stokes and Lean", 09/2026 —
  https://blog.computationalcomplexity.org/2026/09/navier-stokes-and-lean.html
- L. Fortnow, "Respect the P v NP Problem", 10.06.2026 —
  https://blog.computationalcomplexity.org/2026/06/respect-p-v-np-problem.html
- OpenAI, "On the Navier–Stokes Millennium Prize Problem" — https://openai.com/index/navier-stokes-solution/
- Anthropic, "Claude's progress on the Riemann hypothesis" — https://www.anthropic.com/research/riemann-zeta
- Anthropic, "Formalizing Fermat's Last Theorem" — https://www.anthropic.com/research/formalizing-fermats-last-theorem
  · PDF: https://www-cdn.anthropic.com/9e431dff043da6538d99d6c2d231b670aa3da263.pdf
- T. Tao, "Mathematics in the age of AI", ICM 2026 — https://teorth.github.io/tao-web/slides/age-of-ai-icm-2026.pdf
- DeepMind AlphaProof (IMO 2024) — https://deepmind.google/blog/ai-solves-imo-problems-at-silver-medal-level/
- Zulip-Archiv, "Computational Complexity Theory" —
  https://leanprover-community.github.io/archive/stream/113488-general/topic/Computational.20Complexity.20Theory.html

**Presse (zur Rekonstruktion des Navier–Stokes-Vorgangs, durchweg `[NUR-SNIPPET]`)**
- Quanta Magazine, 08.09.2026 — https://www.quantamagazine.org/ai-has-solved-one-of-maths-1-million-millennium-prize-problems-20260908/
- Fortune, 08.09.2026 — https://fortune.com/2026/09/08/openai-says-it-cracked-navier-stokes-math-grand-challenge-buckmaster-accusation-cheating-intimidation-tao-lament/
- implicator.ai (Clay Institute) — https://www.implicator.ai/clay-institute-navier-stokes-openai-proof-claim/
- SiliconANGLE (Astra, 02.08.2026) — https://siliconangle.com/2026/08/02/openais-astra-solves-10-long-open-math-problems-publishes-proofs/
- Axios (Axiom Math, 26.05.2026) — https://www.axios.com/2026/05/26/axiom-ai-math-journal
- TNW (FLT/Buzzard) — https://thenextweb.com/news/anthropic-claude-fermat-last-theorem-lean-buzzard
