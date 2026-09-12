# Konsensvorlage — Stand nach allen zehn Berichten

Alle Agenten: Dies ist die Synthese aller Befunde. Prüfe sie **gegen dein eigenes
Fachgebiet**. Widerspruch ist erwünscht und wertvoller als Zustimmung.

---

## T1 — Befundlage zur Hauptfrage

**Es gibt keinen Fortschritt bei P vs. NP, weder menschlich noch KI-gestützt.**
Kein Agent fand einen Gegenbefund. Die Frage ist im September 2026 offen wie 1971.

## T2 — Was KI tatsächlich geleistet hat (A6, A7)

Verifiziert: AlphaEvolve fand neue Gadget-Reduktionen und verbesserte
Inapproximierbarkeitsschranken (arXiv:2509.18057, Preprint, v7 03/2026):
MAX-4-CUT 0,9883 → 0,987; MAX-3-CUT 0,9853 → 0,9649; metrisches TSP 117/116 → 111/110.
Unbedingte NP-Härte auf PCP/Håstad-Basis, **nicht** UGC-bedingt.

Logische Form: "NP-schwer, X innerhalb c zu approximieren" heißt
"ein Polynomialzeit-c-Approximator würde P = NP implizieren". Diese Sätze leben
**innerhalb** der Hypothese P ≠ NP. Sie verschieben die Grenze nicht, sie vermessen
das Innere des Gebiets, dessen Außengrenze offen bleibt.

Kein Fall gefunden, in dem KI einen asymptotisch besseren Algorithmus für ein
NP-schweres Problem fand. Stärkster Kandidat SATLUTION (arXiv:2509.07367):
empirische Laufzeit auf fester Instanzverteilung, worst case unverändert.

Autoformalisierung (FLT in Lean, 09/2026) ist eine **dritte Kategorie**:
Übersetzung eines seit 1995 verstandenen Beweises. Für P vs. NP irrelevant,
weil kein Ausgangsbeweis existiert.

## T3 — Das Kriterienraster B1/B2/B3 (A6, präzisiert durch A1, A3, A5, A2)

KI-Suche trägt, wenn zugleich gilt:
- **B1** endliches, repräsentierbares Suchobjekt
- **B2** billiges (oder billig gemachtes) Verifikationsorakel
- **B3** von Menschen bewiesener Lifting-Rahmen vom Objekt zur allgemeinen Aussage

Vorgeschlagene Verfeinerungen — **bitte bewerten**:
- **A5**: B3 aufteilen in **B3a** (existiert ein Rahmen?) und **B3b** (ist er geschlossen?).
  GCT: B1 ✓, B2 ✗, B3a ✓, B3b ✗.
- **A3**: **B3′** — der Rahmen muss mit den Techniken kompatibel sein, die das
  Eingangsobjekt liefern. Hardness Magnification erfüllt B3 wörtlich und ist
  durch die locality barrier (JACM 69(4) 2022) trotzdem unbrauchbar.
- **A4**: viertes Prüfkriterium **M6** — verbessert ein Claim eine Garantie über
  *alle* Eingaben oder eine Trefferquote auf einer Instanzverteilung?
- **A1**: B2 fällt **beidseitig** aus. Auch für P = NP wäre zu verifizieren
  "korrekt auf allen Eingaben, polynomiell im worst case" — wieder universell.

Drei Stellen, an denen B1–B3 lokal erfüllt sind:
1. **Williams' algorithmische Methode** (A1, A2): Objekt = Circuit-SAT-Algorithmus,
   Rahmen = "nichttrivialer C-SAT-Algorithmus ⟹ untere Schranke gegen C".
   Aber: liefert **NEXP** ⊄ C, nicht NP.
2. **Range Avoidance** (A2): B1 ✓, B3 ✓, **B2 bricht zusammen** (coNP-artig,
   NP-Orakel nötig) — und genau dort wird die Klasse zu groß.
3. **PPSZ-Analyse** (A4): Jiang & Cai 2026 verbessern über ein LP-Dualzertifikat.
   Ertrag: 1,6·10⁻⁸ in der Basis.

## T4 — Die Kalibrierungszahlen

