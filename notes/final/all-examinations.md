# Midterm Practice Exam

## Question 1

This is a practice exam to illustrate some additional kinds of questions that can appear on the midterm and final exam.

This practice exam will not be graded, and the result will not be considered for the course grades. 

Which sentences can be derived with the following grammar?

```
s ::= a s b  |  s b  |  ε
```

[C] aabb
ba
aab
[C] abb
[C] aabbb
aababb
[C] bb
[C] ab


## Question 2

Consider the following excerpt from a context-free grammar for expressions.

```
exp  ::=  ...  |  exp + exp  |  exp++
```

Define the abstract syntax for the shown productions as a Haskell data type.

> `data Exp = ... | Plus Exp Exp | Inc Exp`

## Question 3

Consider the abstract syntax for the following expression language. The constant `Zero` denotes 0, and the operation `Succ` computes successors. The operation `Sum` takes a list of expressions as arguments and computes its sum. `IfPos` returns the value of the first expression if its value is greater than 0. Otherwise, it returns the value of the second expression. You may use the function `sum :: [Int] -> Int` in your solution.

```hs
data Expr = Zero 
          | Succ Expr 
          | Sum [Expr] 
          | IfPos Expr Expr
```

Define the semantics of the expression language as a function sem of the following type.

Answer:

```hs
sem :: Expr -> Int
sem Zero      = 0
sem (Succ e)    = 1 + sem e
sem (Sum e)     = sum (map sem e)
sem (IfPos ea eb)
          | sem ea > 0 = sem ea
          | otherwise = sem eb
```

## Question 4

Consider the abstract syntax of a language for describing movements on a two-dimensional grid. The meaning of JumpTo p is to immediately go to the position indicated by p, regardless of the current position. In contrast, the command UpBy i moves from the current position up by i units. The operation Right yields the position given by 1 unit to the right of the current position. Finally, Seq m m' first performs the move m and then the move m'.

```hs
type Pos = (Int,Int)

data Move = JumpTo Pos
          | UpBy Int
          | Right
          | Seq Move Move
```

Define the semantics of the language Move by completing the following function definition.

Answer:

```hs
sem :: Move -> Pos -> Pos
sem (JumpTo p)  _     = p
sem (UpBy n)  (x, y)    = (x, y+n)
sem Right     (x, y)    = (x+1, y)
sem (Seq m1 m2) p     = sem m2 $ sem m1 p
```

## Question 5

Determine the type/behavior for each of the following expressions under static and dynamic typing. Remember that even is a function of type Int -> Bool, that fst selects the first element of a pair, and that head/tail select the first element/rest list of a list. Note carefully:

- Assume strong typing.
- Make optimistic type assumptions for the variable x that make the expression as type correct as possible.
- If applicable, mention the type assumed for x.
- Where applicable, mention the condition(s) under which dynamic typing yields what types.

Here is an example:

if x=3 then x+1 else not x
Static: Type Error
Dynamic: Int if x=3, otherwise Type Error
Type of x: Int

(a) if x<2 then even x else x
> Static: Type Error: even returns bool, while else x returns int

> Dynamic: if x<2 then Type Error else Int

> Type of x: Int

(b) if head x then x else tail x
> Static: [Bool]

> Dynamic: [Bool]

> Type of x: [Bool]

(c) if x then x+1 else x-1
> Static: Type Error: if requires bool, +/- require Int

> Dynamic: Type Error: can do neither + nor - on Bool

> Type of x: Bool

(d) if False then "Hello" else x
> Static: String

> Dynamic: String

> Type of x: String

(e) if fst x==1 then x+1 else fst x
> Static: Type Error

> Dynamic: if fst x == 1 then Type Error else Int

> Type of x: Tuple (Int, a)

## Question 6

