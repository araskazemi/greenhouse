# Transportsystemets digitalisering
Digitaliseringen av transportsystemet går i en rasande fart. Nya teknologier som artificiell intelligens, uppkopplade fordon, autonoma transportlösningar och smart infrastruktur förändrar snabbt hur människor och varor rör sig. Denna transformation öppnar upp för nya möjligheter, men medför också komplexa utmaningar, särskilt när det gäller att skapa säkra och effektiva plattformar för datadelning. 

I detta papper fokuserar vi på hur vi kan forma en trygg och säker utrullning av <mark>C-ITS (Cooperative Intelligent Transport Systems)</mark> i Sverige.

## Vad är C-ITS – och varför spelar det roll?
**Cooperative Intelligent Transport Systems** (C-ITS) är ett “nätverk” av fordon, vägar och trafikledningar, som “pratar” digitalt med varandra för att 
dela läges- och trafikinformation. Tanken är säkrare vägar, smidigare trafik och mindre stress/utsläpp. Man kan beskriva det som ett “trafik-chattflöde” 
gemensamt för alla inom ett geografiskt område för kunna varna och hjälpa varandra i realtid. 

### Vem pratar med vem?
V2V: fordon ↔ fordon (t ex “jag bromsar hårt nu!”)

V2I: fordon ↔ infrastruktur (t ex “hjulspinn/ABS-aktivering – varning för halt väglag!”)

I2V: infrastruktur ↔ fordon (t ex “tillfällig hastighetsbegränsning”) 

V2X: fordon ↔ “allt” runtomkring (fordon, trafikljus, vägskyltar, driftledningar).

I Europa koordineras utrullningen via C-Roads-plattformen som ser till att saker funkar likadant över gränserna. 

### Vad skickas?
C-ITS inbegriper olika meddelandetyper. De viktigaste är:
* <mark>CAM (Cooperative Awareness Message)</mark>: “Här är jag, så här fort kör jag, den här riktningen” – korta “pulsar” som håller andra uppdaterade om fordons/RSU:s närvaro.
* <mark>DENM (Decentralized Environmental Notification)</mark>: händelser/varningar – t ex “hal is”, “stillastående fordon”, “olycka”.
* <mark>IVIM (Infrastructure-to-Vehicle Information Message)</mark>: ger information från infrastruktur, t ex vägmärken eller vägstatus.
* <mark>SPATEM (Signal Phase and Timing Extended Message)</mark>: beskriver signalstatus (trafikljus) och tidsplan.
* <mark>MAPEM (MAP Extended Message)</mark>: tillhandahåller geometri för korsningar och vägnät.
* <mark>SREM (Signal Request Message)</mark>: begär prioritet i trafiksignaler (t ex för utryckningsfordon eller kollektivtrafik).
* <mark>SSEM (Signal Status Message)</mark>: svar på SREM, anger status på begäran.

Dessa meddelanden är standardiserade i ETSI och används i dagliga C-ITS-tjänster. 

(RSU: enhet som placeras längs vägar, ofta på stolpar, trafikljus eller skyltar, för att möjliggöra kommunikation mellan fordon och infrastruktur.)

### Hur skickas det?
Det finns två huvudvägar, vilka också kan kombineras:
* <mark>ITS-G5 (802.11p)</mark>: dedikerad kortdistanskommunikation, ofta 5.9 GHz, direkt fordon-till-fordon.
* <mark>IP-baserad kommunikation (4G/5G)</mark>: via mobilnät och/eller direkt sidelänk.

Alla C-ITS-meddelanden signeras kryptografiskt enligt ETSI TS 103 097 för att säkerställa integritet och autenticitet.

I Europa förespråkas hybrid kommunikation (både ITS-G5 och mobilnät) för att täcka fler scenarier. Utöver att meddelanden måste vara äkta och orörda, kräver det auropeiska systemet att C-ITS inte får avslöja vem föraren är. Europa använder därför ett gemensamt C-ITS-PKI (se EU CCMS nedan) med kortlivade pseudonyma 
certifikat. Själva formatet och säkerhetshuvudet definieras i ETSI TS 103 097. 

### Exempel på användningsfall
Några exempel på anvädningsfall:
* Halka runt kröken: En bil aktiverar antispinn → skickar DENM “halt väglag” → bilar bakom sänker farten innan de passerar isfläcken.
* Stillastående fordon på motorvägen: Bilen som stannat sänder varning → ankommande fordon får tidig heads-up i instrumentklustret. 
* Vägarbete: RSU skickar varning + rekommenderad hastighet → mjukare flöde och färre “sista-sekunden-inbromsningar”. 

### Utmaningar
#### Införandegrad: nyttan växer när många fordon och vägar är uppkopplade.
C-ITS bygger på nyttan av att “alla pratar samma språk” – men värdet blir stort först när många bilar och vägar är med.

Hönan-och-ägget-problemet: Fordonstillverkare tvekar att lägga in tekniken om infrastrukturen är begränsad, och väghållare tvekar att investera i utrustning om få fordon kan använda det.

Exempel: Om endast 8 % av fordon och RSU:er kan varna för halka, då ser 92 % aldrig varningen → liten samhällsnytta. Men vid 30–40 % penetration börjar effekten bli märkbar. Först då blir C-ITS riktigt användbart.

#### Interoperabilitet & tillit
När fordon rör sig mellan städer och länder måste budskapen i meddelandena tolkas på samma sätt. ETSI och CEN har format och regler, men små skillnader i implementation kan skapa problem. Uppkomst av olika dialekter kan medföra at alla inte förstår alltid allt.