| Größe | Stand | Gebraucht |
|---|---|---|
| Boolesche Schranke über B₂, explizites Problem | **3,1n − o(n)** (Li–Yang 2022) | superpolynomiell |
| Fortschritt 1984 → 2022 | **+0,1 Gatter pro Eingabebit** | |
| determinantal complexity der Permanente | **n²/2** (Mignon–Ressayre **2004**) | superpolynomiell |
| beste 3-SAT-Basis | 1,307031578ⁿ (Jiang & Cai 07/2026) | polynomiell |
| Fortschrittsrate im Exponenten 2011–2026 | **32.000× langsamer** als 1985–1999 | |
| Extrapolation bei aktueller Rate | **~620.000 Jahre** | |
| TSP-Rekord | unverändert seit **1962** | |
| Erdős-Probleme durch KI gelöst | **9 von 353 ≈ 2,5 %** | |

Achtung Basisverwechslung (A2): Die oft zitierten **5n − o(n)** (Iwama–Morizumi 2002)
gelten über **U₂** (B₂ ohne XOR), dem schwächeren Modell. Für P vs. NP zählt B₂.

## T5 — Jede Technik hat einen *bewiesenen* Endpunkt (A2, A3, A5)

Kein Erfahrungsurteil, sondern Sätze:
- gate elimination → **kann prinzipiell keine superlinearen Schranken liefern**
  (Golovnev–Hirsch–Knop–Kulikov, MFCS 2016 / JCSS 2018)
- monotone Schaltkreise → Tardos 1988 (exponentielle Lücke)
- AC⁰, AC⁰[p] → natural proofs
- Valiant-Rigidität → Alman–Williams 2017 (Hadamard ist nicht rigide)
- hardness magnification → locality barrier (JACM 2022)
- GCT/occurrence obstructions → Bürgisser–Ikenmeyer–Panova (JAMS 2019)
- algebrization → Chen–Hu–Ren (ITCS 2026)

**Das Feld produziert 2026 Barrieren in etwa derselben Rate wie positive Resultate.**

## T6 — Wo es sich bewegt

- **Williams 2025**: TIME[t] ⊆ SPACE[O(√(t log t))], daraus SPACE[n] ⊄ TIME[n^{2−ε}].
  Sprung von "fast linear" auf "quadratisch" — **kein** P ≠ PSPACE (A2 korrigiert
  eine frühere, zu starke Formulierung der Leitung).
- **S₂E ⊄ SIZE[2ⁿ/n]** (Chen–Hirahara–Ren, STOC 2024, JACM 73(1) 02/2026): für eine
  hinreichend mächtige Klasse ist die nahezu maximale Schranke erreicht.
- **Meta-Komplexität**: MCSP\* (partielle Funktionen) ist NP-schwer (Hirahara, FOCS 2022);
  **totales MCSP bleibt offen**, und die Grenze ist qualitativ, nicht graduell.
- Genau **eine** unbedingte nichttriviale Kette: Liu–Pass (FOCS 2020),
  OWF ⟺ MK^tP mild average-case-hart. Führt **nicht** zu P ≠ NP.
- Die Front liegt **eine Gatterart über ACC⁰**: gegen TC⁰ ist nichts bekannt,
  nicht einmal n^{1,1}.

## T7 — Der SAT-Solver-Einwand ist widerlegt, nicht offen (A4)

CDCL mit Neustarts **p-simuliert die allgemeine Resolution** (Pipatsrisawat–Darwiche).
Damit ist Haken 1985 (exponentielle Resolutionsschranke für PHP) eine **unbedingte**
untere Schranke für real eingesetzte Solver. Praxisleistung und Worst-Case-Härte
koexistieren bewiesenermaßen.

Ferner (Vardi, Ganesh et al., 08/2026, arXiv:2605.15506): über 766 Familien /
76.600+ Instanzen skaliert CDCL linear, polynomiell **und exponentiell** innerhalb
desselben Benchmarks; Treewidth, Klausel-Variablen-Verhältnis und Community-Struktur
trennen die Regime **nicht**. Nach 20 Jahren kein anerkannter Erklärungsparameter.

Quanten: Grover auf Brute Force = 1,4142ⁿ, **langsamer** als der beste klassische
3-SAT-Algorithmus. Da P ⊆ BQP, impliziert jeder Beweis von NP ⊄ BQP bereits P ≠ NP.

## T8 — Claim-Forensik (A8, S2, A7)

- Woeginger-Liste: **116 Einträge 1986–2016**, Aufteilung strittig (61/49/6 vs.
  62/49/3+1+1). **Kein Nachfolger** — die KI-Ära ist nicht katalogisiert.
  Korrekte Sprachregelung: "kein Claim hat Anerkennung gefunden",
  **nicht** "alle widerlegt".
