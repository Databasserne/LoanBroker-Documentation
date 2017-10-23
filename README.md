# LoanBroker-Documentation

## Gruppe
* Kasper Worm
* Jonas Simonsen
* Martin Karlsen
* Alexander Steen

Vores LoanBroker projekt er delt op i flere repositories for de enkelte komponenter
* [Translator BankJSON og BankXML](https://github.com/Databasserne/LoanBroker-Translator) 
* [Translator BankSOAP](https://github.com/Databasserne/LoanBroker-CSharp/tree/master/BankSOAP)
* [Translator BankNODE](https://github.com/Databasserne/LoanBroker-Translator-NodeBank)
* [Normalizer](https://github.com/Databasserne/LoanBroker-Normalizer)
* [GetCreditScore](https://github.com/Databasserne/LoanBroker-CSharp/tree/master/CreditScore)
* [BankSOAP](https://github.com/Databasserne/LoanBroker-CSharp/tree/master/BankSOAP)
* [BankNODE](https://github.com/Databasserne/LoanBroker-NodeBank)
* [GetBanks](https://github.com/Databasserne/LoanBroker-GetBanks)
* [RuleBase](https://github.com/Databasserne/LoanBroker-RulesBase)
* [BankRouter](https://github.com/Databasserne/LoanBroker-CSharp/tree/master/BankRouter)
* [Aggregator](https://github.com/Databasserne/LoanBroker-CSharp/tree/master/Aggragator)
* [Dimmer](https://github.com/Databasserne/LoanBroker-CSharp/tree/master/Dimmer)
* [Frontend](https://github.com/Databasserne/LoanBroker-Frontend)

## Indholdsfortegnelse
* [Languages Used](#languages)
* [Class Diagrams](#classdiagrams)
* [Sequence Diagram](#sequencediagrams)
* [Testability](#testability)
* [Bottlenecks & Improvements](#bottheneck/improvement)
* [ScreenShot](#screenshots)

### Languages
Vores program er skrevet i forskellige sprog, vi har brugt Java, C#, JavaScript(Node.js)
De enkelte komponenter:

Translator BankJSON og BankXML: Java <br>
Translator BankSOAP: C# <br>
Translator BankNODE: JavaScript(Node.js) <br>
Normalizer: Java <br>
GetCreditScore: C# <br>
BankSOAP: C# <br>
BankNODE: JavaScript(Node.js) <br>
GetBanks: Java <br>
RuleBase: Java <br>
BankRouter: C# <br>
Aggregator: C# <br>
Dimmer: C# <br>
Frontend: Angular <br>

### ClassDiagrams
#### Dimmer
![Dimmer](https://github.com/Databasserne/LoanBroker-Documentation/blob/master/UML-Pictures/dimmer.png)

Dimmeren er det første og sidste component som bliver kaldt i vores pipeline. 
Til at starte med finder den ud af om der allerede er et request igang på den givne SSN, 
hvis ikke sender den requested videre i pipelinen. 
Hvis der allerede er et request i gang med det SSN vil den ikke sende noget videre men bare huske hvor den skal sende responset tilbage til. 
Så hvis der er blevet lavet 2 request på samme SSN sørger dimmeren for at der kun kommer 1 ud i systemet af gangen men at begge clients modtager svaret.

#### GetCreditScore
![GetCreditScore](https://github.com/Databasserne/LoanBroker-Documentation/blob/master/UML-Pictures/getCreditScore.png)

GetCreditScore tager og consumer en SOAP Web Service som er blevet stillet til råde fra skolen. Den tager så og sender den credit score som den får tilbage videre til banks rules.

#### GetBanks
![GetBanks](https://github.com/Databasserne/LoanBroker-Documentation/blob/master/UML-Pictures/getBanks.png)

GetBanks komponenten sørger for at kontakte RulesBase komponentet, med creditScoren, og dermed samle bankerne og sende dem videre til routeren.
RulesServiceImpl.class er den som skaber forbindelsen til RulesBase komponentet.

#### RuleBase
![RuleBase](https://github.com/Databasserne/LoanBroker-Documentation/blob/master/UML-Pictures/ruleBase.png)

RulesBase er et SOAP web service som er lavet med jax-ws. Alle bankerne implementerer interfacet IBank, som har metoden isRequirementMet(int, double, int); På denne måde kan man let tilføje nye banke, med forskellige requirements.

#### BankRouter
![BankRouter](https://github.com/Databasserne/LoanBroker-Documentation/blob/master/UML-Pictures/bankRouter.png)

Bank router modtager requestet 1 gang med en liste af banker som skal have det request. Den tager så og sender requestet til hver bank.

#### Translator
![Translator](https://github.com/Databasserne/LoanBroker-Documentation/blob/master/UML-Pictures/translator.png)

Translatorerne reciever en besked fra vores bankRouter i vores eget JSON format, derefter sørger den for at messageFormatteren laver det om til det rigtige format den givne bank skal bruge. Efterfølgende sender vi den formatterede message sammen med et correlationId til banken, vi sætter et correlationId for holde styr på hvilken bank normalizeren modtager svar fra. 

#### Bank
![Bank](https://github.com/Databasserne/LoanBroker-Documentation/blob/master/UML-Pictures/banks.png)

BankNode er lavet i Node.js. Når den modtager et request, giver den altid samme interest (3,14), som den sener til replyTo queue’en.
BankSOAP er lavet i C# og er en SOAP Web Service. Den giver en tilfældig rente mellem 2 og 6.

#### Normalizer
![Normalizer](https://github.com/Databasserne/LoanBroker-Documentation/blob/master/UML-Pictures/normalizer.png)

Normalizeren lytter efter svar fra alle bankerne. Den modtager et svar i form af en message med et correlationId fra bankerne, efterfølgende bruges messageFormatteren til at formattere den indkomne message om til vores eget JSON format inden den sendes videre til Aggregatoren.  

#### Aggregator
![Aggregator](https://github.com/Databasserne/LoanBroker-Documentation/blob/master/UML-Pictures/Aggregator.png)

Aggregatoren samler alle inputs fra normalizeren. Den lytter efter det første input den får fra normalizeren og sætter derved en timer i gang hvor den venter 10 sekunder mens den får andre input fra normalizeren. Når de 10 sekunder er gået tager den, den laveste rate og sender videre til dimmeren.

### SequenceDiagrams
![Sequence](https://github.com/Databasserne/LoanBroker-Documentation/blob/master/UML-Pictures/sequence.png)

### Testability
Et eksempel kan være at tage translatoren som eksempel. 
Translatoren får en besked fra recipientList og derefter så håndterer beskeden.
Man sender en test besked afsted, som bliver modtaget i receive() metoden, 
som sender beskeden videre til send() metoden som starter med at kalde messageFormatter(), 
for at få det rigtige format.
I dette tilfælde vil man kunne checke om start beskeden er blevet formateret til det rigtige format, 
altså teste messageFormatter() metoden, altså unit teste den metode.
[Normalizer Test Class](https://github.com/Databasserne/LoanBroker-Normalizer/blob/master/src/test/java/com/databasserne/loanbroker/normalizer/NormalizerTest.java)
En måde man kunne have gjort disse tests bedre på var at lave parameterised tests, som kører samme test flere gange, men med forskellige parametre.

### Botthenecks/Improvements
Vi er usikre på om dette er fejl i vores kode, 
eller for meget belastning på rabbitmq serveren.
Nogle gange modtager vores normalizer ikke alle de beskeder vi sender.
Det er ofte midt på dagen i primetime dette sker. 
Når det testes om aftenen er der ingen problemer og alt går igennem som det skal.

Når man declare queues/exchanges skal man være opmærksom på at det skal gøres på den samme måde, 
hvis man bruger det samme navn, da det ellers vil skabe konflikt og give en runtimeException


### Screenshots
Flowet kan ses i screenshots, som kommer i rækkefølge
![dimmer](https://github.com/Databasserne/LoanBroker-Documentation/blob/master/ScreenShots/Dimmer.png)
![creditscore](https://github.com/Databasserne/LoanBroker-Documentation/blob/master/ScreenShots/CreditScore.png)
![getbanks]()
![rulebase]()
![bankrouter](https://github.com/Databasserne/LoanBroker-Documentation/blob/master/ScreenShots/BankRouter.png)
![translatorNODE]()
![translatorSOAP](https://github.com/Databasserne/LoanBroker-Documentation/blob/master/ScreenShots/TransSOAP.png)
![translatorXML]()
![translatorJSON]()
![bankSOAP]()
![bankNODE]()
![normalizer]()
![aggregator](https://github.com/Databasserne/LoanBroker-Documentation/blob/master/ScreenShots/Aggra.png)

