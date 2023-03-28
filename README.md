# TopDesignE - Opdracht 2

## Inleiding
U vertrekt van het gegeven netwerk waarin de basis switching setup van de firma Topdesign uit
opdracht 1 reeds gemaakt werd. U werkt deze start-setup verder uit met:
- Een VTP server
- VLAN’s conform de vereisten die u verderop kan terugvinden
- Access poorten
- Trunking poorten
- Native VLAN
- Voice VLAN
- Een router-on-a-stick setup om aan inter VLAN routering te doen

De computers en IP phones werden aan de juiste switchpoorten verbonden, zoals beschreven in het
patchplan dat u reeds kreeg.

TopDesign heeft ondertussen ook extra budget bij elkaar gehaald om 6 access points aan te kopen
om een wifi netwerk op te zetten in hun nieuwe gebouw.

Deze access points werden ook aangesloten op de juiste switch poorten conform het patch plan.

Op de core switch staan alle ongebruikte poorten gedisabled, behalve poort fa0/1. Dit om toe te
laten dat management laptop van IT die hierop aangesloten is (zit dan in management VLAN 99) alle
switchen en de router via SSH kan benaderen. 

**Het moet tegen het einde van dit labo mogelijk zijn om alle switchen + de router via de management laptop van de IT dienst te bereiken via SSH.**

De CLI optie van Packet Tracer werd namelijk uitgeschakeld om de realiteit beter te benaderen. Dit wil zeggen dat u de console laptop zal moeten gebruiken om via de consolepoort de nodige
configuraties te doen om de SSH toegang zo snel mogelijk in orde te brengen

Alle wachtwoorden (console, enable, ssh) staan reeds ingesteld op **r&stop** (afkorting van:
routering & switching topdesign). De gebruikersnaam waarmee u kan aanmelden is telkens
beheerder. Alle vty lijnen laten enkel toegang via ssh toe (staat ook reeds ingesteld)

| witch hostname    | Management VLAN     |SVI IP adres         |
|---                |---                  |---                  |
|TD1                | 99                  |  172.16.99.1 /24    |
|TD2                | 99                  |  172.16.99.2 /24    |
|TD3                | 99                  |  172.16.99.3 /24    |
|TD4                | 99                  |  172.16.99.4 /24    |
|TD5                | 99                  |  172.16.99.5 /24    |
|TD6                | 99                  |  172.16.99.6 /24    |
|CORE               | 99                  |  172.16.99.7 /24    |

# VLAN definities

Iedereen moet richting het WAN (internet) kunnen op termijn. Dit wil voor de setup nu zeggen dat
alle endpoints met hun communicate tot aan de router moeten kunnen geraken.

Identificeer op de netwerktopologie welke lijnen u moet als **trunklijnen configureren.** Configureer de juiste poorten dan ook als trunkpoorten. Gebruik VLAN5 als **native vlan.**

Op termijn willen we kunnen instellen dat de personen die via de wifi connectie hebben, of die
ingeplugd zitten op een netwerkaansluiting in de vergaderzaal, hun verkeer niet kan terechtkomen
bij interne resources zoals de servers, de productiemachines, etc ...

Met andere woorden de juiste trunklijnen moeten geconfigureerd worden zodat ze het verkeer van
VLAN40 en VLAN60 niet zullen transporteren.

# IP adressering
U gebruikt onderstaande IP gebieden voor de verschillenden VLANs

| VLAN id           | End-points        |Soorten adressen   |
|---                |---                |---                |
|10                 | 172.16.10.0 /24   |  Static / Dynamic |
|20                 | 172.16.20.0 /24   |  Dynamic          |
|30                 | 172.16.30.0 /24   |  Dynamic          |
|40                 | 172.16.10.0 /24   |  Dynamic          |
|50                 | 172.16.50.0 /24   |  Dynamic          |
|60                 | 172.16.60.0 /24   |  Dynamic          |
|99                 | 172.16.99.0 /24   |  Static           |

Zoals u hierboven kan zien zijn er 6 IP gebieden waarin er dynamisch adressen moeten toegekend
worden aan end-points uit de overeenkomstige VLANs. Omdat we op dit punt in de cursus nog niet
geleerd hebben hoe je op een router een DHCP configuratie moet maken, ga je voor testdoeleinden
toestellen voorlopig een static IP in de juiste range moeten geven.
DHCP gaat later in het netwerk geïntegreerd worden in de derde en laatste TopDesign opdracht.
Subinterfaces aanmaken op de router
Configureer voor de VLANs, behalve voor native vlan5, de overeenkomstige subinterfaces volgens
onderstaande tabel.

