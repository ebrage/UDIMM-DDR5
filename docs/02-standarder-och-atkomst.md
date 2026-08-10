# Standarder och åtkomst

Senast kontrollerat 2026-08-10. Revisionsnumret ska återkontrolleras vid varje
grind eftersom katalogposter kan ändras. Fulltexten i styrande standard går
före denna sammanfattning.

## Styrande DDR5-dokument

| Dokument | Identifierad utgåva | Användning | Status |
|---|---|---|---|
| JESD79-5D | 2025 | DDR5 SDRAM-funktion, timing, AC/DC och ball maps | `ACCESS/BLOCKER` |
| JESD308B | v1.2, 2025 | Gemensamma elektriska/mekaniska krav för 288-pin 1,1 V UDIMM | `ACCESS/BLOCKER` |
| JESD308-U0-RCA | v1.1 | x8, en package rank/1Rx8-råkort | `ACCESS` om 1Rx8 väljs |
| JESD308-U0-RCB | v1.1 | x8, två package ranks/2Rx8-råkort | `ACCESS/BLOCKER` för rekommenderad Rev A |
| MO-329 | aktuell utgåva krävs | Modulomriss, notch, card edge och mekaniska toleranser | `ACCESS/BLOCKER` |
| JESD400-5D.01 | release 1.4, 2025-09 | DDR5 SPD-innehåll, inklusive hastigheter till 9200 | `ACCESS/BLOCKER` |
| JESD300-5B.01 | v1.5.1, 2024-05 | SPD5118-hubb, register och protokoll | `ACCESS/BLOCKER` |
| JESD403-1C.01 | kontrollera vid köp | DDR5-modulens I3C Basic-sideband | `ACCESS` |
| JESD301-2 | 2022 | PMIC5100 för klient-DIMM | `ACCESS/BLOCKER` |
| JESD301-6 | v1.0, 2025-02 | PMIC5120; alternativ nyare klient-PMIC-familj | `ACCESS` om delen används |

JESD308B och raw-card-dokumentet behövs samtidigt. Det gemensamma dokumentet
ersätter inte komponentplacering, topologi och toleranser i RCA/RCB.

## Tillverkning och acceptans

| Dokument | Identifierad utgåva | Roll |
|---|---|---|
| IPC/JEDEC J-STD-020F | 2022-11 | Komponentens MSL- och reflowklassning |
| IPC/JEDEC J-STD-033E | 2024 | Hantering, torrpackning, floor life och bakning |
| IPC-7095E | 2024-08 | Design-, assembly-, inspektions- och reworkvägledning för BGA/FBGA |
| IPC J-STD-001J | 2024 | Processkrav för lödda elektronikmontage |
| IPC-A-610J | 2024 | Acceptanskriterier för färdig PCBA |
| IPC-6012F | 2023 | Kvalifikation/prestanda för rigid PCB |
| IPC-2221/2222 | aktuell kontrollerad utgåva | Generell/rigid PCB-design; raw-card-regler är fortfarande styrande |
| IPC-7525 och J-STD-005B | aktuell kontrollerad utgåva | Stencil respektive lodpasta |

Projektet ska i inköpsordern ange vald produktklass och vem som avgör konflikt
mellan ritning, komponentdatablad, JEDEC, J-STD-001 och IPC-A-610. “Tillverka
enligt IPC” utan revision, klass och särskilda krav är inte tillräckligt.

## Kvalifikation och miljöprov

Ett internt EVT-kort behöver inte automatiskt genomgå full produktkvalifikation.
Om modulen ska säljas ska en testplan härledas från produktens miljö och
livslängd. Relevanta familjer att granska med testlaboratorium är JESD47 och
JESD22-metoder för bland annat preconditioning, temperaturcykling,
högtemperaturliv, fukt och mekanisk belastning. Använd aktuell fulltext och
skriv inte testnivåer från minnet.

## Åtkomstregister

För varje köpt eller leverantörslåst dokument ska ett internt register innehålla:

- dokument-ID, titel, revision och publiceringsdatum;
- laglig källa, licensinnehavare och åtkomstbegränsning;
- lokal kontrollerad sökväg utanför det publika repot;
- SHA-256 och datum då konstruktören verifierade revisionen;
- vilka krav, CAD-regler eller tester dokumentet styr;
- ändringsanalys när en ny revision kommer.

Publicera aldrig standardens fulltext, tabeller, raw-card artwork eller
leverantörsmodeller om licensen inte uttryckligen tillåter det. Repot får
publicera egna krav-ID:n och korta, icke-ersättande sammanfattningar.

## G1-dokumentpaket från leverantörer

För varje aktiv komponent krävs minst:

- fullständigt datablad och errata;
- paketritning, land pattern och MSL/reflowgräns;
- IBIS- och relevant package/interconnect-modell med revisionsspårning;
- effekt-/strömdata för vald hastighet och temperatur;
- kvalitets-/reliabilitetsdata, PCN/EOL-process och materialdeklarationer;
- full register-/programmeringsbeskrivning för PMIC/SPD-hubb;
- skriftligt besked om orderkod, livscykel, MOQ, lead time och provkvantitet.

Marknadsföringssida eller kortformigt datablad stänger inte G1.
