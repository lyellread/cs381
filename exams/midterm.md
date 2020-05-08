+------------+
| Question n |
+------------+

Determine the type/behavior of the following expressions under static and dynamic typing. 

`if even x then x else even (x+1)`

+------------+
| Question n |
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

+------------+
| Question n |
+------------+

Consider the following syntax excerpt from a language for computing with numbers and lists of numbers.

`exp  ::=  ...  |  num  |  []  |  exp:exp  |  head exp`

Which of the following type definitions for D are appropriate semantic domains for defining the denotational semantics of the language?

`type D = (Int,[Int])`
`data D = I Int | L [Int]`
`data Val = I Int | L [Int]`, `type D = Maybe Val`
`data D = I Int | L [Int] | Error`

+------------+
| Question n |
+------------+

Determine the type/behavior of the following expressions under static and dynamic typing. 

`tail [even x]`

+------------+
| Question n |
+------------+

Consider the following excerpt from a context-free grammar for statements.

`stmt  ::=  ...  | if exp then exp else stmt  |  while exp do stmt`

Define the abstract syntax for the shown productions as a Haskell data type. Assume that a type Exp has already been defined; you can use in your definition.

+------------+
| Question n |
+------------+

Determine the type/behavior of the following expressions under static and dynamic typing. 

`not (tail [x,True])`

+------------+
| Question n |
+------------+

Determine the type/behavior of the following expressions under static and dynamic typing. 

`(head [x,True]) + 1`

+------------+
| Question n |
+------------+

Determine the type/behavior of the following expressions under static and dynamic typing. 

`head [1 `div` x,x]`

+------------+
| Question n |
+------------+

Which sentences can be derived with the following grammar?

`d  ::=  a d a  | b d b  |  ε`

abba
abaaba
aa
abab
aabb
aabaa
