# Opdracht_Security_Evaluatie
Herexamen Software Security: Opdracht Security Evaluatie

# Technieken en Tools

Voor de security evaluatie heb ik gekozen om het RFID Payment System te evalueren. Hierbij heb ik gebruik gemaakt van een SCA tool (Snyk) en een SAST tool (SonarQube). Ik heb geen DAST tool gebruikt omdat ik de code niet in production kon gaan uitvoeren. Een DAST tool zorgt er juist voor dat het een aantal kwetsbaarheden kan gaan opsporen binnen het project. Ik heb geen penetrating testing gedaan AKA ethical hacking omdat ik geen testen kon gaan uitvoeren op het netwerk waar de app gehost was om kwetsbaarheden op te sporen.
Snyk: Deze tool vond 9 kwetsbaarheden terug binnen het systeem waarvan 2 high severity en 7 medium severity. De 2 high severity waren Cross-Site Scripting attacks. Een oplossing voor die Cross-Site Scripting attacks zou zijn: 
-	Zuiver gegevensinvoer in een HTTP-request voordat u het terugkaatst, zorg ervoor dat alle gegevens worden gevalideerd, gefilterd of ontsnapt voordat u iets terugstuurt naar de gebruiker, zoals de waarden van queryparameters tijdens zoekopdrachten.
-	Converteer speciale tekens zoals ?, &, /, <, > en spaties naar hun respectievelijke HTML- of URL-gecodeerde equivalenten.
-	Geef gebruikers de mogelijkheid om client-side scripts uit te schakelen.
-	Ongeldige verzoeken omleiden.
-	Detecteer gelijktijdige aanmeldingen, inclusief die van twee afzonderlijke IP-adressen, en maak die sessies ongeldig.
-	Gebruik en handhaaf een Content Security Policy om alle functies uit te schakelen die kunnen worden gemanipuleerd voor een XSS-attack.
-	Lees de documentatie voor een van de bibliotheken waarnaar in uw code wordt verwezen om te begrijpen welke elementen embedded HTML toestaan.

