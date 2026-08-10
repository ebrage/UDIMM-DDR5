# Tillverkning och montering

## Rekommenderat flöde

Låt en CM med dokumenterad FBGA-/DDR-erfarenhet montera hela första EVT-loten,
profilera varje kortsida och röntga samtliga DRAM. Det ger den bästa chansen att
skilja designfel från lödfel.

Att “löda DRAM själv” är tekniskt möjligt endast som ett kontrollerat SMT-
processflöde. DRAM-FBGA har dolda lödfogar; frihandsplacering och varmluft är
inte en reproducerbar produktionsmetod.

## Alternativ för egen DRAM-montering

| Flöde | Bedömning | Villkor |
|---|---|---|
| CM monterar allt | Rekommenderat för EVT och pilot | DFM, processprofil, lotspårning och 100 % röntgen |
| CM monterar PMIC/SPD/passiver; DRAM monteras senare | Hög risk | CM och komponentleverantörer måste godkänna extra reflowcykler; selektiv stencil, maskinplacering och ny profil krävs |
| Bare PCB + komplett kit, all SMT monteras lokalt | Endast för välutrustat labb | Stencilprinter, vision pick-and-place, profilerad ugn, torrskåp/bake och tillgång till röntgen |
| Handplacerad/varmluft | Inte godkänt som designbevis | Kan användas för enstaka rework/experiment, aldrig för att frisläppa layouten |

För 2Rx8 sitter DRAM normalt på båda sidor. Monteringssekvensen måste därför
hantera tvåsidig paste/placement/reflow, komponenternas vikt och tillåtet antal
värmecykler. Ett “halvbestyckat” PCBA kan ge fler värmecykler än ett normalt
CM-flöde och är inte automatiskt skonsammare.

## MSL och materialstyrning

- Registrera MSL och floor-life-start per öppnad komponentrulle/tray.
- Förvara DRAM, PMIC och SPD-hubb i MBB/torrskåp enligt leverantör och
  J-STD-033E.
- Baka endast enligt komponentens och PCB:ns tillåtna schema; godtycklig
  övergräddning kan försämra lödbarhet och laminat.
- Kontrollera moisture indicator card och förpackningsskada vid mottagning.
- Hantera alla komponenter ESD-säkert och med vakuum/nozzle som inte skadar
  package eller kulor.
- Dokumentera pastealloy, fluxklass, stencil-ID, utskriftsliv och kylprofil.

## Stencil, placering och reflow

Stenciltjocklek och aperturreduktion ska optimeras mot exakt FBGA-ball,
land pattern, paste och CM:s processkapabilitet. Kopiera inte en generell
BGA-tabell utan DFM/DOE.

Processen ska innehålla:

1. SPI efter stenciltryck med definierade volym-/offsetgränser.
2. Visionbaserad maskinplacering; komponentens orientering och lot loggas.
3. Termoelement på representativa DRAM-, PMIC- och PCB-punkter på ett
   profilkort.
4. Reflowprofil som håller samtliga komponentkroppar inom deras egna
   ramp/soak/liquidus/peak-gränser; ugnens setpoint är inte beviset.
5. Kontrollerad kylning och flatness/warpage-kontroll.
6. Separat profil och dokumentation för sida två om processen är dubbelsidig.

J-STD-020 beskriver klassificeringsprofil/tålighet, inte automatiskt den
optimala produktionsprofilen. Den verkliga profilen tas fram med paste-,
package-, PCB- och ugnsdata.

## Inspektion

- AOI för polaritet, passiver, synliga joints och komponentnärvaro.
- 2D/3D-röntgen av 100 % av DRAM och andra bottom-terminated components i EVT.
- X-ray-gränser för bridges, opens/insufficient solder, voids och ball shape
  ska avtalas med CM; vissa head-in-pillow/open-fel kräver kompletterande test.
- Elektrisk nettest före funktionell boot där fixtur kan nå relevanta nät utan
  höghastighetsstubbar.
- Vid återkommande fel: cross-section eller dye-and-pry på representativa kort,
  inte upprepad blind rework.

## Rework

FBGA-rework kräver profilerad reworkstation, bottom preheat, vision alignment,
kontrollerad site preparation och efterföljande röntgen. Sätt ett högsta antal
reworkcykler per site efter leverantörsdata. Kortet ska få nytt serienummer-/
reworkstatus i testdatabasen. Reworkkort får inte blandas med obrutna kort i
processkapabilitetsstatistik.

## CM-acceptanspaket

Inköpsordern ska bifoga:

- PCB fabrication drawing, stackup, impedans- och card-edge-krav;
- ODB++/IPC-2581 eller Gerber X2, NC drill och netlist;
- centroid/rotation, BOM/AVL, assembly drawing och paste-data;
- komponenternas MSL/reflow/package notes;
- stencil-, SPI-, AOI-, X-ray- och profilkrav;
- J-STD-001J/IPC-A-610J-klass och särskilda acceptansregler;
- serienummer-/lotspårning, avvikelsehantering och first-article report;
- golden sample och regler för materialsubstitution – inga byten utan skriftlig
  ECO och ny SI/PI/assembly-bedömning.

