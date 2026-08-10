# Krav och arkitektur

## Produktdefinition

Målet är en 288-pin DDR5 UDIMM för klientplattformar med följande primära
krav:

| Egenskap | Mål | Kommentar |
|---|---|---|
| Kapacitet | 32 GB | 32 GiB adresserbar modulkapacitet enligt minnesbranschens märkning |
| Databredd | x64, non-ECC | Två oberoende 32-bitars DDR5-subkanaler |
| Formfaktor | Unbuffered DIMM | Inte RDIMM, LRDIMM, SO-DIMM eller CAMM |
| Kontakter | 288 | Mekanik och pinout styrs av JESD308B/MO-329 |
| Baseline | JEDEC DDR5-6000B | PC5-6000, 3000 MHz CK, 6000 MT/s, 48-48-48, nominellt 1,1 V |
| Fallback | 4800 och 5600 | Ska finnas för bring-up och kompatibilitet |
| OC-profiler | Separat option | Intel XMP 3.0 och AMD EXPO får inte blandas ihop med JEDEC-baslinjen |
| Primär konfiguration | 1 DIMM per kanal | 2DPC är ett separat och betydligt hårdare valideringsmål |
| Miljö | Klient/kommersiell | Temperaturgränser måste låsas mot exakt DRAM och produktkrav |

`DDR5-6000` är inte i sig ett fullständigt krav. Det ska alltid följas av
timing, spänning, rank/DPC, plattform, temperatur och acceptanskriterium.

## DDR5-modulens funktionsblock

- 64 DQ-signaler fördelade på två 32-bitars subkanaler.
- DQ/DQS går punkt-till-punkt inom respektive byte-lane; kommando/adress,
  klocka och chip-select följer raw-card-standardens topologi.
- Åtta x8-enheter per rank ger 64 bitar. 2Rx8 använder därför 16 enheter och
  1Rx8 åtta.
- En SPD5118-kompatibel hubb ger SPD-NVM, I2C/I3C Basic-sideband och vanligtvis
  temperaturmätning.
- En klient-DIMM-PMIC skapar VDD, VDDQ, VPP och sidebandrälsar från 5 V
  `VIN_BULK`.
- Varje DRAM behöver ZQ-kalibreringsmotstånd enligt leverantör/raw-card; de
  bifogade referensmodulerna använder 240 ohm ±1 %.

## Topologival

| Alternativ | Bestyckning | Fördelar | Risker | Rekommendation |
|---|---|---|---|---|
| 2Rx8 | 16 × 16 Gbit x8 | Två bifogade 32 GB-referenser, etablerad deldensitet, två ranks kan gynna viss last | Högre last, fler FBGA, högre effekt och svårare 6000-marginal | **Rev A-baslinje** om fulla data för 16 Gbit-delen erhålls |
| 1Rx8 | 8 × 32 Gbit x8 | Färre laster, komponenter och lödfogar; bättre SI-förutsättningar | Sämre småkvantitetstillgång, färre öppna modulreferenser, modeller ofta konto-/NDA-låsta | **Elektriskt slutmål**, men endast efter G1 |

MIR/clamshell, fysisk sida, komponentordning, byte-lane-swizzling och rankval ska
tas från vald raw-card och exakt DRAM-ball map. De får inte extrapoleras från
ett foto eller från DDR4.

## Prestandaprofiler

### JEDEC-baslinje

Microns officiella modulnumreringsguide anger `DDR5-6000B` som 6000 MT/s,
3000 MHz klocka och primära timingar 48-48-48. En kommersiell 1,1 V,
32 GB-modul med DDR5-6000 CL48 finns också hos TeamGroup, vilket visar att
JEDEC-6000 som klientmål är kommersiellt verkligt. Den produktsidan bevisar
inte rank eller råkort för detta projekt.

### XMP/EXPO

XMP 3.0 och EXPO är överklockningsprofiler och egna leverabler. De kräver:

- en säker JEDEC-profil som alltid kan starta;
- vald profilspänning och full uppsättning sekundära/tertiära timingar;
- PMIC som lagligt och tekniskt kan programmeras till dessa rälsar;
- plattformstest och program-/varumärkesvillkor;
- tydlig märkning att överklockning kan ligga utanför CPU:ns garanterade
  minnesspecifikation.

Intel beskriver upp till fem XMP 3.0-profiler (tre leverantörsprofiler och två
omskrivbara) och ett self-certification-flöde. AMD publicerar kompatibilitets-
listor för EXPO men ingen komplett publik bytekodningsspecifikation hittades i
denna insamling. Profilformat får därför inte reverse-engineeras in i en
produkt utan rätt programunderlag.

## Plattformskonsekvens

6000 MT/s är överklockning på flera vanliga plattformar:

- AMD Ryzen 9 7950X anger officiellt DDR5-5200 vid 2x1R/2x2R och 3600 vid
  fyra DIMM-konfigurationer.
- AMD Ryzen 9 9950X anger DDR5-5600 vid 2x1R/2x2R och 3600 vid fyra DIMM.
- Intel Core i9-14900K anger upp till DDR5-5600.
- Intel Core Ultra 9 285-familjen anger upp till DDR5-6400, beroende på
  konfiguration.

Det innebär att ett allmänt påstående om “6000-kompatibel” måste knytas till
en explicit moderkorts-/CPU-/BIOS-matris. Se valideringsplanen.

## Icke-mål för första revisionen

- ECC x72, RDIMM/LRDIMM eller serverplattformar.
- CUDIMM/CKD; JEDEC introducerade klient-råkort med clock driver för 6400 MT/s
  och högre, så CKD är inte ett krav enbart för 6000.
- Garanti för 2DPC eller blandning med andra DIMM-modeller.
- Egen DRAM-kiselkarakterisering eller egen PMIC-design.
- Att publicera kopior av licensierade JEDEC-/IPC-dokument.

