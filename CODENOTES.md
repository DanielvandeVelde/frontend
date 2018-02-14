# Notes

## Simpson, Kyle, You Don't Know JS: Scope & Closures (chapter 1)
* Chapter 1: What is Scope?
* Chapter 2: Lexical Scope
* Chapter 3: Function vs. Block Scope
* Chapter 4: Hoisting
* Chapter 5: Scope Closures

### Chapter 1: What is Scope?
#### De Scope
Waardes opslaan in variabele is het meest fundamentele concept.
Waar de variabele opgeslagen worden, vinden en waar deze opgehaald worden zitten regels aan vast. 
Deze set met regels noemen we een **Scope**.

#### Compiler theory
1. Tokenizing/Lexing

_Tokenizing_ is het opdelen in stukjes zoals ``var a = 2;`` in ``var``, ``a``, ``=``, ``2`` en ``;`` (en de witruimte als dat nodig is)
Verschil tussen _Tokenizing_ en _Lexing_ is dat met een state of staatloos worden opgeslagen.
Als er wordt gekeken of ``a`` een los onderdeel is of ergens onderdeel van is dan spreken we van _lexing_.

2. Parsing

Een stroom van tokens omzetten in een boom van elementen die de structuur van het programma vormen. Deze boom noemen we een "AST" een **A**bstract **S**yntax **T**ree.
De boom van ``var a = 2;`` kan beginnen met een ``VariableDecleration`` die een ``Identifier`` heeft met de waarde ``a``, die zelf een ``AssignmentExpression`` heeft met een _child_ ``NumericLiteral`` met de waarde ``2``.

3. Code-Generation
Het gebruiken van een _AST_ om uitvoerbare code te generen. Dit ligt voornamelijk aan de code zelf en het bedoelde platform.
Dit zet de eerdergenoemde ``var a = 2;`` om in een zet instructies die er voor zorgen dat er een variabele _gemaakt_ wordt genaamd ``a`` (tevens ook geheugen reserveerd, etc) en deze een waarde krijgt.

Natuurlijk is de _JavaScript Engine_ moeilijker da enkel dat, maar dit is wat op dit moment belangrijk is.
Het compileren van JavaScript duurt vaak slechts milliseconde, deze gebruikt diverse trucjes (zoals _JIT_ en recompilen) maar dit valt buiten onze **_Scope_** van discussie. Javascript wordt dus _eerst_ gecompiled en dan klaargezet om uit te voeren, vaak vrijwel direct.

#### Understanding Scope
Om Scope te illusteren gebruiken we een gesprek als voorbeeld.
Onze hoofdpersonen zijn:
1. Engine: Verantwoordelijk voor het A tot Z compileren en executie van het JavaScript programma.
2. Compiler: Een van _Engine_ zijn beste vrienden; doet al het _parsing_ en _code generatie_ werk.
3. Scope: Nog een vriend van _Engine_; verzamelt en houdt een lijst bij met alle gedeclareerde _identifiers_(variabelen), en hanteerd een set harde regels over hoe deze bereikt kunnen worden.

Een programma zoals ``var a = 2;`` lijkt slechts een (1) programma maar dat is niet hoe _Engine_ dit ziet, die ziet namelijk twee verschillende statements.

Dit is hoe _Engine_ en zijn vrienden aan de slag gaan met het programma:
1. _Compiler_ gebruikt lexing om er tokens van te maken en _parsed_ dit daarna in een tree.
2. _Compiler_ ziet ``var a``, vraagt vervolgens aan _Scope_ of variabele ``a`` al bestaat. Zo ja, dan wordt dit genegeerd en anders vraagt _Compiler_ aan _Scope_ om deze toe te voegen aan zijn scope collectie.
3. _Compiler_ maakt vervolgens code zodat _Engine_ ``a = 2`` kan uitvoeren. _Engine_ vraagt eerst aan _Scope_ of een variabele ``a`` toegankelijk is in de scope selectie. Zo ja, gebruikt _Engine_ deze variabele. Zo niet, kijkt _Engine_ ergens anders.
Als deze eenmaal een variabele vind geeft deze de waarde ``2`` daar aan. Zo niet, dan komt er een error!

