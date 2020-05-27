# Quiz 4 Learning Notes

### Prolog
- untyped
- computations are expressed with rules that define relations on objects
- running a program / computing a value: formulating a goal or a query
- result of program execution: Yes / No and a binding of free variables
- no higher order predicates

#### Facts, Rules and Prolog
```
For All X: Hunam(x) --> Mortal(x) <-- Rule 			\
Human(Socrates)					  <-- Fact 			,`-> Prolog Program
Therefore: Mortal(Socrates)		  <-- Goal / Query  ---> Program Execution
```

#### Predicates and Sets
- for all intents and purposes, predicates (the basic entities in Prolog) are equivalent to sets
	- i.e. predicates ~= relaion ~= set

### Prolog Facts
- to define the set `color = {red, blue}`:
```prolog
color(red).
color(blue).
```
- to define set `likes = { (john, red), (mary, red), (john, mary) }`:
```prolog
likes(john, red).
likes(mary, red).
likes(john, mary).
```
- above, we are defining the `likes` predicate.

### Prolog Goals and Queries
- after defining the `color` predicate, we can ask prolog for the goal: 
	- `?- color(red)` will result in `true.`
	- `?- color(green)` will result in `false.` (closed world assumption)
- `false.` means that it cannot be determined with the information given.

### Variables
- we can set variables: `?- color(X)` will return (see def's above) `X = red`. 
	- if we hit return, we are satisfied
	- if we enter `;`, we want to see what else X can be (namely `X = blue`)
- another example is doing `?- likes(X, blue)` which will return `false.`
- variables are first order, cannot be used for predicates

### Conjunctions
- comma `,` means and, so to join two queries, do `likes(john, mary), likes(mary, john).`

### Rules
- rules consist of head (`marry(X, Y)`) and body (`likes(X, Y), likes (Y, X).`)
- `:-` reads as `if`
- free variables: `friends(X, Y) :- likes(X, Z), likes(Y, Z).` X is a free variable.

### Overloading and Arity
- consider the pl:
```
car(bmw).						<-- car/1
car(honda, green).				<-- car/2
car(X, Y) :- car(X), color(Y).	<-- car/2, car/1
faster(car, bike)				
```

### Recursion
```
inside(X, Y) :- contains(Y, X).
inside(X, Y) :- contains(Y, Z), inside(X, Z).
```
- will result in:
```
?- inside(mouse, house).
true.
```
for 
```
contains(house, bathroom).
contains(house, kitchen).
contains(kitchen, fridge).
contains(fridge, mouse).
```

### Inner Prolog Functioning 
- this is tough. refer to slides. 

