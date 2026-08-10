# Projektstyrning och riskregister

## Arbetsströmmar

| WP | Innehåll | Huvudleverabel | Stängs vid |
|---|---|---|---|
| WP0 | Scope, krav, marknad/prototyp och licens | Signerad kravbaseline | G0 |
| WP1 | Standarder, leverantörsdata, inköp och AVL | Kontrollerat G1-underlag och reserverade prover | G1 |
| WP2 | Arkitektur, schema, footprints och generatorer | ERC-clean schema, pin-/DQ-map-evidens | G2 |
| WP3 | Pre-layout SI/PI, stackup och constraints | Signerad modell-/marginalrapport | G3 |
| WP4 | Placering, routing, post-layout, DFM/DFA | Fryst fabrication/assembly package | G4 |
| WP5 | SPD/PMIC-generator, jig och provisioning | Reproducerbar safe/4800/5600-release | G5 |
| WP6 | EVT-fab/assembly och bring-up | Serienumrerade EVT-resultat och failure analysis | G5 |
| WP7 | 6000, XMP/EXPO och plattformsmatris | Signerad 6000-release | G6 |
| WP8 | Pilot, kvalitet, compliance och release | DVP&R, teknisk fil och produktionsrelease | G7 |

Arbetsströmmar får gå parallellt men får inte kringgå grindarna. Exempelvis kan
servicejiggen utvecklas medan standarder köps, men layouten får inte frysas
innan G1/G3.

## Roller

En person kan bära flera roller, men varje beslut ska ha namngiven ägare:

- product owner: scope, kostnad, claim och grindgodkännande;
- hardware lead: arkitektur, schema, CAD och designändringar;
- SI/PI owner: modeller, constraints, simulering och mätkorrelation;
- component/procurement owner: AVL, traceability, PCN/EOL och leverantörskontakt;
- fab/CM owner: stackup, DFM/DFA, process och quality escapes;
- test owner: jig, automation, dataformat och failure triage;
- quality/compliance owner: DVP&R, material, regelverk, teknisk fil och recall;
- independent reviewer: kontrollerar grindens evidens utan att vara författare
  till allt underlag.

## Beslut och ändringsstyrning

Alla beslut med påverkan på gränssnitt, timing, mekanik, BOM eller claim ska ha
ett ADR/decision record med:

- fråga och datum;
- valda och avvisade alternativ;
- källor/modeller och antaganden;
- konsekvens för schema, layout, SPD, PMIC, test och compliance;
- vem som godkände och vilken grind som berörs.

Efter G2 används ECO. Byte av DRAM die revision, PMIC/SPD-suffix, induktor,
stackupmaterial, card-edge-process, paste eller reflowprofil är inte ett
“likvärdigt” inköpsbyte utan teknisk ändring. ECO ska ange vilka analyser och
tester som måste köras om.

## Repository- och releasebevis

När CAD börjar ska projektet använda ungefär följande struktur:

```text
hardware/          KiCad/EDA source och egna bibliotek
constraints/       maskinläsbara net classes och SI/PI assumptions
simulation/        körscript och öppet delbara resultat; vendor models exkluderas
spd/               schema, generator, tester och signerade images
pmic/              läsbar config, generator/export och verifiering
manufacturing/     versionslåsta releasepaket per PCB-revision
validation/        procedures, run manifests, raw log pointers och summaries
compliance/        publik scope/index; konfidentiell teknisk fil lagras separat
```

Varje release ska innehålla ett manifest med git-commit, CAD-/generatorversion,
artefakthash, PCB/BOM/SPD/PMIC-revision och kända avvikelser. En PDF eller
Gerber utan källcommit är inte en release.

## Riskregister

