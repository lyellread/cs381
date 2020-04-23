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
