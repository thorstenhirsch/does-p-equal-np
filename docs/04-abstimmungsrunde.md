# Abstimmungsrunde am fertigen Papier

Alle zehn Agenten erhielten das fertige `KONSENSPAPIER.md` mit dem Auftrag, es gegen
den eigenen Fachbericht zu prüfen. **Alle zehn haben geantwortet.** Zusammenfassung
der Rückläufe; die daraus folgenden Textänderungen sind in §10.5 des Papiers
tabellarisch nachgewiesen.

---

## S1 — Strategie

Zog die eigene Behauptung zurück: Hatte das Fortnow-Pseudozitat im Strategiebericht
mit Konfidenz *hoch* geführt. Anmerkung dazu: „Mein §D.2.4 (Snippet-Quarantäne) hat
auf seinen eigenen Verfasser nicht gewirkt."

Votum zum Raster: **beidseitiges B2** und **M6** ins Papier; **B3a/B3b** nur als
Notiz; **B3′** als Überkomplizierung ablehnen. Zur Kernthese: trägt, und ist präziser
als der ursprüngliche Angriffsplan — „eine Aussage über Struktur statt über Bilanz".
Strategie in der Diagnose bestätigt, in der Priorisierung widerlegt.

## S2 — Red Team

Schärfster Rücklauf. Vier Angriffe, alle eingearbeitet:

- Die **AC⁰-Zeile** stand fälschlich in der Endpunkt-Tabelle — dort funktioniert die
  Technik, weil es in AC⁰ keine PRFs gibt.
- **„Gödelpreis 2024"** ist eine Preisträgerzuordnung — genau die Angabenklasse, die
  §9 selbst für unbelastbar erklärt.
