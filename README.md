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

Als men bijvoorbeeld een aankoop is uitgevoerd zonder dat men dit wou kan een persoon dit ontkennen. Zorg er dus dan ook voor dat deze zaken monitoring bevatten en zorg voor een eventueel pdf document dat is uitgevoerd.
Wat goed gedaan is in het systeem is dat men rollen is beginnen toekennen in het systeem en dat bepaalde sites hierdoor afgeschermd zijn als men eventueel geen access mag hebben tot een bepaalde link of actie.
Aangezien het systeem bedoelt is voor de school kon men perfect de Azure AD van school gaan gebruiken want Azure beschikt over een tal van functies die zeer handig kunnen zijn voor je systeem. Zo presteert je systeem optimaal en is de veiligheid van je systeem goed gehandhaafd. 