*Samenvattend:* Eerst declareerd _Compiler_ een variabele (als deze er niet al in in de scope) en vervolgens wanneer deze uitgevoerd wordt zoekt _Engine_ de variabele op in _Scope_ en wijst deze waarde toe.

Wanneer _Engine_ de code uitvoerd zoekt het de variabele op dmv _Scope_ maar het type van dit opzoeken wat _Engine_ gebruikt heeft invloed op de uitkomst.
Het opzoeken in deze zin is een "LHS", de andere manier is "RHS".
Dit staat voor "Left-Hand Side" and "Right-Hand Side" van een **toewijzing**.

LHS is wanneer die aan de linkerkant staat en zoekt de variabele container zelf zodat het dit kan toewijzen. (TARGET)
RHS is 'niet links' en zoekt dus enkel de waarde. (SOURCE)

**_Dit "LHS en RHS" betekend dus niet dat deze altijd links of rechts moeten staan_**  
**Voorbeelden!**  
``console.log( a );``  
``a`` is een RHS referentie want ``a`` heeft geen waarde hier. We zoeken de waarde op zodat deze gegeven kan worden. (SOURCE)

``a = 2;``  
``a`` is een LHS regerentie. Het maakt niet uit wat de waarde is, we hoeven hem alleen te vinden om hem de waarde twee gegeven kan worden. (TARGET)


Stuk code met gespreksvoorbeeld:
```javascript
function foo(a) {
  console.log( a ); //2
}

foo( 2 );
```

> **Engine:** Hey _Scope_, ik heb een RHS referentie voor ``foo``, ken je dit?  
> **Scope:** Ja, _Compiler_ heeft het gedeclareerd. Het is een functie.  
> **Engine:** Top, dan doe ik nu ``foo``  
> **Engine:** Hey _Scope_ Ik heb een LHS referentie voor ``a``, ken je dit?  
> **Scope:** Ja, _Compiler_ heeft het als een parameter voor ``foo``  
> **Engine:** Top, nu ``2`` naar ``a``verwijzen.  
> **Engine:** Hey _Scope_ ik heb een RHS referentie voor ``console``, ken je dit?  
> **Scope:** Ja, ``console`` is ingebouwd. Alsjeblieft.  
> **Engine:** Top, ik zoek ``log()`` op... het is een functie.  
> **Engine:** Hey _Scope_, ik heb een RHS referentie voor ``a``. Ik ken het maar wil het double-checken.  
> **Scope:** Ja, zelfde waarde. Niks aangepast.  
> **Engine:** Top, dan gaat de waarde van ``a``, wat ``2`` is, naar ``log(..)``.  

Als eerst vraagt de _Engine_ om een RHS binnen een functie wanneer een variabele daar is aangeroepen. 
Dit is een **Nested scope**.  
Wanneer die niet is gevonden dan vraagt _Engine_ om een RHS buiten deze functie.  
Dit blijft doorgaan tot hij de **Globale scope** bereikt.  

#### Errors

Wanneer een RHS zoek faalt in de nested _Scopes_ dan is dit een "niet gedeclareerde" variabele, omdat deze niet gevonden is in de _Scope_.  
Als deze nergens wordt gevonden is dit een ``ReferenceError``.  
Bij een LHS wanneer de globale _Scope_ bereikt wordt creeert _Scope_ een nieuwe variabele en geeft deze aan de _Engine_.  
Mocht er iets gedaan worden met een opgehaalde RHS variabele wat niet mogelijk is zoals uitvoeren als functie terwijl het geen functie is komt er een ``TypeError``.  
``ReferenceError`` is dat _Scope_ oplossing faalt.  
``TypeError`` is dat een onmogelijke actie wordt uitgevoerd.  

