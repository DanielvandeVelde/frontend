# Simpson, Kyle, You Don't Know JS: Scope & Closures
## Chapter 3: Function vs. Block Scope

### Nesten
Scope bestaat uit verschillende buckets, bubbels, containers.. Hoe je ze ook wilt noemen.  
Maar wat is precies dit nesten, enkel functies.  

### Scope uit functies
Als functies binnen functies genest zitten dan hebben die een eigen scope bubbel.   
Deze zijn dan niet beschikbaar buiten deze bubbel.   
In dit voorbeeld:  

```javascript
function foo(a) {
	var b = 2;

	// some code

	function bar() {
		// ...
	}

	// more code

	var c = 3;
}
```
Is het onmogelijk om in de globale scope ``bar();`` te doen of ``a``,``b`` of ``c`` op te vragen.  
Aangezien deze in ``bar()`` zitten in ``foo()``.  

### Verstoppertje
Variabele kan je dus in verschillende scopes van functies stoppen en deze op die manier verstoppen.  
Je kan deze ook in de globale scope zetten maar volgens het __software design principle "Principle of Least Privilege"__ is het beter om zo min mogelijk openbaar te maken.  

#### Ontwijken
Door deze variabelen te nesten in meerdere scopes van functies zorg je er ook voor dat deze elkaar niet overschrijven.  
Zo vermijd je conflicten en botsingen tussen je belangrijke variabelen met waardes.  
Als je __Libraries__ niet verstopt/correct nest dan kan het zijn dat variabelen met elkaar botsen omdat deze worden gebruikt in verschillende __Libraries__
__Modules__ kunnen ook als methode gebruikt worden om er voor te zorgen om variabele in een andere scope te declareren zodat deze niet in botsing met elkaar komen.

### Functies als scopes
Het stoppen van regels code in een functie, verstopt alle variabele of functie declaraties in die functie zijn scope.   
Door een ``()`` om de functie te zetten zoals dit: ``(function foo(){...})();`` is de functie geen declaratie.  
De functie is dan een functie-expressie en geen standaard declsaratie meer.   
Het grootte verschil hierin is dat er dus geen rommel achterblijft maar je deze ook moeilijker kan aanroepen.  

### Anoniem vs. Naam
``function(){...}`` is een anonieme functie expressie.
Deze hebben echter als nadelen dat ze:  
1. Geen naam in stack traces wat debuggen moeilijker maakt
2. Zonder naam kan het amper naar zichzelf verwijzen
3. Anonieme functies maken het moeilijker om de code te lezen en begrijpen. Een beschrijvende naam werkt beter.

``function timeoutHandler() {...}`` is een inline functie expressie.  
Deze heeft een naam en daardoor zie je gelijk wat deze doet.

### Direct functie expressies aanroepen
Als je functie een expressie is door het te omringen dmv ``()`` kan je deze ook direct uitvoeren door een ``()`` om het einde.   
Dit ziet er dan als volgt uit: ``(function foo(){ ... })()``.    
Dit is een bekend patroon en is daarom door __de community__ **IIFE** genoemd.   
**I**mmediately **I**nvoked **F**unction **E**xpression.   

### Blocks als Scopes
Een blockscope is het declareren van een variabele zo dicht en locaal mogelijk waar ze gebruikt worden.  
Een voorbeeld hiervan is de veelgebruikte ``i`` in een for-loop.  
Echter is deze ``i`` ook beschikbaar in de for-loop zelf en niet enkel in de declaratie.  
``with`` is een vorbeeld van een blockscope wat enkel binnen het ``with``-statement leeft.  
``try/catch`` als hier binnen een declaratie wordt gedaan is deze block-scoped tot het ``catch`` blok
``let`` is naast ``var`` een manier om variabele te declareren. ``let`` pakt de volledige blockscope om zijn variabele te declareren.  
Wanneer dit in een for-loop gebruikt wordt is deze enkel bij de declaratie beschikbaar en niet verder in de for-loop zelf.

Blockscope is geen vervanging voor de ``var`` functie scope.  
Gebruik een van deze wanneer ze nodig zijn en ze leven samen in harmonie samen om betere code te creeren.  