Consider the following abstract syntax for a language for nested pairs of booleans, that is, pairs that can contain booleans and other pairs. Fls and Tru represent boolean constants. The operation Pair constructs a pair from its arguments. We can extract the first component of a pair using First, and the operation Swap exchanges the first and second component of a pair.

```hs
data Expr = Fls
          | Tru
          | Pair Expr Expr
          | First Expr
          | Swap Expr
```

Which of the following expressions should be considered to be not type correct by a type checker for that language?

> -> First (First (Pair Fls Tru))   : cant get first of Tru as First expects ()

> -> Swap (Pair Tru)          : Incomplete Pair

> -> First (Fls Tru)          : No Constructor

> -> Swap (First (Pair Fls Tru))    : Cant swap item

> Swap (Pair Tru (Pair Tru Fls))

> Swap (Swap (Pair Fls Tru))

> First (Swap (Pair Fls Tru))

# Midterm - CS381

## Question 1

Determine the type/behavior of the following expressions under static and dynamic typing. 

`if even x then x else even (x+1)`

Answer: 

```
Static: Type Error (even(x+1) is bool, while x is int
Dynamic: dynamic: Int if even x; otherwise Bool
Type of X: Int
```

## Question 2

Consider the following abstract syntax for describing actions on an account. The operation Deposit adds money to the balance while Withdraw takes money off the balance, but no more than is available in the account, i.e., the balance must not become negative. The Fee operation deducts a fee of 1 from the account, and finally, Sequ a1 a2 first performs the account action a1 and then the action a2.

```
type Balance = Int

data Action = Deposit Int
            | Withdraw Int
            | Fee
            | Sequ Action Action
```

Define the semantics of the expression language as a function sem of the following type.

`sem :: Action -> Balance -> Balance`

The structure for the first case is spelled out as an example..

`sem (Deposit x) b = ...`

Answer: 

```hs
sem :: Action -> Balance -> Balance
sem (Deposit x) b = b + x
sem (Withdraw x) b
    | x > b = 0
    | otherwise = b - x
sem Fee b = b - 1
sem (Sequ a1 a2) b = sem a2 $ sem a1 b
```

## Question 3

Consider the following syntax excerpt from a language for computing with numbers and lists of numbers.

`exp  ::=  ...  |  num  |  []  |  exp:exp  |  head exp`

Which of the following type definitions for D are appropriate semantic domains for defining the denotational semantics of the language?

- `type D = (Int,[Int])`
- `data D = I Int | L [Int]`
- [C]: `data Val = I Int | L [Int]`, `type D = Maybe Val`
- [C]: `data D = I Int | L [Int] | Error`

## Question 4

Determine the type/behavior of the following expressions under static and dynamic typing. 

`tail [even x]`

Answer:

```
Assumption: tail has signature [a] -> [a], therefore, [Bool] can be operated on using head and tail.

Static: static: [Bool] dynamic: [Bool] variables: x :: Int
Dynamic: returns empty list or error (depending on how implemented tail works)
```

## Question 5

Consider the following excerpt from a context-free grammar for statements.

`stmt  ::=  ...  | if exp then exp else stmt  |  while exp do stmt`

Define the abstract syntax for the shown productions as a Haskell data type. Assume that a type Exp has already been defined; you can use in your definition.

Answer:

```hs
Correct
data Stmt = While Exp Stmt
               | If Exp Exp Stmt
```
## Question 6

Determine the type/behavior of the following expressions under static and dynamic typing. 

`not (tail [x,True])`

Answer:

```
Assumption: X is a value, such as Int, Bool, not a list or part of the list. I.e. this cannot 'expand' to [1, 2, 3, 4, True]
Assumption: tail here has signature [a] -> [a]

Static Typing: Type Error [Bool] cannot be treated as bool.
Dynamic Typing:  Error [Bool] cannot be treated as bool.
```

## Question 7

Determine the type/behavior of the following expressions under static and dynamic typing. 

`(head [x,True]) + 1`

