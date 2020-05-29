# Quiz 3

## Question 2

Evaluate the following expression and write down the result.

```
let x=1
    y=(x,x+1)
 in let x=(y,4)
     in (x,y)
```

> `(((1,2),4),(1,2))`

## Question 3

Consider the following block. Draw the runtime stacks that result immediately after the statements on lines 6, 3, and 7 have been executed.

```
 1 { int u := 2;
 2   { int f(int v) {
 3       u := u*v;
 4       return (u-3);
 5     };
 6     { int u := 3;
 7       u := f(u+2);
 8     };
 9   };
10 }
```

(a) Draw the three runtime stacks (after lines 6, 3, and 7) for dynamic scoping.

(b) Draw the three runtime stacks (after lines 6, 3, and 7) for static scoping.

Answer: 

```
a6 [u:3, f{}, u:2]
a3 [[u:15, v:5], u:3, f{}, u:2]
a7 [u:12, f{}, u:2]

b6 [u:3, f{}, u:2]
b3 [[u:10, v:5], u:3, f{}, u:10] //both u:10 are same var, just one is in function frame for ease of reading
b7 [u:7, f{}, u:10]

//function 'frame' showed for visibility /readability only
```
## Question 4

Illustrate the evolution of the runtime stack from line 3 up until after the assignment on line 8 under the parameter passing schemes Call-by-Reference and Call-by-Value-Return.

```
1 { int x := 3;
2   int y := 2;
3   int f(int p) {
4     x := p*2;
5     p := x-1;
6     return (x+p);
7   };
8   y := f(x);
9 }
```

(a) Call-by-Reference

(b) Call-by-Value-Return

Answer:

```
[x:3]
[y:2, x:3]
[f{}, y:2, x:3]
3: [p->x, f{}, y:2, x:3]
4: [p->x, f{}, y:2, x:6]
5: [p->x, f{}, y:2, x:5]
6: [ret:10, p->x, f{}, y:2, x:5]
8: [f{}, y:10, x:5]

cbvr
[x:3]
[y:2, x:3]
[f{}, y:2, x:3]
3: [p:3, f{}, y:2, x:3]
4: [p:3, f{}, y:2, x:6]
5: [p:5, f{}, y:2, x:6]
6: [ret:11, p:5, f{}, y:2, x:6]
8: [f{}, y:11, x:5]
```