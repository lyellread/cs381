# Quiz 2 Learning Notes

### Function Composition
- `funcName xs = f1(f2 xs)` can be rewritten as `funcName = f1 . f2`. 
- safer since we are not handling variables.

### Implicit Argument Passing
```hs
positive :: Int -> Bool

--non implicit
positive x = x>=0

--on the fly constructed function
positive x = (>=0) x

--implicit
positive = (>=0)
```

### Data Constructors
```hs
data Nat = Zero
			| Succ Nat
```
- here, natural numnber `two` can be represented as `Succ(Succ Zero)`. 
- List type can be remade like this, too, wherein
```hs
data List = Empty
			| Cons Type List
```
- here, this creates a linked list (on the heap). When initialized like `xs = Cons 1 (Cons 2 Empty)`, we see a linked list in the format `Cons 1 --> Cons 2 --> Empty`

#### Avoiding Sharing
- Some definitions, like
```hs
data List = Empty
			| Cons Nat List

one = Succ Zero
two = Succ one
ys = Cons one (Cons two Emty)
```
- we have `one` referenced by (~pointed to by) both `two` and `ys`, as `two` is `Succ one`
- can be fixed by replacing `two = Succ one` with `two = Succ (Succ Zero)`
- sharing increases efficiency slightly, but makes garbage collection a bit harder.

#### Cyclic Data Structures
- when a constructor points to itself, or a previous constructor. Some examples
```hs
data List = Empty
			| Cons Int List

ones = Cons 1 ones -- points to itself
morse = Cons 1 (Cons 0 morse) -- points to Cons 1 (...)

zeros = Cons 0 zeros -- that's a lot of 0's
big = Cons 1 zeros -- one then alot of 0's
```

### Ignoring Errors
```hs
head [] --ERROR

ignore :: a -> String
ignore x = "Nope"

ignore(head []) -- "Nope"
```
- in haskell, you pass the whole expression into ignore, which is not evaluated as it is not needed. Therefore, the error creating function is not evaluated.

### Type Derivations
```hs
data Grade = A | B | C | D | F
			deriving Eq ...

instance Show Grade where
	show A = "1.0" 
	...
```
- This will not be on quizzes or tests

### Grammar
- Grammar is given by a set of rules == productions
- production looks like `A ::= B C`, where A makes up the `LHS`, B and C make up the `RHS`, and A, B and C are all symbols, or strings.
- example grammar
```
sentence ::= noun verb noun		(R1)
noun     ::= dogs				(R2)
noun     ::= teeth				(R3)
verb     ::= have				(R4)
```
- `sentence`, `noun`, `verb` above are nonterminal symbols, as they can be replaced by their RHS.
- `dogs`, `have`, `teeth` are terminal symbols, as they cannot be expanded further.
- we can derive sentences from nonterminal symbols, through repeated substitution.  
- empty rules are epsilon rules. They are represented like `bin ::= Є`
- derivation is the process of producing a sentence in accordance with the rules of the grammar.

#### Context Free Grammar
- grammar where LHS contains only one symbol.
- each iteration of LHS is replaced by RHS no matter context

### Syntax Tree
- all leaves are terminal symbols
- all internal nodes are nonterminal symbols
- nonterminal in the tree root indicates the type
- derivation order is not captured in the tree, as we do not care abou the derivation process. All we care about is that the final output is a valid grammar.
- can also replace internal nodes with rulenames

### Abstract vs Concrete Syntax
#### Concrete Syntax
- concrete syntax is the set of all sentences or strings that can be developed from a grammar. 
- it is more verbose, and more readable
- contains extra keywords and symbols to help parsing

#### Abstract Syntax
- abstract view is the set of all syntax trees that a grammar can produce
- more concise
- represents essential language structure
- basis for analysis and transformations

### Abstract Syntax
- abstract syntax tree is synonimous with the value of a haskell data type.
- some syntax trees can be reduced, by removing redundant rules, for example a rule where there is only one possible expansion that has start and end that are in the graph, the intermediate node with rule Rn does not need to be present
- can be represented linearly, in text, where:
```
sentence ::= noun verb noun			(R1)
			| sentence and sentence (R2)
noun     ::= dogs | teeth			(R3, R4)
verb     ::= have					(R5)

Tree:
		R1
	   / | \
	 R3 R5 R4
	/    |   \
  dogs have teeth

Simplified Tree
		R1
	 /   |   \
  dogs have teeth

Linearly
R1 dogs have teeth

More complex (different sentence)
R2(R1 dogs have teeth)
  (R1 dogs have dogs)
```

#### Abstract Syntax and Haskell
- abstract syntax allows us to do do a bunch of interesting stuff, using hashell as a meta language.
- Translation steps:
	- for each nonterminal, define a data type
	- for each rule, define a constructor