```
Assumption: x cannot be expanded into list, and is a typed object, such as Int, Bool. 
Assumption: head here has signature [a] -> a

Static: Type Error: type of X not defined. 
Dynamic: if type(x) == Int, _ else Type Error : invalid operation for type(x) '+'
-> assumed type of x is Int
```

## Question 8

Determine the type/behavior of the following expressions under static and dynamic typing. 

``head [1 `div` x,x]``

Answer:

```
Static: Int dynamic: Int ("Runtime Error if x==0" is also OK)
Dynamic: if type(x) != Int: Type Error (invalud type for div), else _
-> assumed type of x is int
```

## Question 9

Which sentences can be derived with the following grammar?

`d  ::=  a d a  | b d b  |  ε`

- [C] abba
- [C] abaaba
- [C] aa
- abab
- aabb
- aabaa

## Question 10

Consider the following abstract syntax for a language for non-nested integer lists, that is, lists that can only contain integers and not other lists. With N we represent integer constants. The constant Empty denotes an empty list, and the operation Cons adds an integer (given as the first argument) to a list. We can extract the first element of a list using Head, and the operation Length represents a function to compute the length of a list.

```hs
data Expr = N Int
          | Empty
          | Cons Expr Expr
          | Head Expr
          | Length Expr
```

Which of the following expressions should be considered to be type correct by a type checker for that language?

- `Cons Empty (Cons (N 1) Empty)`
- [C] `Cons (N 1) Empty`
- [C] `Head Empty`
- [C] `Cons (Length Empty) Empty`

# Quiz 1

## Question 2

True or false? A Haskell list may contain elements of different types.

> False
 
## Question 3

True or false? Every Haskell function has a type.

> True
 
## Question 4

True or false? Variables and constructors can be part of a pattern.

> True
 
## Question 5

True or false? Values can be part of a pattern.

> True 
 
## Question 6

Consider the following function definition.

```hs
f _ [] = []
f y (x:xs) | x>y       = x:f y xs
           | otherwise = f y xs
```

Describe in your own words what f applied to a value v and list l is doing.

> `f` only keeps elements of `l` if they are larger than `v`.

 
## Question 7

Complete the following Haskell function definition downFrom that produces a consecutive list of numbers that starts with a number given as argument and ends in 1. For example, downFrom 3 results in [3,2,1]. Give a recursive definition. (You can ignore negative arguments.) 

```hs
downFrom :: Int -> [Int]
downFrom 0 = []
downFrom ___ = ________________
```

Parameter: 

> `n`

Result expression: 

> `n : downFrom (n-1)`
 
## Question 8

As a reminder of how functions are evaluated in Haskell, here is the definition of the function `fac` and the evaluation trace for the expression `fac 3`.

```hs
fac 1 = 1
fac n = n * fac (n-1)
```

Evaluation trace for `fac 3`:

```
fac 3
= 3 * fac (3-1)
= 3 * fac 2
= 3 * 2 * fac (2-1)
= 3 * 2 * fac 1
= 3 * 2 * 1
= 6
```

Now consider the following definition of the function `sum`.

```hs
sum [] = 0
sum (x:xs) = x + sum xs
```

Show the evaluation trace for the expression `sum [5,2]`.

Your Answer:

```
sum [5,2]
= 5 + sum [2]
= 5 + 2 + sum []
= 5 + 2 + 0
= 7
```
 
## Question 9

Consider the following definition, which we assume is type correct.

`ys = map reverse xs`

Which of the following statements are correct?

- [C] xs must be a list of lists 
- The last element in xs is the first element in ys 
- xs can be a list of numbers 
 
## Question 10

Consider the following definition, which we assume is type correct.

`ys = map head xs`

Which of the following statements are correct?

- [C] ys can be a list of numbers
- [C] Each element of xs must be a list 
- [C] ys has the same length as xs 

# Quiz 2

## Question 2

