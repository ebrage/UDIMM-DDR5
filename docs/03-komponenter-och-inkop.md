# Komponenter och inköp

Detta är en kandidatlista, inte en godkänd AVL. Exakt orderkod får flyttas till
schema/BOM först när alla G1-underlag finns och ett spårbart köp är möjligt.

## DRAM-kandidater

| Topologi | Kandidat | Publikt verifierbart | Saknas före val |
|---|---|---|---|
| 2Rx8 | Micron `MT60B2G8RZ-64B:H` (produkt-URL `...-64b-h`) | 16 Gbit, x8, upp till 6400 MT/s, 1,1 V, 78-ball-familj | Fullt Die Rev H-datablad, exact ball map/package, modeller, power data, provlager |
| 2Rx8 industriell | Micron `MT60B2G8RZ-64B:IT-H` | Temperaturvariant identifierad i katalogen | Samma paket som ovan plus kostnad/temperaturkrav |
| 1Rx8 | Micron `MT60B4G8AT-64B:B` (produkt-URL `...-64b-b`) | 32 Gbit, x8, upp till 6400 MT/s, 1,1 V-katalogfamilj | Fullt datablad och modeller är konto-/supportberoende; köpbar småkvantitet ej bevisad |
| 1Rx8 familj | Samsung 32 Gbit DDR5 | Samsung anger 16–32 Gbit DDR5 och har annonserat 32 Gbit-kisel | Exakt klientorderkod, package, modeller och distributionskanal |
| 1Rx8/2Rx8 familj | SK hynix DDR5 6,4 Gb/s | 1bnm DDR5-6400-familj offentligt beskriven | Exakt orderkod och allt konstruktionsunderlag |

De två exakta Micron-delarna är tekniskt rimliga kandidater, inte inköpsklara
val. Microns produktsidor visar att fler tekniska dokument och modeller kräver
konto/supportkontakt.

### DRAM-inköpsregel

- Köp första EVT-loten direkt från tillverkare eller auktoriserad distributör.
- Kräv obruten MSL-förpackning, moisture indicator card, lot/date code,
  CoC/traceability och tillverkarens etikett.
- Blanda inte die revisioner eller loter i samma korrelationsgrupp.
- Köp reserv för processinställning, destruktiv analys och rework.
- Använd inte märkesskrapade marketplace-delar eller donor chips som
  valideringsbevis.
- Kontrollera om kulmetallurgi, MSL eller paket ändrats genom PCN.

## PMIC-kandidater

| Kandidat | Styrka | Kritisk kontroll |
|---|---|---|
| Richtek `RTQ5136` | Aktiv; uttryckligen för både normal och overclocking DDR5 UDIMM/SODIMM; 3 buck + 2 LDO; I2C/I3C; SWA/SWB programmerbara upp till 2,2 V | Fullt datablad, referens-BOM/layout, MTP/security och tillåten orderkod måste begäras |
| Renesas `P8911-Y0Z001FNG/G8` | Aktiv klient-DIMM-PMIC; 4,25–5,5 V, 3 buck + 2 LDO, programmability/telemetry | Publik sida visade ingen lagerkvantitet; short-form räcker inte för design |
| Rambus `P1535Gxx` | PMIC5100, klient UDIMM/CUDIMM-stöd | Suffix, konfiguration, referensdesign och kommersiell åtkomst |
| Rambus `P2535Gxx` | PMIC5120, nyare standardfamilj | Kontrollera behov/kompatibilitet mot vald raw-card och SPD |

Richtek RTQ5136 är den starkaste OC-kandidaten i öppna data, men ingen PMIC får
väljas på maximal utspänning ensam. Verifiera ström per rail, transienter,
induktorer, switchfrekvens, termik, skydd, sequencing och hostens policy.

För en eventuell 1,35 V-profil ska VDD/VDDQ och VPP behandlas separat; anta
inte att alla rails höjs. VPP för standard-DDR5 är nominellt 1,8 V.

## SPD-hubbkandidater

| Kandidat | Publika egenskaper | Före schema |
|---|---|---|
| Montage `M88SPD5118` | 8 Kbit NVM, I2C/I3C Basic hubb, temperaturgivare, 16 NVM-block, upp till 12,5 MHz | Package/orderkod, skrivskydd, fulla register och provisioneringsflöde |
| Renesas `SPD5118-Y1B000NCG8` | DDR5 SPD5118-familj med temperaturgivare | Lös motsägande generiska webbdata med det fulla databladet |
| Rambus `SPD5118-Gxx` | UDIMM-stöd, integrerad temperatur, I2C/I3C | Exakt suffix och åtkomst till fulla data |

Hubbens NVM är 1024 byte/8 Kbit i de aktuella familjerna. Exakt blocksäkerhet,
I/O-spänning och adressering ska tas från vald orderkod och JESD300-5B.01.

## Passiver och mekanik

Följande kategorier ska finnas i BOM men värden/antal ska genereras från vald
raw-card och referensdesign:

- ZQ-motstånd per DRAM; 240 ohm ±1 % är verifierat i de bifogade 2Rx8/1Rx8-
  moduldokumenten men ska bekräftas mot exakt kisel;
- DRAM-avkoppling på VDD, VDDQ och VPP med anti-resonanskontrollerad blandning;
- PMIC-induktorer, input/output-kondensatorer, bootstrap, straps och sense;
- SPD-/sideband-pullups, adressering och avkoppling;
- termiska vias/kopparytor där PMIC-tillverkaren föreskriver dem;
- 288-pad card edge med standardstyrd form, notch, fasning och finish.

Lås inte generiska kondensatorvärden eller en “10-/12-lagers stackup” i BOM.
PDN och fabrikens impedanskrav avgör detta.

## RFQ-underlag

Komponent-RFQ ska minst fråga efter:

1. exakt manufacturer part number och die/package revision;
2. active/NRND/EOL-status och planerad livslängd;
3. provkvantitet, MOQ, prissteg, lead time och NCNR-villkor;
4. auktoriserad spårbarhet, CoC och land of origin;
5. MSL, floor life, bake/reflow, RoHS/REACH/halogen/material declaration;
6. datasheet, errata, IBIS/package/power models och verktygsversion;
7. PCN-notifikation och change-control;
8. för PMIC/SPD: register guide, programmeringsverktyg och secure-state vid leverans.

Svar och bilagor ska versionslagras internt; licensbegränsat material ska bara
refereras med dokument-ID i detta publika repo.