- Die **Extrapolation** („620.000 Jahre") wurde als methodisch wertlos bezeichnet und
  trotzdem prominent geführt. „Entweder/oder."
- **Klarster D.4-Verstoß:** Das Urteil „M1 nicht erfüllt, M3 verletzt…" über Xu/Zhou
  war Begutachtung eines ungelesenen Volltexts. Ebenso die Aufstufung von „Kritiken"
  zu „Widerlegungen" und die Motivzuschreibung „umgebucht".
- Zu §12: B1/B2/B3 als **„Kriterium"** ist nicht gedeckt — post hoc an sechs Fällen
  gebildet, nie prospektiv getestet. Und „hat sie nicht verstanden" macht aus *kein
  Rahmen bekannt* ein *kein Rahmen findbar*.

## A1 — Kanon & Barrieren

- **Rechenfehler gefunden:** 9 % von 124 sind rund elf Personen, nicht sieben.
- **Natural Proofs** fehlte die dritte Eigenschaft *usefulness* — genau die, die die
  Schranke liefert.
- §3.2 unvollständig: **schwach vs. stark NP-vollständig** fehlte; bei Clique fehlte
  der Quantorenmechanismus (∀k∃c statt ∃c∀k).
- §7.2: **„B1 fällt aus" ist zu stark** — für die P=NP-Richtung existiert das endliche
  Objekt, dort scheitert B2. Sonst kollabiert die beidseitige Pointe.
- Zur Gödel-Laudatio: nennt Relativization + Natural Proofs, nicht Algebrization.
- Zu §9: „korrekt und **nicht** übertrieben, eher untertrieben."

## A2 — Untere Schranken

- **Williams-Kompressionsfehler:** „fast linear → quadratisch" beschreibt die
  *abgeleitete Zeitschranke*, nicht die *Simulation* (t/log t → √(t log t)).
- Endpunkt-Tabelle braucht die Spalte **unbedingt/bedingt** — nur natural proofs ist
  bedingt.
- „Hadamard nicht rigide" zu absolut: nur im für Valiants Programm nötigen
  Parameterbereich; gegenläufiges Preprint 2026.
- §7.4 („erst ETH umstoßen") **widersprach §11.1** — die Hürde gilt nur für
  allgemeine Schaltkreise; Williams bewies ACC⁰ ohne ETH-Bezug.
- §11.1 zu optimistisch: „gegen TC⁰ nichts bekannt" verwechselt *keine unteren
  Schranken* mit *keine SAT-Algorithmen*; und **B2 ist nur teilweise erfüllt** — die
  Korrektheit eines Kandidatenalgorithmus ist selbst eine Beweispflicht.

## A3 — Meta-Komplexität

- **Dissens, teilweise aufrechterhalten:** Die Ablehnung von B3′ wird akzeptiert, aber
  die angekündigte Konsequenz war nicht vollzogen — die B3-Definition blieb
  unverändert und wurde von Hardness Magnification erfüllt. Nebensatz nachgetragen.
- §4.5 endete 2023, obwohl das Kapitel „lebendigste Front" heißt: Hirahara–Ilango
  (FOCS 2025) und die Gegenrichtung Mazor–Pass (CCC 2024) fehlten.
- Random-Oracle-Einwand unfair verkürzt: Ilango argumentiert, die Relativierung sei
  gezielte Barriereumgehung.
- **Ben-David–Halevi zu breit** wiedergegeben: Es geht um Unabhängigkeit von PA plus
  allen wahren Π₁-Sätzen, nicht um ZFC-Unabhängigkeit allgemein.

## A4 — Obere Schranken

- **„Konvergenz gegen c ≈ 0,386 — genau die ETH" ist zu stark**, in zwei Punkten: Die
  ETH behauptet nur s₃ > 0, keinen Grenzwert; und die Reihe misst den Stand der
  *Analysetechnik*, nicht s₃ selbst.
- §5.2 „widerlegt, nicht offen" ist gedeckt — bezieht sich auf den *Einwand*, nicht
  auf P vs. NP. Zwei Präzisierungen: idealisierte CDCL-Modelle mit unbeschränkten
  Neustarts; Haken schließt nur effiziente *Widerlegung* von PHP aus.
- Compute-Ertrag: Hardware und Timeouts zwischen den Tracks nicht identisch.
- Bestätigt, dass das Weglassen des 327-vs-276-Vergleichs richtig war (Scheintrend).

## A5 — Algebraisch & GCT

- Über **endlichen Körpern gilt der VP/VNP-Transfer unbedingt** — der Gegenpol zur
  GRH-Klausel fehlte.
- „Chasm at Depth Four" ist eine Kette (Agrawal–Vinay, Koiran, Tavenas), kein
  Einzelresultat.
- Bestätigt die Zuspitzung „Ein endliches Suchobjekt allein nützt nichts" als
  tragfähig: GCT erfüllt B1 tatsächlich und scheitert trotzdem.
- B3a/B3b als Notiz akzeptiert: „Als eigene Achse hätte sie nur an einem einzigen Fall
  Erklärkraft."

## A6 — KI-Algorithmenentdeckung

- **„Verifizierte Resultate"** im Tabellenkopf widersprach der eigenen Markierung →
  „Berichtete Resultate".
- Nachtrag, der den naheliegendsten Einwand entkräftet: Die finalen Gadgets wurden
  **per Brute Force unabhängig nachverifiziert**; die evolvierte Prozedur beschleunigt
  nur die Suche.
- **„Das Weglassen war ein Fehler"** — der Matrixmultiplikations-Befund gehört ins
  Papier, nicht wegen der Matrixmultiplikation, sondern weil es der einzige Fall im
  Material ist, in dem eine **KI-Mathematik-Meldung selbst** nachweislich falsch ist.
  Als §8.6 aufgenommen.

## A7 — KI-Beweisen

- **Faktenfehler gefunden:** „Seed-Prover: 5 von 6 IMO-Aufgaben" ist falsch. Unter
  Wettbewerbsbedingungen 4 vollständige + 1 teilweise = 30 Punkte, Silber; die fünfte
  entstand nachträglich per extended search. „Genau die Art Aufrundung, die §9
  anprangert."
- **Ironie in §9:** Das Kapitel setzte gegen ein Pseudozitat *ein weiteres ungeprüftes
  Wortlautzitat*. A7 kann „No, it isn't" nicht bestätigen; belegt sind nur zwei andere
  Aussagen. Entsprechend markiert.
- §8.4 braucht den Halbsatz, dass die Autoren die Verbindung **im eigenen Abstract
  offengelegt** haben — sonst liest es sich als Überführung statt als
  Redlichkeitsindiz. Und: Wir wissen nicht, ob GPT-4 das Argument im Dialog zugeführt
  bekam oder aus Trainingsdaten kannte.
- Anthropics eigene Aussage zur Riemann-Schranke — die Techniken führten nicht zum
  Beweis — ist „der stärkste Beleg der These, vom Hersteller selbst".

## A8 — Claim-Auditor (Vetorecht)

**Kein hartes Veto.** Fünf Nachbesserungen, eine davon der wertvollste Einzelbefund
der ganzen Runde:

- **Fall 4 — Zahlenmigration.** §8.5 nennt „17 Stunden Formalisierung"
  (Navier–Stokes); zwei Abschnitte später verwirft das Papier „17 Stunden" für den
  Erdős-Vorfall. Mit hoher Wahrscheinlichkeit die **Herkunft** des Briefing-Fehlers:
  eine Zahl, die den Fall gewechselt hat. „Das Papier besitzt damit einen
  Kontaminationsnachweis, nicht nur eine Korrektur."
- V8 leicht verletzt: Wortlautzitate in §8.3 ohne lokalen Marker.
- §6.1 braucht eine Lesehilfe („kleinerer Wert = stärkere Härteschranke"), sonst liest
  die Tabelle als Fortschrittsbalken.
- §11.1 „die einzige bekannte Stelle" ist das eigene Fehlermuster F9 → „die einzige,
  die dieses Projekt identifiziert hat".
- Zwei Fehlermuster fehlten: **F11 Endorsement-Wäsche**, **F12 Gap-Minimierung**.
- Fairness-Urteil: „Belegdecke trägt; kein Misconduct-Vorwurf."
