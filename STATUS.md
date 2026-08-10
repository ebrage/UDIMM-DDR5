# Projektstatus och stoppgrindar

Forskningsstatus: **sammanställt 2026-08-10**. Produktstatus: **inte byggklar**.

## Statuskoder

| Kod | Betydelse |
|---|---|
| `DONE` | Insamlat och källbelagt för nuvarande fas. |
| `ACCESS` | Dokument eller modell är identifierad men måste köpas/begäras lagligt. |
| `DECISION` | Projektägaren måste låsa ett alternativ. |
| `GENERATE` | Ska skapas från den slutliga konstruktionen, inte gissas. |
| `VALIDATE` | Måste bevisas med simulering, mätning eller provning. |
| `BLOCKER` | Stoppar schema/layout/inköp eller frisläppning. |

## Röda blockerare

| ID | Blockerare | Vad som stänger den | Status |
|---|---|---|---|
| B-01 | Styrande DDR5/UDIMM/raw-card-standarder saknas i repot | Anskaffa kontrollerade kopior av JESD79-5D, JESD308B och vald RCA/RCB samt registrera revision/checksumma utan att publicera filerna | `ACCESS/BLOCKER` |
| B-02 | Minnestopologi och exakt DRAM-orderkod är inte låsta | Välj 2Rx8 eller 1Rx8 och säkra datablad, paketritning, IBIS/paketmodell, power data, PCN/EOL och provkvantitet från spårbar kanal | `DECISION/BLOCKER` |
| B-03 | PMIC och SPD-hubb saknar fullständiga konstruktionsunderlag | Leverantörsbekräftad orderkod, referensschema, BOM, register-/MTP-data och programmeringsflöde | `ACCESS/BLOCKER` |
| B-04 | Fab-stackup och kantkontakt är inte kontrakterade | DFM-godkänd 1,27 mm stackup, impedanskuponger, mekanik och kontaktfinish från vald fabrik mot styrande standard | `DECISION/BLOCKER` |
| B-05 | Ingen pre-layout SI/PI-analys finns | Simulering med verklig stackup, kontakt-/paketmodeller och tydliga pass/fail-kriterier vid 4800/5600/6000 | `GENERATE/BLOCKER` |
| B-06 | Ingen egen SPD-image finns | Generera från slutlig netlist/BOM enligt aktuell SPD-standard; validera CRC, DQ-map, fallback och skrivskydd | `GENERATE/BLOCKER` |
| B-07 | XMP/EXPO-åtkomst och varumärkesvillkor är inte lösta | Partner-/self-certification-väg, tekniska profilkrav och testmatris godkända av Intel respektive AMD | `ACCESS` |
| B-08 | CM och monteringsprocess är inte kvalificerade | DFM, stencil, MSL-logg, profilerad reflow, AOI/röntgen och spårbar processrapport | `DECISION/BLOCKER` |

## Beslut som ska låsas i ordning

1. Användningsfall: intern FoU-prototyp eller produkt för försäljning.
2. Baslinje: JEDEC-6000B endast, eller även XMP/EXPO.
3. Topologi: 2Rx8 Rev A eller 1Rx8 Rev A.
4. Exakt DRAM, PMIC, SPD-hubb och tillåtna alternativ.
5. EDA/SI-verktyg, vald PCB-fabrik, CM och testpartner.
6. Prestanda- och miljökrav: 1DPC/2DPC, temperatur, livslängd och garanti.
7. Öppen hårdvarulicens samt licenser för dokumentation och mjukvara.

## Grindplan

| Grind | Inträdeskriterium | Utträdeskriterium |
|---|---|---|
| G0 – Scope | Projektmål beskrivet | Alla beslut 1–3 ovan signerade |
| G1 – Underlag | G0 klar | B-01 till B-04 stängda; komponentprover reserverade |
| G2 – Schema | G1 klar | ERC 0 fel; 288/288 pin-map, rails, DQ-map och sideband maskinkontrollerade |
| G3 – Pre-layout | G2 klar | SI/PI-modeller visar godkänd marginal för vald baseline |
| G4 – Layout | G3 klar | Post-layout SI/PI, DFM/DFA och tillverkningsgranskning godkända |
| G5 – EVT | Kort monterade | Kontrollerad första kraft, SPD/PMIC, boot och 4800/5600 stabilt |
| G6 – 6000 | G5 klar | Stabil 6000-profil på hela plattformsmatrisen med loggade marginaler |
| G7 – Pilot | G6 klar | Processkapabilitet, miljöprov, regelverk och spårbarhet godkända |

Ingen grind får passeras enbart med en programvaruskärmbild. Varje PASS ska ha
rådata, verktygsversion, DUT-serienummer, plattform/BIOS, miljö och ansvarig
granskare.

## Närmast genomförbara arbete

- Begär offert/åtkomst för standardpaket och fulla komponentunderlag.
- Skicka samma RFQ till minst två auktoriserade komponentkanaler och två
  DDR5-erfarna PCB/PCBA-leverantörer.
- Lås topologi först när både modeller och provkvantitet går att få.
- Skapa kravspårning från [`data/requirements.csv`](data/requirements.csv).
- Rita inte råkort eller SPD-bytefält från äldre PDF:er innan G1 är stängd.

