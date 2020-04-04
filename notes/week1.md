# Lecture 1 Notes

## Haskell Learning Notes

```haskell
main = do
	...
```

### General
- prefix operator ``mod 4 3``, comes before values
- infix operator with backticks ``4 `mod` 3``
- constant definition `name :: Type` or `name = val :: Type`
- lists `[a..b]` is valid, will fill in between a and b
- negatives must be enclosed in parenthises `(-12)`
- reading info about function (from `:t` in `ghci`, i.e. `:t sqrt`): `sqrt :: Floating a => a -> a` means it works with floats, and takes one input and outputs one output.
- `fromIntegral val` returns `Floating` from `Int`
- logical not: `not(a)`
- `repeat n` repeats n forever.
- `replicate a b` replicate b a times

### I/O
- `putStrLn "text"` and `var <- getLine` for I/O

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

### Tuples
- `a = (b, c)` where `b` and `c` do not have to share same type (unlike list)
- first element `fst a` where a is the tuple
- second element `snd a` where a is the tuple
- create tuples from lists `num = [1, 2, 3]`, `chars = ['a', 'b', 'c']`: `zip num chars` --> `[(1, 'a'), (2, 'b'), (3, 'c')]`.

### Functions
- `let funcname x = x^9` will return the ninth power of the number provided.
- ```haskell
addMe :: Int -> Int -> Int
addMe x y = x + y
```
- blank argument `fname _ = 1337` will return `1337` no matter input

