# Schema, PCB och SI/PI

## Schematisk blockindelning

Schemat ska delas i granskningsbara block:

1. 288-pin card edge och mekaniska/elektriska pinnamn.
2. Subkanal A: fyra byte-lanes per rank, DQS och DM_n/DBI_n enligt standard.
3. Subkanal B: motsvarande fyra byte-lanes.
4. CK, CA, CS och övriga kontrollsignaler enligt vald raw-card.
5. Rank-/MIR-/clamshell-konfiguration och ZQ.
6. `VIN_BULK`, PMIC, induktorer, rails, power-good och enable/sequence.
7. SPD-hubb, temperatur, I2C/I3C Basic och hårdvaruskrivskydd.
8. DFT/testpunkter som inte skapar oacceptabla höghastighetsstubbar.

## Maskinella schemakontroller

G2 kräver scriptbar evidens, inte bara manuell granskning:

- exakt 288/288 kontaktpad-mappning mot den styrande pin-tabellen;
- alla DRAM-bollar klassade som anslutna, verkligt NC eller med styrd strap;
- 64 unika DQ och rätt åtta DQ per DQS-par/byte-lane;
- korrekt ranktillhörighet, subkanal, chip-select och MIR;
- ZQ-komponent per DRAM och inga felaktiga korskopplingar;
- alla power pins på rätt rail, avkoppling och PMIC-sense;
- inga blandade 1,0/1,1/1,8/5 V-nät;
- sideband-adress, pullups, write-protect och power-good/enable;
- genererad DQ-map för SPD från samma netlist som layouten;
- ERC utan dolda globala etiketter, undantag eller waivers som saknar beslut-ID.

Varje export ska ha CAD-version, källcommit och SHA-256-manifest.

## Mekanik och card edge

Äldre Micron-underlag i bilagorna ger 133,35 × 31,25 mm, 1,27 ±0,10 mm
korttjocklek, 288 kontakter och 0,85 mm pitch som startdata. De är inte
produktionsmått. JESD308B/MO-329 och vald raw-card styr:

- kontur, notch och spärrurtag;
- padlängd, pitch, keepouts, fasning och toleranser;
- koppar-/nickel-/guldfinish och slitstyrka;
- bow/twist, tjocklek och datumplan;
- komponenthöjd, sida och värmespridarkuvert.

Begär fabrikens skriftliga DFM-godkännande av card edge före beställning.
Hårdguldets tjocklek eller fasvinkel får inte gissas i repot.

## Stackup och routing

Antalet lager är ett resultat av fältlösarberäkning, breakout och plane/return-
krav. En 10- eller 12-lagers referens är inte i sig en godkänd stackup.

Stackupen ska tas fram tillsammans med vald fabrik och ange:

- pressad total tjocklek med tolerans och card-edge-konsekvens;
- materialnamn, Dk/Df vid relevant frekvens, kopparprofil och glasväv;
- varje dielektrikums tjocklek och varje kopparvikts slutvärde;
- single-ended/differential impedansmål och tillverkningstolerans från
  raw-card-standard, inte från en FPGA-applikationsnot;
- kuponggeometri och TDR-acceptans;
- via-/microvia-struktur, aspect ratio, backdrill om relevant och registrering;
- kontinuerliga referensplan och stitching/return-path-policy.

Routing constraints ska importeras från vald RCA/RCB och komponentmodeller.
Minst följande grupper ska vara explicit definierade: DQ per byte, DQS-par,
CK-par, CA-grupper, CS/rank och sideband. Matchning ska bedömas i elektrisk
fördröjning, inklusive package/via, inte bara geometrisk millimeterlängd.

## Pre-layout SI

Simulera innan placering/routing fryses:

- vald 1Rx8/2Rx8-topologi och raw-card-placement;
- card-edge launch och host-/fixturegränssnitt med dokumenterade antaganden;
- DRAM I/O och on-die termination över relevanta process/voltage/temperature-
  hörn;
- package, via, trace, pad och connector; inte idealiska punktmodeller;
- DQ/DQS write/read, CK och CA/CS vid 4800, 5600 och 6000;
- crosstalk, reflection, return-path discontinuity och via-stubbar;
- variation i fabimpedans, driver/ODT, timing och temperatur;
- eye/mask/timing-marginal med pass/fail härledd från aktuell standard.

Använd endast modeller vars licens och revision kan spåras. Om CPU-/hostmodell
inte kan erhållas ska gränsvillkoret definieras och risken dokumenteras; en
simulering med godtycklig källa och last är inte ett kvalifikationsbevis.

## PI och PMIC

Power-integrity-arbetet ska omfatta:

- DC IR-drop från `VIN_BULK` genom PMIC, planes och package;
- PMIC-effektivitet, induktorsaturation, strömgräns och termik vid worst case;
- transient droop/overshoot på VDD, VDDQ och VPP för read/write/activate-
  mönster;
- PDN-impedans över frekvens med komponenters bias-/temperaturderating och ESL;
- anti-resonans och placering av bulk/högfrekvensavkoppling;
- rail-sequencing, enable, power-good och brownout/fault recovery;
- samtidig switchning och koppling mellan switchregulatorer och DQ/CK/CA;
- mätpunkter med låginduktiv anslutning för rail ripple/transienter.

Strömvärdena i bilagorna är referensmoduldata upp till 5600 och får inte
användas som enda dimensioneringsunderlag för 6000 eller en annan die revision.

## Post-layout och release

Efter layout ska extraherade nät och PDN simuleras med den faktiska stackupen.
G4 kräver:

- constraint report utan odeklarerade avvikelser;
- SI/PI-rapport med hörn, marginaler, modellrevisioner och råfiler;
- DRC/ERC 0 eller formellt granskad waiver per avvikelse;
- DFM/DFA-granskning från både fab och CM;
- kontroll av solder-mask slivers, paste, courtyard, polarity och assembly side;
- CAM-review av edge, drill, backdrill, impedanskuponger och panelisering;
- netlist-to-Gerber/ODB++-jämförelse och tillverkningsmanifest.

