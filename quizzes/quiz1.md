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