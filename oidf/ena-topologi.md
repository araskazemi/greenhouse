# Inledning
Infrastrukturtjänsten *tillitsankare* i *OpenID Federation* utgör den yttersta noden i en tillitskedja och kan beskrivas som roten i en federationskontext. 
På så sätt ramar tillitsankaren in alla underliggande anslutna tjänster som kan verifieras och ingå i samma tillitssammanhang (federationskontext). 
När en part vill verifiera en annan parts metadata, börjar den från sitt definierade tillitsankare och följer en signerad kedja av metadata-uttalanden ner till den entitet som ska verifieras.
Eftersom kedjan är kryptografiskt signerad kan mottagaren lita på att informationen inte har manipulerats.
Detta gör det möjligt att skapa ett skalbart system där parter inte behöver ha direkta avtal med alla andra parter – det räcker att de delar samma tillitsankare.
Detta väcker frågan om hur många tillitsankartjänster som behövs i Ena &ndash; Sveriges digitala infrastruktur &ndash; och varför en modell med flera tillitsankare skulle kunna vara en fördel i en nationell federationsinfrastruktur.

## Varför inte ett enda nationellt tillitsankare?
* **Single point of failure och incidentpåverkan**: Kompromiss, felaktig nyckelrullning eller juridisk tvist påverkar alla sektorer samtidigt. Åtgärder blir svårkoordinerade.
* **Olika regelverk och riskprofiler**: Vård, skola, kommunal förvaltning och statliga e-tjänster har skilda krav (lagrum, sekretess, tillgänglighet, driftfönster). Ett enda policy­paket blir antingen för strikt för vissa eller för svagt för andra.
* **Förändringstakt**: Skolan och kommunal IT rör sig ofta snabbare än kliniska vårdflöden. En gemensam release-takt låser snabbare domäner.

## Varför inte “många små” tillitsankare?
* **Fragmentering och leverantörsbörda**: Molnleverantörer och systemhus som betjänar flera sektorer måste kvalificera sig om och om igen.
* **Otydlig ansvarsfördelning**: När incidenter sker blir det svårt att avgöra vem som leder, vem som informerar och vilken åtgärdsordning som gäller.
* **Interoperabilitetskostnad**: Fler broar, fler översättningar (SAML ↔ OIDC), fler profilavvikelser.

## Rekommenderad arkitektur (”gemensam policy-rot, få ankare”)
### Nationell profil (“root policy set”)
* En enhetlig, offentlig profil som definierar: miniminivåer för kryptografi, metadataformat, signerings-/rullningskrav, incident­hantering, loggning, och hur trust marks används.
* Undvik ord som "tillitsmärken" och “tillitsnivå” – tala istället om policykrav per användningsfall och säkerhetsattribut.
### Kontextuella operativa tillitsankare (2–4 st snarare än 1 eller 20)
Typiskt:
* Vården
* Skolan
* Högre utbildningar och forskning
* m.m.

Dessa är federation entities som förvaltar egna policy-tillägg men är bundna till den nationella profilen.
Ömsesidig tillit via OpenID Federation-mekanismer, tillitskedjor och taggningar (OIDC-trust marks) som stärker eller stödjer tilliten.

Delegering i kedjan i stället för “superrot”: Dvs om man ändå vill ha en symbolisk nationell topp kan den vara policybärare (ledningsaktören), medan signering och drift delegeras till sektorsankare (mindre blast radius, samma nationella linje).

### Relation till SAML2, mTLS och Radius
SAML-, mTLS och Radius-federationer körs parallellt under respektive federationsoperatör genom metadata­aggregat/MDQ med samma policyfamilj.
Man kan använda entity categories/attributprofiler som speglar OIDC-trust marks för att minska översättningsfriktion. Men troligtvis kommer många mjukvaror inte förstå den typen av metadata. 
Därför vore lämpligt att ge operatören i uppdrag att skapa flöden av metadata som begränsar omfattningen utifrån entiteter som erhållit ett specifikt OIDC-trust mark.
En idé kan vara också att definiera metadata i OIDC för operatörens metadataflöde och tilldela entiteten med tillitsmärke.

## När ska man dela upp till fler tillitsankare?
Ställ följande frågor (om svaret är ”ja” på flera → separera):
* Avvikande reglering/sekretess (ex. patientsäkerhet vs. skriva prov)
* Annan incident-/on-call-modell (krav på 24/7/365 i vård, annan för skola)
* Olika livscykler för entiteter (t.ex. läsår/terminer i skolan vs. vårdens journalsystem)
* Skilda attributkällor och styrningsdomäner (t.ex. HSA-katalog vs. elevregister)
* Marknadsstruktur (många leverantörer som korsar sektorer? då krävs gemensam profil och trust marks, inte fler ankare)

## Styrning, juridik och drift (praktiskt upplägg som jag upplever vi är överens om)
* **Operatörroll vs. Policyroll**: Separera Policy Authority (sätter krav, ackrediterar) från Federationsoperatör (drift, nyckelhantering, metadata­publicering). En aktör kan bära båda, men separationen minskar intressekonflikter.
* **Nyckelrullning och flerårig överlapp**: För tillitsankare och mellanled – planera tidsöverlapp och pre-publishing av nya nycklar i kedjan för att undvika avbrott.
* **Incident- och sanktionsprocess**: Definiera transparent revocation (spärra trust marks eller entiteter), grace periods och kommunikationskanaler per sektor.
* **Ackreditering och revision**: Lätta, återkommande kontroller för de flesta; djupare för känsliga roller (t ex OP/AS i vården). Publicera status öppet.
* **Leverantörsvänligt**: ”Once qualified, usable everywhere” – ett leverantörscase ska räcka för flera sektorer om trust marks och kravmatchning stämmer.