Meddelanden måste vara signerade så att mottagaren kan lita på att de är äkta. Det innebär att certifikat som är utfärdat i ett land måste accepteras också av alla andra länder. Om tilliten brister eller om länder börjar införa egna alternativa trust modeller kan man få “tillitsluckor” – t ex en tysk bil som inte litar på en svensk väginfrastruktur.

#### Styrning och konflikter
Teknikval, certifikathantering och integritetspolicy måste hålla över tid. Det finns dock flera risker:
* EU rekommenderar “hybrid”, dvs att man kombinerar kortdistans (ITS-G5) och mobilnät (LTE/5G). Då täcker man både direkta säkerhetskritiska varningar (annat fordon bromsar framför dig) och tjänster som kräver nät (t ex trafikinformation från en tjänst i molnet). Industrin har delade åsikter: Vissa förespråkar C-V2X (5G), andra ser värde i ITS-G5. Risken: parallella ekosystem som inte kan prata fullt ut med varandra.
* Det pågår diskussioner om vilket radiospektrum C-ITS ska få, särskilt eftersom mobilindustrin vill ha samma frekvenser för 5G. Det kan skapa regulatoriska konflikter.
* Vägar, fordon, mobilnät och myndigheter ägs av olika aktörer med olika incitament. Det krävs stark samordning.
* Fordon har en livslängd på ca 15–20 år. Infrastruktur ännu längre. Beslut som fattas nu måste hålla i decennier, annars riskerar man “tekniska återvändsgränder”.
* Ekonomisk utmaning: vem betalar? Fordonstillverkare, staten, operatörer, eller en mix? – olika i varje land!

## Tillit och säkerhet i det europeiska systemet
I Europa bygger C-ITS på <mark>EU CCMS (EU C-ITS Security Credential Management System)</mark> – en PKI-struktur som ska skapa tillit mellan fordon och infrastruktur när de utbyter säkerhetskritiska meddelanden (t ex CAM och DENM).

Tillitskedjan inkluderar flera roller:
* <mark>CPA (Certificate Policy Authority)</mark>: ansvarar för policydokument och godkänner Root CAs.
* <mark>TLM (Trust List Manager)</mark>: skapar och publicerar den europeiska certifikatlistan (ECTL) med alla betrodda Root CAs.
* <mark>CPOC (C-ITS Point of Contact)</mark>: fungerar som nav mellan Root CAs och TLM, tar emot certifikat, publicerar ECTL och tillhandahåller ankre i tillitskedjan (trust anchors).
* <mark>Root CA</mark>: kan vara statliga eller privata aktörer. Utfärdar certifikat till:
    * <mark>EA (Enrolment Authority)</mark>: utfärdar <mark>Enrolment Credentials (EC)</mark> till C-ITS-enheter (fordon och RSU:er).
    * <mark>AA (Authorization Authority)</mark>: utfärdar <mark>Authorisation Tickets (AT)</mark>, som är kortlivade pseudonyma certifikat för att skydda integriteten.

C-ITS-enheter (fordon, RSU) använder EC och AT för att signera och verifiera meddelanden.

### Vad som utmärker C-ITS jämfört med klassisk PKI
#### Pseudonymitet och integritetsskydd
Till skillnad från klassisk PKI, där identiteten ofta är knuten till certifikatet, använder C-ITS pseudonyma certifikat (ATs) som byts ofta för att förhindra spårning av fordon. Kopplingen till fordonets verkliga identitet hålls skyddad av EA/AA och exponeras inte i kommunikationen.

#### Korta livstider i nyckelhanteringen
Authorisation Tickets (AT) har mycket kort giltighetstid och måste bytas ofta (sekretesskrav). Detta skiljer sig från traditionella PKI-certifikat som ofta är giltiga i månader eller år.

#### Multirot-struktur och central trust list
Flera Root CAs kan samexistera, men alla listas i en central European Certificate Trust List (ECTL) signerad av TLM. Detta är annorlunda än en klassisk PKI där en organisation ofta har en hierarki med en root och underordnade CAs, men inte en gemensam europeisk trust anchor.

#### Separering av roller (EA och AA)
En viktig säkerhetsdesign är separationen mellan enrolment (EC) och authorization (AT) för att undvika att någon enskild aktör kan både identifiera och spåra ett fordon. I en traditionell PKI finns inte denna uppdelning – CA utfärdar certifikat direkt till slutanvändare.

#### Specifika C-ITS-protokoll och standarder
Certifikat och signaturer följer ETSI TS 103 097 och relaterade standarder, optimerade för bandbredd och realtidskrav i fordonskommunikation.
Även särskilda "butterfly key"-mekanismer används för att möjliggöra massgenerering av pseudonyma certifikat.

#### Striktare operativa och fysiska kontroller
Rollerna (Root CAs, EA, AA, TLM) omfattas av detaljerade krav på revision, fysiska säkerhetskontroller och procedurer som fastställs av EU (via CP, Security Policy och CPOC Protocol). Traditionell PKI följer ofta en CA/Browser Forum eller eIDAS-baserad policy, men C-ITS har sin egen europeiska styrmodell.

#### Manuella och fysiska processer för nyckelhantering
Root CA-certifikat lämnas ofta fysiskt (med kurir) till CPOC för att minska risken för kompromettering. Detta är ovanligt i klassisk PKI, där processerna i större utsträckning är automatiserade.

## Hur går det till i praktiken
Uppdateras inom kort.

## C-ITS i Sverige
Uppdateras inom kort.

## Slutsatser
Uppdateras inom kort.
