# UDIMM-DDR5

Kunskaps- och konstruktionsunderlag för en egen 32 GB, 288-pin, non-ECC,
unbuffered DDR5 UDIMM med målhastighet 6000 MT/s.

> **Projektstatus 2026-08-10:** förstudie och kravinsamling. Inget schema,
> layout eller validerat kort finns ännu. Projektet är inte byggklart förrän de
> röda stoppgrindarna i [STATUS.md](STATUS.md) är stängda.

## Rekommenderad första arkitektur

- 32 GB, x64, non-ECC UDIMM med två oberoende 32-bitars subkanaler.
- Rev A: 2Rx8 med 16 × 16 Gbit x8 DRAM, eftersom de bifogade Micron- och
  Kingmax-moduldokumenten ger verifierbara referenser för denna topologi.
- Säker bring-up vid DDR5-4800/1,1 V; därefter 5600 och sist 6000.
- JEDEC DDR5-6000B (PC5-6000, 48-48-48, 1,1 V) och XMP/EXPO 6000 är separata
  leverabler. XMP/EXPO kräver dessutom program-, varumärkes- och
  plattformsvalidering.
- Ingen CKD/CUDIMM behövs enbart för 6000 MT/s.

Alternativet 1Rx8 med 8 × 32 Gbit x8 har lägre elektrisk belastning, men får
inte låsas förrän en verkligt köpbar DRAM-del med fullständigt datablad,
paketritning, IBIS-/paketmodell och leverantörsstöd är säkrad.

## Läsordning

1. [STATUS.md](STATUS.md) – beslut, blockerare och nästa grind.
2. [Krav och arkitektur](docs/01-krav-och-arkitektur.md).
3. [Standarder och åtkomst](docs/02-standarder-och-atkomst.md).
4. [Komponenter och inköp](docs/03-komponenter-och-inkop.md).
5. [Schema, PCB och SI/PI](docs/04-schema-pcb-si-pi.md).
6. [SPD, PMIC och profiler](docs/05-spd-pmic-profiler.md).
7. [Tillverkning och montering](docs/06-tillverkning-och-montering.md).
8. [Bring-up och validering](docs/07-bringup-och-validering.md).
9. [Regelverk och licenser](docs/08-regelverk-och-licenser.md).
10. [Bilagor och referensprojekt](docs/09-bilagor-och-referenser.md).
11. [Projektstyrning och riskregister](docs/10-projektstyrning.md).
12. [Källregister](docs/SOURCES.md).

Maskinläsbara arbetslistor finns i [`data/`](data/): krav, BOM-kandidater och
valideringsmatris.

## Viktig säkerhets- och kvalitetsgräns

DRAM-kretsarna är FBGA-komponenter. En reproducerbar modul kräver stencil,
maskinell placering, profilerad konvektionsreflow, MSL-styrning och röntgen.
Handlödning eller frihandsplacering med varmluft är inte ett godkänt
produktionsflöde. Se [monteringsplanen](docs/06-tillverkning-och-montering.md).

## Källpolicy

Projektet återpublicerar inte JEDEC-/IPC-standarder, leverantörsdokument under
inloggning/NDA eller de fem tillhandahållna PDF-filerna. Repot lagrar i stället
spårbara sammanfattningar, dokument-ID, revisionskrav och länkar till laglig
anskaffning. En sammanfattning är aldrig ersättning för den styrande utgåvan.
