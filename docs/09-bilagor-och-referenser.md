# Bilagor och öppna referensprojekt

De fem tillhandahållna PDF-filerna har analyserats lokalt. De ingår inte i det
publika repot; tabellen gör analysen spårbar utan att redistribuera dem.

## PDF-register

| ID | Dokument | SHA-256 | Användbart | Begränsning/källkritik |
|---|---|---|---|---|
| P1 | Micron, *DDR5: Client Module Features*, 2021, 6 sidor | `45af4651c63ac2d2fd3dfc0b68359631698a84ad779ce92d28059e57f235d6cb` | Två subkanaler, PMIC, SPD-hubb, fly-by/punkt-till-punkt, MIR/ODT | Introduktion; inga produktionsregler |
| P2 | Micron `MTC16C2085S1UC`, 32 GB 2Rx8, Rev F, 2021 | `a1ccf35c1d80046fb6ce98643b17e2fad21e0a22486d3f8e61cba8d8f30c738e` | 16 × 16 Gbit x8, DQ-swizzles, 240 ohm ZQ, rails, 4800/5600-artiklar | Ingen 6000-data; originalfilen har trasig PDF-xref men 8 dokumentsidor kunde renderas/läsas |
| P3 | Micron `MTC8C1084S1UC`, 16 GB 1Rx8, Rev F, 2021, 8 sidor | `3808acc800bcb5abf67a703053c034c08bbdc17e265fe323e0cb0a975d38d328` | En-rank-referens, DQ-map, blockdiagram, PMIC/SPD/ZQ | Fel kapacitet för 16 Gbit-delen; ingen 6000-data |
| P4 | Micron, *DDR5 SDRAM UDIMM Core*, Rev E, 2021, renderat 20 sidor | `c719ee99656a07f9be29733fae46a4d652732bf715f93c93a4e13cc549045e50` | Äldre 288-pin core, mekanik, 5 V bulk, PMIC/SPD, temperatur och SI-krav | Äldre än aktuella standarder; sannolikt tryckfel om PMIC-spänningsprocent; ska inte styra raw-card |
| P5 | Kingmax, 8/16/32 GB DDR5 UDIMM med Spectek IC, 2023, 16 sidor | `6b0e25d238e9d359b78edb96bbea325a2a5ec292c51eddf9de7abcf175e1b20c` | 32 GB 2Rx8, 16 Gbit x8, rails/ström och 4800/5600-artiklar | Skannat; bandbreddsenhet är feltryckt och refresh-tabell motsäger Micron |

### Verifierade detaljer från P2/P3/P5

- P2: `MTC16C2085S1UC48BA1` = 4800, 40-39-39;
  `MTC16C2085S1UC56BA1` = 5600, 46-45-45.
- P2:s 4800-data vid 5 V inklusive PMIC: burst read 757 mA, burst write
  993 mA och bank-interleave read 825 mA; 5600-rader är TBD i denna revision.
- P3 använder åtta 16 Gbit x8 och illustrerar fyra byte-lanes per subkanal.
- P5:s 32 GB-konfiguration är 16 × 2G × 8; 5600-tabellen anger burst read
  892 mA, burst write 1141 mA och bank-interleave read 1260 mA.
- Samtliga är referensvärden för sina exakta moduler, inte dimensionering för
  valt 6000-kisel.

## Öppna GitHub-referenser

### HimaSava `udimm_ddr5_tester`

- Kontrollerad commit: `7fb42da53f9efe6a5311a4351931b52e4c3c4152` (2024-11-25).
- Apache-2.0.
- KiCad-baserat FPGA-/DIMM-testkort med tolv kopparlager, stackupdata,
  length-tuning-rapport och fab-exporter.
- Användbart som exempel på DRC, stackup/length-report och testkortets
  dokumentpaket.
- **Inte** en DDR5 UDIMM-design eller en verifierad raw-card-källa. Dess
  impedanser/placement får inte kopieras till modulen.

### Open Memory Initiative

- Kontrollerad commit: `4644068ab3d9341d34afc466ed38542165e775be` (2026-06-02).
- DDR4, 8 GB, 1Rx8 UDIMM; schemastadium, ingen PCB eller byggd hårdvara.
- Styrka: dokument-first-metod, 288/288 pin-mapkontroll, ERC 0,
  exportmanifest och ärlig separation mellan L0-evidens och fysisk validering.
- Användbar som processreferens, **inte** som DDR5 pinout/layout.

### Bus Pirate DDR5-adapter

- Öppet dokumenterad adapter och procedur för DDR5-SPD/PMIC över sideband.
- Användbar för servicejigg, offline read/write och recovery.
- Den ansluter inte till den höghastiga DDR5-databussen och kan inte bevisa
  6000 MT/s-signalintegritet.

## Återanvändningsregel

Innan någon extern fil eller kod kopieras ska en provenance-post ange:

- upstream-URL och exakt commit/tag;
- fil, upphovsrätt och licens;
- om materialet kopierats, modifierats eller endast inspirerat metod;
- vilka notices/source obligations som följer;
- teknisk lämplighet för DDR5/denna topologi.

Ingen extern CAD har kopierats i den här insamlingen.

