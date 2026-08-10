# Regelverk, marknad och licenser

Detta är en teknisk compliance-checklista, inte juridisk rådgivning. Den
slutliga klassningen ska bekräftas av ansvarig ekonomisk aktör/compliance-
specialist innan produkten sätts på marknaden.

## Två skilda lägen

### Intern FoU-prototyp

Ett kort som stannar i ett professionellt FoU-labb och inte görs tillgängligt
på marknaden kan omfattas annorlunda av vissa produktregler. Behåll ändå
materials-, säkerhets-, avfalls- och spårbarhetsdata; prototypundantag får inte
användas som generell väg till försäljning.

### Produkt för Sverige/EES

En retail-DIMM är en elektronisk komponent/subassembly avsedd att sättas in av
slutanvändaren. EU:s EMC-guide anger dator-plug-in-kort som exempel på delar
som kan omfattas när de kan generera eller påverkas av elektromagnetiska
störningar. Att en DIMM sannolikt omfattas av EMC-direktivet är därför en
rimlig inferens, men ska formellt verifieras för exakt säljmodell.

## Compliance-matris

| Område | Projektbedömning | Krävd handling före försäljning |
|---|---|---|
| EMC 2014/30/EU | Sannolikt tillämpligt för fristående retail-modul | Klassning, risk/essential requirements, harmoniserad teststrategi, teknisk fil, EU DoC och CE om tillämpligt |
| RoHS 2011/65/EU + (EU) 2015/863 | Elektrisk/elektronisk utrustning; sannolikt tillämpligt | Leverantörsdeklarationer/materialdata, conformity assessment, teknisk fil/DoC och CE |
| LVD 2014/35/EU | Normalt inte tillämpligt: scope börjar vid 75 V DC och DIMM använder cirka 5 V max | Dokumentera scope-bedömningen; annan produktsäkerhet kvarstår |
| REACH | Tillämpliga artikel-/SVHC-skyldigheter | Full materialdeklaration; kundinformation om Candidate List-ämne ≥0,1 vikt-%; bedöm SCIP-anmälan |
| WEEE/producentansvar | Sannolikt om elutrustning görs tillgänglig i Sverige | Registrering/rapportering, finansiering/insamling, märkning och återvinningsinformation enligt svensk regel |
| GPSR (EU) 2023/988 | Gäller konsumentprodukter sedan 2024-12-13 där sektorsregler inte täcker alla risker | Riskanalys, spårbarhet/kontakt, säkerhetsinformation, klagomål/incident/recall-process |
| Förpackningsansvar | Bedöm för retailförpackning i Sverige | Producentansvar, materialdata och rapportering enligt aktuell svensk/EU-regel |

CE får inte sättas dit “för säkerhets skull”. Märket används endast när en
tillämplig harmoniseringsakt kräver det och efter att tillverkaren genomfört
conformity assessment, teknisk fil och EU-försäkran om överensstämmelse.

## Teknisk fil

Planera minst följande:

- produktbeskrivning, avsedd användning, varianter och riskanalys;
- schema, PCB, BOM/AVL, SPD/PMIC-release och konfigurationsstyrning;
- tillämpliga lagar/standarder och motivering för scope/undantag;
- EMC-/säkerhets-/material-/reliabilitetsrapporter och rådata;
- leverantörernas RoHS/REACH/material/CoC/PCN-underlag;
- tillverknings- och kvalitetsflöde, serienummer/lot och avvikelser;
- etikett, bruks-/installations-/säkerhetsinformation på relevanta språk;
- EU DoC, ansvarig tillverkare/importör och kontaktadress;
- post-market surveillance, klagomål, incident, corrective action och recall;
- lagringstid enligt respektive regelverk; EMC-guiden anger tio år för teknisk
  dokumentation/DoC efter att produkten satts på marknaden.

## EMC-teststrategi

Klassningen av DIMM som komponent och den avsedda värdmiljön styr metoden.
Planen ska täcka emission och immunity i representativa datorplattformar,
worst-case 6000-trafik, kablage/chassi/PSU och både JEDEC/OC-profiler där de
marknadsförs. Ett stabilitetstest i en öppen testbänk ersätter inte en formell
EMC-bedömning.

## Öppen källkod och hårdvarulicens

Licens ska beslutas innan externa bidrag eller kopierat designmaterial tas in.
Ett rimligt, ännu ej beslutat upplägg är:

| Artefakt | Kandidat | Konsekvens |
|---|---|---|
| KiCad/schema/PCB/HDL | CERN-OHL-S-2.0, -W-2.0 eller -P-2.0 | Välj stark, svag eller permissiv reciprocitet medvetet |
| Dokumentation | CC BY 4.0 eller CC BY-SA 4.0 | Attribution; SA kräver samma licens för bearbetningar |
| Generator-/testmjukvara | Apache-2.0 eller MIT | Apache-2.0 har uttryckligt patentbidrag |
| Binära leverantörsmodeller/standarder | Ingen publik redistribution utan tillstånd | Förvara internt; publicera metadata/hash och anskaffningsväg |

Open Memory Initiative använder vid den kontrollerade referenscommitten
CERN-OHL-S-2.0 för hårdvara, CC BY-SA 4.0 för dokumentation och Apache-2.0 för
mjukvara. HimaSavas DDR5-testkort är Apache-2.0-licensierat. Denna kunskapsbas
har endast granskat dem som metod-/verktygsreferenser och har inte kopierat CAD.

XMP och EXPO innefattar också varumärkes-/programvillkor. En öppen SPD-
generator ger inte automatiskt rätt att använda märken eller göra certifierade
kompatibilitetsclaims.

## Export, inköp och informationsrättigheter

- Kontrollera export-/sanktionsvillkor hos komponent- och modellleverantörer.
- Acceptera inte NDA som omöjliggör tillverkning, reparation eller lagstadgad
  teknisk dokumentation utan en tydlig intern accessplan.
- Spara vem som har rätt att använda varje IBIS-/package-/registerfil och för
  vilka produkter/fabriker den får delas.
- Publicera inte de fem bifogade PDF-filerna förrän rättighetshavaren uttryckligen
  tillåter redistribution.