Describe in your own words precisely what sentences/words can be derived from N with the following grammar.

`N  ∶∶=  aN b  ∣  ε`

> This grammar can either create no sentences/words, or it can create a sentence/word that is any number 1..infinity, where n=1 would be a b and n=4 would be aaaa bbbb and so on

## Question 3

One nonterminal symbol can have multiple terminal and nonterminal children in a syntax tree.

> True

## Question 4

Given a syntax tree for a sentence that was obtained from a derivation, we can reconstruct the derivation.

> False

## Question 5

We can always construct the syntax tree for a sentence from a derivation for the sentence.

> True

## Question 6

A nonterminal symbol always has the same number of children in a syntax tree.

> False

## Question 7

The following data type definition is a good representation of binary numbers.

```hs
data Digit = Zero | One
data Bin = Single Digit | Many Digit Bin
```

Why is the following type not a good representation for binary numbers?

```hs
data Bin = Single Digit | Many Digit Bin Digit
```

> It can only create binary numbers of odd length

## Question 8

Which of the following data type definitions are acceptable as representations of a grammar for binary numbers?

> All the following are:

```hs
data Digit = Zero | One
data Bin = A Digit | B Digit Digit | C Bin Digit
```

```hs
data Bin = A | B | Conc Bin Bin
```

```hs
data Bin = Single Bool | Many Bool Bin
```

## Question 9

Consider the following grammar for describing meeting times that can be given by either time values or intervals. The rules for the nonterminal time are not important here; assume that the Haskell type Time represents specific time values.

```
mtg ::= at time | from time to time
```

Which of the following data type definitions for Mtg are a correct representation of the abstract syntax for the mtg grammar? Explain for each case that you consider to be incorrect, why it is incorrect.
```
(a) data Mtg = At Time | From Time Time
(b) data Mtg = From Time Time | At Time
(c) data Mtg = At Time | From Time To Time
(d) data Mtg = From Time | At Time Time
(e) data Mtg = Time | From Time Time
```


> `(a) data Mtg = At Time | From Time Time`
> Correct

> `(b) data Mtg = From Time Time | At Time`
> Correct

> `(c) data Mtg = At Time | From Time To Time`
> Incorrect: To is out of place. 

> `(d) data Mtg = From Time | At Time Time`
> Correct, though confusing constructor names

> `(e) data Mtg = Time | From Time Time`
> No constructor for at time, violates grammar to dt rule "each case of data type must have constructor" or no argument.

## Question 10

Consider the following context-free grammar defining commands for controlling a video player.

```
control ::= start | stop | forward num seconds
```

Define an abstract syntax for this grammar in the form of a Haskell data type definition.

>`data Control = Start | Stop | Forward Int`

# Quiz 3

## Question 2

Evaluate the following expression and write down the result.

```
let x=1
    y=(x,x+1)
 in let x=(y,4)
     in (x,y)
```

> `(((1,2),4),(1,2))`

## Question 3

Consider the following block. Draw the runtime stacks that result immediately after the statements on lines 6, 3, and 7 have been executed.

```
 1 { int u := 2;
 2   { int f(int v) {
 3       u := u*v;
 4       return (u-3);
 5     };
 6     { int u := 3;
 7       u := f(u+2);
 8     };
 9   };
10 }
```

(a) Draw the three runtime stacks (after lines 6, 3, and 7) for dynamic scoping.

(b) Draw the three runtime stacks (after lines 6, 3, and 7) for static scoping.

Answer: 

```
a6 [u:3, f{}, u:2]
a3 [[u:15, v:5], u:3, f{}, u:2]
a7 [u:12, f{}, u:2]

b6 [u:3, f{}, u:2]
b3 [[u:10, v:5], u:3, f{}, u:10] //both u:10 are same var, just one is in function frame for ease of reading
b7 [u:7, f{}, u:10]

//function 'frame' showed for visibility /readability only
```
## Question 4

