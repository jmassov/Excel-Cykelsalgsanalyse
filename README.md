## Cykelsalgsanalyse – Excel (Power Query)

Analyse af et offentligt datasæt på **113.036 ordrelinjer** med fokus på ét spørgsmål: *hvor kan en cykelforhandler øge profitten ved at fokusere på de rigtige produkter, kundegrupper og markeder?* Projektets kerne er lige så meget **kildekritisk datavalidering** som selve analysen — flere fund begrænsede, hvilke spørgsmål data overhovedet kunne besvare.

**Værktøjer:** Excel · Power Query · pivottabeller

---

## Om datasættet

Kilde: "Bike Sales in Europe" fra Kaggle — 113.036 ordrelinjer, angivet til at dække 1. januar 2011 til 31. juli 2016 for en cykelforhandler med salg i seks lande. Data er offentligt tilgængeligt og stammer ikke fra en rigtig virksomhed.

## Proces og metode

1. **Kontrol af datagrundlaget** i Power Query, før nogen analyse blev påbegyndt (se næste afsnit).
2. **Rensning og strukturering** i Power Query: fjernelse af dubletter, omkategorisering af fejlplacerede rækker og beregnede kolonner til profitabilitets- og markedsanalyse.
3. **Analyse** med pivottabeller på tværs af produkter, kundesegmenter og markeder.
4. **Formulering af konklusioner** afgrænset til det, data reelt kunne bære.

## Datavalidering

Kontrollen afdækkede og dokumenterede **seks datakvalitetsproblemer**, som hver fik betydning for analysen:

- **Årene er ikke uafhængige observationer** — parvis identiske rækketal, og 30,7 % af ét års transaktionssignaturer optræder identisk i et andet (mod 0,6 % mellem kontrolår). → *Analyse af udvikling over tid blev udeladt.*
- **Delår i 2014 og 2016** (kun januar-juli). → *Sæsonmønstre kan ikke opgøres; det tilsyneladende efterårsfald er et dækningshul, ikke et salgsmønster.*
- **1.000 eksakte dubletrækker** (0,9 %). → *Fjernet; absolutte beløb angives med forbehold.*
- **`Unit_Price` er en listepris, ikke en transaktionspris** — afveg fra faktisk omsætning i 94 % af rækkerne, systematisk pr. land (Canada 0,99, Australien 0,84). → *Alle marginberegninger blev foretaget på Revenue og Cost, ikke på enhedspriser.*
- **Ingen `Order_ID` eller `Customer_ID`.** → *Gennemsnit kun pr. ordrelinje; kundeloyalitet kan ikke analyseres.*
- **En fejlkategoriseret produktrække** (en cykel lå under Tøj/Vests). → *Omkategoriseret i Power Query; rettede en forvansket margin.*

En metodenote i rapporten dokumenterer også en faldgrube: Power Query profilerer som standard kun de øverste 1.000 rækker, hvilket gav en forkert minimumsværdi, indtil profilen blev stillet til hele datasættet.

## Udvalgte fund og anbefalinger

- **Lavmargin-produkter** — kasketter og trøjer har omkostninger på over 85 % af realiseret omsætning, langt over sammenlignelige produkter. → *Anbefaling om omkostningsomlægning, med fokus på trøjer (større omsætning).*
- **Kundesegmentering** — de 25-40-årige driver **55 %** af omsætningen, men virksomhedens primære segment var defineret som "Voksne (35-64)". → *Anbefaling om revideret, snævrere segmentering.*
- **Marked** — Australien realiserer kun **83,8 %** af listeværdien mod Canadas 99 %. Margingabet er et **prisrealiseringsproblem, ikke et omkostningsproblem** (enhedsomkostningen er identisk på tværs af markeder). → *Anbefaling om at undersøge rabat- og prissætningspraksis; en what-if viser, at margin ville stige fra 31,8 % til 42,3 % ved Canadas realiseringsniveau.*

## Hvad analysen ikke kan svare på

For at være tro mod datagrundlaget afgrænser rapporten eksplicit sit scope: den kan **ikke** udtale sig om udvikling over tid, sæsonmønstre, år-til-år-vækst, gennemsnitlig ordreværdi, kundeantal eller kundeloyalitet — og absolutte kronebeløb angives med forbehold. Datasættet er egnet til **strukturelle sammenligninger** på tværs af produkter, kundegrupper og markeder, fordi disse er egenskaber ved den enkelte ordrelinje og derfor upåvirkede af, at årene ikke er uafhængige.

## Filer i dette repo

- `bike_sales_analysis.xlsx` — projektfil med Power Query-trin og pivottabeller
- `rapport.pdf` — analyserapport med bilag (fuld validering og metode)
