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