![image](https://user-images.githubusercontent.com/64362709/183904576-a834e08a-8954-450b-a307-8b11aa34fb01.png)

![image](https://user-images.githubusercontent.com/64362709/183904630-d56bee59-bd3a-43ee-803b-af3973df6538.png)

3 van de medium severity zijn Use of Password Hash With Insufficient Computational Effort. Men gebruikt in het systeem SHA-1 wat onveilig is een oplossing hiervoor is: 
-	Gebruikmaken van een beter en veiliger password hashing scheme zoals de functie password_hash()

![image](https://user-images.githubusercontent.com/64362709/183904703-ea85abf7-d092-425a-8149-a100e35ab6e0.png)

![image](https://user-images.githubusercontent.com/64362709/183904717-38aadf6b-fa36-458d-9611-d51b53529981.png)

![image](https://user-images.githubusercontent.com/64362709/183904743-3d5225a5-93ab-4984-81f8-fa84c7561a3c.png)

De laatste 4 medium severity van Snyk waren Use of Hardcoded Credentials. Zorg ervoor dat je geen hardcoded credentials bloot laat liggen in je code. Een oplossing hiervoor is:
-	Hidden files in een folder plaatsen en voorkom access door gebruik te maken van .htaccess of andere Apache directives.
-	Sterke one-way hashes aan de credentials toe te passen en die hashes dan opslaan in een config file.

![image](https://user-images.githubusercontent.com/64362709/183904844-7f52c264-eed6-4a89-9d99-7010d9706a00.png)

![image](https://user-images.githubusercontent.com/64362709/183904865-3b930a3d-669f-43b9-8e0e-65f766a3c430.png)

![image](https://user-images.githubusercontent.com/64362709/183904895-cf7c7dd1-3a6c-460d-be5e-68ff7f16b7fb.png)

![image](https://user-images.githubusercontent.com/64362709/183904923-d9e0f19a-c6e6-4758-b36d-26eb68bdec93.png)

SonarQube gaf 83 Code Smells en 13 Security Hotspots. De Code Smells duiden gewoon zwakte aan in het ontwerp en kunnen in de toekomst het risico op bugs en programmafouten vergroten binnen het systeem. Dit los je best op door de code te gaan herbekijken en ervoor te zorgen dat je die zaken dan aanpakt. 

![image](https://user-images.githubusercontent.com/64362709/183905005-c33decfe-0f21-49db-88cf-9454bed3831b.png)

![image](https://user-images.githubusercontent.com/64362709/183905034-dc2b8489-564d-4420-a945-3a3fc3d213fe.png)

De 13 Security Hotspots toonde een 1 medium priority en 12 low priority. De medium priority was een DoS waarbij men moet opletten dat een bepaalde regex die men gebruikt er niet voor zorgt dat een DoS wordt uitgevoerd.

![image](https://user-images.githubusercontent.com/64362709/183905089-77ff882b-f7e7-4697-9dbe-97c2332a0e89.png)

1 low priority was een Log Injection waarbij the logger configuration veilig moet zijn.

![image](https://user-images.githubusercontent.com/64362709/183905151-c203de88-444e-4961-b0c1-2361318b4624.png)

En de andere 11 zijn ook low priority waarvan er een paar aangeeft dat de resource integrity die men gebruikt veilig is en dat men geen zwakke hash algoritmen gaat gebruiken in een gevoelige context.

![image](https://user-images.githubusercontent.com/64362709/183905210-632fcd8e-b61a-453f-afbc-53848a7b73d6.png)

![image](https://user-images.githubusercontent.com/64362709/183905229-e5681371-aea7-4f1c-8ee1-1b4a918eb8cc.png)

# Verdere Evaluatie

In verband met de opdracht rond security vereisten waren toch een bedreigingen aanwezig die eventueel kunnen plaatsvinden. Zoals ik dan getest heb met de tools Snyk en SonarQube kunnen we zien dat er toch een aantal bedreigingen zijn die konden optreden zoals een DoS attack, Injection, Cross-Site Scripting en blootleggen van credentials.

De evaluatiecriteria in verband met wachtwoorden: In de code kon ik zien dat als men een user ging aanmaken er bepaalde zaken niet aanwezig waren zoals: wachtwoord minstens 8 karakters lang en een zeer lang wachtwoord kiezen van minstens 64 karakters. Bij verschillende inlog pogingen wordt geen tijdsinterval ingeschakeld waardoor men dus kan blijven proberen inloggen. Bij herhaalde mislukte pogingen wordt het account niet geblokkeerd. Er werden ook wachtwoorden gebruikt voor authenticatie maar deze wachtwoorden werden niet goed gehashed. Een goede oplossing hiervoor is om gebruik te maken van de Azure AD van school zodat men gebruik kan gaan maken van de MFA functie. Zo kan men bijvoorbeeld geen brute force attacks gaan uitvoeren.

De evaluatiecriteria in verband met HTTPS: Als de website local gehost wordt zorg er dan voor dat SSL geactiveerd wordt in XAMPP.

De evaluatiecriteria in verband met beveiliging tegen typische web vulnerabilities: In register.php wordt er gebruikt gemaakt van een inline script dat uitgevoerd wordt. Voorzie een apart bestand voor je inline script en link dan de src van je script. In de register.php wordt ook een post request uitgevoerd waarbij er geen X-Content-Type-Options: nosniff in de header wordt geplaatst. Plaats X-Content-Type-Options: nosniff in je header om MIME sniffing tegen te gaan. Clickjacking zou zeker een probleem kunnen vormen ook al is het een interne webapp. Het kan zijn dat een insider iets plaatst in de code waarop de gebruiker eventueel op klikt waardoor ervoor gezorgd wordt dat er credentials worden vrijgegeven, een betaling die uitgevoerd wordt of malware dat gedownload wordt. Clickjacking kan je voorkomen door X-Frame-Options te gaan toevoegen in de HTTP header.

De evaluatiecriteria in verband met de API: In de api wordt er in de header gebruikt gemaakt van application/json media type voor zowel request als response. Bij het uitvoeren van een request wordt de nodige message teruggestuurd aan de hand van de status van de response.

Als men bijvoorbeeld een aankoop is uitgevoerd zonder dat men dit wou kan een persoon dit ontkennen. Zorg er dus dan ook voor dat deze zaken monitoring bevatten en zorg voor een eventueel pdf document dat is uitgevoerd.
Wat goed gedaan is in het systeem is dat men rollen is beginnen toekennen in het systeem en dat bepaalde sites hierdoor afgeschermd zijn als men eventueel geen access mag hebben tot een bepaalde link of actie.
Aangezien het systeem bedoelt is voor de school kon men perfect de Azure AD van school gaan gebruiken want Azure beschikt over een tal van functies die zeer handig kunnen zijn voor je systeem. Zo presteert je systeem optimaal en is de veiligheid van je systeem goed gehandhaafd.

# Risico's die aangepakt en niet aangepakt zijn in het systeem

## Spoofing
Spoofing wordt niet echt aangepakt in het project men kan dus meerdere malen proberen inloggen in het systeem. Hierdoor ontstaan er brute force attacks op het systeem.
### Hoe aan te pakken?
Gebruikmaken van Azure AD Multi-Factor Authentication. Dit zorgt ervoor dat je meerdere verificatiemethoden kan gebruiken om te gaan inloggen in je webapp. Dus je kan voorzien dat er alleen ingelogd kan worden met een EhB account.

## Tampering
Met Snyk had ik 2 high severity meldingen waarbij Cross-Site Scripting attacks konden gebeuren. 
### Hoe aan te pakken?
Een oplossing voor die Cross-Site Scripting attacks zou zijn: 
-	Zuiver gegevensinvoer in een HTTP-request voordat u het terugkaatst, zorg ervoor dat alle gegevens worden gevalideerd, gefilterd of ontsnapt voordat u iets terugstuurt naar de gebruiker, zoals de waarden van queryparameters tijdens zoekopdrachten.
-	Converteer speciale tekens zoals ?, &, /, <, > en spaties naar hun respectievelijke HTML- of URL-gecodeerde equivalenten.
-	Geef gebruikers de mogelijkheid om client-side scripts uit te schakelen.
-	Ongeldige verzoeken omleiden.
-	Detecteer gelijktijdige aanmeldingen, inclusief die van twee afzonderlijke IP-adressen, en maak die sessies ongeldig.
-	Gebruik en handhaaf een Content Security Policy om alle functies uit te schakelen die kunnen worden gemanipuleerd voor een XSS-attack.
-	Lees de documentatie voor een van de bibliotheken waarnaar in uw code wordt verwezen om te begrijpen welke elementen embedded HTML toestaan.

Gebruikmaken van SQL Threat Detection binnen Azure. Dit zorgt ervoor dat het bedreigingen detecteert waardoor je erop kan reageren zodra die zich voordoen door beveiligingswaarschuwingen te geven over die afwijkende activiteiten. Encryptie van data in transit. Bij cross-site scripting ervoor zorgen dat je geen onbetrouwbare gegevens in uw html-invoer plaatst. Als u niet vertrouwde gegevens toch plaats in uw html zorg er dan voor dat ze encoded zijn binnen uw html. Vooraleer je ook niet vertrouwde data in een url query string plaatst zorg dan ook voor dat ze encode is in uw url.

## Repudiation
Dit is iets dat we kunnen aanvaarden binnen de webapplicatie, maar als men dit toch wil aanpakken dan kan dit zeker. De monitoring op Azure gebruiken als bepaalde zaken uitgevoerd worden. Non-Repudiation dus ervoor zorgen dat aankopen een signatuur bevat bv: een pdf met de aankoop en daarop de signatuur van beide personen zodat de aankoop wel degelijk gebeurd is.

## Information Disclosure
Rollen werden goed toegekend binnen de webapplicatie. Hardcoded credentials waren wel aanwezig binnen de applicatie waardoor dit wel voor problemen kan zorgen bv: iemand die toegang tot uw DB krijgt gaat misschien data verzamelen die hij/zij niet mag hebben. Data die verstuurd wordt over netwerk wordt niet geëncrypteerd.
### Hoe aan te pakken?
Zorg ervoor dat data die verstuurd wordt op het netwerk geëncrypteerd wordt. Zoveel mogelijk generieke foutmeldingen geven zodat aanvallers niet onnodige aanwijzingen ontdekken. Hardcoded credentials uit de toepassing halen. Debugging afsluiten zodat de aanvaller geen informatie kan zien van het gedrag van de webapp. Gebruikmaken van TLS.

## Denial of Service
Met SonarQube heb ik een DoS threat ontdekt binnenin het systeem. Men moet hier opletten dat men met die regex geen DoS aanval kan gaan creeëren.
### Hoe aan te pakken?
Gebruikmaken van de Azure DDos Protection. Deze functie zorgt ervoor dat continue monitoring is en automatische beperkingen van netwerkaanvallen.

## Elevation of Privilege
De webpagina's worden afgeschermd per rol en is aangepakt geweest. Een eventuele oplossing kan zijn om rollen te gaan toekennen aan bepaalde acties die een gebruiker mag uitvoeren. Azure AD Multi-Factor Authentication dit zorgt ervoor dat je meerdere verificatiemethoden kan gebruiken om te gaan inloggen in de webapp. Waardoor de ingelogde gebruikers hun rol gaan hebben (admin, cashier, loaner) door roles te gaan koppelen binnenin Azure AD.

## Insiders
Is ook een probleem die we kunnen aanvaarden binnen de webaplicatie. Als men dit toch wil gaan aanpakken kan men gebruikmaken van Microsoft Purview Insider Risk Management. Helpt om interne risico’s te minimaliseren door u in staat te stellen van kwaadaardige en opzettelijke activiteiten in uw organisatie op te sporen, te onderzoeken en erop te reageren. Zorg voor een Root User die het systeem opstart en eventuele andere admin zaken kan gaan uitvoeren.

## Bot attacks
Wordt niet aangepakt hierdoor kan een aanvaller van op het web gebruikmaken van geautomatiseerde web requests om een website, applicatie, API of eindgebruikers te manipuleren, te bedriegen of te verstoren.
### Hoe aan te pakken?
Azure Web Application Firewall service die een bescherming aanbiedt tegen webhackingtechnieken, zoals SQL injection en beveiligingsproblemen als site-overschrijdende scripts.
