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
* [Languages Used](#Languagesused)
* [Class Diagrams](#ClassDiagrams)
* [Sequence Diagram](#SequenceDiagrams)
* [Testability](#Testability)
* [Bottlenecks & Improvements](#Bottheneck/Improvement)
* [ScreenShot](#Screenshots)

### Languages
Vores program er skrevet i forskellige sprog, vi har brugt Java, C#, JavaScript(Node.js)
De enkelte komponenter:
Translator BankJSON og BankXML: Java
Translator BankSOAP: C#
Translator BankNODE: JavaScript(Node.js)
Normalizer: Java
GetCreditScore: C#
BankSOAP: C#
BankNODE: JavaScript(Node.js)
GetBanks: Java
RuleBase: Java
BankRouter: C#
Aggregator: C#
Dimmer: C#
Frontend: Angular

### ClassDiagrams
##### Dimmer
![dimmer] (https://github.com/Databasserne/LoanBroker-Documentation/blob/master/UML-Pictures/dimmer.png)

text

text

text

text

text

### SequenceDiagrams
text

text

text

text

text

text

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

