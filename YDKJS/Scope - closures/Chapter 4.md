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
