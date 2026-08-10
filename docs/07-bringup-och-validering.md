# Bring-up och validering

## Bevisnivåer

| Nivå | Fråga | Minsta evidens |
|---|---|---|
| L0 – Artifact integrity | Är designunderlaget självkonsistent? | ERC/DRC, pin-/DQ-map-test, manifest och reproducerbar export |
| L1 – Incoming/assembly | Byggdes rätt kort med rätt material? | CoC/lot, fab coupon/TDR, profil, SPI/AOI/X-ray och first-article report |
| L2 – Unpowered | Finns shorts, opens eller felvända delar? | Visuell/X-ray, nettest och rail resistance/diode-signatur mot golden limits |
| L3 – Sideband/power | Kan SPD/PMIC nås och ger rails rätt förlopp? | Full dump, register-ID, oscilloskopkurvor, ripple/transient och termik |
| L4 – First boot | Tränar modulen säkert? | POST/BIOS-logg, korrekt SPD-decode och första boot vid 4800 |
| L5 – Speed ladder | Är 4800, 5600 och 6000 stabila var för sig? | Tränings-/fel-/temperaturloggar och samma testsvit per hastighet |
| L6 – Platform matrix | Är beteendet reproducerbart på målplattformar? | CPU/moderkort/BIOS/slot/DPC/rank per körning |
| L7 – Margin/environment | Finns faktisk elektrisk och termisk marginal? | SI-mätning, railmargining, temperaturhörn och reproducerade gränser |
| L8 – Qualification | Är produkt/process redo att säljas? | Pilotstatistik, miljö-/reliabilitet, regelverk, traceability och signerad release |

## Första kraftsättning

1. Kontrollera kortrevision, serienummer, komponentorientering och assembly-
   rapport utan nätspänning.
2. Mät varje rail mot jord och jämför med gränser från en analyserad golden
   design; använd inte ett universellt ohm-tal för DRAM-rails.
3. Anslut endast via en DIMM-/servicefixtur med definierad pinout,
   strömbegränsning och nödstopp.
4. Läs SPD-/PMIC-identitet i read-only-läge innan någon NVM/MTP-skrivning.
5. Aktivera `VIN_BULK`/enable enligt PMIC-sekvensen och mät alla rails med
   låginduktiva prober. Stoppa på fel sekvens, overshoot, ström eller temperatur.
6. Sätt modulen i en testplattform med känd god CPU/board/BIOS och endast en
   DIMM i rekommenderad slot.
7. Starta med JEDEC-4800 och säkra BIOS fallback/CMOS reset innan 5600/6000.

## Funktionell teststege

Följande är en initial projektbaseline som ska ratificeras i G0, inte en JEDEC-
produktkvalifikation:

- minst 10 kalla starter och 10 varma omstarter per releaseplattform;
- full MemTest86-svit, föreslaget minst fyra kompletta passes, utan fel;
- Linux `stressapptest` eller motsvarande randomiserad memory-interface-last,
  föreslaget minst åtta timmar per hastighet;
- slutlig 6000-profil: föreslaget 24 timmar kombinerad minnes-/I/O-last på varje
  primär plattform;
- test vid definierad låg/rum/hög modultemperatur, med termisk stabilisering;
- power-cycle och suspend/resume där plattformen stödjer det;
- separat test av en och två moduler; 2DPC markeras unsupported tills det
  uttryckligen klarats.

Längre körtid får inte kompensera för avsaknad av elektrisk marginalmätning.
MemTest86:s egen dokumentation påpekar att testet också belastar CPU, cache och
moderkort; ett fel pekar inte automatiskt ut DIMM:en.

## Diagnostik och isolering

Vid fel, byt en variabel i taget:

- kör samma DUT i annan verifierad slot/plattform;
- kör en känd god kommersiell DIMM i samma platform/slot;
- sänk till 4800 och sedan 5600 utan att samtidigt ändra spänning/timing;
- logga BIOS training retries, WHEA/MCA/korrigerade fel och OS-kernel log;
- läs PMIC-fault/telemetry och SPD-temperatur före, under och efter last;
- korrelera bitadress till subkanal/byte/rank genom den genererade DQ-mappen;
- jämför lot, assembly X-ray och reworkhistorik;
- ta ett felkort till SI/PI-mätning eller fysisk analys innan designen ändras.

Ett `PASS` från stressverktyget godkänns inte om loggen samtidigt innehåller
miscompare, CRC, WHEA/MCA eller korrigerade fel.

## Plattformsmatris

Minsta rekommenderade matris:

| Grupp | Exempel | Syfte |
|---|---|---|
| Intel modern | Z890 + Core Ultra 200S | 6000 ligger inom familjens annonserade maxområde för vissa konfigurationer |
| Intel äldre/OC | Z790 + 14:e gen Core | 6000 är över officiella 5600 och testar XMP/fallback |
| AMD modern | X870E/B850 + Ryzen 9000 | EXPO och 6000-OC på aktuell AM5 |
| AMD etablerad | X670E/B650 + Ryzen 7000 | BIOS-/memory-controller-varians och fallback |

Exakta moderkortsmodeller väljs efter QVL, tillgång till BIOS-logg/margining och
minst två olika board vendors. Lås BIOS-version och stäng av automatiska
uppdateringar under en testkampanj.

## Mätutrustning

Baslabb:

- ESD-arbetsplats, mikroskop och termisk kamera;
- strömbegränsad, loggbar försörjning/servicefixtur;
- DMM och oscilloskop med lämpliga låginduktiva rail-prober;
- SPD/PMIC-sidebandverktyg och känd god testplattform;
- temperaturkammare eller kontrollerad lokal uppvärmning/kylning med kalibrerad
  sensor;
- tillgång till 2D/3D-röntgen.

Elektrisk DDR5-korrelation kräver långt mer än en vanlig passiv skopprob.
Keysight erbjuder DDR5 Tx-/Rx-complianceverktyg och Teledyne LeCroy erbjuder
systemnivåtest vid DRAM-BGA via interposer, JEDEC-specifika ögon/masker och
automatisk rapport. Budgetera hyr-/partnerlabb om denna utrustning saknas.

## Releasekriterium för “DDR5-6000”

En revision får märkas DDR5-6000 först när:

- 6000-profilens exakta timing/spänning och rank/DPC är versionslåsta;
- post-layout SI/PI och fysisk rail-/signal-mätning har godkänd marginal;
- alla primära plattformar klarar kall/varm boot och testsvit utan tysta fel;
- temperatur- och lotvariation är representerad, inte bara ett golden sample;
- SPD/PMIC är reproducerbart provisionerade och skyddade;
- assembly yield och failure analysis visar att inga öppna systematiska fel
  återstår;
- claimen tydligt säger JEDEC, XMP eller EXPO – aldrig bara “6000”.