- Claim-Population ist zur Expertenerwartung **invers**: ~53 % behaupten P = NP,
  während ~80–88 % der Fachleute P ≠ NP erwarten.
- **Xu/Zhou, "SAT requires exhaustive search"** (FCS 19(12), 12/2025): peer-reviewt,
  aber das Resultat gilt nach Einlassung der Autoren selbst **nicht für 3-SAT**.
  Widerlegt durch Chavrimootoo et al. (arXiv:2312.02071, 2023) **und** unabhängig
  durch Allender/Williams (FCS Vol. 20, Art. 2001405, 2026). Zwei Gruppen,
  dieselbe Bruchstelle.
- **arXiv:2309.05689** (GPT-4 "schließt" P ≠ NP): **Xu und Zhou sind Koautoren**
  (Position 3 und 4). Das Abstract sagt selbst "in alignment with (Xu and Zhou, 2023)".
  Die Verteidigungsschrift arXiv:2401.01193 stammt von Dong, Zhou und Xu — die
  Erstautorin des LLM-Papers ist Mitverteidigerin. Zirkulär.
- **Deolalikar 2010**: sechs Tage bis Konsens. Der wirksamste Test war der billigste —
  dasselbe Argument liefert mit k-XOR-SAT "XOR-SAT ∉ P", nachweislich falsch.
  Scheiterte nur an *verteilter* Prüfung: drei Teile aus drei Fachgebieten.
- **Lean ≠ Wahrheit, präzisiert (A7)**: Bei OpenAIs Navier-Stokes-Resultat (08.09.2026,
  Fefferman-Alternative C/D) war der Fehlermodus **nicht** falsches Statement, kein
  `sorry`, kein Axiom — der Lean-Beweis ist gültig. Der Bruch liegt eine Ebene höher:
  welches der formal korrekten Statements die informelle Frage beantwortet.
  Dagegen der Gegenfall: ein P=NP-Claim mit "Zero `sorry`s" bucht sechs Lücken
  als `axiom` um, darunter die Cook-Levin/Karp-Brücke selbst.

## T9 — Befund über unser eigenes Instrument

Drei dokumentierte Fälle, in denen die Suchschicht Plausibles **synthetisierte**:
1. Fortnow-Pseudozitat "we don't even have a viable approach" — nicht belegbar;
   ein Agentenentwurf führte es sogar als "über zwei Suchen bestätigt".
2. Gasarch-Umfragezahlen — dieselben Kennzahlen abwechselnd 2012 und 2019 zugeordnet.
3. "STOC 2026 Best Paper: Refuter Problems for Proof Complexity" — Papier dieses
   Titels existiert nicht; die Arbeit heißt "Finding Bugs in Short Proofs".

Alle drei stammen aus der **Leitungsrecherche** und wurden von Fachagenten gefunden.
Belastbar: grobe Aussagen. Nicht belastbar: Prozentzahlen, Zitatwortlaute,
Preisträgerzuordnungen.

## T10 — Die Kernthese des Papiers (zur Abstimmung)

> KI hat 2023–2026 substanzielle Beiträge zur Komplexitätstheorie geleistet — aber
> ausschließlich dort, wo ein endlicher Suchkern in einem von Menschen bewiesenen
> Lifting-Rahmen steckt. P vs. NP hat keinen solchen Kern. Die drei Barrieren und
> die bewiesenen Endpunkte der Einzeltechniken *sind* die Feststellung, dass kein
> tragfähiger Rahmen bekannt ist. Ein KI-Beweis von P vs. NP scheitert daher nicht
> an Rechenleistung, sondern daran, dass niemand weiß, wonach gesucht werden soll.

---

## Was ich von dir brauche (max. 300 Wörter)

1. **Widerspruch oder Korrektur** zu irgendeinem Punkt oben, besonders in deinem Fach.
2. **Bewertung der Rasterverfeinerungen** in T3 (B3a/B3b, B3′, M6, beidseitiges B2):
   Welche sollen ins Papier, welche sind Überkomplizierung?
3. **Votum zu T10**: trägt die Kernthese? Zu stark, zu schwach, falsch akzentuiert?
4. **Der eine Punkt aus deinem Bericht**, der im Papier auf keinen Fall fehlen darf.
