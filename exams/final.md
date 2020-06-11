# Final Exam

## Question 1

20 points

Consider the following excerpt from a context-free grammar for Python statements. (Note that I am using italics for nonterminals and boldface for terminal symbols.)

```
stmt  ::=  ...
          |   while cond : stmt [else : stmt]
          |   if cond : stmt (elif cond : stmt)* [else : stmt]
 ```

Remember that s* means "0 or more occurrences of s" and [s] means "optional s", that is, 0 or 1 occurrences of s.

(a) Define the abstract syntax for the shown productions as a Haskell data type. Assume that a type Cond has already been defined; you can use in your definition.

For parts (b), (c), and (d) assume that c1 and c2 are Haskell expressions of type Cond that represent the abstract syntax of some Python conditions p and q, respectively, and similarly that s1, s2, and s3 are Haskell expressions of type Stmt that represent the abstract syntax of some Python statements a, b, and c, respectively.

Using your abstract syntax definition in part (a) plus c1, c2, s1, s2, and s3, define the abstract syntax of the following Python statements.


(b)
`while p : a`

(c)
```
if p : a
elif q : b
else : c
```

(d)
```
while p : a
else :
 if q : b
```

## Question 2

20 points

What types or errors will be reported by a static and dynamic type checker for each of the following expressions?

Note:

Make best-case assumptions for the types of all variables used, that is, if possible, assume types for variables that would make type checking work (as much as possible) and be as general as possible.
Assume a strongly typed language.
Mention in your answers the assumptions needed for the types of all variables (x, y, and f).

(a)
`if head x==head y then x else x+1`

(b)
`if f(x)==y then [x] else y`

(c)
`if f(f(x)) then x else f(x)`

(d)
`if f(x) then x+y else not y`

## Question 3

15 points (?)

Consider the following block for parts (a) and (b).

```
{ int x, z;
  x := 3;
  z := 4;
  int f(int z) { return (x+z) };
  { int y;
    y := 6;
    z := 9;
    int g(int x) { return f(1) };
    x := g(11);
  };
}
```

(a) Show the contents of the run-time stack just before the expression x+z is evaluated.

(b) What is the final value of x under static and dynamic scoping right after the last assignment statement?

(c) Consider the following block.

```
{ int x;
  x := 2;
  int f(int y) { return (x+y) }
  int g(int x) { return f(x+1) }
  x := g(4);
}
```

What is the final value of x at the end of the block after the last assignment under static and dynamic scoping?

## Question 4

15 points (?)

Determine what the value of x is at the end of the following program, and show the development of the runtime stack under the different parameter passing schemes. (You should always assume static scoping.)

```
{ int x;
  x := 2;
  int f(int y) {
    x := y*2;
    return y+1
  };
  x := f(x+3)-1;
}
```

(a) Show the evolution of the runtime stack and the final value of x under call-by-name.

(b) Show the evolution of the runtime stack and the final value of x under call-by-need.

## Question 5 

30 points

Note:

Don't define any auxiliary predicates.
Don't use any built-in/predefined predicates other than comparison or arithmetic operators.
Consider the following facts about stores selling items for a certain price.

```
sell(winco,bread,2).
sell(winco,apples,3).
sell(moc,grapes,5).
sell(moc,bread,3).
sell(coop,apples,5).
```

(a) Write down a Prolog goal that computes all stores where you can buy apples for less than 5 dollars.

(b) Define the predicate cheapest/3 that finds for a given item the store with the lowest price for that item. You can assume that each item is available at most in 2 stores.

(c) Define the predicate pay/3 that computes for a given shopping list of items and a given store the total amount that has to be paid for all the items in the list.

(d) Suppose you need to buy bread. Write a single goal using pay/3 that computes what other item you could buy (and at which store) if you want to pay less than 9 dollars.