## Functions

All Mil programs consist of a set of function definitions, followed by a single
expression to be executed. A function definiition takes a name, a list of
parameters, and an expression.

The general function syntax is
```clojure
(fn <function-name> (<symbols>) <body-expression>)
```

A full mil program demonstrating function syntax
```clojure
(fn square (x)
    (* x x))

; Returns 42
(square 21)
```

#### Language Comments
The reason functions are defined at the top level only is that functions have
no sense of definition-scope. Unlike on a traditional machine where functions
have a memory location, the MelVM program counter can only move forward, not
backward, and so can't point to a function body in memory.

Instead, functions act more like the Rust/Racket's hygenic macros. All
invocations of a function are expanded to their body. Arguments are
stored in memory and all occurences are replaced with loading from memory.

Expanding on the above example, invoking `(square 21)` would expand to
```clojure
(let (x 21)
    (* x x))
```

As of now, all let bindings are stored on the heap. Bindings are freed from
memory at the end of a function body. As mil becomes more
optimized, this may change to become circumstantial.
