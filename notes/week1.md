# Lecture 1 Notes

## Haskell Learning Notes

```haskell
main = do
	--...
```

### General
- prefix operator ``mod 4 3``, comes before values
- infix operator with backticks ``4 `mod` 3``
- constant definition `name :: Type` or `name = val :: Type`
- lists `[a..b]` is valid, will fill in between a and b
- negatives must be enclosed in parenthises `(-12)`
- reading info about function (from `:t` in `ghci`, i.e. `:t sqrt`): `sqrt :: Floating a => a -> a` means it works with floats, and takes one input and outputs one output. I.e. for all `Floating a`, this function does `a -> a`
- `fromIntegral val` returns `Floating` from `Int`
- logical not: `not(a)`
- `repeat n` repeats n forever.
- `replicate a b` replicate b a times
- `show a` prints var `a` as string
- `\=` is the not equal operator.
- `$` denotes precedence - anything after it takes precedence over anything before it

### I/O
- `putStrLn "text"`
- `var <- getLine`

### File I/O
- `fileVar <- openFile "name.txt" WriteMode`
- put data to file with `hPutStrLn fileVar ("lala")`
- read data from file with `contents <- hGetContents fileVar`
- close with `hClose fileVar`

### Lists
- create list: `[val1, val2]`
- concat lists: `list1 ++ list2`
- construct list: `a : b : c : []` will create `[a, b, c]`
- `length a` gives length of list `a`
- `reverse a` reverses list `a`
- check if list is empty with `null a`
- get item at index i in list a: `a !! i`
- `head` (first), `last`, `init` (all but last)
- `take n a` get first n of a
- `drop n a` remove first n of a
- ``b `elem` a`` b in a?
- infinite lists `[1, 2 ..]`
- `cycle a` will replicate a forever.
- `sum = zipWith (+) a b` will sum the lists together.
- `filter (>5) a` will filter a list by elements beign greater than 5
- `takeWhile (<=20) [1, 2 ..]` will keep getting elements from `[1, 2, ..]` until condition not met
- `foldr` and `foldl` (from right or left respectively) will do operation on each element, i.e. `foldl (*) 1 a` will take the product of all items in a

#### List Comprehensions
- simple:  `[3^n | n <- [1..10]]` generates powers of three for each item in `1..10`
- listcomp with `[x * 2 | x <- [1..10], x * 2 <= 8]` will generate `[2, 4, 6, 8]`, more filters can be added separated by commas, i.e. `[x * 2 | x <- [1..10], x * 2 <= 8, x < 4]` or whatever.
- listcomp nesting: `a = [[x * y | y <- [1..10]] | x <- [1..10]]` will generate nested list.

#### List Segmenting
- you can get certain items from a list by using `(x:y:...:xs)` xs is the rest, x is the first element.
- collect the whole parameter in variable `all` using `funcName all@(x:xs)`

### Tuples
- `a = (b, c)` where `b` and `c` do not have to share same type (unlike list)
- first element `fst a` where a is the tuple
- second element `snd a` where a is the tuple
- create tuples from lists `num = [1, 2, 3]`, `chars = ['a', 'b', 'c']`: `zip num chars` --> `[(1, 'a'), (2, 'b'), (3, 'c')]`.

### Functions
- `let funcname x = x^9` will return the ninth power of the number provided.
```haskell
addMe :: Int -> Int -> Int
addMe x y = x + y
```
- blank argument `fname _ = 1337` will return `1337` no matter input

### Guards
```haskell
	funcName :: Int -> Bool
	funcName n
		| n `mod`2 == 0 = False # return false if even
		| otherwise = True
```
- You can include functions within some or all guards bu using a `where` clause: same indent as the last guard, but no guard bar. `where fname = operations`

### Higher Order Functions
```haskell
t4 :: Int -> Int
t4 n = n * 4

listt4 = map t4 [1..10]
```
- `map a b` applies function a to all in b
- Function defenition for a higher order function includes `(passed_func) -> other`, i.e. `(Int -> Int) -> Int` in the case of a function that gets passed a function (first arg) that takes an int, and returns another.

### Lambda Functions
- Defined using the notation: `outer = map (\x -> x * 2) [1..10]` will perform the unnamed function `x -> x * 2` on each element of `1..10` using map.

### Conditional Structures
- if statement: 
```haskell
funcName a = 
	if (condition)
		then b
		else c
```
- case statement: 
```haskell
funcName :: Int -> String
funcName n= case n of
	a -> b
	...
	c -> d
	_ -> -- otherwise
```

### Type Things
#### Enumerated Types
```haskell
data TypeName = val1 | val2 | ... | valn
```
- can be followed by `deriving Show` to ne able to use as string

#### Custom Types
```haskell
data Name = Name t1 t2 ... tn
	deriving show
```
- Instantiate with `varName = Name val1 ... valn`
- retrieve information with a function like ```haskell
get3rd :: Name -> outType
get3rd (Name _ _ a) = a
```

#### Custom + Enumerated Types
```haskell
data Shape = Circle Float Float Float | Rectangle Float Float Float Float
	deriving Show
```
- This can be used by defining functions that depend on type passed: 
```haskell
area (Circle _ _ r) = pi * r ^ 2
area (Rectangle a b c d ) = #...
```

#### Type Classes
- i.e. `Num`, `Eq`, `Show`
- Correspond to sets of types with defined functions
	- ex: `Num` type class is used for `(+)` operator
- defining new type with classes: 
```haskell
data Name = Name { var :: type,
					...
				  	varn :: typen} deriving (Classes)
```
- above, the `Classes` can be type classes that can be used with this type. These can be like `Eq`, `Show` ...
- Using Type Enumeration and overriding builtin type class: 
```haskell
data ShirtSize S | M | L
instance Eq ShirtSize where
	S == S = True
	M == M = True
	L == L = True
	_ == _ = False

instance Show ShirtSize where
	show S = "Small"
	show M = "Medium"
	show L = "Large"
```
- Custom type class: 
```haskell
data ShirtSize S | M | L

class MyEq where
	areEqual :: a -> a -> Bool

instance MyEq ShirtSize where
	areEqual S S = True
	areEqual M M = True
	areEqual L L = True
	areEqual _ _ = False
```

### Modules
- We load modules to get at functions. 
- defined at top of module file in format `module Name (f1, ..., fn) where` followed by function defenitions.


## Lecture Notes

Where `[Int]` would denote a list of ints, `[a]` denotes a list of anyting.