| ID | Risk | Sannolikhet | Konsekvens | Motåtgärd/stopp |
|---|---|---|---|---|
| R-01 | Rätt raw-card/full standard saknas | Hög | Fel topologi eller mekanik | Stoppa G1; köp JESD79-5D/JESD308B/RCA eller RCB/MO-329 |
| R-02 | DRAM väljs från katalogsnippet utan modeller | Hög | Ball-map-/SI-/powerfel | AVL kräver full data och provlot |
| R-03 | 5600-referens antas bevisa 6000 | Hög | Instabilitet/låg yield | Separat 6000-bin, SI/PI och lotvalidering |
| R-04 | PMIC-suffix levereras låst eller olämpligt för OC | Medel–hög | Profil kan inte programmeras/recoveras | Eval/serviceprov och läsback före PCB freeze |
| R-05 | Fel SPD eller DQ-map | Hög | Ingen boot eller bitfel | Netlistgenerator, CRC/round-trip och recoveryjigg |
| R-06 | 2Rx8 saknar 6000-marginal | Medel | Rev A stannar vid 5600 | Simulera tidigt; 1Rx8 fallback när 32 Gbit-data/supply finns |
| R-07 | Dolda FBGA-lödfel | Hög vid lokal process | Intermittenta fel maskerar designen | CM EVT, profilerad reflow, 100 % X-ray och fysisk analys |
| R-08 | Extra reflow skadar förmonterat PCBA | Medel | Flyttade/degraderade delar | Godkända cykler, MSL-logg och full profil; undvik halvmonterat flöde |
| R-09 | Fab byter stackup/material | Hög utan kontrakt | Lotberoende SI | Låst stackup, kupong/TDR, no-substitution och ECO |
| R-10 | XMP/EXPO fungerar på endast ett kort | Hög | Returer och felaktig claim | Minst två board vendors/plattform; konservativ JEDEC fallback |
| R-11 | Förfalskad eller blandad DRAM | Medel–hög | Variabel bin/yield | Auktoriserad kanal, CoC, lot/date-code och incoming inspection |
| R-12 | Standard eller vendorfil publiceras olagligt | Medel | Takedown/licensproblem | Metadata/hash i repo; fulltext i kontrollerad intern lagring |
| R-13 | Compliance tas in efter design freeze | Medel | Omtest/omdesign/lanseringsstopp | Scope i G0; specialist och materialdata före pilot |
| R-14 | Programtest döljer host-/CPU-fel | Medel | Felaktig root cause | Golden DIMM, cross-platform A/B och elektrisk korrelation |
| R-15 | PCN/EOL under utveckling | Medel | Omgörning eller obuyable BOM | Lifecycle/PCN-avtal, second source utvärderad före pilot |

Riskägare, nästa åtgärd och datum ska föras i issues; tabellen ovan är
baseline, inte en ersättning för levande uppföljning.

## Issue- och granskningsdisciplin

Rekommenderade labels: `blocker`, `decision`, `access-required`, `standard`,
`component`, `si-pi`, `spd-pmic`, `assembly`, `validation`, `compliance` och
`evidence`. Varje tekniskt issue ska ha krav-ID, acceptanskriterium, ägare och
vilken grind det blockerar. Stäng inte issue med “fungerar för mig”; länka
commit, rapport och rådata.

## Budgetposter

Sätt budget efter offerter, inte generella internetpriser. Minst följande
kostnadsbärare ska finnas:

- JEDEC-/IPC-standarder och vendor/EDA-åtkomst;
- DRAM/PMIC/SPD-prover, MOQ, reserv och tull/frakt;
- SI/PI-verktyg eller konsult/lab;
- controlled-impedance PCB, kuponger, NRE/panel och flera revisioner;
- stencil, assembly, profilkort, X-ray och failure analysis;
- jig/socket, testplattformar, BIOS/CPU/moderkort och temperaturprov;
- EMC/material/compliance och pilotkvalifikation;
- yield loss, rework, PCN/ECO och minst en omspinn.

Håll prototyp-, NRE-, kapitalutrustnings- och återkommande styckkostnad separat.

## Definition av “insamlingen är komplett”

Förstudien är komplett när varje nödvändigt underlag är antingen:

1. öppet insamlat och källbelagt;
2. exakt identifierat med laglig anskaffningsväg och ägare;
3. markerat som projektbeslut med deadline; eller
4. definierat som en artefakt som ska genereras/mätas senare med verifieringsmetod.

Det betyder inte att licensierade standarder eller NDA-modeller kan samlas i
det publika GitHub-repot. De röda `ACCESS/BLOCKER`-posterna i `STATUS.md` är den
återstående vägen från kunskapsbas till byggbar konstruktion.
