# Quiz 3 Learning Notes

Syntax: Form of a program
Semantics: Meaning of a program

### Semantics
- Curry Paradox: S = if S is true, then Y. Meaningless, not well defined, and essentially recursion without a base case.
- semantics allow us to compare languages, design languages, judge correctness of a program, prove properties about a language...
- For example, the meaning of the language of boolean expressions is a boolean.
- we will use Denotational Semantics, the transformation of representation from abstract syntax to semantic domain.

### Defining Semantics
- define abstract syntax, S, aka syntactic domain
- define semantic domain, D, aka semantic domain
- define the semantic function / valuation  S -> D

### Example with `Expr`
Abstract Syntax Definition in Haskell Metalanguage:
```haskell
data Expr = N Int
			| Plus Expr Expr
			| Neg Expr
```

Semantic Definition in Haskell Metalanguage:
```haskell
sem :: Expr -> Int
sem (N i) 			= i
sem (Plus e e')		= sem e + sem e'
sem (Neg e) 		= -(sem e)
```

Grammar Syntax Definition:
```
Expr ::= Num | Expr + Expr | -Expr
```

Mathematical Semantics Definition:
```
⟦.⟧:Expr -> Int
⟦n⟧			= n
⟦e + e'⟧	= ⟦e⟧ + ⟦e'⟧
⟦-e⟧		= -⟦e⟧
```
> Note: the '+' on the left side is the syntax, while the character on the right is the semantic operation.

### Expressions vs Values
- for the example `tail [1..4],` both the `tail` operation and the `..` are syntactic, and cannot be the result of evaluating the semantics of that operation. The correct answer is `[2,3,4]`

### Advanced Semantic Domains
- for errors, we can use the `Maybe` data type
- for union types, we can use corresponding datatypes.
- for states, we can use function types.

#### Error Domains
- T is type of regular values
```haskell
data Maybe T = Just T | Nothing
```
- Example in use:
```haskell
data Expr = N Int
			| Plus Expr Expr
			| Neg Expr
			| Div Expr Expr
			deriving Show

sem :: Expr -> Maybe Int
sem (N i) 			= Just i
sem (Neg e)			= case sem e of 		-- because e is maybe integer, we cant be sure that e is valid (we could have a dib by 0 err => Nothing)
						Just i 		-> Just (-i)
									-> Nothing
sem (Plus e e')		= case (sem e, sem e') of 
						(Just i, Just j)	->Just (i+j)
											-> Nothing
sem (Div e e')		= case (sem e, sem e') of 
						(Just i, Just j)	-> if j/=0 then Just (i `div` j)
												else Nothing
											-> Nothing
```

#### Union Domains
- when your semantic domain can be two different values, you can use `Either`.
```haskell
data Val = I Int
		`| B Bool

-- can be

type Val = Either Int Bool
```
- expr example
```haskell
data Expr = N Int
			| Plus Expr Expr
			| Equal Expr Expr
			| Not Expr
			deriving Show
data Val = I Int
			| B Bool

sem :: Expr -> Val
sem (N i)			= I i
sem (Plus e e')		= case (sem e, sem e') of
						(I i, I j) -> I (i+j)
sem (Equal e e')	= case (sem e, sem e') of
						(I i, I j) -> B (i==j)
						(B b, B c) -> B (b==c)
sem (Not e)			= case (sem e) of
						B b -> B (not b)
```

#### Function Domains
- if a language operates on a state that can be represented by T, then the semantic domain is a function type of type `T -> T`
```haskell
sem :: S -> D
-- s is syntax, D is domain
type D = T -> T
--creates
sem :: S -> T -> T
-- s is program, start state is first T, out state is second T
```
- essentially, this allows you to set a function type as a semantic domain: `sem :: a -> b -> b == type c = b -> b; sem :: a -> c`
- this allows lambda notation, so you can do this:
```haskell
sem :: <def>
sem (LD i)	_ = i
sem INC		c = c + 1
sem DUP 	c = c * 2

-- can become 

sem :: <def>
sem (LD i) 	= \_ -> i
sem INC		= \c -> c + 1
sem DUP 	= \c -> c * 2

-- or with sectioning

sem :: <def>
sem (LD i) 	= (const i)
sem INC		= (+1)
sem DUP 	= (*2)
```

### Haskell to Mathematical Denotational Semantics
- replace all **type definitions** with **sets** or **CPO's**
- replace all **patterns** with **grammar productions**
- replace all **nonterminals** with **variables**
- replace all **function names** with **semantic brackets** that enclose only syntactic objects.

