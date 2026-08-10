# SPD, PMIC och prestandaprofiler

## Tre separata konfigurationer

1. **PMIC-konfiguration**: railnivåer, sequencing, telemetry, limits och secure-
   state.
2. **JEDEC SPD**: modulorganisation, timing, DQ-map, tillverkardata, CRC och
   fallbackprofiler.
3. **XMP/EXPO**: frivilliga överklockningsprofiler med egna programkrav.

De får inte utvecklas som en enda opak binärfil. Varje del ska ha en läsbar
källrepresentation, generator, verifierare och signerad releaseartefakt.

## SPD-arbetsflöde

1. Lås JESD400-5D.01 och JESD300-5B.01-revisionerna.
2. Exportera organisation, density/rank/device width och DQ-map från
   schema/netlist – skriv dem inte för hand på två ställen.
3. Ange JEDEC-fallback vid 4800 och 5600 innan 6000 aktiveras.
4. Lägg till 6000B endast när exakt DRAM AC/DC/timing och SI-resultat stödjer
   profilen.
5. Generera alla CRC-fält och verifiera mot en oberoende decoder.
6. Lägg till egna tillverkar-/produkt-/serienummerfält; kopiera inte en annan
   modultillverkares identitet eller hela SPD-image.
7. Skriv först till en olåst utvecklingsenhet och läs tillbaka byte-för-byte.
8. Testa kallstart, varmstart, CMOS reset, fallback och recovery.
9. Aktivera permanent/blockskydd först efter signerad release och bevarad
   golden image.

Repot ska senare innehålla exempelvis:

```text
spd/schema/         läsbar fältdefinition och källvärden
spd/generator/      deterministisk imagegenerator
spd/tests/          bounds-, CRC-, DQ-map- och round-trip-tester
spd/releases/       binär, dekoderutskrift, hash och release-manifest
```

Ingen påhittad SPD-binär ingår i denna förstudie eftersom den skulle vara
beroende av standardfält och komponentval som ännu är blockerade.

## Programmeringsjigg

Bus Pirates officiella DDR5-adapter/dokumentation visar ett öppet serviceflöde
för att läsa/skriva SPD och nå PMIC via DIMM:s sideband. Den är lämplig för
utveckling och recovery men är inte en DDR5-minnesbuss- eller SI-testare.

Jiggen ska innehålla:

- mekaniskt nycklad 288-pin socket/adapter med strömbegränsad försörjning;
- separat, mätbar `VIN_BULK` och korrekt sideband-I/O-spänning;
- enable/power-good-kontroll och möjlighet att isolera värdsystemet;
- loggning av verktygsversion, enhetsadress, före/efter-dump och hash;
- read-only standardläge och explicit `--enable-write`/bekräftelse för skrivning;
- recovery-instruktion för felaktig SPD och PMIC-programmering.

Linux `spd5118`-drivrutinen kan exponera temperatur och SPD-NVRAM via EEPROM-
gränssnitt på adresser 0x50–0x57. Det gör den bra för läsning/övervakning, men
produktionens skrivning bör ske offline i den kontrollerade jiggen.

## PMIC-flöde

Före första kortstart:

1. dokumentera PMIC:ens leveranstillstånd, programmeringsläge och security;
2. verifiera passiva värden, regulatorfaser och sense-nät mot referensdesign;
3. kraftsätt strömbegränsat utan DRAM om leverantören uttryckligen godkänner
   detta, annars med definierad dummy/testlast;
4. kontrollera sekvens, rise/fall, overshoot, ripple och power-good;
5. läs tillbaka register/telemetry och jämför med källkonfigurationen;
6. kvalificera felvägar: UVLO, OCP/OTP, disable, brownout och omstart;
7. lås MTP/secure-register först efter EVT.

PMIC-konfiguration ska versionsstyras som läsbar YAML/JSON plus generator,
inte enbart som skärmdump från ett leverantörs-GUI. Om GUI/exportformatet är
licensbegränsat lagras hash och intern referens i stället.

## Profilstege

| Steg | Profil | Avsikt | Får aktiveras när |
|---|---|---|---|
| P0 | Safe/offline | SPD/PMIC-service utan minnesbuss | Sideband och rails validerade |
| P1 | JEDEC 4800, 1,1 V | Första boot och felsökning | L1–L3 i valideringsplanen PASS |
| P2 | JEDEC 5600, 1,1 V | Referens mot bilagor/vanliga plattformar | P1 stabil över temperatur |
| P3 | JEDEC 6000B, 1,1 V | Produktens baseline | Fulla DRAM-data, SI/PI och plattformsmatris PASS |
| P4 | XMP 6000 | Intel-option | Intel-programvillkor och self-certification klar |
| P5 | EXPO 6000 | AMD-option | AMD-programvillkor och testmatris klar |

En OC-profil ska aldrig vara enda startväg. Misslyckad träning måste kunna
återgå till en känd JEDEC-profil utan att modulen blir obrukbar.

## Profilbevis

Varje release ska binda samman:

- SPD- och PMIC-källcommit samt binärhash;
- exakta DRAM/PMIC/SPD-orderkoder och loter;
- modul-PCB-revision och serienummer;
- CPU/moderkort/BIOS, DIMM-slot och DPC/rank;
- uppmätt rail-/temperaturdata och testloggar;
- eventuella WHEA-/MCA-/korrigerade fel, inte bara applikationens slutrad;
- vem som godkände profilen och när.

