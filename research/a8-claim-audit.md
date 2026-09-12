# A8 — Claim-Auditor & Crank-Forensik

**Agent:** A8 (Pflichtrolle, Vetorecht)
**Stand:** 12. September 2026
**Methodik:** Ausschließlich WebSearch (WebFetch projektweit gesperrt). Kein Volltextzugang.
Jeder nichttriviale Claim mit ≥2 unterschiedlich formulierten Suchanfragen trianguliert.
Genutzte Suchanfragen: 30 (Budget ca. 35).

> **Generalvorbehalt `[NUR-SNIPPET]`:** Praktisch *alles* in diesem Bericht ist aus
> Suchmaschinen-Snippets und -Synthesen rekonstruiert, nicht am Volltext geprüft.
> Wir können in der Regel belegen, **dass** eine Arbeit existiert und **wie sie
> typischerweise referiert wird** — nicht zuverlässig, **was exakt** in ihr steht.
> Wörtliche Zitate unten sind Snippet-Zitate und als solche zu behandeln.

---

## 1. Vorrangige Aufgabe: Verifikation der vier Hypothesen H1–H4

### H1 — Allender/Williams-Kommentar — **BESTÄTIGT** (Konfidenz: hoch)

`[VERIFIZIERT]` `[NUR-SNIPPET]`

- **Autorschaft bestätigt:** Eric Allender (Rutgers) und Ryan Williams (MIT).
  Drei unabhängig formulierte Suchen liefern konsistent dieselben zwei Namen;
  zusätzlich existiert eine Preprint-Kopie auf Allenders eigener Institutsseite
  (`people.cs.rutgers.edu/~allender/papers/allender.williams.pdf`) — das ist ein
  starkes, vom Verlag unabhängiges Autorschaftsindiz.
- **Venue-Korrektur gegenüber der Hypothese:** Die Hypothese nennt nur die DOI.
  Die Suchen datieren den Kommentar auf **Frontiers of Computer Science,
  Vol. 20, Artikelnr. 2001405 (2026)** — also *nicht* im selben Heft wie der
  Originalartikel (19(12), Dez. 2025). DOI-Präfix `10.1007/s11704-025-53000-5`
  (Jahr 2025 im DOI, Erscheinen 2026) ist konsistent mit Springer-Praxis
  (Online-First). **Für unser Papier: Venue als „FCS 20:2001405 (2026)" zitieren.**
- **Wortlaut Kernvorwurf — bestätigt:** Konsistent über drei Suchen:
  das Argument gehe „far short of a proof", weil es „an assumption about all
  possible SAT algorithms that is unwarranted" mache.
  Konfidenz hoch (identische Formulierung in mehreren unabhängigen Quellen-Snippets,
  offenkundig aus dem Abstract).
