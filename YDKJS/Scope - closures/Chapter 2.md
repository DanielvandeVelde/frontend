# Simpson, Kyle, You Don't Know JS: Scope & Closures
## Chapter 2: Lexical Scope

### Lexical Scope
Eerder in hoofdstuk 1 staat wat de Scope is en hoe het werkt.  
In dit hoofdstuk staat de meeste gebruikte, de **Lexical Scope**, de andere is **Dynamic Scope**.

### Lex Time
De eerste fase van een standaard compiler is lexing (of tokenizing).   
Deze kijkt naar karakters en geeft deze de semantische betekenis    
Hierover en over nested scopes verwijs ik door naar hoofdstuk 1.  

### Look-ups
Scope look-up stops na de eerste match.  
Globale variabele zijn onderdeel van het globale object.  
Deze kunnen dus ook aangesproken worden als ``window.a``.  

### Cheating Lexical
Is er een mogelijkheid om onder Lexical scope bij run-time uit te komen?  
__Ja__, maar dit zorgt er wel voor dat de performance van verlaagt wordt.  
Dit zijn dus geen goede/correcte opties om te gebruiken.  
**eval()** neemt een string als argument en kan dus code uitvoeren alsof het er al die tijd al geweest was.  
**with()** neemt een object met 0 of meer properties en behandeld deze als een volledig opzichzelf staande lexicale scope, de properties zijn dus lexicaal gedefineerd binnen die scope.  
``eval`` past dus een bestaande lexicale scope aan terwijl ``with`` een nieuwe lexicale scope creert.

** Door het gebruik van deze twee functies loopt de code langzamer, dit is dus geen optimalisatie **
** Gebruik deze dus niet **

