# Root CA-krav i EU-CCMS L0 och L1
## Inledning
För att en organisation ska kunna fungera som Root CA i EU-CCMS på nivå L1 krävs en lång rad organisatoriska, tekniska och procedurmässiga åtgärder. Kraven är fastlagda i Certificate Policy (CP), Security Policy (SP) samt i CPOC-protokollet. Här är en detaljerad lista sammanställd från dessa dokument:

Nivå L0 är en ”pre-production / trial-miljö”, alltså till för uppstart och test inför full drift i L1/L2. Kraven är därför enklare, men det finns fortfarande tydliga regler.

## Root CA i L1
### 1. Ansökan, godkännande och formella processer
**Enrolment & ansökan**:
* Skicka in <mark>RCA Enrolment Form</mark> (administrativ information, auktoriserade representanter, revisionsrapport) till CPA.
* Skicka in <mark>RCA Application Form</mark> inklusive hash av det självsignerade RCA-certifikatet och övriga metadata.
* Få <mark>CPA-godkännande</mark> genom RCA Application Approval Form, som sedan tillåter CPOC/TLM att inkludera RCA i ECTL.

**PKI-revision**:
En ackrediterad PKI-auditor måste genomföra revision av Root CA:s CPS (Certificate Practice Statement) innan drift.

Auditrapporter skickas till CPA, som beslutar om godkännande.

Revisioner måste genomföras regelbundet enligt CP (se kapitel 8 i Certificate Policy for Deployment and Operation of European Cooperative
Intelligent Transport Systems (C-ITS), EU C-ITS Certificate Policy, Release 3.0 May 2024).

### 2. Organisatoriska krav
**Certificate Practice Statement (CPS)**: Root CA ska publicera och underhålla en CPS som är i full överensstämmelse med CP. Alla ändringar ska granskas och godkännas av ackrediterad PKI-auditor innan införande.

**Roller och separation**: Root CA måste definiera och separera trusted roles, med tydliga ansvarsområden, ansvar och åtkomstkontroller. Separation of duties krävs för kritiska funktioner.

**Personal**: Personal måste ha säkerhetsprövning, bakgrundskontroller, utbildning och dokumenterade roller. Dokumenterade rutiner för sanktioner, omplacering och regelbunden utbildning.

### 3. Fysiska och proceduriella kontroller
**Fysisk säkerhet**: Root CA-anläggningen ska vara fysisk skyddad (säker byggnad, åtkomstkontroller, brand- och översvämningsskydd, redundans för ström och kyla). Off-site backuper krävs.

**Procedurkontroller**: Minst två personer för känsliga uppgifter (t.ex. nyckelgenerering). Loggning och revision av alla säkerhetskritiska aktiviteter. Incident- och katastrofhanteringsplan, inkl. nyckelkomprometteringsprocedurer.

### 4. Tekniska säkerhetskrav
**Nyckelhantering**: Root CA:s privata nycklar ska genereras i FIPS 140-2 L3 eller motsvarande HSM. Nyckellängder och algoritmer ska följa CP:s krav (BrainpoolP384r1 eller motsvarande). Nycklar ska aldrig exporteras i klartext; backuper krypteras. Maximal giltighetstid för ett Root CA-certifikat: 5 år.

**Certifikatutfärdande**: Root CA får endast utfärda certifikat till EA (Enrolment Authorities) och AA (Authorisation Authorities). Certifikaten ska vara i ETSI TS 103 097 format och uppfylla EU:s krav för appPermissions och certIssuePermissions.

**Säker drift**: Root CA ska ha starka systemskydd (nätverkssäkerhet, livscykelkontroller, patchhantering, intrångsdetektering).

### 5. Integration i EU-CCMS L1
**ECTL (European Certificate Trust List)**: Root CA:s certifikat måste införas i ECTL genom CPOC/TLM efter CPA-godkännande. RCA-certifikat måste uppfylla de bindande tekniska kraven på attribut och värden enligt CPOC-protokollets Annex I.