- **Der konkretere Vorwurf (downward self-reducibility) — bestätigt, aber
  vorsichtiger zu formulieren:** Die Suchen bestätigen, dass es um die Annahme geht,
  ein Algorithmus für **Model RB** müsse „a certain structure that can leverage
  downward self-reducibility" haben, d. h. Instanzen der Größe *n* auf Instanzen
  der Größe *n−1* zurückführen. Die in der Hypothese unterstellte Zuspitzung
  („jeder SAT-Algorithmus müsse …") ist inhaltlich richtig getroffen, aber ich habe
  den exakten Satz nicht am Volltext. Konfidenz mittel-hoch.

### H2 — Chavrimootoo et al., arXiv:2312.02071 — **BESTÄTIGT** (Konfidenz: hoch)

`[PREPRINT]` `[NUR-SNIPPET]`

- **Vollständige Autorenliste (aufgelöst):** Michael C. Chavrimootoo, Yumeng He,
  Matan Kotler-Berkowitz, Harry Liuson, Zeyu Nie — Gruppe an der
  **University of Rochester** (Umfeld Lane Hemaspaandra; Hemaspaandra erscheint
  laut Snippet in der Danksagung, nicht als Koautor).
  Semantic-Scholar-Kurzform „Chavrimootoo-He" bestätigt die ersten beiden Namen
  unabhängig. Eingereicht **4. Dezember 2023**.
- **Natur des Einwands bestätigt:** Fehler in den **Haupttheoremen**; die für
  downward self-reducibility benötigte Struktur sei „not guaranteed to exist";
  Folge: „neither proves SETH to be true nor proves P ≠ NP". Wortgleich über
  zwei unabhängige Suchen.
- **Unabhängigkeit bestätigt und wichtig:** Dieser Einwand (Dez. 2023) ist
  **zwei Jahre älter** als der Allender/Williams-Kommentar (2026) und stammt aus
  einer völlig anderen Gruppe — und beide identifizieren **denselben** Fehler.
  Das ist der stärkste Einzelbefund dieses Audits: unabhängige Konvergenz zweier
  Prüfteams auf denselben Strukturmangel.

### H3 — Zirkularität des GPT-4-„Beweises" — **BESTÄTIGT** (Konfidenz: hoch)

`[PREPRINT]` `[NUR-SNIPPET]`

Dies war die wichtigste Hypothese; ich habe sie über **drei** unterschiedlich
formulierte Suchen geprüft, zusätzlich fiel ein vierter, unabhängiger Beleg an.

- **Autorenliste arXiv:2309.05689** („Large Language Model for Science: A Study on
  P vs. NP", 11.09.2023): **Qingxiu Dong, Li Dong, Ke Xu, Guangyan Zhou, Yaru Hao,
  Zhifang Sui, Furu Wei.**
  → **Ke Xu und Guangyan Zhou sind Koautoren. Bestätigt.**
  Zwei unabhängige Suchen liefern dieselbe Liste; Semantic Scholar führt das Paper
  als „Dong-Dong" (Qingxiu Dong / Li Dong), konsistent.
- **Der Zirkelschluss ist im Abstract selbst dokumentiert:** GPT-4 schließt nach
  97 Dialogzügen „P ≠ NP", „**which is in alignment with (Xu and Zhou, 2023)**".
  Die referenzierte Arbeit ist genau „SAT Requires Exhaustive Search" **derselben
  Koautoren**. Ein Snippet einer Community-Diskussion nennt das explizit
  „the reference … of some intersecting authors".
- **Vierter, unabhängiger Beleg:** arXiv:2401.01193 („Further Explanations on
  ‚SAT Requires Exhaustive Search'") hat die Autorenliste
  **Qingxiu Dong, Guangyan Zhou, Ke Xu** — die Erstautorin des LLM-Papers
  ist Koautorin der Verteidigungsschrift des SAT-Papers. Die Personenüberlappung
  ist damit in beide Richtungen belegt.
- **`[EIGENE EINSCHÄTZUNG]`, Konfidenz hoch:** Der Befund „GPT-4 kommt zu P ≠ NP"
  hat **null evidentiellen Wert** für P vs. NP. Er ist bestenfalls ein Befund über
  Socratic-Reasoning-Prompting, und selbst dort ist die „Übereinstimmung mit
  vorheriger Forschung" kein Gütesiegel, sondern Übereinstimmung mit einer
  Arbeit der eigenen Koautoren, die inzwischen zweifach als fehlerhaft
  kritisiert wurde. **Vetokandidat Nr. 1.**

### H4 — Woegingers Zahlen — **IM KERN BESTÄTIGT, mit dokumentiertem Widerspruch**

`[VERIFIZIERT]` (Existenz/Umfang) / `[CLAIM]` (exakte Aufteilung) — Konfidenz: mittel

Gemäß Triangulationspflicht §1 des Team-Briefings berichte ich **alle drei
abweichenden Zahlenangaben und glätte nicht:**

| Quelle (Suchsynthese) | Gesamt | „P=NP" | „P≠NP" | Sonstiges | Stichtag |
|---|---|---|---|---|---|
| A | 116 | **61** | 49 | 6 (u. a. unentscheidbar) | Sept. 2016 |
| B | 116 | **62** | 49 | 3 unbeweisbar/unentscheidbar, 1 „kein Claim", 1 „beides" | Sept. 2016 (eine Variante: April 2016) |
| C | „>100" | — | **50** | 2 unbeweisbar, 1 unentscheidbar | 1986–2016 |

**Bewertung:**
- **Bestätigt (hoch):** Gesamtumfang **116** Einträge, Zeitraum **1986 bis 2016**,
  letzte Revision der Seite **26. September 2016**.
- **Bestätigt (hoch):** Die Mehrheit der Einträge behauptet **P = NP** (61 oder 62),
  eine Minderheit **P ≠ NP** (49 oder 50), ein kleiner Rest Sonstiges (3–6).
- **Nicht auflösbar (`[CLAIM]`):** 61 vs. 62 und 49 vs. 50. Die Differenz rührt
  vermutlich von unterschiedlicher Behandlung des Eintrags, der *beides* behauptet,
  und der Sonstiges-Kategorie. Ohne Volltextzugang zur Liste nicht entscheidbar.
- **Zur Vorrecherche-Abweichung („>100, ca. 50 für P≠NP"):** Das ist **kein anderer
  Stichtag**, sondern dieselbe Liste in gerundeter Wikipedia-Formulierung.
  49 ≈ „ca. 50". Der Widerspruch löst sich auf.
- **`[EIGENE EINSCHÄTZUNG]`, hoch:** Die inhaltlich interessante Zahl ist nicht
  116, sondern das **Verhältnis**: ~53 % der Beweisversuche behaupten P = NP,
  während ~80–90 % der Fachleute P ≠ NP erwarten. Die Claim-Population ist zur
  Expertenerwartung **invers** verteilt. Das ist ein starker Indikator dafür, dass
  die Claims nicht aus dem Feld kommen, sondern von außen — und dass „P = NP"
  attraktiver ist, weil ein einzelner konstruierter Algorithmus subjektiv leichter
  vorzeigbar wirkt als eine untere Schranke.

---

## 2. Basisrate: Woegingers P-versus-NP-Seite und ihre Nachfolge

`[VERIFIZIERT]` `[NUR-SNIPPET]`, Konfidenz hoch (Existenz/Status), mittel (Nachfolge)

- **Seite:** „The P-versus-NP page", betrieben von Gerhard J. Woeginger (TU Eindhoven,
  später RWTH Aachen). Erreichbar unter `wscor.win.tue.nl/woeginger/P-versus-NP.htm`
  (früher `win.tue.nl/~gwoegi/P-versus-NP.htm`). Das Verzeichnis
  `.../P-versus-NP/` mit den archivierten PDFs (u. a. `Deolalikar.pdf`) ist
  weiterhin online.
- **Status:** Letzte Revision **26.09.2016**. Woeginger verstarb am **1. April 2022**.
- **Nachfolge: NEGATIVBEFUND.** Ich konnte **keinen** Nachfolger und **keine**
  fortgeführte kuratierte Liste identifizieren. Die Seite ist eingefroren und wird
  offenbar nur noch von der TU/e gehostet. `[EIGENE EINSCHÄTZUNG]:` Die Liste ist
  seit 2016 tot — d. h. die gesamte KI-Ära der Crank-Literatur (2023 ff.) ist
  **nicht erfasst**. Wer die Basisrate für 2026 abschätzen will, hat **keine**
  systematische Datenquelle mehr. Das ist ein struktureller Datenverlust, und wir
  sollten ihn im Papier als solchen benennen.
- **Wie viele Claims haben Bestand? — Erwartung bestätigt: null.** Aber die
  präzise Formulierung ist wichtig, sonst behaupten wir selbst zu viel:
  - `[VERIFIZIERT]` Kein Claim ist von der Community akzeptiert; P vs. NP ist
    weiterhin offenes Millennium-Problem (Clay), nur die Poincaré-Vermutung ist
    von den sieben gelöst. Stand 2026 unverändert.
  - `[NUR-SNIPPET]` Ein Snippet zur Liste formuliert selbst die
    erkenntnistheoretisch korrekte Einschränkung: *niemand hat alle 116 Einträge
    durchgearbeitet*; für viele ist die Fehlerhaftigkeit leicht zu zeigen, aber
    es existiert keine publizierte Gesamtwiderlegung.
  - **Korrekte Formulierung für unser Papier:** „Kein Beweisversuch hat
    Anerkennung gefunden; für keinen liegt eine akzeptierte Verifikation vor."
    **Nicht:** „Alle 116 wurden widerlegt."

---

## 3. Fallstudie Deolalikar 2010 — kollektive Fehlerfindung, die funktionierte

`[VERIFIZIERT]` `[NUR-SNIPPET]`, Konfidenz hoch

**Ablauf (Chronologie aus mehreren Suchen konsistent):**

| Datum 2010 | Ereignis |
|---|---|
| Fr., 6. Aug. | Vinay Deolalikar (HP Labs) verschickt per E-Mail ein 103-seitiges Manuskript „P ≠ NP" an Fachkollegen. |
| ~7./8. Aug. | Stephen Cook leitet weiter mit der Einschätzung, dies sei „a relatively serious claim to have solved P vs NP" — das ist der Grund, warum der Fall überhaupt Aufmerksamkeit bekam. |
| 8. Aug. | Richard Lipton bloggt „A Proof That P Is Not Equal To NP?" auf *Gödel's Lost Letter and P=NP*. Öffentliche Prüfung beginnt. |
| 10. Aug. | Lipton/Regan: „Update on Deolalikar's Proof". Parallel entsteht die Seite *Deolalikar P vs NP paper* im **Polymath-Wiki** (Michael Nielsen), als zentrale Sammelstelle. |
| 11. Aug. | „Deolalikar Responds To Issues About His P≠NP Proof". |
| 12. Aug. | **„Fatal Flaws in Deolalikar's Proof?"** — Wendepunkt. Presseecho (TechCrunch: „Attempt At P ≠ NP Proof Gets Torn Apart Online"). |
| 13. Aug. | Gowers: „Could anything like Deolalikar's strategy work?" |
| 15. Aug. | Lipton: „The P≠NP ‚Proof' Is One Week Old" — Konsens: fatal flawed. |
| 17. Aug. | Der Draft wird von Deolalikars Seite entfernt. |
| 20.–23. Aug. | HP-Technical-Report erscheint und wird wieder entfernt. |

**Beteiligte (bestätigt):** Richard Lipton und Ken Regan (Blog als Hauptforum),
Terence Tao, Timothy Gowers, Neil Immerman, Steven Lindell, Scott Aaronson
(flankierend, siehe unten), Stephen Cook (Initialweiterleitung), Michael Nielsen
(Wiki-Infrastruktur).

**Woran es scheiterte — drei getrennte Bruchstellen:**

1. **Finite model theory (Immerman).** Immerman, Fachmann für FMT, identifizierte
   zwei Fehler im FMT-Teil, die als „extremely damaging" beschrieben werden;
   u. a. eine Konfusion darüber, ob die betrachtete Logik erster Stufe eine
   Ordnungsrelation enthält oder nicht. (Das ist keine Kleinigkeit: mit Ordnung
   sind die Ausdrucksstärke-Resultate der FMT völlig andere.)
2. **Das „tupling"-Problem (Lindell).** Wird in den Snippets als „most serious
   specific issue" bezeichnet.
3. **Statistische Physik / Random-k-SAT — die Übergeneralisierung.** Das ist der
   fatale Punkt und der, den wir im Papier brauchen: Deolalikar nutzte die
   bekannte Verbindung zwischen Random-k-SAT und statistischer Physik
   (Cluster-/Shattering-Struktur des Lösungsraums). Der Einwand: **ersetzt man im
   Argument k-SAT durch k-XOR-SAT, folgt dieselbe Konklusion — und
   k-XOR-SAT ist nachweislich in P** (Gauß-Elimination über GF(2)). Ein Snippet
   fasst es präzise: Deolalikar habe „accidentally amplified the power of his
   tools from something too weak to establish his claim to something too strong"
   und etabliere dadurch die *falsche* Aussage, k-XOR-SAT sei nicht in P.
   Auf die direkte Frage nach XOR-SAT antwortete er mit vagen Verweisen auf
   spätere Fassungen.

**Ergebnis:** Starker Konsens, dass die gefundenen Fehler **nicht reparabel**
(„unlikely to be fixable") sind. Nie in einem peer-reviewten Journal erschienen.

**Was man über kollektive Fehlerfindung lernt (`[EIGENE EINSCHÄTZUNG]`, hoch):**

- **Geschwindigkeit:** Von Ankündigung bis belastbarem Konsens **ca. eine Woche**.
  Ein klassischer Journal-Review hätte 6–18 Monate gebraucht. Das ist der Grund,
  warum das Deolalikar-Verfahren ein *Positivbeispiel* ist.
- **Arbeitsteilung nach Expertise:** Der Beweis hatte drei Teile (FMT, statistische
  Physik, Komplexität). Kein einzelner Gutachter deckt alle drei ab — genau deshalb
  scheiterte er an einer *verteilten* Prüfung und hätte einen Einzelgutachter
  vermutlich überstanden. **Das ist das strukturelle Argument gegen
  Einzel-Peer-Review bei Millennium-Claims.**
- **Der wirksamste Test war der billigste.** Nicht die tiefe FMT-Analyse hat die
  Sache entschieden, sondern die Frage: „Warum scheitert dein Argument bei
  XOR-SAT?" Das ist ein Zweizeiler, und er war tödlich.
- **Infrastruktur zählt:** Ein Blog mit Kommentarspalte plus ein Wiki reichten aus.
  Der Fall lief formal *nicht* als Polymath-Projekt, nutzte aber dessen Format und
  Wiki. Das Verfahren ist reproduzierbar und kostenlos.
- **Grenze:** Es funktionierte, weil ein Autorität (Cook) die Sache initial ernst
  nahm. Bei einem unbekannten Einreicher wäre dieselbe Arbeit ignoriert worden.
  Kollektive Fehlerfindung skaliert **nicht** auf die Masse — siehe §5.

---

## 4. Fallstudie „SAT requires exhaustive search" (Xu/Zhou) — Urteil nach M1–M5

### 4.1 Der Vorgang

`[VERIFIZIERT]` (Existenz, Venue) `[NUR-SNIPPET]` (Inhalt), Konfidenz hoch

- **Original:** Ke Xu, Guangyan Zhou, „SAT requires exhaustive search",
  *Frontiers of Computer Science* **19(12)**, Artikelnr. **1912405**, Dez. 2025,
  DOI `10.1007/s11704-025-50231-4` (Springer Nature / Higher Education Press).
  Preprint: **arXiv:2302.09512** (Feb. 2023, mindestens **neun** Versionen, v9).
- **Behauptung:** Für **Model RB** (ein random CSP-Modell von Xu/Li) sei keine
  Lösung ohne exhaustive search möglich; daraus folge, dass SAT für jede Konstante
  δ ∈ (0,1) mindestens 2^(δn) Zeit braucht — also **SETH**, und daraus **P ≠ NP**.
- **Pressearbeit:** EurekAlert-Pressemitteilung vom **17.07.2025**
  („computer scientists have mathematically proven … there simply is no clever
  shortcut"). Beworben mit „mehr als 30 Experten" mit „highly positive feedback",
  darunter Gregory Chaitin mit „truly revolutionary".

### 4.2 Der entscheidende Punkt: lange Klauseln, nicht 3-SAT — **BESTÄTIGT**

`[PREPRINT]` `[NUR-SNIPPET]`, Konfidenz hoch (zwei unabhängige Suchen)

Die Autoren räumen in **arXiv:2401.01193** („Further Explanations …",
Dong/Zhou/Xu, 02.01.2024) **selbst ein**, dass ihr Resultat **nicht** für k-SAT
mit **konstanter** Klausellänge gilt, also insbesondere **nicht für 3-SAT**.
Ihr Gegenstand ist „long clause SAT" — Instanzen, deren Klausellänge mit n wächst
(was für die Model-RB-nach-SAT-Übersetzung strukturell erzwungen ist).
Snippet-Zitat aus dem Original-Abstract, von ihnen selbst hervorgehoben:
„proving lower bounds for many problems, such as 3-SAT, can be challenging because
these problems have various effective strategies available for avoiding
exhaustive search."

**`[EIGENE EINSCHÄTZUNG]`, Konfidenz hoch — das ist der Kern des Falls:**
Der **Titel** sagt „SAT requires exhaustive search". Der **Inhalt** sagt
„eine bestimmte Familie von Instanzen mit wachsender Klausellänge, hergeleitet aus
Model RB, unter einer Strukturannahme über Algorithmen". Die Distanz zwischen
Titel und Inhalt ist die eigentliche Geschichte. Wer nur den Titel liest — und
Pressemitteilungen, Zitationsdatenbanken und LLM-Trainingskorpora lesen
überwiegend Titel und Abstract — bekommt eine P-≠-NP-Lösung präsentiert.

### 4.3 Der Tenor der Prüfungen

- **Chavrimootoo et al. (Dez. 2023, `[PREPRINT]`):** Fehler in den Haupttheoremen,
  die vorausgesetzte Struktur für downward self-reducibility existiere nicht
  notwendig; weder SETH noch P ≠ NP bewiesen.
- **Allender/Williams (FCS 20:2001405, 2026, `[VERIFIZIERT]`):** „far short of a
  proof"; unwarranted assumption über **alle möglichen** SAT-Algorithmen.
- **Allender-Gastbeitrag im Fortnow/Gasarch-Blog (04.08.2025, `[NUR-SNIPPET]`):**
  Ausgelöst durch „an (incorrect) P ≠ NP proof recently published in Springer
  Nature's Frontiers of Computer Science". Allender berichtet aus seiner Zeit als
  Editor-in-Chief von *ACM ToCT*, dass er P-vs-NP-Einreichungen regelmäßig **selbst**
  begutachten musste, weil sich keine Gutachter finden lassen.
- **`[NUR-SNIPPET]`, Konfidenz mittel, aber gravierend — Interessenkonflikt:**
  Mehrere Snippets halten fest, dass **einer der Autoren Deputy Editor-in-Chief
  genau der Zeitschrift ist**, die den Artikel publizierte. Ich konnte diesen Punkt
  nicht am Impressum verifizieren (kein Volltextzugang) und kennzeichne ihn als
  starkes, aber unbestätigtes Indiz.
- **`[NUR-SNIPPET]`, Konfidenz mittel, ebenfalls gravierend — Zitatsmissbrauch:**
  Gregory Chaitin habe erklärt, er habe **das Paper nicht gelesen** und sei **aus
  dem Kontext zitiert** worden. Kolportiert wird, er habe sinngemäß gesagt,
  das Resultat wäre revolutionär *wenn es stimmt*, man solle den Beweis daher
  *besonders sorgfältig* prüfen — woraus in der Werbung „truly revolutionary" wurde.
  Zwei Suchen, konsistent; ich habe keine Primäraussage Chaitins.

**NICHT VERIFIZIERT (Negativbefund):** Der Auftrag nahm an, die Zeitschrift habe
**mehrere** Expertenkommentare publiziert. Ich konnte **genau einen** publizierten
Kommentar finden (Allender/Williams). Die „30 Experten" sind Werbematerial der
Autorenseite, **keine** begutachteten Kommentare. Ein Snippet erwähnt, dass die
Autoren um Verlinkung **ihrer Erwiderung** auf Allender/Williams gebeten hätten —
eine publizierte Reply konnte ich nicht auffinden.

### 4.4 Urteil nach den Muss-Kriterien M1–M5

| Kriterium | Befund | Bewertung |
|---|---|---|
| **M1 Barrierenrechenschaft** (Relativization / Natural Proofs / Algebrization) | In keinem Snippet, in keinem Abstract, in keiner Pressemitteilung, in keiner der Verteidigungsschriften findet sich eine Auseinandersetzung mit den drei Barrieren. Diskutiert wird stattdessen ein „white-box diagonalization method". Diagonalisierung ist **genau** die Technik, die von Baker–Gill–Solovay relativiert. Wer Diagonalisierung als Innovation anbietet, muss zeigen, warum sie hier nicht relativiert. **Das fehlt.** | **NICHT ERFÜLLT** |
| **M2 Übergeneralisierungs-Test** | Formal *besteht* der Claim diesen Test — aber nur, weil er sich auf long-clause-Instanzen zurückzieht. Genau dieser Rückzug (M2-Immunität durch Verengung des Gegenstands) macht ihn zugleich für P vs. NP wertlos: die NP-Vollständigkeit von 3-SAT ist der kanonische Weg, und den decken die Autoren erklärtermaßen nicht ab. | **FORMAL BESTANDEN, INHALTLICH ENTWERTET** |
| **M3 Keine Strukturannahme über Algorithmen** | **Die Kernkritik beider unabhängiger Prüfungen ist exakt M3.** Angenommen wird, ein Algorithmus für Model RB müsse eine bestimmte, downward self-reducibility ausnutzende Struktur haben. Das ist eine Quantifizierung über eine *eingeschränkte* Algorithmenklasse, verkauft als Quantifizierung über alle. | **KLAR VERLETZT** |
| **M4 Lean ≠ Wahrheit** | Nicht einschlägig — keine Formalisierung vorgelegt. | **n/a** |
| **M5 Keine Axiomschmuggelware** | Die Strukturannahme aus M3 wird als natürliche Eigenschaft von SAT-Algorithmen präsentiert statt als zusätzliche Voraussetzung. Zusätzlich: die Verengung auf long clauses wird im Titel und in der Pressemitteilung **nicht** mitgeführt. | **VERLETZT** |

**Gesamturteil A8, `[EIGENE EINSCHÄTZUNG]`, Konfidenz hoch:**
Der Claim ist **nicht anerkannt und mit hoher Wahrscheinlichkeit falsch**.
M1, M3 und M5 verletzt. Er darf in unserem Papier **ausschließlich als Fallstudie
über Publikationsversagen** erscheinen, nie als Fortschrittsbefund.
Die peer-reviewte Venue ändert daran nichts — sie ist im Gegenteil der interessante
Teil des Befunds.

---

## 5. Bonus-Fallstudie: Blum 2017 — die kürzeste Widerlegung

`[VERIFIZIERT]` `[NUR-SNIPPET]`, Konfidenz hoch.
Aufgenommen, weil sie den schnellsten dokumentierten Zyklus zeigt.

- **11.08.2017:** Norbert Blum (Bonn, etablierter Komplexitätstheoretiker)
  stellt „A Solution of the P versus NP Problem" auf arXiv (1708.03486).
- Kern: Theorem 6 — wenn ein monotones Problem polynomiell große monotone
  Schaltkreise mit einem CNF-DNF-Approximator im Sinne von **Berg–Ulfberg** zulässt,
  dann auch allgemeine Schaltkreise ⇒ superpolynomielle allgemeine Schaltkreiskomplexität.
- **14.–15.08.2017:** Luca Trevisan und John Baez bloggen; binnen Tagen wird ein
  **bekanntes Gegenbeispiel von Tardos** ins Feld geführt, das genau diesen
  Schritt zerstört (Tardos' Funktion hat exponentielle monotone, aber polynomielle
  allgemeine Komplexität).
- **30.08.2017:** Blum zieht zurück, Kommentar: **„The proof is wrong."**
- **2020:** überarbeitete Fassung unter neuem, viel bescheidenerem Titel
  („On the Approximation Method and the P versus NP Problem"), ohne den Lösungsanspruch.

**Lehre (`[EIGENE EINSCHÄTZUNG]`, hoch):** 19 Tage von Claim bis Rückzug durch den
Autor selbst. Der Unterschied zu §4: Blum war im Feld, kannte die Norm, und zog
zurück. Das Verfahren funktioniert — wenn der Autor mitspielt. Die Fehlerart ist
außerdem lehrreich: der Sprung von **monotoner** zu **allgemeiner**
Schaltkreiskomplexität ist die bekannteste Falle des Gebiets, und Tardos'
Gegenbeispiel ist seit 1988 publiziert. **Ein Literaturtest hätte genügt.**

---

## 6. KI-verstärkte Crank-Literatur

### 6.1 Der Mengenbefund — quantifiziert

`[VERIFIZIERT]` `[NUR-SNIPPET]`, Konfidenz hoch

Härteste Zahlen, die ich finden konnte, betreffen arXiv insgesamt (nicht nur P-vs-NP):

- **Ablehnungsquote arXiv:** jahrelang ~4 %, **jetzt ~10–12 %**. Ursache: seit
  Anfang 2025 exponentiell steigende „AI slop"-Einreichungen, beginnend in
  Computer Science, dann in andere Felder ausgreifend.
- **Fabrizierte Referenzen, zwölffacher Anstieg seit 2023:**
  2023 ≈ 1 Paper von 2828 mit ≥1 erfundener Referenz → 2025 ≈ 1 von 458 →
  **erste sieben Wochen 2026 ≈ 1 von 277.**
- **Maßnahme:** arXiv verhängt **einjährige Sperren** für Autoren, die
  offensichtlich AI-generierte Arbeiten ohne Prüfung einreichen.
- **Täterprofil:** „almost all … comes from first-time submitters", überwiegend
  sehr junge Forschende mit Publikationsdruck.

**Für P vs. NP speziell (`[NUR-SNIPPET]`, mittel):** Gasarch, „P v NP Papers Galore"
(Blog, 30.04.2025): normalerweise **ca. eine** Einreichung pro Monat an ihn
persönlich, in einem Zeitraum von **zwei Wochen aber sieben**. Kanäle: E-Mail mit
PDF, DMs auf X, Telefonanrufe, LinkedIn-Nachrichten auf Spanisch, in einem Fall
41 Folge-E-Mails. Das ist anekdotisch, aber es ist die einzige Zeitreihe, die
wir für dieses Teilfeld überhaupt haben — **die systematische Erfassung endete 2016
(§2).**

### 6.2 Die Belege im Einzelnen

- **Zenodo, „AI Principle Series and P≠NP Structural Proof — Preprint Collection
  (2025 Edition)"** — `[CLAIM]`, Konfidenz mittel, `zenodo.org/records/18107880`.
  Bestätigt existent. Selbstbeschreibung laut Snippet: zeigt eine „structural
  inconsistency" im P-vs-NP-Problem auf, **beansprucht ausdrücklich keine Lösung**,
  sondern „a new perspective for future discussion". Das Manuskript sei **von einer
  KI geschrieben**; der Text formuliert das selbst als Paradox (wenn die Aussage
  wahr ist, ist das „AI Principle" falsch; ist es richtig, kann keine KI das
  geschrieben haben). `[EIGENE EINSCHÄTZUNG]`: Das ist keine Beweisbehauptung,
  sondern eine performative Textsorte — auffällig ist, dass Zenodo als
  Publikationskanal keinerlei Moderation ausübt und diese Texte trotzdem DOIs,
  Zitierbarkeit und Indexierung erhalten. Zenodo enthält zusätzlich Einträge wie
  „P = NP THE COMPLETE PROOF: Resolving the Clay Millennium Prize via Interface
  Geometry" (Feb. 2026). **Zenodo ist der neue Hauptkanal, nicht arXiv** — weil
  arXiv moderiert und Zenodo nicht.
- **„Khanukov 2026"** — `[CLAIM]`, Konfidenz mittel. Dmitry Khanukov, GitHub-Repo,
  beansprucht eine vollständige Formalisierung von P ≠ NP: alle Hauptpublikationen
  analysiert und in Code übersetzt, Gesamt-Beweisstruktur plus „a small number of
  identified gaps". Methodenanspruch: **Mensch + „specialized LLM orchestra" +
  formale Verifikation in Lean**, nach eigener Aussage erstmalig.
  Er räumt ein, dass einzelne LLMs keine echte Selbstkritik/Kreativität haben,
  argumentiert aber, dass durch menschlich orchestriertes Feedback zwischen
  mehreren Modellen „something larger" entstehe.
  **Reaktion:** Fortnow/Gasarch, „Respect the P v NP Problem" (Blog, Juni 2026),
  adressiert Khanukov direkt und rät vom Lean-Weg ab: Komplexitätstheorie sei
  technisch sehr unsauber zu formulieren, und man bekomme von KI nicht einmal
  einen vollständigen Lean-Beweis für etwas so Triviales wie „P ist unter
  Komplement abgeschlossen". Wenn ein P-≠-NP-Beweis komme, werde er einem
  intuitiven, nicht einem formalistischen Zugang folgen.
  **`[EIGENE EINSCHÄTZUNG]`, hoch:** „a small number of identified gaps" ist
  Musterbeispiel eines Kategoriefehlers — in einem Beweis gibt es keine kleine
  Zahl von Lücken, es gibt einen Beweis oder keinen. Deckt sich exakt mit M4.
- **Fortnow/Gasarch, „AI and Research Papers" (Blog, Januar 2026)** —
  `[NUR-SNIPPET]`, mittel. Rahmung: 2026 als Jahr der KI-Disruption der gesamten
  Academia. Einschätzung der Autoren: fürs **Schreiben** richte KI mehr Schaden als
  Nutzen an, weil sie Dinge erfinde und Zitate fabriziere; nützlich sei sie für
  Grammatik und LaTeX-Tabellen. Das deckt sich mit den arXiv-Zahlen in §6.1.

### 6.3 Der GPT-5/Erdős-Vorfall (Oktober 2025) — Hergang verifiziert

`[VERIFIZIERT]` `[NUR-SNIPPET]`, Konfidenz hoch für den Hergang,
**niedrig für die Zahl „17 Stunden"**

| Datum 2025 | Ereignis |
|---|---|
| 12. Okt. | Sébastien Bubeck (OpenAI) postet auf X: „GPT-5-pro is superhuman at literature search. It just ‚solved' Erdős problem No. 339 … by realizing it had been solved 20 years ago." — **Man beachte: „solved" steht bei ihm bereits in Anführungszeichen, und „literature search" steht explizit da.** |
| 18. Okt. | Mark Sellke (OpenAI) meldet, man habe nach tausenden Anfragen an GPT-5 zu **10** als „unsolved" gelisteten Erdős-Problemen Antworten gefunden. |
| 18. Okt. | Kevin Weil (VP of Science, OpenAI) verstärkt ohne die Einschränkungen: „GPT-5 just found solutions to 10 (!) previously unsolved Erdős problems, and made progress on 11 others." |
| 18./19. Okt. | Thomas Bloom (Betreiber erdosproblems.com) widerspricht öffentlich: Weils Post sei „a dramatic misrepresentation"; „open" auf seiner Website heiße nur „I personally am unaware of a paper which solves it". Demis Hassabis nennt den Vorgang „embarrassing". Weils Post wird gelöscht; Bubeck rudert zurück und löscht den Ursprungspost. |
| 19. Okt. | TechCrunch: „OpenAI's embarrassing math". Gary Marcus: „Erdosgate". |

**Was tatsächlich geschah:** GPT-5 hat existierende Literatur **wiedergefunden**,
nicht neu bewiesen. Die Hypothese des Auftrags ist damit **inhaltlich bestätigt**.

**Was ich NICHT verifizieren konnte:** die Angabe „**Rücknahme nach ca. 17 Stunden**".
Die Quellen sagen „within hours" bzw. „the next day". Der Vorfall vom 18. auf den
19. Oktober ist mit ~17 Stunden **verträglich**, aber ich habe keine Quelle, die
diese Zahl nennt. **Empfehlung: im Papier „binnen weniger als einem Tag" schreiben,
nicht „17 Stunden".**

**`[EIGENE EINSCHÄTZUNG]`, hoch — warum dieser Fall für uns zentral ist:**
Der Fehler entstand nicht im Modell, sondern in der **Übersetzungskette**:
Bubeck sagte korrekt „literature search"; Sellke sagte „in unsolved state gelistet";
Weil machte daraus „previously unsolved … solutions". Jeder Schritt entfernte eine
Einschränkung. Das ist **strukturgleich** mit §4: „long-clause-Instanzen von Model RB
unter einer Strukturannahme" → Titel „SAT requires exhaustive search" →
Pressemitteilung „no clever shortcut". **Die KI ist in beiden Fällen nicht die
Fehlerquelle; die Fehlerquelle ist der Verlust von Quantoren und Vorbedingungen
beim Weiterreichen.** Für unser Papier ist das die wichtigere Lehre als jede
Aussage über Modellfähigkeiten.

---

## 7. Taxonomie der Fehlermuster

Geordnet nach Häufigkeit/Gefährlichkeit. Jedes Muster mit **Beispiel** und dem
**billigsten aufdeckenden Prüftest**.

### F1 — Übergeneralisierung („das Argument beweist zu viel")
- **Beschreibung:** Das Argument nutzt keine Eigenschaft, die NP-vollständige von
  polynomiell lösbaren Problemen unterscheidet. Es liefert daher auch für Probleme
  in P das Resultat „nicht in P".
- **Beispiel:** Deolalikar 2010 — dasselbe Argument mit k-XOR-SAT statt k-SAT
  liefert „k-XOR-SAT ∉ P", falsch (Gauß über GF(2)).
- **Prüftest (Aaronson-Test, der wirksamste überhaupt):**
  *„Erkläre in zwei Sätzen, warum dein Beweis bei 2-SAT, XOR-SAT und Horn-SAT
  scheitert."* Wer das nicht sofort kann, hat keinen Beweis. Aaronsons Liste
  („Eight Signs A Claimed P≠NP Proof Is Wrong", Aug. 2010) führt genau das als
  Zeichen 1 und nennt es „historically … the single most important sanity check".
- **Variante F1b:** Der Beweis „kennt" die bekannten Algorithmentechniken nicht —
  dynamische Programmierung, lineare und semidefinite Programmierung, holographische
  Algorithmen (Aaronsons Zeichen 2).

### F2 — Implizite Beschränkung der Algorithmenklasse (**M3**)
- **Beschreibung:** Es wird behauptet, über *alle* Algorithmen zu quantifizieren,
  tatsächlich aber nur über solche mit einer unterstellten Struktur.
  **Das häufigste Muster in ernstzunehmenden Versuchen.**
- **Beispiel:** Xu/Zhou — Annahme, ein Algorithmus für Model RB müsse downward
  self-reducibility ausnutzen (Chavrimootoo et al. 2023; Allender/Williams 2026:
  „an assumption about all possible SAT algorithms that is unwarranted").
- **Prüftest:** *Markiere im Manuskript jede Stelle, an der über einen Algorithmus
  gesprochen wird. Frage bei jeder: „Warum muss ein Algorithmus so aussehen?"
  Wenn die Antwort „weil man es sonst nicht sinnvoll machen kann" lautet, ist es
  eine unbewiesene Strukturannahme.* Zweiter Test: Lässt sich ein künstlicher,
  absurder, aber korrekter Algorithmus angeben, der die Annahme verletzt?

### F3 — Titel-Inhalt-Divergenz bei **peer-reviewter** Publikation (das geforderte Zusatzmuster)
- **Beschreibung:** Ein begutachteter Titel behauptet mehr, als der Inhalt trägt.
  Der Reviewprozess prüft (bestenfalls) den Inhalt, **nicht** die Angemessenheit
  des Titels, des Abstracts und der Pressemitteilung. Zitationszählungen,
  Datenbanken, Suchmaschinen und LLM-Trainingskorpora propagieren jedoch **Titel**.
  Der Peer-Review-Stempel wandert dabei vom Inhalt auf die Überschrift.
  **Verschärfend:** Verlust der Einschränkungen entlang der Weitergabekette
  (Abstract → Titel → Pressemitteilung → Sekundärberichterstattung).
- **Beispiel A:** „SAT requires exhaustive search" (FCS 19(12), 2025) — gilt
  erklärtermaßen **nicht** für 3-SAT/k-SAT mit konstanter Klausellänge, sondern nur
  für long-clause-Instanzen aus Model RB, und auch dort nur unter einer
  Strukturannahme (F2).
- **Beispiel B:** GPT-5/Erdős — „superhuman at literature search" (korrekt) →
  „found solutions to 10 previously unsolved problems" (falsch), in sechs Tagen,
  über drei Personen.
- **Prüftest:** *Schreibe die stärkste Aussage auf, die der Beweis tatsächlich
  stützt — mit allen Quantoren und Vorbedingungen. Lege sie neben den Titel.
  Prüfe getrennt: (a) folgt der Titel aus den Theoremen? (b) welche Vorbedingung
  fehlt in der Pressemitteilung, die im Theorem steht?* Zusätzlich:
  **Prüfe auf Interessenkonflikt in der Redaktion** — im FCS-Fall soll einer der
  Autoren Deputy Editor-in-Chief der publizierenden Zeitschrift sein
  (`[NUR-SNIPPET]`, nicht am Impressum verifiziert).

### F4 — Zirkularität / Selbstbestätigung
- **Beschreibung:** Die „Bestätigung" eines Resultats stammt aus derselben
  Personen- oder Textquelle wie das Resultat.
- **Beispiel:** arXiv:2309.05689 — GPT-4 schließt „P ≠ NP", „which is in alignment
  with (Xu and Zhou, 2023)"; **Xu und Zhou sind Koautoren des LLM-Papers**.
  Sonderform bei LLMs: Das Modell reproduziert, was in seinen Trainingsdaten
  oder im Prompt-Kontext steht, und die Autoren lesen das als unabhängige Bestätigung.
- **Prüftest:** *Schnittmenge der Autorenlisten von Claim und Bestätigung bilden.
  Ist sie nichtleer, ist es keine Bestätigung.* Bei LLM-Ergebnissen zusätzlich:
  *War die „bestätigte" Aussage im Prompt, im Kontextfenster oder in den
  Trainingsdaten? Wenn ja, ist der Output Wiedergabe, nicht Herleitung.*
  (Das ist exakt der Erdős-Fall: Wiederfinden ≠ Beweisen.)

### F5 — Ignorieren der Barrieren (**M1**)
- **Beschreibung:** Der Versuch nutzt eine Technik, die nachweislich nicht
  ausreicht — Diagonalisierung (relativiert, Baker–Gill–Solovay 1975),
  „natürliche" Schaltkreiseigenschaften (Razborov–Rudich 1994/97),
  algebrisierende Argumente (Aaronson–Wigderson 2008) — ohne zu erklären, warum
  die jeweilige Barriere hier nicht greift.
- **Beispiel:** Xu/Zhou bewerben „white-box diagonalization" als Innovation, ohne
  Relativierungsrechenschaft (Snippet-Befund; kein Barrieren-Abschnitt auffindbar).
- **Prüftest:** *Suche im Manuskript nach den drei Begriffen. Kommt keiner vor,
  ist der Versuch nicht auf Höhe des Feldes — unabhängig von seinem Inhalt.*
  Bei Treffern: Steht dort ein Argument oder nur eine Behauptung der Umgehung?

### F6 — Monoton → allgemein (und verwandte Klassensprünge)
- **Beschreibung:** Eine untere Schranke wird für ein eingeschränktes Berechnungsmodell
  bewiesen (monotone Schaltkreise, beschränkte Tiefe, beschränkter Speicher,
  Entscheidungsbäume, DPLL/Resolution) und dann auf das allgemeine Modell übertragen.
- **Beispiel:** Blum 2017 — Berg–Ulfberg-Approximator von monotonen auf allgemeine
  Schaltkreise übertragen; **Tardos' Gegenbeispiel** zerstört genau diesen Schritt.
- **Prüftest:** *Benenne exakt das Berechnungsmodell jeder unteren Schranke.
  Existiert ein publiziertes Beispiel, das monotone und allgemeine Komplexität
  exponentiell trennt? (Ja — Tardos.) Überlebt der Übertragungsschritt dieses Beispiel?*
  Sonderfall für SAT-Solver-Argumente: untere Schranken für **Resolution** sind
  bewiesen und implizieren **nichts** über P vs. NP.

### F7 — Pseudopolynomiell verwechselt mit polynomiell
- **Beschreibung:** Ein Algorithmus mit Laufzeit polynomiell im **Zahlenwert** der
  Eingabe gilt als Polynomialzeit-Algorithmus. Er ist es nicht, weil die Zahl W
  in der **Eingabelänge** log W exponentiell ist.
- **Beispiel:** Knapsack-DP mit O(nW) als „P = NP"-Beleg — der Klassiker unter den
  P=NP-Claims, konsistent mit dem Befund aus §1/H4, dass die Mehrheit der
  Crank-Claims P=NP behauptet (ein vorzeigbarer Algorithmus wirkt attraktiver
  als eine untere Schranke).
- **Prüftest:** *Setze W = 2^n. Bleibt die Laufzeit polynomiell in der Bitlänge
  der Eingabe?* Ergänzend: Ist das behandelte Problem **stark** NP-vollständig?
  Dann kann es (außer P = NP) gar keinen pseudopolynomiellen Algorithmus geben.

### F8 — Durchschnitts- statt Worst-Case
- **Beschreibung:** Eine Aussage über zufällige Instanzen (Random k-SAT bei
  bestimmter Klauseldichte, Model RB an der Phasenübergangsschwelle) wird als
  Aussage über das Problem gelesen. P vs. NP ist eine **Worst-Case**-Frage.
  Die Richtung ist asymmetrisch: Average-Case-**Härte** impliziert nicht
  Worst-Case-Härte in der für P≠NP nötigen Form, und Average-Case-**Leichtigkeit**
  widerlegt nichts.
- **Beispiel:** Deolalikar (Random-k-SAT-Clusterstruktur) und Xu/Zhou
  (Model RB als random CSP) hängen beide an Average-Case-Strukturaussagen.
- **Prüftest:** *Welche Instanzverteilung liegt zugrunde? Ist das Resultat
  „für zufällige Instanzen" oder „für alle Instanzen"? Und: Welche Aussage bleibt
  übrig, wenn ein Gegner die Instanz wählen darf?*

### F9 — „Kein bekannter Algorithmus" ≠ „kein Algorithmus"
- **Beschreibung:** Aus dem Scheitern aller bisher versuchten Verfahren (oder aller
  Verfahren, die dem Autor einfallen) wird auf Nichtexistenz geschlossen.
  Epistemisch: Unwissen wird als Beweis verkauft.
- **Beispiel:** Die Rhetorik „SAT requires exhaustive search" trägt dieses Muster
  im Titel; sie ist zugleich die alltagssprachliche Form von F2.
  Der Erdős-Fall ist die **Umkehrung** desselben Fehlers: Bloom sagt explizit,
  „open" auf seiner Seite heiße nur „I personally am unaware of a paper which
  solves it" — und OpenAI-Mitarbeiter lasen es als „ungelöst".
- **Prüftest:** *Ersetze im Manuskript jedes „es gibt keinen Algorithmus, der …"
  durch „ich kenne keinen Algorithmus, der …". Bricht das Argument zusammen?*

### F10 — Formalisierung als Wahrheitsbeweis (**M4**)
- **Beschreibung:** Lean/Coq/Isabelle verifizieren die **Ableitung aus den
  angegebenen Voraussetzungen**, nicht die Angemessenheit der Voraussetzungen,
  nicht die Übersetzung des informellen Problems in die formale Aussage, und
  schon gar nicht die Vollständigkeit gegenüber nicht formalisierten Lücken.
- **Beispiel:** „Khanukov 2026" — Lean-Verifikation plus „a small number of
  identified gaps". Fortnow/Gasarch (Juni 2026) halten dagegen, dass man KI nicht
  einmal zu einem vollständigen Lean-Beweis von „P ist unter Komplement
  abgeschlossen" bringt.
- **Prüftest:** *Welche Aussage genau wurde formalisiert? Lies das Lean-Statement,
  nicht den Fließtext. Gibt es `sorry`/Axiome/`admit` in der Datei? Wie viele
  Definitionen wurden händisch gesetzt und stimmen sie mit der Standarddefinition
  überein?*

### F11 — Endorsement-Wäsche
- **Beschreibung:** Zustimmung prominenter Personen wird eingeworben oder
  zitatverkürzt, um Prüfung zu ersetzen. Charakteristisch ist die Umwandlung
  eines konditionalen Lobs („wäre revolutionär, **wenn** korrekt — prüft es
  sorgfältig") in ein unkonditionales („truly revolutionary").
- **Beispiel:** Xu/Zhou-Pressearbeit — „mehr als 30 Experten", Chaitin
  „truly revolutionary"; Chaitin habe erklärt, das Paper **nicht gelesen** und
  **aus dem Kontext zitiert** worden zu sein (`[NUR-SNIPPET]`, Konfidenz mittel).
- **Prüftest:** *Für jedes Lob: Liegt der volle Wortlaut vor? Enthält er ein
  „if"/„wenn"? Hat die Person das Manuskript nachweislich gelesen?
  Ist das Lob unabhängig auffindbar oder nur in Materialien der Autorenseite?*

### F12 — Gap-Minimierung („nur noch ein paar Lücken")
- **Beschreibung:** Ein unvollständiger Beweis wird als fast fertig dargestellt.
  Bei P vs. NP ist die Restlücke aber typischerweise **das ganze Problem**.
- **Beispiel:** Khanukovs „a small number of identified gaps"; Deolalikars
  Verweise auf spätere Fassungen bei der XOR-SAT-Frage.
- **Prüftest:** *Nimm die Lücke als Annahme heraus und formuliere sie als eigenes
  Theorem. Ist dieses Theorem leichter oder schwerer als P ≠ NP?*

---

## 8. Prüfprotokoll für unser eigenes Papier

Für die **Leitung**, anzuwenden auf **jeden** Claim vor Aufnahme.
Reihenfolge ist bewusst nach Kosten sortiert: die billigen Tests zuerst,
weil sie erfahrungsgemäß die meisten Claims erledigen.

**Stufe 0 — Existenz und Identität (Minuten)**
- [ ] **0.1** Existiert die Arbeit? Venue, Jahr, DOI/arXiv-ID, vollständige
  Autorenliste **notiert**. Keine Quelle ohne Jahr und Venue.
- [ ] **0.2** Status korrekt vergeben: `[VERIFIZIERT]` / `[PREPRINT]` / `[CLAIM]` /
  `[EIGENE EINSCHÄTZUNG]`.
- [ ] **0.3** Ist der Befund **volltextgeprüft** oder Snippet? Wenn Snippet
  (= Normalfall): `[NUR-SNIPPET]` setzen. **Keine wörtlichen Zitate ohne
  Volltextbeleg** — nur indirekte Rede mit Quellenangabe.
- [ ] **0.4** ≥2 unterschiedlich formulierte Suchen durchgeführt? Widersprechen
  sie sich? → **Beide Zahlen berichten**, Widerspruch kennzeichnen (Vorbild:
  Woeginger 61 vs. 62, §1/H4).

**Stufe 1 — Zuständigkeit und Unabhängigkeit (Minuten)**
- [ ] **1.1** Wer hat es geprüft? Namentlich. Aus welcher Gruppe?
- [ ] **1.2** **Autorenlisten-Schnittmenge** zwischen Claim und Bestätigung
  gebildet? Nichtleer ⇒ keine Bestätigung (F4).
- [ ] **1.3** Bei prominenten Zustimmungen: voller Wortlaut? konditional?
  gelesen? unabhängig auffindbar? (F11)
- [ ] **1.4** Interessenkonflikte bei der publizierenden Instanz geprüft
  (Autor in der Redaktion? Sonderheft? Gastherausgeberschaft?) (F3)

**Stufe 2 — Aussage vs. Titel (Minuten, höchste Trefferquote)**
- [ ] **2.1** **Stärkste tatsächlich gestützte Aussage ausformuliert**, mit
  *allen* Quantoren und Vorbedingungen — und neben den Titel gelegt (F3).
- [ ] **2.2** Für welche **Instanzklasse** gilt sie? Konstante Klausellänge oder
  wachsende? 3-SAT abgedeckt oder nicht? (F3, Xu/Zhou-Test)
- [ ] **2.3** **Worst-Case oder Average-Case?** Welche Verteilung? (F8)
- [ ] **2.4** **Berechnungsmodell** benannt? Allgemein oder eingeschränkt
  (monoton / beschränkte Tiefe / Resolution / Entscheidungsbaum)? (F6)
- [ ] **2.5** Bei Algorithmen-Claims: **W = 2^n einsetzen.** Noch polynomiell in
  der Bitlänge? Ist das Problem stark NP-vollständig? (F7)

**Stufe 3 — Die Muss-Kriterien M1–M5 (Stunden)**
- [ ] **3.1 (M2/F1) Übergeneralisierungs-Test:** Warum scheitert das Argument bei
  **2-SAT, XOR-SAT, Horn-SAT**? Muss in zwei Sätzen beantwortbar sein.
  **Wenn nicht beantwortbar: Claim wird nicht aufgenommen.**
- [ ] **3.2 (M3/F2) Strukturannahmen-Scan:** Jede Stelle markiert, an der über
  „einen Algorithmus" gesprochen wird; bei jeder gefragt, warum er so aussehen muss.
- [ ] **3.3 (M1/F5) Barrieren-Scan:** Relativization, Natural Proofs, Algebrization
  im Text vorhanden? Argument oder bloße Behauptung?
- [ ] **3.4 (M4/F10) Formalisierung:** Lean-Statement selbst gelesen?
  `sorry`/Axiome/`admit`? Definitionen standardkonform?
- [ ] **3.5 (M5) Axiomschmuggel:** Welche Zusatzannahme wird als harmlos
  präsentiert? Als eigenes Theorem formuliert: leichter oder schwerer als
  P ≠ NP? (F12)
- [ ] **3.6 (F9) Quantorentest:** „es gibt keinen Algorithmus" → „ich kenne keinen
  Algorithmus" ersetzen. Bricht das Argument?

**Stufe 4 — Formulierung im Papier**
- [ ] **4.1** Keine Behauptung über **Inhalt** einer Arbeit ohne Volltextbeleg —
  stattdessen: „wird referiert als", „laut Abstract", „nach Angabe der Autoren".
- [ ] **4.2** Konfidenz (hoch/mittel/niedrig) **explizit** an jeder Bewertung.
- [ ] **4.3** Negativbefunde **als Befund** ausweisen, nicht weglassen
  (z. B. §2: keine Nachfolgeliste nach 2016).
- [ ] **4.4** Keine Zahl schärfer als die Quelle: „binnen weniger als einem Tag",
  nicht „17 Stunden" (§6.3).
- [ ] **4.5** Bei KI-Befunden: **suchbarer endlicher Zeuge oder unendliche
  Quantifizierung?** (Leitachse, §9) Explizit einordnen.
- [ ] **4.6** **Sanity-Check am Schluss:** Enthält unser Papier irgendwo einen
  Satz, den ein Außenstehender als „wir haben eine Lösung/einen Durchbruch"
  lesen könnte? Dann umformulieren. Wir sind gegen F3 nicht immun.

---

## 9. Antwort auf die Leitachse (§2 des Team-Briefings)

**Mein Befund bestätigt die Leitachse — und zwar aus der Negativseite, was sie
stärker macht als eine weitere Positivbestätigung.** `[EIGENE EINSCHÄTZUNG]`,
Konfidenz hoch.

Alle geprüften Fehlschläge scheitern an **derselben Stelle**: der unendlichen
Quantifizierung über alle Algorithmen.

- **Xu/Zhou (F2/M3):** ersetzen die Quantifizierung über alle Algorithmen durch
  eine über Algorithmen mit downward-self-reducibility-Struktur. Zwei unabhängige
  Prüfteams identifizieren exakt diesen Schritt.
- **Deolalikar (F1):** sein Apparat unterscheidet nicht zwischen SAT und XOR-SAT —
  er quantifiziert über etwas, das die relevante Grenze nicht sieht.
- **Blum (F6):** ersetzt „alle Schaltkreise" durch „monotone Schaltkreise" plus
  einen Übertragungsschritt, den Tardos' Gegenbeispiel zerstört.
- **Khanukov (F10/F12):** ersetzt den Beweis durch eine Formalisierung mit Lücken —
  Lean quantifiziert perfekt über die *angegebenen* Voraussetzungen und sagt nichts
  über deren Angemessenheit.
- **GPT-5/Erdős (F4/F9):** das Modell war **exzellent** bei genau der Aufgabe, die
  ein endliches, prüfbares Erfolgskriterium hat (Literaturwiederfindung — Bubecks
  ursprüngliche, korrekte Aussage), und die Fehlmeldung entstand erst beim
  Umdeuten in „neu bewiesen".

Der letzte Punkt ist die eigentliche Pointe: **AlphaEvolve findet Gadgets
(endliche Zeugen, maschinell prüfbar) — GPT-5 findet Literatur (endlicher Zeuge,
maschinell prüfbar). Beides funktioniert. Sobald der Anspruch auf „für alle
Algorithmen gilt …" wechselt, fehlt das Verifikationsorakel, und es entsteht kein
Fortschritt, sondern Werbetext.** Die Fehlmeldungen entstehen systematisch an der
Nahtstelle zwischen beiden Regimen, beim Weiterreichen der Aussage.

**Kein Gegenbefund.** Ich habe in diesem Audit **keinen** Fall gefunden, in dem
KI-Methoden substantiellen Fortschritt an der unendlichen Quantifizierung erzielt
hätten. Der Negativbefund ist hier das Ergebnis.

---

## 10. VETO-LISTE

Folgende Aussagen dürfen **nicht** oder **nur in dieser eingeschränkten Form**
ins Abschlusspapier. Die Nummern sind zum Zitieren in der Redaktion gedacht.

### V1 — HARTES VETO: „SAT requires exhaustive search" als Fortschrittsbefund
Der FCS-Artikel 19(12):1912405 (2025) darf **nicht** als Beleg für Fortschritt an
P vs. NP, an SETH oder an unteren SAT-Schranken auftauchen. M1, M3 und M5 verletzt;
zwei unabhängige Prüfungen (Chavrimootoo et al. 2023; Allender/Williams 2026)
identifizieren denselben Fehler. **Zulässige Verwendung ausschließlich:** Fallstudie
zu F2/F3 (Strukturannahme + Titel-Inhalt-Divergenz + Publikationsversagen), stets
mit dem Hinweis, dass das Resultat erklärtermaßen nicht für 3-SAT gilt.

### V2 — HARTES VETO: GPT-4 „leitet P ≠ NP her" (arXiv:2309.05689)
Darf **nicht** als KI-Befund zu P vs. NP zitiert werden. Zirkulär (F4): die
„Übereinstimmung mit vorheriger Forschung" ist Übereinstimmung mit einer Arbeit
zweier **Koautoren desselben Papers** (Ke Xu, Guangyan Zhou), die ihrerseits
zweifach als fehlerhaft kritisiert wurde. **Zulässige Verwendung:** Fallstudie zu F4.

### V3 — HARTES VETO: „GPT-5 hat Erdős-Probleme gelöst"
In jeder Form. Verifiziert ist: GPT-5 hat **existierende Literatur wiedergefunden**;
Bubecks ursprüngliche Aussage sagte das korrekt; die Fehlmeldung entstand in der
Weiterreichung und wurde binnen eines Tages zurückgezogen.
**Zulässige Verwendung:** Fallstudie zu F3/F9.

### V4 — HARTES VETO: „Khanukov 2026" als Fortschritt
Keine Verifikation, keine Begutachtung, nach eigener Aussage mit Lücken;
vom Fachblog (Fortnow/Gasarch, Juni 2026) direkt zurückgewiesen. `[CLAIM]`.

### V5 — HARTES VETO: Zenodo-„Beweise"
„AI Principle Series and P≠NP Structural Proof" und verwandte Zenodo-Einträge
(z. B. „P = NP THE COMPLETE PROOF", Feb. 2026) dürfen nur als **Beispiele für
unmoderierte KI-Publikation** erscheinen, nie als Literatur.
Hinweis: Die erstgenannte Sammlung beansprucht **selbst keine Lösung** — wer sie als
Lösungsclaim zitiert, begeht F3 seinerseits.

### V6 — EINSCHRÄNKENDES VETO: Woeginger-Zahlen
„116 seit 1986, Stand Sept. 2016" ist zulässig. Die Aufteilung ist **als strittig zu
kennzeichnen**: 61 oder 62 × „P=NP", 49 (eine Quelle: 50) × „P≠NP", 3–6 × Sonstiges.
**Nicht** glätten, nicht auf eine Zahl reduzieren.
Ebenfalls einschränkend: „alle 116 sind widerlegt" ist **unbelegt** — korrekt ist
„kein Claim hat Anerkennung gefunden; eine publizierte Gesamtwiderlegung existiert
nicht."

### V7 — EINSCHRÄNKENDES VETO: „17 Stunden"
Die Zahl ist nicht belegbar. Zulässig: „binnen weniger als einem Tag" bzw.
„18.→19. Oktober 2025".

### V8 — EINSCHRÄNKENDES VETO: wörtliche Zitate
Jedes wörtliche Zitat in unserem Papier, das aus diesem Bericht stammt, ist
Snippet-basiert. Entweder als indirekte Rede formulieren oder ausdrücklich als
`[NUR-SNIPPET]` kennzeichnen. Betrifft insbesondere: „far short of a proof",
„an assumption about all possible SAT algorithms that is unwarranted",
„truly revolutionary", „The proof is wrong", „a dramatic misrepresentation".

### V9 — EINSCHRÄNKENDES VETO: Interessenkonflikt-Behauptung
Der Punkt „einer der Autoren ist Deputy Editor-in-Chief der publizierenden
Zeitschrift" ist **mehrfach kolportiert, aber von mir nicht am Impressum
verifiziert**. Nur mit explizitem Vorbehalt aufnehmen — oder vor Drucklegung
am Impressum prüfen lassen. Dasselbe gilt für Chaitins Dementi
(„nicht gelesen, aus dem Kontext zitiert").

### V10 — EINSCHRÄNKENDES VETO: „mehrere Expertenkommentare" im FCS
Unbelegt. Ich habe **genau einen** publizierten Kommentar gefunden
(Allender/Williams, FCS 20:2001405, 2026). Die „30 Experten" stammen aus
Werbematerial der Autorenseite und sind keine begutachteten Kommentare.
Eine publizierte Erwiderung der Autoren konnte ich nicht auffinden.

---

## 11. Was ich NICHT verifizieren konnte (Negativbefunde)

1. **Exakte Aufteilung der Woeginger-Liste** (61 vs. 62; 49 vs. 50). Widerspruch
   dokumentiert, nicht auflösbar ohne Volltextzugang.
2. **Nachfolger der Woeginger-Liste** — keiner gefunden. Systematische Erfassung
   endete 2016; für die KI-Ära existiert **keine** Datenbasis.
3. **„17 Stunden"** beim Erdős-Vorfall — nur „within hours"/„the next day" belegt.
4. **„Mehrere Expertenkommentare" im FCS** — nur einer auffindbar.
5. **Publizierte Erwiderung von Xu/Zhou** auf Allender/Williams — Existenz
   angedeutet („authors asked to link to their reply"), nicht auffindbar.
6. **Interessenkonflikt (Deputy Editor-in-Chief)** — mehrfach kolportiert,
   nicht am Impressum verifiziert.
7. **Chaitins Dementi** — kein Primärbeleg, nur Sekundärberichte.
8. **Aaronsons „Eight Signs" vollständig** — nur Zeichen 1 (XOR-SAT/2-SAT-Test)
   und Zeichen 2 (Unkenntnis bekannter Algorithmentechniken) belastbar aus
   Snippets rekonstruierbar; die übrigen sechs nicht.
9. **Exakter Wortlaut** sämtlicher Kernvorwürfe — kein Volltextzugang (WebFetch gesperrt).
10. **P-vs-NP-spezifische Einreichungszahlen** für die KI-Ära — nur
    arXiv-Gesamtzahlen und Gasarchs Anekdotik verfügbar.

---

## 12. Quellen

**Zu H1 / Allender–Williams**
- https://link.springer.com/article/10.1007/s11704-025-53000-5 — E. Allender, R. Williams, „Comment on ‚SAT requires exhaustive search'", *Frontiers of Computer Science* 20:2001405 (2026).
- https://people.cs.rutgers.edu/~allender/papers/allender.williams.pdf — Autorenkopie (Rutgers), unabhängiges Autorschaftsindiz.

**Zu H2 / Chavrimootoo et al.**
- https://arxiv.org/abs/2312.02071 — M. C. Chavrimootoo, Y. He, M. Kotler-Berkowitz, H. Liuson, Z. Nie, „Evaluating the Claims of ‚SAT Requires Exhaustive Search'", arXiv, 04.12.2023.
- https://www.semanticscholar.org/paper/Evaluating-the-Claims-of-%22SAT-Requires-Exhaustive-Chavrimootoo-He/a7e58669e0174753752a99f4660fab9633769200
- https://www.cs.rochester.edu/u/mchavrim/ — Affiliation.

**Zu H3 / LLM-Paper**
- https://arxiv.org/abs/2309.05689 — Q. Dong, L. Dong, K. Xu, G. Zhou, Y. Hao, Z. Sui, F. Wei, „Large Language Model for Science: A Study on P vs. NP", arXiv, 11.09.2023.
- https://huggingface.co/papers/2309.05689 — Community-Diskussion.
- https://arxiv.org/abs/2401.01193 — Q. Dong, G. Zhou, K. Xu, „Further Explanations on ‚SAT Requires Exhaustive Search'", arXiv, 02.01.2024.

**Zu H4 / Woeginger-Liste**
- https://wscor.win.tue.nl/woeginger/P-versus-NP.htm — „The P-versus-NP page", letzte Revision 26.09.2016.
- https://wscor.win.tue.nl/woeginger/P-versus-NP/ — Archiv der eingereichten PDFs.
- https://en.wikipedia.org/wiki/P_versus_NP_problem
- https://en.wikipedia.org/wiki/Gerhard_J._Woeginger
- https://news.ycombinator.com/item?id=15012359 — „A list of 116 previous ‚solutions'".
- https://www.cursor.tue.nl/en/news/2022/april/week-1/in-memoriam-gerhard-woeginger/

**Zu Xu/Zhou (Original und Presse)**
- https://link.springer.com/content/pdf/10.1007/s11704-025-50231-4.pdf — K. Xu, G. Zhou, „SAT requires exhaustive search", *FCS* 19(12):1912405, Dez. 2025.
- https://journal.hep.com.cn/fcs/EN/10.1007/s11704-025-50231-4
- https://dl.acm.org/doi/10.1007/s11704-025-50231-4
- https://arxiv.org/abs/2302.09512 — Preprint (mind. v9).
- https://www.eurekalert.org/news-releases/1091677 — Pressemitteilung, 17.07.2025.
- https://blog.computationalcomplexity.org/2025/08/some-thoughts-on-journals-refereeing.html — E. Allender (Gastbeitrag), 04.08.2025.

**Zu Deolalikar 2010**
- https://michaelnielsen.org/polymath/index.php?title=Deolalikar_P_vs_NP_paper — Polymath-Wiki, zentrale Dokumentation.
- https://michaelnielsen.org/polymath/index.php?title=Online_reactions_to_Deolalikar_P_vs_NP_paper
- https://rjlipton.com/2010/08/08/a-proof-that-p-is-not-equal-to-np/
- https://rjlipton.com/2010/08/10/update-on-deolalikars-proof-that-p%E2%89%A0np/
- https://rjlipton.com/2010/08/11/deolalikar-responds-to-issues-about-his-p%E2%89%A0np-proof/
- https://rjlipton.com/2010/08/12/fatal-flaws-in-deolalikars-proof/
- https://rjlipton.com/2010/08/15/the-p%E2%89%A0np-proof-is-one-week-old/
- https://scottaaronson.blog/?p=458 — „Eight Signs A Claimed P≠NP Proof Is Wrong", Aug. 2010.
- https://techcrunch.com/2010/08/12/fuzzy-math/
- https://wscor.win.tue.nl/woeginger/P-versus-NP/Deolalikar.pdf — archivierte Fassung.
- https://michaelnielsen.org/papers/mcm.pdf — Gowers/Nielsen, „Massively collaborative mathematics", *Nature* 461:879 (2009).

**Zu Blum 2017**
- https://arxiv.org/abs/1708.03486 — N. Blum; v1 „A Solution of the P versus NP Problem" (11.08.2017), zurückgezogen 30.08.2017 („The proof is wrong"), v3 (2020) unter neuem Titel.
- https://lucatrevisan.wordpress.com/2017/08/15/on-norbert-blums-claimed-proof-that-p-does-not-equal-np/
- https://johncarlosbaez.wordpress.com/2017/08/15/norbert-blum-on-p-versus-np/

**Zu KI-verstärkter Crank-Literatur**
- https://blog.computationalcomplexity.org/2025/04/p-v-np-papers-galore.html — Gasarch, 30.04.2025.
- https://blog.computationalcomplexity.org/2026/01/ai-and-research-papers.html — Fortnow/Gasarch, Jan. 2026.
- https://blog.computationalcomplexity.org/2026/06/respect-p-v-np-problem.html — „Respect the P v NP Problem", Juni 2026 (zu Khanukov/Lean).
- https://zenodo.org/records/18107880 — „AI Principle Series and P≠NP Structural Proof — Preprint Collection (2025 Edition)".
- https://zenodo.org/records/18464652 — „P = NP THE COMPLETE PROOF …" (Beispiel).
- https://www.science.org/content/article/arxiv-preprint-server-clamps-down-ai-slop
- https://www.404media.co/new-arxiv-rules-ai-generated-papers-ban/
- https://thenextweb.com/news/arxiv-ai-slop-ban-researchers-preprint

**Zum GPT-5/Erdős-Vorfall (Okt. 2025)**
- https://techcrunch.com/2025/10/19/openais-embarrassing-math/
- https://garymarcus.substack.com/p/erdosgate
- https://www.eweek.com/news/openai-math-hype/
- https://news.ycombinator.com/item?id=45633482
- https://github.com/teorth/erdosproblems/wiki/AI-contributions-to-Erd%C5%91s-problems

**Allgemein**
- https://www.claymath.org/millennium/p-vs-np/ — Status: offen (Stand 2026).
- https://en.wikipedia.org/wiki/Millennium_Prize_Problems
