# Jämförelse: Root CA-krav i EU-CCMS (L0 vs L1)

| **Kategori**            | **L0 (Level 0)**                                                                 | **L1 (Level 1)**                                                                                 |
|--------------------------|----------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| **Syfte**                | Test- och pilotmiljö. Ej för produktion.                                         | Första produktionsnivån. Används i skarp drift av C-ITS-tjänster.                                 |
| **ECTL/TLM**             | Separat **L0-ECTL**, signerad av särskilt TLM-certifikat med suffix `_L0`.       | Ingår i produktions-ECTL signerad av TLM.                                                        |
| **Certifikatutfärdande** | Samma ETSI TS 103 097-format, men enklare krav på attribut.                      | Fullt CP/SP-kompatibla RCA-certifikat med strikt attributs- och rättighetskontroll.               |
| **CPS (Policy)**         | Enkel CPS (Certificate Practice Statement) som beskriver grundläggande rutiner.  | Fullständig CPS i linje med EU-CCMS Certificate Policy, godkänd av ackrediterad auditor.          |
| **Ansökan**              | RCA Enrolment Form + Application Form → förenklad CPA-godkännande.               | RCA Enrolment Form + Application Form → full CPA-granskning och godkännande.                      |
| **Revision**             | Intern eller förenklad extern audit. Ackrediterad PKI-auditor **ej krav**.       | Revision av ackrediterad PKI-auditor **krav**. Årlig rapportering till CPA.                       |
| **Personal & roller**    | Minst en auktoriserad representant (AR). Separation av roller rekommenderas.     | Strikt separation av trusted roles, personalprövning, bakgrundskontroller och utbildning krävs.   |
| **Nyckelhantering**      | Nycklar kan genereras i enklare HSM eller säker mjukvara.                        | Root-nycklar ska genereras och lagras i FIPS 140-2 L3 (eller högre) HSM.                          |
| **Fysisk säkerhet**      | Grundläggande fysisk säkerhet (åtkomstkontroller, backup, redundans).            | Full fysisk säkerhet (säker anläggning, kontrollerat tillträde, miljöskydd, redundans).            |
| **Giltighetstid certifikat** | Kortare (t.ex. < 2 år) för att spegla testkaraktären.                          | Max 5 år per RCA-certifikat enligt CP.                                                            |
| **Utfärdade certifikat** | Kan användas för EA/AA i testmiljö, ej för produktionsfordon.                    | Kan utfärda giltiga certifikat till EA och AA i produktion.                                        |
| **Integration i EU-CCMS**| L0-klienter (C-ITS stationer) accepterar endast L0-ECTL.                         | Alla produktionsstationer accepterar L1-ECTL (del av EU-gemensam trust chain).                    |
| **Uppgradering**         | Syftar till att samla erfarenheter. Kräver uppgradering till L1 för produktion.  | Redan kvalificerad för produktion, kan senare uppgraderas till L2 med ytterligare krav.            |

