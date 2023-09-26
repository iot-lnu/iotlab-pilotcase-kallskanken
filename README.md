# Kallskänken pilote case 
I detta pilotprojekt har olika LoRaWAN-nätverk och sensorer undersökts för att kunna användas inom restauranger. Syftet var att mäta temperatur och luftfuktighet i restaurangmiljöer, registrera antalet besökare samt övervaka bullernivån. Bullernivån är särskilt viktig eftersom olika kommuner i Sverige har fastställt olika riktvärden för bullernivåer beroende på källan till ljudet. Detta innebär att restauranger och krogar behöver anpassa sig efter dessa riktlinjer. Resten av detta dokument kommer att gå igenom vilka nätverk, sensorer och plattformar som har använts.

# Innehållsförteckning
* [Kallskänken pilote case](#kallskänken-pilote-case)
* [Innehållsförteckning](#innehållsförteckning)
* [LoRaWAN Nätverk](#lorawan-nätverk)
   * [Helium](#helium)
      * [Logga in](#logga-in)
      * [Lägg till enhet](#lägg-till-enhet)
      * [Skicka Downlink](#skicka-downlink)
      * [Länka enhet till datacake](#länka-enhet-till-datacake)
      * [Byt namn på Helium enhet](#byt-namn-på-helium-enhet)
   * [Actility](#actility)
      * [Kalmar Energi Portalen](#kalmar-energi-portalen)
      * [API](#api)
      * [Device API](#device-api)
      * [Device Data API](#device-data-api)
      * [Device Data Latest API](#device-data-latest-api)
* [Sensorer](#sensorer)
   * [Sensative Strips](#sensative-strips)
      * [Medföljande Strips](#medföljande-strips)
      * [Ansluta sensorn till LoRaWAN-nätverket](#ansluta-sensorn-till-lorawan-nätverket)
      * [Konfigurera Strips](#konfigurera-strips)
         * [Använd Strips configuration aplication](#använd-strips-configuration-aplication)
   * [Ljudsensor](#ljudsensor)
      * [Installation](#installation)
      * [Konfiguration av sensorn](#konfiguration-av-sensorn)
         * [NFC-konfiguration](#nfc-konfiguration)
         * [Konfiguration via downlinks](#konfiguration-via-downlinks)
      * [Parametrar för applikationen](#parametrar-för-applikationen)
   * [Besöksräknare](#besöksräknare)
      * [NFC-konfiguration](#nfc-konfiguration)
      * [Konfiguration via downlinks](#konfiguration-via-downlinks)
* [Visualisering av data](#visualisering-av-data)
   * [Inloggning](#inloggning)
   * [Lägg till enhet Datacake](#lägg-till-enhet-datacake)
   * [Byt namn på Datacake enhet](#byt-namn-på-datacake-enhet)
   * [Dashboards](#dashboards)
      * [Skapa Dashbords](#skapa-dashbords)
* [Länka till google kalkylark](#länka-till-google-kalkylark)
   * [How it works](#how-it-works)
   * [Example of datas](#example-of-datas)

# LoRaWAN Nätverk

I detta projekt testades nätverken Helium och Actility (via Kalmar Energi). till en början kopplades några sensorer till Helium och några till Actility. Från Helium skickas data vidare till google-sheets och Datacake. På Datacake skapades 2 dashboards där data från sensorerna visualiserades. Eftersom det visade sig att täckningen för Helium inte var tillräcklig på restaurangerna lades alla sensorer över till Kalmar energis portal. Från Kalmar energis portal är data tillgängligt via ett API, det går även att skapa dashboards på portalen. 

![](https://hackmd.io/_uploads/BygivmCT2.png)


## Helium

Helium använder ett decentraliserat lågkostnadsnätverk där användarna betalar med Helium-tokens för att skicka data. Det är möjligt att köpa tokens eller tjäna dem genom att köpa och hålla igång enheter som kallas Hotspots. Hotspots ger nätverkstäckning. Det är en kostnadseffektiv lösning för IoT-dataöverföring som görs möjlig av privatpersoner.

[Värdet av Helium-tokens](https://helium.plus/data-credits-calculator)

Eftersom täckningen är beroende av privatpersoner som håller igång Heliums Hotspots, ser den olika ut på olika platser. Ju fler Hotspots det finns i ett område, desto bättre täckning.

[Hotspot Karta](https://explorer.helium.com/)

### Logga in

1. Se till att du är inloggad på [gmail](https://mail.google.com/) med mejladressen som är listad i [Autentiseringsuppgifter](#Autentiseringsuppgifter)
2. Gå till: https://console-vip.helium.com/.
3. Klicka på 'continue with google' och använd mejladressen som är listad i [Autentiseringsuppgifter](#Autentiseringsuppgifter)


### Lägg till enhet

1. Klicka på "Devices" i listan med alternativ till vänster.
5. Klicka på ikonen som är inringad med rött. ![](https://hackmd.io/_uploads/Hy35sY093.png)
6. Ge enheten ett beskrivande namn.
7. Dev EUI, App EUI och App Key kan hittas på olika sätt beroende på enhet.
8. Valfritt: Lägg till "label". Med "label" är det möjligt att gruppera sensorer och tillämpa samma inställningar på en hel grupp.
9. Spara enheten.

### Skicka downlink

Downlinks kan användas för att konfigurera vissa av sensorerna. I avsnittet [Sensorer](#Sensorer) beskrivs vad som ska skickas för att konfigurera enheten.

1. Gå till "Devices"
2. Klicka på den enhet som du vill skicka en downlink till.
4. Klicka på ikonen som är inringad med rött. ![](https://hackmd.io/_uploads/Hy85FaO23.png)
5. Ange "payload" och den port som den ska skickas till. Klicka på ikonen markerad med rött för att lägga downlinken i kö för att skickas. Meddelandet kommer att skickas nästa gång sensorn skickar något.
    ![](https://hackmd.io/_uploads/ryxK8G933.png
    
### Länka enhet till datacake

När en enhet läggs till kan den länkas till Datacake. Data från enheten skickas från Helium till Datacake, där den kan visualiseras. 
1. Gå till "Flows".
2. Klicka på plustecknet bredvid "NODES". ![](https://hackmd.io/_uploads/Sy-1Xw532.png)
3. Du hittar enheten i "Devices" eller "Labels", labels kan innehålla många enheter.
4. Dra och släpp enheten ut på arbetsytan.
5. Dra en linje mellan den nya enheten och den befintliga datacake-noden. ![](https://hackmd.io/_uploads/S1wQSD9h2.png)

För att data ska visas i Datacake måste enheten läggas till i Datacake. Se [Lägg till enhet Datacake](#Add-device-Datacake) för instruktioner.
    
### Byt namn på Helium enhet 
1. Gå till "Devices".
2. Klicka på den enhet som du vill byta namn på.
3. Klicka på pennikonen bredvid namnet. ![](https://hackmd.io/_uploads/HJWVDzKnh.png).
5. Uppdatera namnet, var noga med att komma ihåg vilken enhet det är.

## Actility

Actility är en leverantör av IoT-anslutningar som erbjuder en plattform för anslutning och hantering av Internet of Things-enheter. Actility driver [Stadshuben] (https://kalmarenergi.se/kalmarstadshubb/) som är en IoT-plattform som tillhandahålls av Kalmar Energi. För tillfället är alla sensorer som ingår i projektet uppkopplade på stadshubben.

### Kalmar Energi Portalen
![](https://hackmd.io/_uploads/rJgNpSGAh.png)

Det är möjligt att filtrera bort sensorer som ligger utanför ett inställt intervall på valfria kanaler som sensorerna skickar. Senaste sensorvärdet visas då även i kartvyn, som visas i bilden nedan.

![](https://hackmd.io/_uploads/Bk4L6SM0n.png)

För att göra detta, klicka på `Filter channels` i det övre högra hörnet av kartan och välj kanalen/värdet du vill filtrera baserat på.

![](https://hackmd.io/_uploads/rytLDUzAh.png)

### Downlink

För att skicka en Downlink behöver man klicka på de tre prickarna längst till höger och sedan på `Messages`. 

![Alt text](/docs/img/image-1.png)

Sedan behöver man klicka på de tre prickarna längst till höger och sedan på `Send Downlink`. Strängen som man skickar med brukar oftast vara i hexadecimal form och skickar över `port 11`. 

![Alt text](/docs/img/image.png)

### API
Det är möjligt att använda dina inloggningsuppgifter (som du använder för att logga in på Stadshubben) för att logga in på vår API-portal - eventuellt behöver du slutföra registreringen för att få tillgång till API-portalen:

Gör så här för att logga in på API-portalen: 
 
1. Navigera till: https://ke-apim.developer.azure-api.net/ .
2. Klicka på `sign in`:

![](https://hackmd.io/_uploads/S1lXHKCph.png)

3. Gå till Azzure Active Directory B2C och använd inloggningsuppgifterna för att logga in. Se bilden:

![](https://hackmd.io/_uploads/Hks4SY0T3.png)

Utforska API:erna och se alla andra ändpunkter.

För att få autentiseringstoken via API:erna måste man använda API:et för kundidentitet: 
POST: /v1/resource-owner/token 


När det gäller autentisering via HTTPS Request måste du anropa följande endpoint https://api.kalmarenergi.se/customer-identity/v1/resource-owner/token med följande information i Request:

- Username: "EMAIL"
- Passsword: "PASSWORD"           
-	responseType: "token id_token"            
-	scope: "openid offline_access https://kalmarenergib2c.onmicrosoft.com/6b6a459a-2596-4213-b42b-153f1e0a9af7/Device.Read.Add"
-	grantType: "password"

Se bilden nedan för ett exempel på hur du hämtar din token:

![](https://hackmd.io/_uploads/Skn1bcE0h.png)




Header är: 

![](https://hackmd.io/_uploads/SJzhULM0n.png)


För att uppdatera ditt token:
POST:  /v1/resource-owner/token 

Payloaden borde se ut som följande:
-	refreshToken: "xxxx"
-	grantType: "refresh_token"

![](https://hackmd.io/_uploads/SyMwW9V02.png)





Node Id kan erhållas via API:erna eller från URL:en. Idt finns i den sista delen av URL:en, den börjar efter sista snedstrecket. Användaren måste logga in på Stadshubben för att gå till `Nodes` och sedan klicka på målnoden. När målnodens sida öppnas kan Id hämtas från URL:en. Se nedan:
![](https://hackmd.io/_uploads/Sy-u8YCp2.png)

Nodens ID för exemplet ovan är: `469f0f0f-12fc-440a-81db-56295513151w`



### Device API
- För att anropa alla noder och se deras: Id, ChannelId, ModelId, etc... Gör en GET request på följande endpoint:

   GET https://api.kalmarenergi.se/device/v1/nodes 
 
- För att anropa en specifik nod och se dess: Id, ChannelId, ModelId, etc... Gör en GET request på följande endpoint:

   GET https://api.kalmarenergi.se/device/v1/nodes/{NodeId}
 
 
### Device Data API

**Hämta data från en nod:**

GET https://api.kalmarenergi.se/device-data/v1/message-values?nodeId={NodeId}6&pageIndex=1&pageSize=-1&sortBy=Message.CreatedAt&sortDescending=false&channelIds={ChannelId}&channelIds={ChannelId}&channelIds={ChannelId}

GET https://api.kalmarenergi.se/device-data/v1/message-values?NodeId={NodeId}

GET https://api.kalmarenergi.se/device-data/v1/message-values?ChannelIds={ChannelId}&NodeId={NodeId} 
 
 
 
### Device Data Latest API
Hämta det senaste meddelandet (data) som anlänt från en nod:

GET https://api.kalmarenergi.se/device-data-latest/v1/message-values?nodes={NodeId}&pageIndex=1&pageSize=100


# Sensorer
Det finns ett stort antal tillgängliga sensorer som är LoRa-kompatibla. I det här projektet användes 3 olika sorter. Comfort strip som mäter temperatur, luftfuktighet och ljus, ERS sound som mäter ljud, temperatur luftfuktighet och ljus, samt en besöksräknare. 

| Sensor        | Antal |
|---------------|-------|
| Comfort strip | 10    |
| ERS sound     | 1     |
| Besöksräknare | 1     |

## Sensative Strips
Strips från Sensative är diskreta sensorer som finns tillgängliga för ett antal användningsområden. De vi använder kallas +comfort. De har sensorer för temperatur, luftfuktighet och omgivande ljus. De kan även användas som dörrsensor. Remsorna är gjorda för inomhusbruk, men de kan hantera temperaturer mellan -20 och 60 C, så det skulle vara möjligt att placera dem i en frys eller ett kylskåp.

![](https://hackmd.io/_uploads/r1aqJ4Aan.jpg)


### Medföljande Strips
Tio remsor ingår för tillfället i projektet, Alla är just nu inlagda på på [Actility/Stadshubben](#Actility). De är märkta "SH1-10". De har också konfigurerats för att skicka relevanta data. De bör vara redo att installeras där de behövs.

### Ansluta sensorn till LoRaWAN-nätverket 
För att ansluta en sensor till LoRaWAN-nätverket behöver magneten avlägsnas efter att nycklarna har matats in i nätverksservern. Då gör sensorn en join-sekvens och ansluter till nätverket. Om man har avlägsnat magneten innan nycklarna har matats in i nätverksservern kan en manuell join-sekvens göras genom att hålla en magnet vid den runda sidan av sensorn och flytta bort den tre gånger.

Följande film som visar hur man tvingar sensorn att göra en join-sekvens: https://www.youtube.com/watch?v=BBRCxrhlpsc&ab_channel=Sensative

Föjande film som visar hur man återställer sensorn till fabriksinställningar: https://www.youtube.com/watch?v=qkWr9diDSNk&ab_channel=Sensative


### Konfigurera Strips
När remsorna är nya är de inställda till att endast fungera som dörr-sensorer. De behöver konfigureras för att läsa av temperatur, luftfuktighet och omgivande ljus. Sensorerna konfigureras genom att en downlink skickas till dem. Sensatives webbplats har en ['Strips configuration aplication'] (https://strips-lora-config-app.service.sensative.net/profiles) som tillhandahåller rätt meddelande att skicka med downlinkarna för att konfigurera remsan.
##### Använd Strips configuration aplication
1. Gå till ['Strips configuration aplication'](https://strips-lora-config-app.service.sensative.net/profiles).
1. "Device model" är "Strips multi sensor +comfort".
2. Välj den profil som passar det remsan ska användas till.
3. Generatorn kommer att beskriva den valda profilen, fortsätt genom att klicka på "proceed". I dessa steg är det möjligt att ändra profilen, men detta kan påverka enhetens batteritid.
4. I det sista steget visas en "payload" överst på sidan. "Payloaden" är meddelandet som ska skickas till remsan, den är i hexadecimal form, under  finns en knapp som gör den bas 64-kodad. Vilket format det ska vara beror på vilket nätverk som används för enheten. ![](https://hackmd.io/_uploads/SJVgNUUs3.png)
6. Skicka meddelandet till din enhet: [Skicka downlink(Helium)](#skicka-downlink).

## Ljudsensor
ERS sound mäter ljud och flera andra saker så som temperatur, luftfuktighet och ljusintensitet.

### Installation 
Ta bort sensorns baksida för att installera de två batterierna. batteritypen är 3,6 V litium AA.

### Konfiguration av sensorn 
Alla sensorinställningar kan konfigureras via en smartphone-applikation med NFC eller via nätverksservern genom att man skickar downlinks till sensorn.

#### NFC-konfiguration
1. Ladda ner ELSYS applikation "Sensor Settings" från Google Play och installera den på en smartphone eller surfplatta. Enheten måste ha stöd för NFC.
2. Aktivera NFC på enheten och starta applikationen.
3. Placera din enhet ovanpå NFC-antennen på sensorn.
4. Ta bort enheten. Aktuella inställningar visas i applikationen.
5. Använd applikationen för att ändra inställningar om det behövs.
6. Tryck snabbt på enheten ovanpå NFC-antennen för att ge de nya inställningarna till sensorn. Se till att applikationen bekräftar dina nya inställningar.
7. Vänta tills sensorn har startat om (5 sek), vilket indikeras av att LED-lampan blinkar. Sensorns inställningar har uppdaterats.

#### Konfiguration via downlinks
Alla inställningar kan konfigureras via din LoRaWAN®-infrastruktur.
Besök supportavsnittet på denna [webbsida](https://www.elsys.se/en/downlink-generator/) för mer information om downlink-protokollet. Generatorn skapar en nedlänk som kan skickas till enheten, t.ex. via [Helium](#Skicka-downlink).

### Parametrar för applikationen
Alla parametrar för applikationen "Sensorinställningar" finns i inställningsdokumentet. Mer information finns i supportavsnittet på denna [webbsida](https://www.elsys.se/en/application-settings/).

## Besöksräknare

Besöksräknaren kan detektera när någon passerar emellan dess två enheter. Den kan också avgöra åt vilket håll personen rörde sig. Med hjälp av dessa egenskaper kan den ge en ungefärlig bild av hur många människor som befinner sig i en lokal.

![](https://hackmd.io/_uploads/HJOHlVRTh.jpg)

Manual: [IMBUILDINGS - LoRaWAN People Counter - doc v1.1](/docs/People-counter/IMBUILDINGS%20-%20LoRaWAN%20People%20Counter%20-%20doc%20v1.1.pdf)

Avancerad guide: [IMBUILDINGS_Reference_Guide_for_System_Integrators](/docs/People-counter/IMBUILDINGS_Reference_Guide_for_System_Integrators.pdf)

### NFC-konfiguration 
För att komma igång och konfigurera enheten via NFC behöver man ladda ner appen (APK) för Android från IMBuildings [Github sida](https://github.com/IMBUILDINGS/Config-App). När man väl har installerat appen kan man hålla telefonen nära sensorn för att göra NFC-avläsningen. I Appen kan man sedan ändra `AppKey` och `AppEUI` såväl som andra inställningar. 

![Alt text](/docs/img/image0.png)

### Konfiguration via downlinks

Du kan se alla inställningar i den ***avancerade guiden*** ivan, men här är en snabb sammanfattning av vad du kan göra:

**Sätta till 10 minuters intervall**
> F1 01 03 1E ==0A==
> interval : 0A (correspond to 10 minutes)

**Ändra avstånd**
> F1 01 04 50 ==1E 96==
> 
> Ignored distance : 0x1E (correspond to 30 cm)
> 
> Detection distance : 96 (correspond to 150 cm)

Det är rekommenderat att baka in flera instruktioner i en enda Downlink. I det här fallet kan du lägga till intruktionen save, vilket skulle ge följande Downlink:

**intruktionen för att spara**
>F1 01 04 50 1E 96 ==03 C8 04==

För att se alla olika typer av nyttolaster kan du kolla den ***avancerade guiden*** ovan.

Mer om Downlinks [här](/docs/IMBUILDINGS_Reference_Guide_for_System_Integrators.pdf)


# Visualisering av data
Datacake användes för att visualisera data från sensorerna i heliumnätverket. För att se den visualiserade data klicka på en enhet eller på en "Dashboard". ![](https://hackmd.io/_uploads/H159ar53n.png) 

![](https://hackmd.io/_uploads/r1hmObR6n.png)

## Inloggning

1. Gå till https://app.datacake.de/login
2. Använd den e-postadress och det lösenord som anges i [Credentials](#credentials)![](https://hackmd.io/_uploads/SJM4kbY33.png)

## Lägg till enhet Datacake

1. Gå till enheter och klicka på "+ Add Device"
3. Välj "LoRaWAN" och klicka på " Next".
4. Välj " New Product from template"
5. Sök och se om det finns en mall för enheten. ![](https://hackmd.io/_uploads/H1w58wqh2.png)
6. Välj den korrekta mallen, klicka på " Next".
7. Välj det nätverk som enheten är ansluten till, till exempel Helium. Klicka på " Next"
8. Lägg till enheten genom att skriva dess DevEUI och ge den ett namn. Den kan också ges en plats, t.ex. " matsal".
9. Det är möjligt att lägga till flera enheter samtidigt genom att klicka på "+ Add another device".

## Byt namn på Datacake enhet

1. Gå till "Devices".
3. Klicka på den enhet du vill byta namn på.
4. Klicka på "Konfiguration". ![](https://hackmd.io/_uploads/r17wYzYn3.png)
5. Uppdatera namnet, var noga med att komma ihåg vilken enhet det är.
6. Spara!

## Dashboards

Datacake låter dig skapa dashbords där data från flera olika sensorer kan visualiseras. För detta projekt skapas två dashbords: "Kallskänken history" och "Kallskänken live". 

### Skapa Dashbords

1. Klicka på "+Add Dashboard" i menyn till vänster.

3. Namnge dashboarden och bestäm vem som ska ha tillgång till den.
4. För att redigera dashboarden, klicka på ikonen markerad med rött. ![](https://hackmd.io/_uploads/Syjbozc2h.png)
5. Klicka på "Add widget" för att utforska vad som kan läggas till i dashboarden och hur data kan visualiseras.

Tips: Ett bra sätt att lära sig vad som kan göras är att redigera befintliga dashboards och undersöka de widgetar som redan finns där. ![](https://hackmd.io/_uploads/rkKuYIc3h.png)

# Länka till google kalkylark
Om du vill använda dina data kan du ha dem i Google Sheets. Du kan sedan enkelt konsultera dem och göra vad du vill med dem.

## Hur det funkar
Vi samlar in data från portalen och bearbetar den sedan med en avkodare för att skicka den.

Avkodaren är en kod som tar en del av data som kallas "Payload" och bearbetar den för att extrahera de önskade värdena.

Datan skickas till dokumentet via ett frågeformulär. I det här fallet använder vi Google-formulär.  Formuläret måste inehålla en fråga för varje del av data som vi vill ha med.

![](https://hackmd.io/_uploads/rk9CEX0a2.png)
Sedan överför google forms uppgifterna till google sheets.

## Exempel på data
Det här är vad du kommer att se i Google Sheets: Datum, temperatur, ljus och luftfuktighet.
Ibland saknas vissa data eftersom de inte har ändrats sedan den senaste mätningen...

![](https://hackmd.io/_uploads/BJEyjfRTn.png)