U geeft elke subinterface telkens het **laatste IP adres** uit het adresgebied dat voorzien is.

| VLAN id           |Subinterface op router    |
|---                |---                       |
|10                 | Gig0/0.10                |
|20                 | Gig0/0.20                |
|30                 | Gig0/0.30                |
|40                 | Gig0/0.40                |
|50                 | Gig0/0.50                |
|60                 | Gig0/0.60                |
|99                 | Gig0/0.99                |


Zorg dat de subinterfaces een corrrect IP hebben (zie boven) en dat ze actief staan.


### Statische adressering
De switchen zijn reeds statisch geadresseerd vanuit opdracht 1. In de .pka file die u kreeg is dat dan ook reeds in orde. Ze hebben allen een IP adres in het management gebied 172.16.99.0/24

De servers en de NAS geeft u onderstaande IP adressen

- Server1: 172.16.10.1 /24
- Server2: 172.16.10.2 /24
- NAS: 172.16.10.3 /24

*Nota: Sommige computers ga je, om je configuratie te testen (zie verder) ook tijdelijk een static IP moeten geven (zie ook opmerking bij onderdeel “IP adressering” hierboven)*

# Verifiëren en controleren
Zorg dat het werk dat je uitvoerde, wordt gecontroleerd door iemand anders, want je kijkt meestal
over je eigen fouten heen. Realiseer je ook dat 100% halen in de .pka file enkel wil zeggen dat alles waar de simulator op kan controleren, in orde is. Maar aangezien de simulator beperkingen heeft wil dit niet zeggen dat dan ook alles correct werkt. **Dit moet getest worden!**
Gebruik ter verificatie de reeds geziene show commands. Exporteer systematisch de output van deze
show commands naar bestanden, zodat je deze desnoods side-by-side kan vergelijken met elkaar en
kan evalueren tegenover de opdrachten die je kreeg in deze instructies.

# Testen en troubleshooten

1. Test je configuratie door gebruik te maken van een ping.

- [ ] Wat moet er volgens jouw wel lukken, wat niet?
- [ ] Kloppen je observaties met wat je dacht dat er ging gebeuren?
- [ ]Indien geobserveerd gedrag afwijkt van hetgeen je had verwacht, ligt het dan aan een fout in je configuratie of is er een verklaring voor die niets metconfiguratiefouten te maken heeft?
2.  Test of alle switchen + de router bereikbaar zijn via ssh vanaf de management laptop
3. Test of alle VLAN’s met elkaar kunnen communiceren (moet dit allemaal lukken?)
4. Troubleshoot de dingen die fout gaan (in de volgorde zoals hieronder!)
- Volg het TCP/IP model wanneer er dingen fout gaan om aan je speurtocht naar de
oplossing te beginnen. Start bij de fysieke laag en werk omhoog. Dit wil onder
andere zeggen: check bekabeling en aansluitingen (= fysieke laag), check trunking
config en poorttoewijzingen (= datalinklaag), check IP adressering (= netwerklaag), ...
- Als communicatie via het ene protocol wel lukt (bvb ping) maar via een ander
protocol niet (bvb ssh) waar ligt dan de fout? Controleer dan de desbetreffende
configuratie ....
- Gebruik simulatiemode om te speuren naar waarom iets lukt of niet lukt. Deze optie
heb je niet in real life, maar wel op het examen.
- Gebruik de “check results” knop en de tab “assesment items” om te kijken of er nog zaken ontbreken in je configuratie. Deze optie heb je niet in real life en ook niet op het examen!
5. Stel eventuele vragen in het forum op Digitap.

# Documenteren
Documenteer je topologie / je netwerkschema zodat je een plan van je netwerk hebt waarop je in
oogopslag belangrijke zaken kan aflezen.
- Plaats een legende van de verschillende VLANs, hun kleurcode, hun doel en hun IP range op de topologie
- Bij toestellen die een permanent static adres hebben gekregen zet je dit adres erbij.
- Bij toestellen die een tijdelijk static adres hebben gekregen om te testen, zet je dit er ook bij.
- Plaats bij de router een overzicht van de subinterfaces en hun IP adressering.