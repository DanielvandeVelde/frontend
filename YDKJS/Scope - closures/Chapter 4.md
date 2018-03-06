# Simpson, Kyle, You Don't Know JS: Scope & Closures
## Chapter 4: Hoisting

### Kip of het ei?
```javascript
a = 2;

var a;

console.log(a);
```
Hoewel het lijkt alsof hier ``undefined`` uit zal komen, geeft de console.log ``2`` terug.
In de code:
```javascript
console.log( a );

var a = 2;
```
Komt er echter undefined uit.
Wat komt er dus eerder, de declaratie of de toewijzing?

### Compiler
``var a = 2;`` wordt behandeld als ``var a``, ``a =2;``.  
Waarbij het eerste de compilatie is en het tweede deel de excecutie.  

Variabele en functie declaraties worden opgehaald naar de start van de code, niet pas waar ze worden toegewezen.   
Dit heet **hoisting**. De declaratie komt voor de toewijzing.
Functie expressies worden niet ge'__hoist__.

Voorbeeld:   
```javascript
foo(); // TypeError
bar(); // ReferenceError

var foo = function bar() {
	// ...
};
```
wordt dus:   
```javascript
var foo;

foo(); // TypeError
bar(); // ReferenceError

foo = function() {
	var bar = ...self...
	// ...
}
```

### Functies eerst
Functies worden eerst gehoist, daarna variabelen.
```javascript
foo(); // 1

var foo;

function foo() {
	console.log( 1 );
}

foo = function() {
	console.log( 2 );
};
```
Dan wordt er ``1`` geprint, ipv ``2`` want function eerst, dan de ``foo();`` dan de declaratie ``foo = function()``