Illustrate the evolution of the runtime stack from line 3 up until after the assignment on line 8 under the parameter passing schemes Call-by-Reference and Call-by-Value-Return.

```
1 { int x := 3;
2   int y := 2;
3   int f(int p) {
4     x := p*2;
5     p := x-1;
6     return (x+p);
7   };
8   y := f(x);
9 }
```

(a) Call-by-Reference

(b) Call-by-Value-Return

Answer:

```
[x:3]
[y:2, x:3]
[f{}, y:2, x:3]
3: [p->x, f{}, y:2, x:3]
4: [p->x, f{}, y:2, x:6]
5: [p->x, f{}, y:2, x:5]
6: [ret:10, p->x, f{}, y:2, x:5]
8: [f{}, y:10, x:5]

cbvr
[x:3]
[y:2, x:3]
[f{}, y:2, x:3]
3: [p:3, f{}, y:2, x:3]
4: [p:3, f{}, y:2, x:6]
5: [p:5, f{}, y:2, x:6]
6: [ret:11, p:5, f{}, y:2, x:6]
8: [f{}, y:11, x:5]
```

# Quiz 4

## Question 2

Consider the following predicate.

```
speaks(sue,english).
speaks(sue,spanish).
speaks(bob,spanish).
speaks(maria,french).
```

Based on speaks/2, answer the following questions. Hint: Requiring the inequality of two variables X and Y can be expressed using the subgoal X \= Y.


(a) Write a goal to find all people who speak Spanish.

> `speaks(X, spanish).`

(b) Define a predicate talk/2 that finds pairs of people that can talk to one another in the same language.

> `talk(X, Y) :- speaks(X, Z), speaks(Y, Z).`

(c) Define a predicate bilingual/1 that yields true for people that can speak two languages.

> `billingual(X) :- speaks(X, B), speaks (X, C), B \= C.`

## Question 3

The following predicates that describe songs (first argument) and their performers (second argument) offered by two different streaming services.

```
itunes(s,p).
itunes(t,p).
itunes(u,q).

spotify(s,p).
spotify(v,q).
spotify(u,q).
```

(a) Define a predicate songs/1 that can produce all the songs available on any of the two streaming services.

> Assumption: "all the songs available on any of the two streaming services." == both streaming services. Logical structure of this sentance is ambiguous in english. "either of the two" or "one of the two" are more precise.
> `songs(X) :- itunes(N, Y); songs(X) :-  spotify(Z, W).`

(b) Using the predicate in (a), write a goal/query that lists all songs that can be listened to, irrespective of the streaming service that offers it.

> `songs(none).`

(c) Define a predicate performedBy/2 that lists all songs by a specific performer that are available on any of the two streaming services.

> NOTE: composer passed in arg B: `performedBy(A, B) :- itunes(T, B); spotify(U, B).`

(d) Using the predicate in (c), write a goal/query that lists all songs  performed by "p", irrespective of the streaming service that offers the song.

> `performedBy(none, p).`

## Question 4

The predicate answer/3 gives facts about how students have answered numbered multiple-choice questions (for example, answer(john,1,b) says that John has selected answer b for question 1). The predicate key/2 shows the correct answer for each question.

```
answer(john,1,b).  answer(mary,1,c).  key(1,c).
answer(john,2,a).  answer(mary,3,b).  key(2,a).
answer(john,3,b).  answer(tim,3,c).   key(3,b).
```

These predicates are the basis for the following questions.

(a) Define a predicate correct/2 that determines for a given question all the students who have answered that question correctly.

> `correct(A, B) :- answer(X, A, B).`

(b) Define a predicate discuss/2 that is true for those pairs of students who have answered a particular question differently.

> `discuss(A, B) :- answer(X, A, B), answer(Y, A, Z), Z \= B, X \= Y.`

(c) How many answers will Prolog produce for the goal discuss(X,Y)?

> 6