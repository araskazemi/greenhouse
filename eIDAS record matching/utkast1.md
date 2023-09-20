## Ett första utkast
Konceptuell modell


```mermaid

%%{init: {'sequence': { 'mirrorActors': true, 'rightAngles': true, 'messageAlign': 'center', 'actorFontWeight': 900, 'actorFontSize': 18, 'noteFontWeight': 100, 'noteFontSize': 12}}}%%
%%{init: {'theme': 'base', 'themeVariables': {'labelBoxBkgColor': '#F5F5F4', 'labelBoxBorderColor': '#998980', 'actorBorder': '#696059', 'actorBkg': '#F1EFEC', 'activationBorderColor': '#CD796E', 'activationBkgColor': '#CD796E', 'noteBkgColor': '#F5E6D7', 'noteBorderColor': '#CE8036'}}}%%

sequenceDiagram
  actor användare
  Note over identitetsmatchning: en onlinetjänst från Digg till alla<br />som har en svensk identitetsbeteckning<br />samt ett e-id från annat land
  användare->>identitetsmatchning: vill koppla sin svenskaidentitetsbeteckning<br />till sitt e-id från annat land
  activate användare
  activate identitetsmatchning
  identitetsmatchning-->>användare: behöver identifiera användaren
  deactivate identitetsmatchning  
  användare-->>Sveriges eIDAS Connector: förflyttar till Sveriges eIDAS Connector
  activate Sveriges eIDAS Connector
  Sveriges eIDAS Connector->>användare: presenterar alternativ över de länder som<br />är anslutna för e-legitimering utanför Sverige
  användare->>Sveriges eIDAS Connector: väljer det land som ska utföra e-legitimeringen
  Sveriges eIDAS Connector-->>användare: omdirigerar till mottagarlandets eIDAS-nod
  deactivate Sveriges eIDAS Connector
  användare-->>mottagarlandets eIDAS Proxy: förflyttas till mottagarlandets eIDAS Proxy
  activate mottagarlandets eIDAS Proxy
  Note right of mottagarlandets eIDAS Proxy: utbudet och förfaranden för<br />e-legitimering skiljer sig åt<br />och hanteras nationellt av<br />respektive medlemsland
  mottagarlandets eIDAS Proxy->>användare: presenterar alternativ för e-legitimering
  användare->>mottagarlandets eIDAS Proxy: väljer sin e-legitimation att kunna identifiera sig
  mottagarlandets eIDAS Proxy-->>användare: omdirigerar till tjänst och/eller ombud för e-legitimering
  deactivate mottagarlandets eIDAS Proxy
  användare-->>e-id i annat land: förflyttas till e-legitimeringstjänst i mottagarlandet
  activate e-id i annat land
  användare-)e-id i annat land: användaren legitimerar sig enligt den metod som tillämpas för det valda landet och e-legitmationslösningen 
  e-id i annat land-->>användare: ett intyg ställs ut i enlighet med det nationella system för e-legitimering i medlemslandet
  deactivate e-id i annat land
  användare-->>mottagarlandets eIDAS Proxy: förflyttas tillbaka till mottagarlandets eIDAS Proxy
  activate mottagarlandets eIDAS Proxy
  mottagarlandets eIDAS Proxy->>användare: förmedlar ett intyg för gränsöverskridande e-legitimering i enlighet med teknisk specifikation för eIDAS
  deactivate mottagarlandets eIDAS Proxy
  användare-->>Sveriges eIDAS Connector: omdirigerar till Sveriges eIDAS Connector
  activate Sveriges eIDAS Connector
  Sveriges eIDAS Connector-->>Sveriges eIDAS Connector: 
  Note right of Sveriges eIDAS Connector: intyg valideras
  Sveriges eIDAS Connector->>användare: förmedlar intyg i enlighet med tekniskt ramverk för Sweden Connect
  deactivate Sveriges eIDAS Connector
  användare-->>identitetsmatchning: förflyttar tillbaka till onlinetjänsten<br />för identitetsmatchning
  activate identitetsmatchning
  identitetsmatchning-->>identitetsmatchning: 
  Note right of identitetsmatchning: beslut om åtkomst
  identitetsmatchning->>användare: användaren blir inloggad
  deactivate identitetsmatchning
  deactivate användare
  Note right of användare:  Användaren kan nu<br />genomföra identitetsmatchning
  

```

