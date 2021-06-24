# Mutation and Memory

Mil does not have many higher-level language constructs, so mutation is needed
for even basic things like iterating over a vector. Mutation is also useful for
writing performant code that the compiler itself may not be able to do.

The `set!` method enables you to re-write the value of an already defined variable.
The general syntax of set! is
```clojure
(set! <variable-name> <expression>)
```

As an example
```clojure
; Although this expression returns 1, variable i
; remains as 0 throughout its scope.
(let (i 0)
    (+ i 1))

; Variable i's value changes to 1, and nothing is returned
(set-let (i 0)
    (set! i (+ i 1)))
```

The latter example also introduced a new form of let-binding. Note that `let` is an
expression because it always contains an inner expression which gets returned.
However, `set-let`, only contains statements. Because nothing is returned,
set-let itself is also a statement.

General form of a let expression
```clojure
(let ([var expr]) [statement] expr)
```

General form of a set-let statement
```clojure
(set-let ([var expr]) [statement])
```

#### Language Comments
Notice that set! does not expect a memory location, but instead a valid
variable. This is an ergonomic design decision in Mil to block arbitrary memory
re-writing. It is possible in the future that Mil will need more
power/flexibility here, and raw memory setting could be enabled to make
pointer variable types possible. However, it is not clear yet that this is
needed since MelVM's persistent data structure interface makes many cases of
mutation unnecessary.

All variables reference a location in memory (the MelVM heap). This location is
determined when a variable is bound in a let-binding. `set!` allows the
location a variable references to be bound to a new value.

Note that the location a variable references is not mutable. This is stuck for
the lifetime of the binding.

Mil follows a simple incremental scheme for assigning locations to variables.
The first few memory addresses (starting at 0) are reserved for preloaded data
like covenenant hash, etc.. When a variable's scope ends, the memory location
is freed simply by decrementing a memory-head-counter. This is safe because
all variables are scoped; no variables can live past their defined scope.
