# Team-Briefing für die Tiefenphase (A1–A8)

Verbindlich für alle Fachagenten. Ergänzt `00-briefing.md` (bitte ebenfalls lesen).

## 1. Harte Umgebungsbeschränkung

**WebFetch ist gesperrt.** Getestet und blockiert: arxiv.org, ar5iv, eccc.weizmann.ac.il,
link.springer.com, blog.computationalcomplexity.org, scottaaronson.blog, acm-stoc.org,
en.wikipedia.org. Es gibt **keinen Volltextzugang**.

Nutzbar ist ausschließlich **WebSearch**.

Daraus folgt verbindlich:
- Keine WebFetch-Versuche. Sie kosten nur Zeit.
- **Triangulationspflicht:** Jeder nichttriviale Claim wird mit mindestens
  2–3 *unterschiedlich formulierten* Suchanfragen geprüft. Eine Suchantwort ist
  eine Synthese, keine Quelle.
- **Markierung `[NUR-SNIPPET]`** für alles, was nicht am Volltext verifiziert ist.
  Das ist der Normalfall, nicht die Ausnahme.
- Wir können belegen, *dass* eine Arbeit existiert — nicht zuverlässig, *was*
  in ihr steht. Formuliere entsprechend vorsichtig.
- Wenn zwei Suchanfragen sich widersprechen: **beide Zahlen berichten** und den
  Widerspruch als solchen kennzeichnen. Nicht glätten.
  (Belegter Fall: Gasarch-Umfrage 2019 — eine einzige Suchantwort lieferte
  66 % und ~80 % für P≠NP nebeneinander. Wer das glättet, erfindet Wissen.)

## 2. Die inhaltliche Leitachse des Projekts

Der bisher stärkste KI-Befund (Leitung, vorab verifiziert über mehrere Suchen):
arXiv:2509.18057 — AlphaEvolve fand neue Gadget-Reduktionen und verbesserte
Inapproximierbarkeitsschranken (MAX-4-CUT 0,9883 → 0,987; MAX-3-CUT → 0,9649;
metrisches TSP 117/116 → 111/110).

Die daraus abgeleitete **Leitunterscheidung**, an der sich alle Agenten abarbeiten sollen:

> **Endlicher Suchkern in einem menschlichen Lifting-Rahmen** vs.
> **Probleme ohne endlichen Suchkern.**

KI-gestützte Suche funktioniert, wenn drei Bedingungen zugleich erfüllt sind:
- **B1**: Das gesuchte Objekt ist endlich und maschinell repräsentierbar
  (ein Gadget, ein Graph, ein Programm).
- **B2**: Es gibt ein billiges oder erzwingbar billig gemachtes Verifikationsorakel.
- **B3**: Es existiert ein **von Menschen bewiesener Lifting-Rahmen**, der vom
  endlichen Objekt auf die allgemeine Aussage schließt (z.B. das PCP-Theorem
  plus Gadget-Reduktionskalkül).

**Korrektur gegenüber einer früheren Fassung dieses Briefings** (Befund A6):
Der Schnitt verläuft *nicht* "endliches vs. unendliches Ergebnis". FunSearchs
Cap-Set-Schranke ist asymptotisch, die Inapproximierbarkeitsresultate sind
universelle Theoreme. Endlich ist allein das **gesuchte Objekt**; die
Allgemeinheit kommt ausschließlich über B3 herein.

P vs. NP verletzt B1, B2 und B3 — am gravierendsten B3: Die drei Barrieren
*sind* genau die Feststellung, dass kein tragfähiger Lifting-Rahmen bekannt ist.

Prüfe für deinen Bereich: Bestätigt oder widerlegt dein Befund diese Achse?
Gegenbefunde sind ausdrücklich erwünscht.

## 3. Verifikationsleads aus Strategie- und Leitungsvorrecherche

Diese Punkte sind **unverifiziert** und sollen von den zuständigen Agenten geprüft werden:
- AlphaEvolves 4x4-Matrixmultiplikation (48 statt 49 Multiplikationen) gilt
  möglicherweise über einer *engeren Ringklasse* als in der Berichterstattung
  suggeriert; Waksman (1970) soll bereits 46 erreicht haben (unter anderen
  Voraussetzungen). → A6 klärt das.
- "Navier–Stokes" (OpenAI, ca. 08.09.2026) und Fortnows Blogpost
  "Navier-Stokes and Lean" (09/2026): Lehrstück dazu, dass Lean die *Ableitung*
  verifiziert, nicht die *Aussage*. → A7 klärt das.
- GPT-5/Erdős-Problem-Claim, Rücknahme nach ca. 17 Stunden (Oktober 2025). → A8.
- "Khanukov 2026" — angeblich ein weiterer LLM-Direktversuch an P vs. NP. → A8.
- Fortnow (Juni 2026) zu P vs. NP in Lean sinngemäß: "we don't even have a
  viable approach". → A1 oder A7, Wortlaut prüfen.
- Gasarch-Umfragezahlen 2002/2012/2019. → A1.

## 4. Muss-Kriterien für die Anerkennung eines Durchbruchs (aus S1, verbindlich)

- **M1 Barrierenrechenschaft:** Adressiert der Claim explizit, wie er
  Relativization, Natural Proofs und Algebrization umgeht?
- **M2 Übergeneralisierungs-Test:** Funktioniert das Argument auch für eine
  Variante, für die die Aussage nachweislich falsch ist? Dann ist es kaputt.
- **M3 Keine Strukturannahme über Algorithmen:** Wird stillschweigend über eine
  eingeschränkte Algorithmenklasse quantifiziert statt über alle?
- **M4 Lean ≠ Wahrheit:** Eine Formalisierung verifiziert die Ableitung aus den
  angegebenen Voraussetzungen, nicht deren Angemessenheit.
- **M5 Keine Axiomschmuggelware:** Werden zusätzliche Annahmen als harmlos verkauft?

## 5. Lieferobjekte

1. Vollständiger Bericht nach `research/<deine-ID>-<thema>.md` (Markdown, deutsch,
   Fachbegriffe englisch, Quellenliste mit URLs am Ende).
2. Rückgabe an die Leitung: Zusammenfassung **500–700 Wörter**, enthaltend:
   - die 3–5 wichtigsten Befunde mit Konfidenz (hoch/mittel/niedrig),
   - explizit: was du **nicht** verifizieren konntest,
   - deine Antwort auf die Leitachse aus Abschnitt 2,
   - falls zutreffend: Widerspruch zu einer Annahme dieses Briefings.

Negativbefunde sind vollwertige Ergebnisse. Nichts auffüllen, nichts aufwerten.
