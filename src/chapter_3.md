# Control flow

Thank to MelVM's persistent data structures, most constructs in Mil are
designed to act expression-like, in that they return something. This was
seen in the built in functions of Chapter 2, and again you'll see that even
if statements are expression-like.

## If statements
The general syntax is
```clojure
(if <test-expression> <true-case> <false-case>)
```

```clojure
; Returns 10
(if (+ 1 0) 10 3)
; Returns 3
(if (+ 0 0) 10 3)
```

An expression can be embedded within the if statement directly because
arguments are evaluated first, and the result of (+ 1 0) will just be a 1 on
the stack. Since MelVM doesn't have native boolean, a 0 as u256 is considered
false for logical functions, and other u256 value to be true.

Under the hood, the if expression just expands to break-if-equal-to-zero (Bez)
and Jmp logic.

## Loops
In order for MelVM programs to have a computable gas cost before they're run,
all loops must have a statically specified upper-bound.

```clojure
; Pushes 5 number 2's to the stack
(loop 5
    (+ 1 1))
```

An example for loop:
```clojure
; Pushes 5 numbers on the stack: 0 2 4 6 8
(let (i 0) (loop 5 (let
    (* i 2)
    (set! i (+ i 1)))))
```

This example also introduces the mutation function, `set!`, which the next
chapter will cover. Another interesting piece in this example is the second use
of `let`. Because `set!` is side effectful and doesn't return a value, we need
a way to express a sequence of operations so that we may "return" something, as
well as do some side effects.

The `let` bind is the only way to execute multiple expressions in a sequence
like this. Note that this same syntax is also used to bind multiple values at once, i.e.

```clojure
(let (i 0
      j 1
      k 2)
    (+ i j k))
```

As always, whitespace here is arbitrary. As long as there is some whitespace,
the compiler will accept it.