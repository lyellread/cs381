+------------+
| Question 1 |
+------------+

Determine the type/behavior of the following expressions under static and dynamic typing. 

`if even x then x else even (x+1)`

Answer: 

```
Static: Type Error (even(x+1) is bool, while x is int
Dynamic: dynamic: Int if even x; otherwise Bool
Type of X: Int
```

+------------+
| Question 2 |
+------------+

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
Correct
sem :: Action -> Balance -> Balance
sem (Deposit x) b = b + x
sem (Withdraw x) b
    | x > b = 0
    | otherwise = b - x
sem Fee b = b - 1
sem (Sequ a1 a2) b = sem a2 $ sem a1 b
```

+------------+
| Question 3 |
+------------+

Consider the following syntax excerpt from a language for computing with numbers and lists of numbers.

`exp  ::=  ...  |  num  |  []  |  exp:exp  |  head exp`

Which of the following type definitions for D are appropriate semantic domains for defining the denotational semantics of the language?

- `type D = (Int,[Int])`
- `data D = I Int | L [Int]`
- [C]: `data Val = I Int | L [Int]`, `type D = Maybe Val`
- [C]: `data D = I Int | L [Int] | Error`

+------------+
| Question 4 |
+------------+

Determine the type/behavior of the following expressions under static and dynamic typing. 

`tail [even x]`

Answer:

```
Assumption: tail has signature [a] -> [a], therefore, [Bool] can be operated on using head and tail.

Static: static: [Bool] dynamic: [Bool] variables: x :: Int
Dynamic: returns empty list or error (depending on how implemented tail works)
```

+------------+
| Question 5 |
+------------+

Consider the following excerpt from a context-free grammar for statements.

`stmt  ::=  ...  | if exp then exp else stmt  |  while exp do stmt`

Define the abstract syntax for the shown productions as a Haskell data type. Assume that a type Exp has already been defined; you can use in your definition.

Answer:

```hs
Correct
data Stmt = While Exp Stmt
			| If Exp Exp Stmt
```

+------------+
| Question 6 |
+------------+

Determine the type/behavior of the following expressions under static and dynamic typing. 

`not (tail [x,True])`

Answer:

```
Assumption: X is a value, such as Int, Bool, not a list or part of the list. I.e. this cannot 'expand' to [1, 2, 3, 4, True]
Assumption: tail here has signature [a] -> [a]

Static Typing: Type Error [Bool] cannot be treated as bool.
Dynamic Typing:  Error [Bool] cannot be treated as bool.
```

+------------+
| Question 7 |
+------------+

Determine the type/behavior of the following expressions under static and dynamic typing. 

`(head [x,True]) + 1`

```
Correct
Assumption: x cannot be expanded into list, and is a typed object, such as Int, Bool. 
Assumption: head here has signature [a] -> a

Static: Type Error: type of X not defined. 
Dynamic: if type(x) == Int, _ else Type Error : invalid operation for type(x) '+'
-> assumed type of x is Int
```

+------------+
| Question 8 |
+------------+

Determine the type/behavior of the following expressions under static and dynamic typing. 

``head [1 `div` x,x]``

Answer:

```
Static: Int dynamic: Int ("Runtime Error if x==0" is also OK)
Dynamic: if type(x) != Int: Type Error (invalud type for div), else _
-> assumed type of x is int
```

+------------+
| Question 9 |
+------------+

Which sentences can be derived with the following grammar?

`d  ::=  a d a  | b d b  |  ε`

- [C] abba
- [C] abaaba
- [C] aa
- abab
- aabb
- aabaa

+-------------+
| Question 10 |
+-------------+

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