**Enrolmentnivå**: På L1 gäller att Root CA måste uppfylla EU:s bindande CP/SP men vissa undantag finns (se "L1 exceptions"). Root CA måste visa upp att deras C-ITS PKI (inkl. sub-CAs) kan leverera säkra enrolment credentials och ATs enligt EU-krav.

**Revisionscykel**: Root CA på L1 måste genomgå återkommande kontroller av ackrediterad PKI-auditor och rapportera till CPA.


## Root CA i L0
### 1. Grundläggande krav för L0
**Syfte**: L0 är en ”entry level” för att testa och utvärdera C-ITS PKI-lösningar innan full L1-godkännande.

**Begränsningar**:
* Certifikat på L0 får inte användas i produktion för säkerhetskritiska tjänster.
* Kommunikation från L0-RCAs ska bara tolkas av L0-klienter (stations-TOEs konfigurerade för L0).
* De ska vara tydligt avskilda från L1/L2 i ECTL (egna listor, egna TLM-certifikat med suffix ”_L0”).

### 2. Ansökan och godkännande
**RCA Enrolment Form**: Organisationen måste fylla i och skicka in denna till CPA, men kraven är enklare än för L1 (audit kan vara begränsad).

**RCA Application Form**: Måste skickas in med hash av självsignerat RCA-certifikat och administrativa data.

**Godkännande från CPA**: Krävs fortfarande, men det räcker ofta med ett förenklat förfarande (”lightweight approval”).

### 3. Certifikatkrav
**Format**: Samma ETSI TS 103 097 certifikatformat som L1, men vissa fält kan vara enklare eller mindre strikt kontrollerade.

**Attribut**: Måste följa minimiuppsättning attribut enligt CP/SP. Validitetstiden kan vara kortare (t.ex. < 2 år) för att spegla testkaraktären.

**Namn/id**: Certifikaten ska bära en tydlig suffix/identifiering som markerar att det är ett L0-RCA.

### 4. Organisationskrav
**CPS (Certificate Practice Statement)**: Måste finnas, men det kan vara en enklare variant än L1. Bör beskriva processer, nyckelhantering, rutiner för testmiljön.

**Personal och roller**: Minimikrav på att utse en auktoriserad representant (AR). Roller och separation behöver inte vara lika strikt kontrollerade som i L1, men ska vara dokumenterade.

### 5. Tekniska säkerhetskrav
**Nyckelhantering**: Nycklar kan genereras i en säkrare mjukvarumiljö eller enklare HSM – det behöver inte vara full FIPS L3-HSM som i L1. Backup och återställning måste beskrivas, men kan vara enklare.

**Driftmiljö**: Ska vara tillräckligt säker för att undvika obehörig åtkomst, men det är inte krav på full fysisk säkerhetsklass som i L1.

### 6. Integration i EU-CCMS L0
**ECTL**: RCA-certifikat publiceras i en särskild L0-ECTL som signeras av ett särskilt TLM-certifikat med suffix ”_L0”.

**Avskildhet**: Ingen korsning mellan L0 och L1/L2-miljöer. Klienter (C-ITS stationer) måste konfigureras så att de vet om de är i L0 och bara accepterar L0-ECTL.

### 7. Revisionskrav
**Audit**: För L0 kan revision göras internt eller av enklare extern part. Det är inte krav på ackrediterad PKI-auditor, men rekommenderas om organisationen siktar på L1-uppgradering.

**Rapportering**: Resultat och erfarenheter ska rapporteras till CPA. Eventuella brister måste åtgärdas innan uppflyttning till L1.

### 8. Övergång till L1
Syftet med L0 är att bygga erfarenhet.
För att gå från L0 → L1 måste organisationen:
* Uppgradera CPS till full CP-kompatibilitet.
* Införa ackrediterad audit.
* Uppfylla alla organisatoriska, tekniska och säkerhetskrav i L1.

## Jämförelse Root CA-krav i EU-CCMS L0 vs L1

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

