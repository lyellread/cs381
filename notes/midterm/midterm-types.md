# Midterm Learning Notes

Type: Collection of programming language elements that share same behavior.
Type System: Formal system to characterize the types of PL elements and their interactions. Can be used to prove absence of type errors
Purpose of Type Systems: Ensure meaningful interaction among different program parts.
Type Error: Illegal combination of PL elements

### Composition Fallacy
- when you infer in the form: a, b, c are of type A, and they all are blind. Therefore, A must be blind.

### Types are Good Because
- they can assure you of partial correctness as a type checker can provide correctness proofs, and this can eliminate (or at least detect) errors
- can help with documentation, summary, optimization

### Type Safety
- types can be checked, and all type errors can be detected.

### Strong and Weak Typing 
- strong: each value has precisely one type
- weak: values can be interpreted in different types
- all safe languages are strong typed, but not all strong languages are safe

### Dynamic and Static Typing
- dynamic: done while running the program
- static: done while compiling
- for example, consider the following code:
```
if 3 < 4 then True, else "a"

- dynamic: Bool
- static: Type Error
```

