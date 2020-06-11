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
sem Zero			= 0
sem (Succ e) 		= 1 + sem e
sem (Sum e)			= sum (map sem e)
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
sem (JumpTo p) 	_			= p
sem (UpBy n) 	(x, y)		= (x, y+n)
sem Right 		(x, y) 		= (x+1, y)
sem (Seq m1 m2) p			= sem m2 $ sem m1 p
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

> -> First (First (Pair Fls Tru)) 	: cant get first of Tru as First expects ()

> -> Swap (Pair Tru) 					: Incomplete Pair

> -> First (Fls Tru)					: No Constructor

> -> Swap (First (Pair Fls Tru))		: Cant swap item

> Swap (Pair Tru (Pair Tru Fls))

> Swap (Swap (Pair Fls Tru))

> First (Swap (Pair Fls Tru))