- Haskell type names must be capitalized.
- above example is converted as follows
```hs
--sentence ::= noun verb noun			(R1)
--			| sentence and sentence (R2)
--noun     ::= dogs | teeth			(R3, R4)
--verb     ::= have					(R5)

data Sentence = Cons1 Noun Verb Noun | Cons2 Sentence Sentence
data Noun = Dogs | Teeth
data Verb = Have

--to create tree
--		R1
--	 /   |   \
--dogs have teeth

Cons1 Dogs Have Teeth

--to create and sentence like
--R2(R1 dogs have teeth)
--  (R1 dogs have dogs)

Cons2 (Cons 1 Dogs Have Teeth) (Cons1 Dogs Have Dogs)
```
- `Cons1`, `Cons2` are constructors for nonterminals. They can be called `Phrase` and `And`, `R1` and `R2`, ...

### Abstract Grammar
- synonymous with Haskell data type
- contains exactly one unique terminal symbol in each rule, and no redundant rules.
- for example, taking the concrete grammar below, we can make the abstract grammar:
```
Concrete Grammar:
cond ::= T | not Cond (cond)
stmt ::= while cond { stmt }
		| noop

Abstract Grammar:
data Cond = T | Not Cond
data Stmt = While Cond Stmt
		| Noop
```
- while a concrete grammar can generage a set of sentences, an abstract one can produce an abstract syntax tree.

### Concrete Syntax Tree
- created as follows
```
Sentence:
while not(not(T)) {
	while T { noop }
}

Grammar
cond ::= T | not (cond)
stmt ::= while cond { stmt }
		| noop

Tree:
```
![Image](q2-tree1.png)

### Abstract Syntax Tree
- for code:
```hs
While (Not (Not (T))
	(While T Noop)

data Cond = T | Not Cond
data Stmt = While Cond Stmt
			| Noop

Tree:

       While
      /     \
   Not      While
    |      /     \
   Not     T     Noop
    |
    T
```

#### CST to AST
Take the the first terminal of each nonterminal, forget the remaining terminal characters. Replace the nonterminal with the terminal.

### What Makes an Acceptable Data Type for Abstract Syntax
- must represent all sentences in a language, no more and no less.
- names and order of arguments to constructors do not matter.
- one constructor may represent multiple productions.
- there may be multiple ways to represent one sentence in different ways.
```
data Ints = One Int | Join Ints Ints

List [2, 4, 5] can be 
- Join (One 2) (Join (One 4) (One 5))
- Join (Join (One 2) (One 4)) (One 5)
```

### Pretty Printing and Parsing
- pretty printing abstract syntax creates concrete syntax
- parsing concrete syntax creates abstract syntax

### Grammar Rules for Lists (Simplifications)
- a string is a list of characters, i.e. we can condense like this:
```
Concrete
string ::= char string | Є
char ::= a | b | c ...

create string aac

Abstract
data Str = Seq Chr Str | Empty
data Chr = A | B | ...

aac = Seq A (Seq A (Seq C Empty))

But, char array is a string, so abstract syntax can also be either

data Str = Seq [Char] (using builtin char)

or

type String = [Char] (using Char and String builtins)
```
- same can happen with numbers.
- concrete syntax `rule*` means that that rule is a list of that rule, i.e. `rule*` expands to `rule rule* | Є`. This becomes [rule] in abstract.
- concrete syntax `rule+` means there is at least one instance of that rule. This expands to `As ::= A As | A`, and in abstract becomes either rule or [rule].

### Data Types vs Types
- difference:
```hs
type Point = (Int, Int)
(3,5) :: Point 		[OK]
(3,5) :: (Int, Int) [OK]
-- essentially, Point is the same as (Int, Int).
```
- versus
```hs
data Point = Pt Int Int
Pt 3 5 :: Point 	[OK]
Pt 3 5 :: (Int, Int)[ERROR]
```
- data type is used when
	- more than one constructor is needed
	- pretty printing is required
	- representation might be hidden (ADT)

### Grammars to Data Types
1. represent each **basic nonterminal** as a **built in type**
2. for each **other nonterminal**, define a **data type**
3. for each **production**, define a **constructor**
4. **argument types** of constructors are given by the productions **nonterminals**.

- **Each case of a data type must have a constructor (see `N`, `Plus`, `While`, `Noop` below).**

```hs
-- exp  ::= num | exp + exp  | (exp)
-- stmt ::= while exp { stmt }
--		| noop

data Exp = N Int | Plus Exp Exp
data Stmt = While Exp Stmt
		| Noop

-- Num is probably defined somewhere else
-- exp and stmt are nonterminals, so we make data types for them
```